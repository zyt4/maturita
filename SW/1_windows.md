# OS - základní pojmy, instalace, upgrade a spouštění OS windows

## Účel OS, multiuser OS, multitasking, multiprocessing, multithreading

OS je základní SW vybavení 
- spravuje a řídá HW a SW pomocí driverů
- poskytuje služby a alokuje HW pro počítačové programy
  - funguje jako sprostředkovatel mzi HW a zbytkem
- jedná se o první program který je zaveden a poslední se vypíná
- jádro OS se nyzývá kernel

Typy
- desktop = klasika windows
- mobilní = android, iOS...
- servery = stejný jako desktop, ale pro servery
- realtime OS = specifické, např řízení strojů v továrně

Multiuser OS
- OS, které umí pracovat s více uživately najednou
- na každém účtu lze pracovat s programy, aplikacemi a periferiemi současně a samostatně
  - windows klienti (připojení více lidí, pomocí vzdálených ploch)
  - windows server (stejným způsobem)

Multitasking
- umožňuje souběžné zpracování úloh v jednom okamžiku
- je to pouze zdárný = ve skutečnosti běží vždy jen jedna, a rychle se třídají
  - přepínání mezi processy se nazývá změna kontextu

Multiprocessing
- podpora práce dvou CPU současně
  - zahrnuje i jádra počítače

Multithreading
- aplikace může být rozdělena do více vláken
  - ty se zpracovávají nezávisle na sobě
- vyžaduje podporu ze strany OS

## Základní funkce OS

Správa HW prostředků
- komunickace mezi HW a aplikacemi
- k tomu využívá ovladače zařízení = drivery
  - umožňují aplikacím ovládat Hw, např grafický driver na GPU
- detekce HW a intalace driverů probíhá automaticky (plug and play)

OS nám poskytuje uživateské rozhraní
- CLI = command line interface
  - textové rozhraní v podobě příkazového řádku
  - ve windows se jedná o cmd nebo PowerShell
- GUI = graphical user interface
  - grafické rozhraní v podobě oken, ikon...
  - hlavní rozhraní windows

Správa souborů a složek
- organizace a přístup k datům na disku
- OS vytváří na disku strukturu pro ukládání dat = filesystem
  - většina podporuje soubory a složky
- samotná struktura se pak tvoří na disku

Správa aplikací
- nalezení aplikací na disku
- při spouštění, načtě aplikace do RAM
  - načtení potřebných knihoven
- správa procesů
  - přiděluje jim samotné HW protředky

## Možnosti isntalace a upgradu OS windows

čistá instalace
- první instalace
- není ovlivněna předchozími soubory, záznamy...
- disk je před instalací zformátovaný

Klonování
- Záloha konfigurace OS z daného počítače
- vytvoří se bitová kopie disku, která se dá nahrát a spustit na jiném PC
- vhodné pro instalaci hodně stejných PC

Obnova do továrního nastavení
- v podstatě provedení čisté instalace s možností zanechat vybrané dokumenty
- dá se udělat z bootovacího média přes možnost opravit

Síťová instalace
- instalace ze sítě pomocí USB
- vzdálená instalace
  - instalační soubor se nachází na serveru
  - klientský PC se k němu připojuje přes síť
  - s klientem pak komunikuje speciální SW balíček (RIS remote instalation service)
    - zajistí uložení souborů, poskytuje klientovy přístup ke všemu potřebnému
  - PXE = preboot execution enviroment
    - prostředí pro spuštění počítače, které zajistí: spuštění základního prostředí, připojení k síti, komunikaci se serverem
- bezobslužná instalace
  - isntalace pomocí vytvoření isntalačního souboru, který vyplní veškeré dotazy
  - dobré pokud potřebujeme intalovat hodně zařízení
  - k vytvoření se používá system image manager

Další možnost je intalace z obrazu na disku
- soubor se většinou nachází na skrytém oddílu

Upgrade
- vylepšuje OS na novější verzy(typ)
  - home na pro
  - win 10 na 11
- update = aktualizace v rámci stejné verze OS
- dá se udělat pomocí čisté instalace s migrací uživatelských dat
  - k tomu slouží: user migration tool, windows easy transfer, PCmover express
- další možnost je in-place upgrade
  - dojde k upgradu OS bez ztráty dat
  - instalační process aktualizuje veškeré komponenty a nastavení

Edice Windows
- Home = vhodné pro běžné uživatele
- Pro = rozšířený o Remote desktop, BitLocker a HyperV
- Enterprise = založené na Pro, všechny funkce, které windows umí
- Education = založené na Enterprise, vhodné pro školy

## Popis spouštěcí sekvence OS windows

![alt text](https://github.com/zyt4/maturita/blob/473e84661efb632080195a474f20525c7db7a7c0/obrazky/spousteni_windows.png)

1. Jako první se inicializuje EFI (EFI files)
2. To zkontroluje HW
3. Pak se spustí bootmanager bootmgr.exe
  - najde zavaděč Windows jako takového = winload.exe
4. Zavaděč Windows (winload.exe) vyhledá potřebné ovladače pro spuštění kernelu
  - následně se spustí kernel
  - využívá se k zavádění jenotlivých komponent a pro zavedení z disku do RAM
5. Kernel načte do RAM systémový registr a ostatní ovladače, které se mají spustit se systémem
6. Kernel předá kontrolu procesu `session manager` (smss.exe)
  - spustí relaci systému
  - načítá a spouští zařízení a další ovladače, které nebyly označeny jako BOOT_START
