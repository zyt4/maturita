# Licencování a správa aplicí v UNIX-like operačníshc systémech

## Licence Open Source a filozofie UNIX-like operačních systémů

Proprietární SW má uzavřený kód a podmínky pouřití předepisuje licence.

Open source SW nemá omezený způsob použití a máme volný přístup ke zdrojovému kódu.

Můžeme používat k libovolnému účelu, šířit, upravoat, kombinovat s jiným SW...

Vznik hnutí je v 2. polovině 20. století s myělenkou že open source je lepší a spolehlivější

UNIX
- znikl 1969, současně ochranná známka
- mohou používat jen cetifikované systémy
- proto vznikly UNIX-like systémy

LINUX
- vyvíjen dobrovolníky = neni zodpovědná žádná organizace
- cílem je varianta UNIXU zdarma
- jádro = kernel, napsáne bez žádného proprietární SW
- vydává se v podobě distribucí = balíčky SW tvořící jeden systém
- různé distribuce
  - RedHat = vyvýjí a prodává RedHat enterprise linux (free verze je fedora)
  - SuSE = placená verze (free je open SuSE)
  - Debian = nejrozsáhlejší verze, vyvíjena komunitou
  - Ubundu = distribuce debianu, orientovaná na desktopy
  - Arch, Kali, Mate...

## Práce s nápovědou

Linux obahuje veškerou dokumentaci pro konfiguraci a používání systému.

Jedná se převážně o nápovědo pro používání příkazů.

V Linuxu máme 4 zdroje nápovědy:
- Man pages
  - jedná se o manuál pro daný příkaz
  - obsahuje nejen volby pro daný příkaz ale podrobnější vysvětlení využití a někdy příklady
  - `man <příkaz>` (dá se použít i na sebe, man man)
- Volba --help/-h
  - vypíše zkrácenou nápovědu k příkazu
  - jendá se o syntaxy příkazu jednoduchý popis voleb jaké má
  - `ls --help`
- Dokumentace v /usr/shace/doc
  - každý naistalovaný nástroj má svůj adresář (ne defaultní jako ls)
  - může obsahovat dodatečné informace, které nejsou v man
  - dále obsahuje šablony pro konfiguraci/konfigurační soubory
- GNU info
  - velmi podrobné jako man pages
  - obsahuje navíc i hypertextové odkazy pro navigaci v dokumentaci
  - nemusí být defaultně nainstalovaný
  - `info ls`

## správa balíčků v UNIX-like operačních systémech

Správci bylíčků se dělí na low-level a high-level

Rozdíl je ten, že low-level nainstaluje pouze ten balíček, který mu řekneme, což může selhat, pokud nemám dependencies(veškerý potřebný SW pro jeho instalaci a fungování)

Proto máme high-level správce, kteří kromě balíčku, který chceme instalují i ostatní potřebný SW pro jeho fungování

Každá distribuce má své vlastní správce balíčků:
|distribuce|low-level|high-level|
|:---:|:---:|:---:|
|debian a odvozeniny|dpkg|**apt-get** / aptitude|
|CentOS|rpm|yum|

Instalace můžeme provádět buď z internetu, nebo z kompilovaných balíčkových souborů(*.deb*)
- z internetu: `sudo apt-get install tree`
- z balíčku pomocí dpkg: `sudo dpkg -i tree.deb`

Výpis aktuálních balíčků: `dpkg -l`

Vyhledání inforamcí o balíčku: `dpkg --search <file_name>`

Aktualizace: `sudo apt update && sudo apt upgrade`

Odstranění balíčku: `sudo apt remove <package_name>`
