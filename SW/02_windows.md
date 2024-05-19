# Základní charakteristika a možnosti správy klientské verze operačního systému Windows

## Základní charakteristika OS windows, přehled verzí, popis záklandích prvků grafické rozhraní

Windows je proprietární OS společnosti Microsoft

Jedná se nejpoužívanější OS na světě

Funguje převážně v GUI, má i CLI, ale většina uživatelů ho neumí používat

Verze Windows

![alt text](https://github.com/zyt4/maturita/blob/9f81d0007035302407d94e1adaf6b11958b2c561/obrazky/verze_windows.png)

Edice Windows
- Home = vhodné pro běžné uživatele
- Pro = rozšířený o Remote desktop, BitLocker a HyperV
- Enterprise = založené na Pro, všechny funkce, které windows umí
- Education = založené na Enterprise, vhodné pro školy

Základní grafické prvky
- Jedná se o klasické GUI prvky = rolovací menu, talšítka, toolbary...
- Hlavní panel
  - nachází se na spodní části obrazovky
  - je zde tlačítko start = pomocí něj se dostaneme do dalších částí windows
  - dále jsou zde ikony právě otevřených aplikací, datum a čas
- Plocha
  - největší část obrazovky
  - výchozí bod pro práci se soubory a plikacemi
  - na ploše jsou ikony, zástupci aplikací, složkyl...
- Okna
  - každá aplikace se spouští ve vlastním okně
  - může se jednat o rozhraní formuláře nebo komplexnější
  - okna lze zavřít, roztáhnou nebo minimalizovat
  - lze je volně přesouvat po pracovní ploše
  - okna se mouhou překrývat, aktivní okno je vždy na vrchu
- Kontextové menu
  - Windows a většina aplikací podporuje otevření kontextového menu kliknutím pravým tlačítkem
  - nabízí další funkcionality
  - nepostradatelné při práci se soubory = přejmenování, další možnosti práce

## Možnosti správy OS pomocí systémových náístrojů v grafickém rozhraní a v prostředí CMD

Nový Windows 11 pracuje převážně s aplikací nastavení.

Ve starších windows se většina nastavení prováděla přes ovládací panely (stále to jde)

Části ovládacích panelů
- otevření buď přes vyhledávání nebo příkaz control
- Systém a zabezpečení
- Síť a internet
- Hardware a zvuk
- Programy
- Uživatelské účty
- Vzhled a přizpůsobení
- HOdiny a oblast
- Usnadnění přístupu
- Každá část spravuje část OS
  - někdy jsou na ně propojená další okna pro správu
 
Nastavení
- postupně nabývá funkcionalit ovládacích panelů
- nabízí další možnosti přizpůsobení
- systém
- Bluetooth a zařízení
- síť a internet
- Přizpůsobení
- Aplikace
- Účty
- Čas a jazyk
- Hraní
- Soukromí a zabezpečení
- **Windows update**

Další možnost správy je přes MMC(microsotf management consol) konzoli
- správa věcí do větší hloubky
- prostředí do kterého přidáváme snap-in moduly podle toho, co právě potřebujeme
- komplexní správa jako např group policy
- využívá se i na win. serveru

Spravovat windows můžeme i přes přákazovou řádku

Máme na výběr dve možnosti: CMD a PowerShell

Spravovat můžeme
- uživatele `net user`
  - můžem přidávat, odebírat, měnit hesla, zobrazovat informace...
- `ipconfig`
  - umožňuje zobrazit aktuální síťovou konfiguraci
  - můžeme pomocí něj měnit ip konfigurace
- `ping`, `tracert`
  - ověření konetivity se zařízením
- `Powercfg`
  - umožňuje nám nastavovat napájení
  - můžem upravovat a mazat schémata, dokáže generovat souhrn o baterce
- `cd`, `del`, `dir`...
  - příkazy pro navigaci systémem

 ## Správa diskův OS windows

 Pro správu diků používáme nástroj disk management

 Umožňuje nám různé operace s disky
 - zobrazení stavu
 - přiřazení nebo změna písmen
 - přidání disků
 - přidání diskových polí
 - nastavení oddílů

stavy disků
- cizí = dynamický disk byl přesunut z jiného PC
- v pořádku = svazek funguje tak jak má
- inicializuje se  = zákadní disk se převádí na dinymický, vytváří se MBR
- chybí = disk chybí nebo je poškozen

Optimalizace disku
- nástroj pro defragmentaci disku přesune data, která k sobě patří na stejné místo
  - zvyšuje rychlost hledání dat na HDD
- optimalizace SSD zajišťuje jejich vlastní řadič

Kontrola chyb
- nástroj kontrola chyb kontroluje integritu souborů a složek
- nástroj opravuje chyby v souborovém systému a vyhledává vadné sektory na disku
- z vydných sektorů se pokouší obnovit data
