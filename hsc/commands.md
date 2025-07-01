# vysvětlivky

módy
- žádný
- enabled
- configure terminal
- configurace interface (dá se mezi nimi přeskakovat bez použití exit mezi)

enabled slouží převážně pro show, nahrání konfigu nebo věci jako ping
> dají se dělat i od jinud, ale je potřeba před daný příkaz dá `do <příkaz>`

Když nevim co dál je potřeba tak `?` za to co jsem zatím napsal vypíše mi to možnosti

Seznam je dělaný jako konfigurace, módy jsou implikovaný předchozímy příkazy

Příkazy se dají zkrátit na minimální délku, kdy je jednoznačné o jaký příkaz se jedná např.
- configure terminal => conf t
- ip address => ip add
- switchport mode access => sw m a

# basic config
enable

show running-config

show startup-config

show ip interface

show ip interface brief

show ipv6 interface brief

show ip route

show ipv6 route

copy running-config startup-config

write

erase startup-config

reload // reloads device (deletes running config)

configure terminal

line vty 0 15

password cisco

login

exec-timeout 1 30

exit

line console 0

password cisco

login

exec-timeout 1 30

exit

enable secret cisco

service password-encryption

login block-for 180 attempts 3 within 60

security passwords min-length 10

hostname R1

banner motd #Authorized Access Only!#

interface gigabitethernet 0/1

description Link to R2

ip address 192.168.1.1 255.255.255.0

ipv6 address 2001:db8:acad:1::1/64

ipv6 address fe80::1 link-local

no shutdown

exit

ipv6 unicast-routing

ip default-gateway 192.168.1.1

hostname S1

line vty 0 15

password cisco

login

login local

exit

interface vlan 1

ip address 192.168.186.1 255.255.255.0

no shutdown

exit

ip domain-name cisco.com

crypto key generate rsa

1024

username admin password cisco

transport input ssh
ssh version 2

# vlan config
## switch

conf t

vlan 10

name STUDENT

exit

int f0/5

switchport mode access

switchport access vlan 10

int g0/1

switchport mode trunk

switchport trunk alloved vlan 10,20

switchport trunk native vlan 999

exit

int vlan 10

ip add 192.168.0.1 255.255.255.0

exit

ip routing (L3 switch)

## router

int g0/0.10

encapsulation dot1Q 10 (vlan number)

ip add 192.168.0.1 255.255.255.0

no shut

# dhcp config
conf t

ip dhcp pool LAN-POOL

network 192.168.0.0 255.255.255.0

default-router 192.168.0.1

dns-server 192.168.0.2

domain-name example.com

ip dhcp excluded-address 192.168.0.1 192.168.0.10 => all between

exit

int g0/0

ip helper-address 192.168.1.9 

no service dhcp

show ip dhcp binding
# dhcpv6
conf t

int g0/0

ipv6 nd other-config-flag = slaac only

end

int g0/0

ipv6 nd managed-config-flag

ipv6 nd prefix default no-autoconfig

end

show ipv6 int g0/0 | begin nd

ipv6 unicast-routing

ipv6 dhcp pool [IPV6-stateless]

dns-server 2001:db8:acad:1::254

domain-name example.com

exit

int g0/0

ipv6 nd other-config-flag

ipv6 dhcp server IPV6-stateless

no shut

ipv6 dhcp pool IPV6-stateful

int g0/0

ipv6 nd managed-config-flag

ipv6 nd prefix default no-autoconfig

ipv6 dhcp server IPV6-stateful

no shut

client ipv6 address dhcp

int g0/0

ipv6 dhcp relay destination 2001.db6:acad:1::2 g0/1

exit

show ipv6 dhcp pool

show ipv6 dhcp binding

# etherchanel
conf t

int range f0/1-4

channel-group 1 mode active

exit

int port-channel 1

sw m t

sw t a v 10,20,30

exit

# fhrp/hsrp
conf t

int g0/0

standby version 2 (1 neni pro ipv6)

standby 1 ip 192.168.1.254

standby 1 priority 150

styndby 1 preempt

# port security
conf t

int f0/1

switchport mode access

swithport port-security

switchport port-security maximum 4

switchport port-security mac-address 11:22:33:44:55:66

switchport port-security mac-address sticky

switchport port-security aging time 10

switchport port-security aging type inacitvity

switchport port-security aging type absolute

switchport port-security violation protect|restrict|shutdown

exit

show port-security

show port-security int f0/1

int g0/1

sw m t

sw nonegotiable

sw t n v 999

ip dhcp snooping

int f0/1

ip dhcp snooping trust

exit

int range f0/2-10

ip dhcp snooping limit rate 6

exit

ip dhcp snooping vlan 5,10,50-52

show ip dhcp snooping

show ip dhcp snooping binding

ip dhcp snooping

ip dhcp snooping vlan 10

ip arp inspection vlan 10

int f0/10

ip dhcp snoping trust

ip arp inspection trust

exit

ip arp inspection validate src-mac

ip arp inspection validate dst-mac

ip arp inspection validate ip

int f0/2

sw m a

spanning-tree portfast

exit

spanning-tree portfast default

int f0/3

