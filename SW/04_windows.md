# Model klient/server u operačních systému, operační systém Windows server

## Pojmy server a klient, výhody modelu klient-server u operačních systémů, role serveru

Server = počítač, který poskytuje služby 
Klient = počítač, který využívá/požaduje služby

Výhody modelu klient server
- ideální pro sdílení prostředků
- bezpečnější = klienti ví jen to, co server poskytne, můžeme kontrolovat jaké služby v síti jsou
- jednodušší správa, jedná se o centralizovaný systém

Možné role serveru
- souborový
- webový
- poštovní
- databázový
- síťový = DNS, DHCP
- ...

## Kritéria pro správný výběr serverových komponent, základní subsystémy serveru, virtualizace serveru

Jako první je třeba určit jakou roli server bude mít
- kolik uživatelů k němu bude přistupovat
- jaké množství dat bude zpracovávat
- kolik služeb na něm poběží

Podle toho volíme vhodný HW
- CPU = rychlost,počet jader a vláken, kompatibilita
- RAM = kompatibilita se základní deskou, velikost, rychlost, error correction mode
- uložistě = HDD, SSD, M.2...
- síťové připojení = počet rozhraní, rychlost
- základní deska = hlavně kompatibilita a rachlost
- napájení = počet kabelů, efektivita, výkon, redundance

Virtualizace serveru
- umožňuje mít víc serverů na jednom HW
- může sloužit k segregaci služeb = vyšší bezpečnost
- dva typy hypervizoru
  - typ 1 = na HW běží systém pro virtualizaci
    - výhodnější při více VM
    - efektivnější
  - typ 2 = běží jako program v systému
    - lehká správa
    - bere prostředky systému, na kterém běží

## Charakteristika operačního sstému Windows server - edice, role a finkce

Založený na Windows NT

Je verzovaná podle let: Win. Server 2016, 2019, 2022

edice
- datacentrum = pro datacesntra acloudy, omezený na počet jader
- standard = fyzická minimálně virtualizovaná prostředí, omezený na počet jader
- essentials = malé podniky, omezeno na jeden server

Role
- Active Directory Domain Services
- DNS server
- DHCP server
- Web server IIS(internet information service)
- File and Storage server

Funkce **nejsou** vázané na role
- containers
- group policy management
- routing services
- windows defender

## Možnosti licencování, aktivace a aktualizace Windows Server

Licencování
- OEM = svázaná s PC, používaná výrobci
- Retail = přenositelná z PC na PC
- Hromadná licence = pro více PC, vhodné pro firmy, školy...

Instalace windows server je stejná jako klasického windows
- čistá instalace
- upgrade ze starší verze
- přes obraz disku
  - používá se sysperp
- bezobslužná instalace
  - uživatel nemusí nic dělat, má xml soubor, který vyplní veškeré volby za něj

Aktivace windows se provádí po isntalaci pomocí aktivačního klíče, který ověřuje legální použití systému
