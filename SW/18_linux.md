# Konfigurace a správa služeb DNS, DHCP a LDAP v UNIX-like operařních systémech

## DNS

DNS provádí překlad doméných jmen na IP a naopak

Pokud se jedná o pár překladů můžeme je zapsat do /etc/hosts
- syntax: ```<IP> <název> <pokud tak aliasy>```

Pokud se jedná o větší sítě, potřebujeme DNS server

V linuxu je nejčastějším deamonem bind/berkley

Stáhneme si balíček s názvem `bind9` (pro testování se hodí dnsutils)

obecná konfigurace v souboru `/etc/bind/named.conf.options`

```
options {
        directory "/var/cache/bind";

        // forwarders { # když nevím kde se má zeptat
                        10.20.1.12;
                        10.20.1.13;
        };                                                    
        dnssec-validation no;
        listen-on {192.168.1.1; 10.0.50.1;}; # kde nasloucá
        allow-query {192.168.1.0/24; 10.0.50.0/24;}; # od koho příjímá dotazy
        forward only;
        listen-on-v6 { any; };
};
```

zóny se definují v souboru `/etc/bind/named.conf.local`
```
// dopřední z ip na doménové jméno
zone "maturitaformalira.cz" { // název zóny
        type master;
        file "/etc/bind/db.maturitaformalita.cz"; // zápis překladů
};

//192.168.1.0/24 => 0.1.168.192 => 1.168.192.in-addr.arpa
zone "1.168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/db.1.168.192.in-addr.arpa";
};

//10.0.50.0/24 => 0.50.0.10 => 50.0.10.in-addr.arpa
zone "50.0.10.in-addr.arpa" {
        type master;
        file "/etc/bind/db.50.0.10.in-addr.arpa";
};
```

konfigurace dané zóny je pak vždy v jejím souboru:

Na začátku souboru musí být obecná část:

```
; BIND db file for test

$TTL 86400

@       IN      SOA     ns.test.com.      ja. (
                        2024051801	; serial number YYMMDDNN
                        28800           ; Refresh
                        7200            ; Retry
                        864000          ; Expire
                        86400           ; Min TTL
			)

                NS      ns.test.com. 


$ORIGIN test.
```

SAO(start of autority) = definuje halavní server, kde se nachází informace o zóně

typy záznamů:

- A = IP
- AAAA = IPv6
- CNAME = jedno se přeloží na druhý
- NS = utoritativní server domény
- MX = mailový server

Dopředný překlad

```
$ORIGIN maturitaformalita.cz.
;A záznam
;domain_name TTL CLASS A ip_address
;ns.maturitaformalita 86400 IN A 192.168.1.1
ns A 192.168.1.1
ns A 10.0.50.1
www A 10.0.50.2

; CNAME záznam (překládání doménového názvu na jiný)
mysql CNAME www
gw CNAME ns
; (mysql = www = 10.0.50.2)
```

zpětný překad = záznam je jen PTR
- název vygenerujem tak, že otočíme IP a odebereme tu část, která se mění

```
$ORIGIN 1.168.192.in-addr.arpa.

;PTR záznam
;ip_opačně ttl class ptr domain_name
;1.1.168.192.in-addr.arpa. 86400 IN PTR ns.maturitaformalita.cz.
;1.1.168.192.in-addr.arpa.       IN PTR ns.maturitaformalita.cz.
;1.1.168.192.in-addr.arpa.          PTR ns.maturitaformalita.cz.
;1                                  PTR ns.maturitaformalita.cz.
1 PTR ns.maturitaformalita.cz. ; 192.168.1.1
```

kontrola: nainstalujeme dnsutils

použijeme příkaz `dig`

```
dig @koho se ptam na co se ptám
dig @192.168.1.1 www.maturitaformalita.cz
```

## DHCP

Jedná se o balíček `isc-dhcp-server`

Nastavení je v souboru: `/etc/dhcp/dhcpd.conf`

```
# obecné informace
option domain-name "maturitaformalita.cz";
option domain-name-servers 192.168.1.1, 10.0.50.1;

default-lease-time 600;
max-lease-time 7200;
ddns-update-style none;

# definování subnetů
subnet 10.0.50.0 netmask 255.255.255.0 {
 range 10.0.50.10 10.0.50.254; # adresy
 option routers 10.0.50.1; # default gateway
}

subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.10 192.168.1.254;
  option routers 192.168.1.1;
}

# rezervace adresy
host server {
  hardware ethernet 08:00:27:b4:cf:ce;
  fixed-address 10.0.50.2;
}
```

Má dvě části
- globální parametry = pro všechyn stejné (lease time, doména, servery...)
- deklarace = definuje síťovou topologii (jednotlivé subnety a jejich nastavení)

## LDAP

Pro přístup k adresářovým službám se používá standart LDAP(lightweight directory access protokol)

Adresářová služba vytváří sdílenou architekturu pro přístup a správu objektů(uživatelů, skupin...)

Základní prvek LDAP je **záznam**
- jednoznačně idetentifikovaný přes DN (distinguished name)
- záznam má atributy a ty mohou mít více hodnot
- atributy jsou označeny řetězci: cn(běžný název), mail(emailová adresa), dc(doména)

Instalujeme open LDAP kde balíček se jmenuje `slapd` a `ldap-utils`

Po instalaci máme k dispozici několik defaultních schémat v /etc/openldap/schema

záznamy se píšou do samostatných souborů typu `user.ldif`:
```
dn: uid=testuser, ou=People, dc=example,dc=com
objectclass: account
gidNumber: 1005
homeDirectory: /home/testuser
loginShell: /bin/bash
```

Pro použití LDAP na klientovy je třeba nainstalovat knihovny pro připjení

Většinu provede balíček `ldap-auth-config`
- je potřeba zadat id serveru, název báze a verzi
- údaje se uloží do /etc/ldap.conf
