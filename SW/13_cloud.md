# Cloudová služby Microsoft 365

## Základní hcarakteristiky služby Microsoft 365

Jedná se o cloudovou službu obsahující:
- office
- windows
- enterprise mobility + security
  - řešení pro správu mobilních zažízení, obsahuje entra Id, Microsoft Intune, Azure information protection

existují různé druhy předplatného:
- pro uživatele
- pro firmy
- pro školy
- ...

podpora hybridní práce
- dá se pracovat z kanceláře i z domova = přístup odkudkoliv
- řeší požadavky obojího
  - možnost se kdykoliv připojit
  - zabezpečení = MFA, antimalware, ochrana dat
  - správa zaměstnanců
  - možnosti spolupráce na dálku

## Přehled řešení v oblasti produktivity a spolupráce u služby Microsoft 365

Microsoft copilot
- asistenční nástroj využívající AI
- využívá AI a Microsoft Graph (přístup k datům) = zvyčuje produktivitu
- částečně integrovaný v M365, pro používání v aplikacích je třeba koupit PRO
- dokáže pracovat jak s internetem tak i našimi soubory
- umí vytvářet obsah, shrnout schůzky, dělat zápisy, prezentace...

Výhody v oblasti produktivity
- možnost oužívání officů online
- přístup, úprava a sdílení souborů online
- spolupráce s ostatními uživateli
- plánovací nástroje = to do, planner
- power apps = automatizace

office
- možnost online v prohlížeči, nainstalované na PC
- podporuje i telefony, tablety, MAC
- neustále aktualizovaný

řešení v oblasti spolupráce
- teams
- sharepoint
- onedrive = patří pod sharepoint
- yammer => viva engage (firemní facebook)
- stream = sdílení a správa videí
- microsoft exchange = poštovní server, umí posílat i zprávy
- oulook = integrovaný s kalendářem, plánování schůzek
- microsoft viva

## Možnosti nasazení a aktualizace aplikací Micrsoft 365

Možnosti nasazení a instalace M364 apps
- uživatelé mohou instalovat jednotlivě co potřebují = **fuj**
- nástroj configuration manager = stáhnout z místního zdroje
- z cloudu pomocí ODT (office deployment tool) = používá xml soubor, dá se vytvořit přes Office Costumization Tool
  - instalace ze sítě CDN = content delivery network
- z místního zdroje pomocí ODT
- automatická instalace z cloudu

aktualizace
- nové funkce se vydávají pravidelně
- typy: funkcí, zabezpečení, kvality

aktualizační kanály
- current channel = doručují se okamžitě, vychází až 3krát do měsíce
- monthly enterprise channel = jednou měsíčně hromadná aktualizace, druhé úterý v měcísi
- semi anual enterprise channel = aktualizace hromadně v lednu a červenci

## Správa koncových uzlů,pricipy správy a možnosti nasazení ve služby Microsoft 365

M365 nybízí ucelené řešení správy koncových bodů. Umožňuje zařízení na dálku:
- zabezpečit
- monitorovat
- spravovat

Microsoft Intune
- správa zařízení
- cloudově orientované řešení pro správu koncových uzlů, přístupu uživatelů a firemních prostředků
- dokáže řešit soulad se zásadami, instalovat aplikace, aktualizace...
- pro správu se pak používá intune admin center
  - přidání zařízení
  - vytvoření uživatele
  - přidání zásad

Configuration manager
- on-prem řešení pro správu on-prem zařízení
- dá se propojit s intune
- lze použít pro nasazení aplikací, aktualizací, analýzu stavu

co-managemnet
- kombinace configuration manageru a Intune
- přístup přes webové rozhraní (endpoint manager)

tenant-attach (v podstatě registruje zařízení do Intune)
- umožňuje nahrát data o zařízení do cloudu a spravovat je online přes konzoly
- zajištění zabezpečení pomocí Intute přes admin center

endpoint analytics
- testuje zařízení
- poskytuje informace o výkonu a "zdraví" zařízení
- dokáže odhalit i hardwarové chyby

windows autopilot
- cloudová služba
- pomocník při instalaci a konfiguraci nových zařízení
  - pro instalaci Windows

Microsoft Entra Id
- Intune používá pro správu uživatelů, zažízení a skupin

