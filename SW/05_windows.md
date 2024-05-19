# Správa uložišť ve Windows Serveru

## Serverová uložiště - typy, charakteristika a využití

technologie uložišť
- SATA = serial ATA (nejšastějí v pc)
- SAS = seral attached small computer system interface, rychlejší než sata, používá se v datacentrech
- MVME = disky připojené přes PCIe sběrnici
- RAID = možnost spojení více disků dohromady
- SAN = storage area network, speciální síť, která propojuje disky a PC a umožňuje přístup k nim
- NAS = network attached storage, zařízení připojené k síti, slouží pro ukládání a sdílení dat v rámci sítě

Typy disků
- HDD = levný, velký obsah, pomalejší
- SSD = dražší, rychlý, stabilnější
- SSHD = kombinace, SSD se používá jako casch

## Disková pole RAID

RAID = redundant array of independent discs

Může být HW i SW

Hotspare = disk, který je k RAIDu připojený a použije se, pokud jeden z disků selže

RAIDy
- 0
  - Stripping
  - Spojuje dva a více disků do jednoho, provádí load balancing
- 1
  - Mirroring
  - Redundance, ukládá dvě kopie dat, na každý disk jednu
- 5
  - Parita
  - Nejedná se o přímou kopii, při selhání disku se pomocí parity dají dopočítat data
  - min tři disky
- 1+0
  - stripping a mirroring
  - min čtyři disky

## Diskoví oddíly, typy disků, typy svatků, souborové systémy, správa disků

Diskové oddíly
- logické i fyzické
- disk může mít cív logických oddílů
- oddíl je dán počtem sektorů
- windows podporuje MBR (primárně) i GPT

Svazky
- svazek na rozdíl od oddílu má filesystem
  - můžeme do něj ukládat data
- typy
  - jednoduchý
  - rozdělený = více fyzických do jednoho logického
  - raidy

Souborové systémy podporované Windows
- FAT16
  - jednoduchý, jeden svazek může mít max 2GB, byl v MS-DOS
- FAT32
  - max 32GB na svazek, používán od win95
- NTFS
  - new technologiy file system, jeden svazek až 16exabytů
  - dnes přeferovaný, odoslonst vůči chybám (podporuje journaling)
  - umořňuje bitlocker
- exFAT
  - vhodný pro USB
- ReFS
  - optimalozovaný pro scaling na serverech, aktivně opravuje chyby

 správa disků
 - hlavní nástroj je `správa disků`
 - možné spravovat i přes CMD pomocí `DISKPART`
 - umožňuje
   - inicializovat disky(MBR, GPT)
   - spravovat standartdí a dinamické disky a převod mezi nimi
   - spravovat asvazky
   - přidávat písmena ke svazkům
   - formátovat disky, měnit objem svazků
