# Příkazy pro správu linuxu

## vytváření souborů a složek

vytvoření složek `mkdir`

- možnost `-p` pro vytvoření všechsložek v cestě
- pro vytvoření více složek najednou je dáme do `{}` oddělené `,`
  - pokud to uděláme pro více levelů oddělými `/` tak se vytvoří všechny kombinace
  - `{}` se dají vkládat do sebe, dá se před ně vytknout společná část
  - pokud chceme více čísel, dá se nahradit pomocí `..` bez čárek mezi
```bash
mkdir -p {pokus,nevim}/{test{1..99},testik{0..55}}
```

k vytvoření souboru můžeme použít `nano` a rovnou ho naplnit obsahem

nebo můžme použít `echo "něco" > test.txt` a přesměrovat výpis do souboru

- `>` přepíše obsah
- `>>` připíše obsah na konec
- `<` použije cestu jako zdroj

## archivace

pomocí `tar` a dalších optionů (stačí napsat tar a dár tab, ono nám to nabídne co dál):

- `f` následně zadáme název výsledného souboru
- `t` vypíše obsah vybraného archivu
- `c` vytvoří archiuv
- `x` extrahuje obsah archivu
- `--gzip` zvolíme metodu komprese
- `-C` zvolíme cíl, kam chceme extrahovat
- na konec pak napíšeme, jaké soubory/složky chceme archivovat oddělené mezerou
  - podle toho, jakou adresaci použijeme, tak budou zapsaný v archivu

```bash
tar cf test.tar.gz --gzip test1 test2
tar ct test.tar.gz
tar xf test.tar.gz -C /kamchci
```

bitová kopie `dd`

- `if` input file
- `of` output file
- `bs` block size
- `count` počet bloků

```bash
dd if/dev/urandom of=/dev/key bs=1k count=4
```

## správa uživatelů a skupin

vytvoření uživatele `useradd`
nodifikace uživatele `usermod`
zmazání uživatele `userdel`

optiony (username se píše nakonec):

- `-m` make/move hemo directory (default: /home/username)
- `-d path` cesta k adresáři
- `-s shell` určení shellu (/bin/bash)
- `-g group` primární skupina(vlastnictví, kvótování)
- `-G group1,group2` sekundární skupiny
- `--help` zobrazení nápovědy
- `-a` append, připojení při přidání sekundárni skupiny
- `-p` určení hesla
- přidání hesla: `sudo passwd username`
  - vynusení změny hesla `-e` (expire)
  - `-l` uzamkne učet
- expirace účtu pomocí příkazu `sudo chage -E YYYY-MM-DD username`
  - `-l` vypíše informace o účtu

vypsání informací o uživateli:

- `id usrname` řekne nám to id skupna, kde uživatel je
- `/etc/passwd` informace o uživatelích
- `/etc/shadow` uživatelská hesla
- `/etc/groups` informace o skupinách

vytvoření skupiny `groupadd`
nodifikace skupiny `groupmod`
zmazání skupiny `groupdel`

## oprávnění

změna oprávnění `chmod`

r = 4, w = 2, x = 1

změna číslem: napíšeme číselně jaká oprávnění to má mít (pořadí: owner, group, other)

```bash
chmod 764 test.txt
```

další možnost je písmenem (u = user, g = group, o = others, a = all)

`+`přidává, `-` odebírá, `=` přímo nastavuje

dají se nastavovat najednou odělený `,`

```bash
chmod u=rwx,g+w,o-r test.txt
```

změna vlastnictví `chown`

- `chown user:group path` nastaví uživatele i skupinu
- `chown user path` nastaví jen uživatele, skupinu nechá
- `chown :graou path` nastaví jen skupiny, uživatele nechá 

vypání oprávnění `getfacl` nebo `ls -l`

## práce s disky a souborovými systémy

### práce s diskem

rozdělení disku na oddíly: `sudo fdisk /dev/sdb` , disk musí být typ gpt

práce s disky:
- `p` informace o disku (aktuální stav tabulky)1
- `g` vytvoří novou gpt tyble
- `n` vytvoří nový oddíl, pak postupujeme podle promptu
- `w` zapíše změny na disk
- `d` smaže oddíl

