# Síťové služby a aplikace v e Windows server

## pojem Wordl Wide Web (WWW), webový server a server FTP ve Windows server

World Wide Web = systém pro prohlížení propojených hypertextových dokumentů
- Prohlížení pomocí internetových prohlížečů (pro vyhldávání máme vyhledávače)
- pro pohyb mezi nimi slouží URl a hypertextové odkazy
  - URL = uniform resource locator
  - hypertext = typ struktury textu

Webový server
- možnost prohlížení webové stránky
- HTTP port 80
- HTTPs port 443

FTP server
- možnost stahování souborů
- DATA port 20
- příkazy port 21
- možnost ověření pro uživatele, nebo anonymní přístup
- šifrované varianty
  - SFTP = přs SSH
  - FTPS = přes ssl(secure socket layer)

Webový i FTP server je poskytovaný službou IIS (internet information service)
- podporuje FTP, SMTP, HTTP

## Možnost podporý více webů ve webovém serveru, virtuální adresář, apliakce, obor aplikací, ověření a zabezpečení v e webovém serveru

Server defaultně naslouchá na všech svých IP adresách na portech 80/443

Popkud chceme více webů na jednom serveru máme dvě možnosti
- přidat další síťovku s novou IP, na které bude naslouchat jiný server
- host headers, weby budou mít stejnou IP, ale na kterýse připojíme se rozhodne podle hostitelské části url

Virtuální adresář IIS webu se defaultně nachází v `%SystemDrive%\inetpub\wwwroot\`
- jedná se o adresář webu, který odpovídá nějkému fyzickému adresáři
- v podstatě defaultní adresář webu, nachází se v nem soubory jako třeba stránky

Obor aplikací
- v podstatě izolované prostředí pro provoz webových aplikací
- derfinuje paměťovou oblast daného serveru
- pokud má každá aplikace vlastní obor, tak se nebudou navzájem ovlivňovat
- pád jedná z aplikací neovlivní ostaní

Ověření a zabezpečení v IIS
- slouží k ověření klientů přistupujících k webu
- možnosti ověření
  - Anonymous
  - basic authetification = username+pass
  - windows auth. = NTLM a Kerberos(authentifikační protokol) v AD
  - AD client certification auth. = authentizace vůči AD pomocí certifikátu
  - IIS client certification auth. = autentizace vůči IIS pomocí certifikátu
- zabezpečení
  - zabezpečení citlivých dat na serveru
  - omezení přístupu
  - způsob ověření uživatelů
  - šifrování obsahu
  - pomocí autorizačních pravidel je možné povolit/odepřít přístup ke konkrétním webům, aplikacím, souborům...

 ## Vzdálený přístup pře VPN, runelovací protokoly, vzdálená správa serveru

 VPN = virtual private network
 - tunelové připojení přes internet
 - šifruje provoz
 - protokoly
   - SSTP = secure socket tunneling protokol
   - L2TP = leyer 2 tunneling protokol
   - PPTP = point2point tunneling protokol
  
Pro zprovoznění VPN musíme na server i na počítače nahrát cetifikáty pro ověření, aby mohlo dojít ke spojení

Musíme naistalovat službu `Routing and Remote access`

Vzdálená správa srveru
- služba vzdálené plochy přes RDP
- licenční režimy
  - standardní = umožňuje nám až dvě relace, standardní režim přo správu
  - remoteapp = speciální režim, který nám umožňuje spusti aplice v jejím vlastním okně
    - aplikce se jeví, jako by byla spuštěna na našem vlastním PC

Brána vzdálené plochy
- umožňuje připojení pomocí klienta
- používá RDp přes HTTPs

## Technologie Hyper-V

Jedná se o virtualizace systému pomocí type2 hypervizoru
- dostupný od win. server 2008
- je potřeba mít dostatek paměti a výkonu
- umožňuje efektivnější využití HW
- je potřeba povolit virtualizace procesoru