spanning-tree bpduguard enable

exit

spanning-tree portfsast bpduguard default


show spanning-tree summary
# routing
## next hop

ip route <co podle cílové ip> <kam next hop ip>

ip route 192.168.1.0 255.255.255.0 172.16.2.2

ipv6 unicast-routing

ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2

## directly connected

ip route 192.168.1.0 255.255.255.0 g0/0

ipv6 unicast-routing

ipv6 route 2001:db8:acad:1::/64 g0/0

show ipv6 route

## fully specified

ip route 172.16.1.0 255.255.255.0 g0/0 172.16.2.2

ipv6 route 2001:db8_acad:1::/64 g0/1 fe80::2

show ip route static

show ip route 192.168.0.0

show running-config | section ip route

## default route

ip route 0.0.0.0 0.0.0.0

ipv6 route ::/0

ip route 0.0.0.0 0.0.0.0 172.16.1.1 

ipv6 route ::/0 2001:db8:acad:2::2

## static floating route

ip route 0.0.0.0 0.0.0.0 172.16.0.1

ip route 0.0.0.0 0.0.0.0 10.10.10.2 5

ipv6 route route ::/0 2001:db8:acad:2::2

ipv6 route route ::/0 2001:db8:feed:10::2 5

## host routes

ip route 209.165.200.238 255.255.255.255 198.51.100.2

ip route 209.165.200.238 255.255.255.255 s0/1/0

ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2

no ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2

ipv6 route 2001:db8:acad:2::238/128 serial 0/1/0 fe80::2

# acl
> vytvoření pravidla pro číselný list
  
access-list access-list-number {deny | permit | remark text} source ip [source-wildcard] [log]

= remark je poznámka

> vytvoření jmenného listu nebo vstoupení do jeho konfigurace (čísla jsou taky jména)

ip access-list standard access-list-name

= zároveň nás dostane do konfigurace daného listu

= pro čísla je místo jména standard číslo

> konfigurace v jmenném listu

{deny | permit | remark text} source ip [source-wildcard]

= už se nezadává jaký list

> aktivace na interface (platí i pro extended)

ip access-group {access-list-number | access-list-name} {in | out}

> zobrazí acl listy

show ip access-lists

= zobrazí i jejich obsah (i seq. number pravidel)

> upravení acl (pouze v módu pro editování acl)

no seq. number

seq. number pravidlo

> vyčistit počítače pro použití pravidel

clear access-list counter jméno listu

> aplikovat acl na vty lines (v konfiguraci linek)

access-class {access-list-number | access-list-name} { in | out }

> extendet acl vytvoření v global

access-list číslo (100-199)

ip access-list extended jméno (dá se použít i číslo)

> extended acl config command

access-list access-list-number {deny | permit | remark text} protocol source ip source-wildcard [source operator {port}] destination ip destination-wildcard [destination operator {port}] [established] [log]

= operator lt=less then, gt=greater then, eq=equal, neq=notequal

= established je pouze pro tcp

= protokoly (icmp, tcp, udp, ip=filtruje pouze podle ip)

= keyword (místo portu jde použít jeho službu)(ftp, domain, telnet, www...)

> zbytek konfigurace je stejný jako u standard

# NAT
> statická NAT spuštění

ip nat inside source static <inside local> <inside global>

ip nat inside source static 192.168.10.254 209.165.201.5

> přiřazení rozhraní na inside/outside

int g0/0

ip add 192.168.10.1 255.255.255.0

ip nat inside/outside

> zobrazení nat

show ip nat translation(s) [verbose]

show ip nat statistics

## dynamická NAT

> vytvoříme nat pool

ip nat pool <název> <první adresa> <poslední adresa> netmast <která část adresy patří k host části>

ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmast 255.255.255.224

> uděláme acl abychom povolili opuze adresy, které mají být přeložené

access-list 1 permit 192.168.0.0 0.0.255.255

> spojíme je dohromady

ip nat inside source list <acl pro filtr zdroje> pool <pool pro outside adresy>

ip nat inside source list 1 pool NAT-POOL1

#nastavíme rozhraní jako outside/inside

int g0/0

ip nat inside/outside

> vymazání překladů

clear ip nat translation *

## PAT
> akorát přidáme oveload na konec nat konfigu

ip nat inside source <acl list> <exit int nebo ip>

ip nat inside source list 1 interface g0/0 overload

> nastavíme interface na inside/outside

### PAT with pool

> zase se jen přidá overload

ip nat inside source list 1 pool NAT-POOL2 overload

> a pak se nastaví inside/outside int
# OSPF
conf t

router ospf 10

network ipsitě reverse mask area oblast

passive-interface loopback 0

router-id 1.1.1.1

default-inforamtion originate //defaul routa se rozešle ostatním

auto-cost reference-bandwith 10000

int g0/1

ip ospf 10 area oblast

ip ospf priority 200

ip ospf hello-interval seconds

ip ospf dead-interval seconds

exit

// v enabled režimu

clear ip ospf process

show ip ospf neghbour

show ip protokols

show ip ospf int

pro OSPFv3 se vždy před dá ipv6

u souter configu pouze před proces

= ipv6 router ospf 10