### práce se souborovými systémy

vytvoření filesystému: `mkfs.typ_systému disk`
```bash
sudo mkfs.ext4 /dev/sdb2
```

zobrazení filesystému na oddílu:`blkid`

připojení ke složce: `mount co kam`

```bash
sudo mount /dev/sdb2 /mnt/test
```
automatické mountování: píše se do /etc/fstab

```
<systém, buď cesta nebo UUID(blkid),nebo label> <kam to připojím> <filesystem> <možnosti> <dump> <pass>
/dev/sdb3 /mnt/testik ext4 defaults 0 1
LABEL="mujswap" none swap defaults 0 0
```
mountování pak provedeme pomocí `sudo mount -a`

vytvoření swap oddílu: `mkswap`, pak je potřeba aktivovat pomocí swap on

```bash
mkswap /dev/sdb4
swap on

# nebo můžeme použít složku
# nejprve alokujeme místo na disku a pak ho použijeme
fallocate -l 20M /mnt/misto
mkswap /mnt/misto
```

vytvoření popisku: `fsystémlabel`

- ext4 = e4label -L název cesta
- swap = swaplabel -L název cesta

metadata:

zobrazení pomocí `dumpe2fs`
úprava metadat `tune2fs`

## pokročilá práce se souborovými systémy

### LVM

- pv = physical volume
- vg = volume group
- lv = logical volume(něco jako oddíly, ale logicky)

nejprve musíme naformátovat oddíly jako pv: `pvcreate cesta`

pak je musíme přidat do jedné skupiny: `vgcreate název pv_cesta`

nakonec vytváříme logické svazky: `lvcreate --name="název" --size=100M skupina`

uložený jsou pak v `/dev/mapper` ve tvaru `skupina-svazek`

s logic volume pak pracujeme stejně jako s každým oddílem

### RAID

vytvoření raidu `mdadm`:

- `--create` pro vytvoření
- `-v` jak se bude jmenovat výsledný disk
- `-l` úroveň raidu (0,1,5...)
- `-n` kolik se použije disků

```bash
mdadm --create -v /dev/md1 -l 0 -n 2 /dev/sdb50 /dev/sdc2
```

pro automatický raid: `mdadm --detail --scan` to se uloží do /etc/mdadm/mdadm.conf

### Kvóty

v /etc/fstab je potřeba změnit options na `usrquota,grpquota`

pak musíme vytvořit databázi kvót: `quotacheck -cug cesta_k_souboru` (Create User Group)

přidání kvót:

- `edquota -u username` pro uživatele
- `edquota -ut` mění dobu překročení soft limitu pro všechny
- `edquota -g group` pro skupinu
- `edquota -gt` mění dobu překročení soft limitu
- `quotaon cesta` zapne kvóty

Editor:

- blocks = místo (100M,250M...)
- inodes = soubory (2000,3650...)
- soft lze překročit na čas
- hard = nelze překročit
- time = slovy (minutes,hours,days...)

## konfigurace služeb: DHCP, DNS, SSH

pro sptrávu služeb máme `systemctl` a `journalclt`

```bash
systemctl start/restart/stop/status název_služby
journalctl -xeu název_služby
```

### nastavení sítě

přes grafickou konzoli `nmtui`

informace si můžu zobrazit přes `ip a`

routy jsou přes `ip r`

### DHCP

jedná se o balíček `isc-dhcp-server`, stáhneme pomocí sudo apt install

nastavení je v souboru: `/etc/dhcp/dhcpd.conf`, odstranění řádku pomocí `CTRL+K`

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

### DNS

balíček se jmenuje `bind9`

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

