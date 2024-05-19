# Adresářová struktura Active Directory

## Základní pojmy, AD, pojmy spracovní skupina a doména, role FSMO, globální katalog, úroveň funkčnosti

Jedná se o adresářovou službu, pracující na LDAP protokolu

Obsahuje a poskytuje přístup k informacím v adresářích
- informace jako: tiskárny, uživatelé, skupiny, zařízení...
- informace jsou řazeny hiearchicky

Active Directory je vyvinuta Microsoftem
- řadič domény
- pracuje na LDAP
- ověření Kerberos a SSO
- používá názvy založené na DNS = vyžaduje DNS

Logická struktura
- domény = na rozdíl od workgroupy, počítače jsou klienti a server je jen jeden
  - workgroup = každý je klient/server, pokud někam chceme, žádáme počítač, který tam je připojený
- strom = víc domén dohromady (domény mají sdílenou hranici a nemůžou mít stejný názvy)
- les = víc stromů, nejvyšší organizační jednotka
- Pokuch chceme povolit přístup do jiné domény, musíme v AD mít vztah důvěrnosti

Fyzická struktura
- tvořena z míst a řadičů domény
- místo
  - jedna nebo několik podsítí
  - návzájem propojeny
  - definováno podle zeměpisného umístění
- řadič domény
  - windows server
  - definuje hranice domény
  - obsahuje kopie údajů jako účty a zabezpečení

Správa AD
- pomocí nástrojů v ovládacích panelech (administrativ tools) nebo jejich modulů v mmc
- AD users and computers
- AD domains and trusts
- AD sites and services
- AD administrativ center
- group policy management console

FSMO = flexible single master operation
- AD požívá multi master replication = neni žádný hlavní řadič domény
- některé funkce může v jednu chvíli dělat jen jeden řadič domény = k tomu je FSMO
  - schema master = spravuje schéma AD
  - domain naming master = spravuje přidávání/odebírání domén
  - PDC emulator = primary domain controler = funguje jako hlavní server pro změny hesel a času
  - RID master = alokuje unikátní identifikároty při vytváření objektů
  - infastructure master = synchronizuje změny vlastnictví skupin

Globální katalogy
- replikují údaje každého objeku ve stromu a v lese
- vytvářejí se na prvním řadiči lesu
  - může ho obsahovat kterýkoliv řadič

Úroveň funčnosti
- záleží na jaké verzi win. serveru právě běží
  - funkčnost se odvíjí od verze OS

## Základní objeky AD a jejich charakteristika, zásady skupiny

Jednotlivé prvky jsou reprezentovány objekty
- počítače, uživatelé, skupiny...
- každý objěkt má své atributy(těm jsou následně přiřazeny hodnoty)
- každý objekt má své GUID

Objekty
- uživatelské účty
  - umožňují uživatelům se přihlásit do PC a domény
  - ověřují identitu uživatele
  - zajišťují oprávnění
  - dají se auditovat
  - existují doménové a lokální
- Účty počítačů
  - slouží k auditování aktivity
  - každů počítač má svůj účet
- skupiny
  - množina účtů
  - využívají se ke zjednodušení správy
  - typy skupin
    - distribuční skupina
    - skupina se zabezpečením
  - rozsahy skupin
    - globální
    - univerzální
  - používá se skupina (Accounts Global Domain Local Permissions)
  - integrované skupiny
    - domain admins
    - domain users
    - account operators
    - authenticated users
    - everyone
    - součástí jsou i přednastavená oprávnění

Zásady skupin
- centralizovaná správa a konfigurace uživatelů v AD
- pomocí zásad skupin můžeme přiřazovat pravidla (délka a stáří hesla)
- při nasazení mají prioritu
  1. místní zásady
  2. zásady skupiny na úrovni místa
  3. zásady skupiny na úrovni domény
  4. zásady skupiny na úrovni organizace

## Konfigurace oprvánění na úrovni souborového systému a na úrovni sdílení

Oprávnění na souborovém systému NTFS se dají přiřadit uživatelům nebo skupinám
- full control
- měnit
- číst a spouštět
- číst
- zapisovat

Každý soubor má svého vlastníka = ten má vždy full control

Typy oprávnění
- explicitní
  - přidělují se přímo souboru/složce
- zděděná
  - nastavená na nadřezené složce
- efektivní
  - výsledná oprávnění po sečtení explicitních a zděděných
  - odepření má přednost před přístupem a explicitní před zděděnými

Při přesunu souboru získává oprávnění složky, kam je přesunut/zkopírován (to platí i pro přesun mezi disky)

Sdílení složek
- pokud chceme přidělit uživatelům přístup ke složkám na serveru
- na dílení složek se vážou další oprávnění, která mají stejnou strukturu
  - full control, měnit, číst

## Správa místních a síťových tiskáren

Typy tiskáren
- místní = může být sdílená
- síťové

Tisk
- fyzická tiskárna(tiskové zařízení) = je třeba připojit a zapnout
- logická tiskárna = SW rozhraní mezi aplikací a fyzickou tiskárnou
  - jsou třeba ovladače pro tiskárnu, abychom mohly komunikovat

Instalace tiskárny
- přes nastavení > bluetooth a zařízení > tiskárny a skenery > přidat zařízení
- přes ovládací panely > hardware a zvuk > zařízení a tiskárny > přidat zařízení
- nainstalovaná tiskárna se pak zobrazuje v rámci správce zařízení
- přes TCP/IP spojení možnés přes: USB, COM...

Oprávnění tiskárny
- pro tiskárny můžeme nastavit oprávnění uživatelům a skupinám
  - tisknout
  - spravovat tiskové úlohy
  - spravovat tiskárnu

Instalace síťové tiskárny
- nainstalovat roli přes internet
- správa tiskárny probíhá přes webový prohlížeč a aplikaci

Fondy tiskáren = možnost dát více tiskáren do jednoho sernamu, uživatel uvidí jednu logickou tiskárnu ale může se jednat ox fyzických

Tisknout pak může z které koliv z nich, jelikž sdílejí tiskové úlohy