na začátku souboru musí být obecná část. vygenerujeme [online](https://pgl.yoyo.org/as/bind-zone-file-creator.php)

typy záznamů:

- A = IP
- AAAA = IPv6
- CNAME = jedno se přeloží na druhý

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

### SSH

ssh user@ip

konfigurace v souboru: `nano .ssh/config`

```
host gate
hostname 10.0.50.12
user fresh
# pak stačí napsat ssh gate
```

pro přeskočení hesla vygeneruje klíč pro připojování: `ssh-keygen`

pak ho nakopírujeme na dený stroj: `ssh-copy-id cíl_ip/zkratka`

přes ssh můžeme tunelovat komunikaci. další řádek v configu
```
localforward kam co
localforward localhost:1080 192.168.34.2:443
```
> přistupujeme k tomu pak jako na vlastní localhost

## konfigurace Apache a MariaDB

### apache

balíček se jmenuje `apache2 libapache2-mod-php`

pak vezmem stránky z /etc/apache2/sites-available a uděláme kopie:

```
sudo cp 000default.conf maturitaformalita.cz.conf #http
sudo cp defaultsssl.conf maturitaformalita.cz.ssl.conf # https
```
> v nich se pak upravuje nastavení stránek

http konf:
```
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/mysite/maturitaformalita.cz # kde jsou stránkové soubory
        ServerName www.maturitaformalita.cz #url
	   #Redirect / https://www.maturitaformalita.cz #možné přesměrování
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

https konf
```
<VirtualHost *:443>
        ServerName www.maturitaformalita.cz # url
        ServerAdmin webmaster@localhost
        DocumentRoot /var/mysite/maturitaformalita.cz #kde jsou soubory

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        SSLEngine on

        SSLCertificateFile      /etc/ssl/certs/maturitaformalita.cz.crt #certifikáty
        SSLCertificateKeyFile   /etc/ssl/private/maturitaformalita.cz.key

        <FilesMatch "\.(?:cgi|shtml|phtml|php)$">
                SSLOptions +StdEnvVars
        </FilesMatch>
        <Directory /usr/lib/cgi-bin>
                SSLOptions +StdEnvVars
        </Directory>

</VirtualHost>
```

pak je třeba vygenerovat ssl certifikáty (přepíšeme názvy a cestu klíčů):

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```

ssl je třeba povolit pomocí `sudo a2enmod ssl`

potřeba je také povolit stránky: `sudo a2ensite stránka` 

a zakázat default stránku `sudo a2dissite 000-default.conf`

nakonec je třeba povolit náš adresář v souboru /etc/apache2.conf na 170 řádku

### MYSQL

balíček se jmenuje: `mariadb-server`

pak naistalujeme pomocí `sudo mysql_secure_instalation` a vždy dáme yes

zapnutí databáze a připojení: `sudo mysql`

pak už pracujeme v sql prostředí:
```SQL
create database test; #vytvoří databázi
describe test; #popíše databázi
use test; #přesuneme se do databáze
create table persons(parametry); # vytvoří tabulku
show tables; #vypíše tabulky
describe persons; # popíše nám sloupce tabulky

create user "admin@%" indentified by "superheslo"; #vytvoří uživatele, za zavináčem je odkud se může připojit, % je odkudkoliv
grant all on test.* to admin@%; # přidáme mu veškerá oprávnění

#přihlášení pomocí tohoto uživatele: mysql -u admin -p 
```

adresa na které server naslouchá se dá změnit v souboru: /etc/mysql/mariadb.conf.d/50-server.cnf

pak už jen změníme bind-adress na to co co chceme

## Firewall

[routing](https://wiki.archlinux.org/title/simple_stateful_firewall)

tables>chains>rules

policy (součást chains) = defaultní akce

chainy = input(pro mě) output(ode mě) forward(přeposílám)

konfiguruje se v iptables

iptables-safe = aktuální sejf

iptables-restore = nahraje do aktuálního firewallu

konfigurace v souboru: `/etc/iptables/rules.v4`

tabulky začínají *

chainy začínají :

polityky jsou hned za tím (default je accept)

- `-A` appent KAM (přidáme k nějakému chainu)
- `-o` output
- `-i` input
- `-j` akce
- `-m` modul (multiport, state...)(state --state=RELATED,ESTABLISHED)(dports --dports=80,443)
- `-p` protokol (icmp, tcp, udp...) (icmp --icmp-type=echo-request)
- `-d` destinace packetu
- `-s` zdroj packetu
- povolení natky v nat (-A POSTROUTING -o enp0s3 -j MASQUERADE)

zapsání a pravidel

iptables-save = uloží pravidla

iptables-retore</etc/iptables/rules.v4 (nahraje daná pravidla do aktivního firewallu)
