# Správa a řízení služeby Azure

## správa nákladů v Azure, chrakteristika nástrojů pro správu nákladů v Azure

Pro kalkulaci nákladů náme dvě kalkulačky:
- TCO = total cost of ownership
  - Porovnává naše aktuální náklady za on-prem s přechodem do cloudu
- pricing calculator = podle aktuálního zatížení a ceny odhaduje cenu dál

Cost manager = pořehled nákladů (kolik, za co, kdy...)
- můžeme nastavit limity, které nesmí překročit
- při překročení nám to pošle oznámení

## Nástroje a funkce Azure pro správné řízení a zajištění souladu

Značkování prsotředků:
- protředkům přidáme značku = název a její hodnotu
- podle značek se dají prostředky hledat
- pro správu prostředků
- pro správu nákladů
- pro správu operací
- zabezpečení (podle zanček určujeme požadavky)
- podle norem, kterých se týkyjí
- vytváříme přes: portál, CLI, PowerShell, ARM šablony...

Azure blueprints
- dokáže vytvořit kopii dané předplatného (kompletní kopie se vším všudy)
- artefakty = sem se ukládá konfigurace prostředků
- dají se verzovat
- dobré pro experimentování, zkoušení, nebo jen zálohování

Microsoft Purview
- práce s daty = správa, třídění podle zanček...
- zajišťuje soulad s M365
- hledání, třídění, zajištění...

Azure policy
- zásady a pravidla pro konfiguraci prostředků
- konfigurace musí být v souladu se standarty
- lze vytvářet na různých úrovních
- iniciativa = příbuzné zásady nebo skupiny zásad
  - hesla, přístupnost, aktualizace...

service trust portal
- pro soulad s vnějšími politikami (nařízení vlády, EU...)
- poskytuje podrobné info o mechanismech a postupech zajištění souladu
- přístup k nástrojům a obsahu, co vše Azure dělá

## Nástroje a funkce Azure pro správu a nasazování prostředků

Portal Azure
- grafické prostředí
- umí vše = vytvářžení, správu a likvidac prostředků

Azhure cloud shell (bash/PowerShell)
- příkazový řádek v portálu
- když chceme dělat věci na webu ale přes příkazový řádek

Azure PowerShell/Azure CLI
- samostatná aplikace pro správu přes dané prostředí(windows/linux)

Azure Arc
- zvládá kompletán správu prostředí
- má přesah i do multicloudu
- takový all-in-one(VM, kubernetees, databáze...)

ARM šablony
- JSON soubory
- jedná se infrastructure as a code = celá infrastruktura je popsaná kódem, dokud něni deploynutá

zámky prostředků
- znemožňují upravovat konfiguraci/smazat prostředek
- dá se udělat odkudkoliv
- dá se použít na cokoliv = prostředek, skupina...
- jdou dědičný
- dva typy
  - delete = můžeme dělat cokoliv, jen ne smazat prostředek
  - readonly = můžeme se podívat na konfiguraci, ale s prostředkem nic nemůžeme dělat

## Nástroje pro monitorvání prostředí Azure

Azure advisor
- nástroj pro vyhodnocování využívůání prostředků Azure
- navrhuje možnosti pro zlepšení: výkonu, optimalizace ceny...
- cílem optimalizovat výkon

Azure service health (služby píoskytované Azurem)
- umožňuje sledovat stav služeb
- azure status = obraz celkového stavu všech služeb v Azure ve všech oblastech(říká o výpadcích a jak nás postihnou)
- service health = konkrétnější pohled na služby, které využíváme a info o nich (odstávky, výpadky...)
- resource health = info o kontrétních stavech prostředků

Azure monitor
- platforma pro shromažďování a anylýzu dat
- dokáže i on-prem a multicloud
- sbíraná data jsou na nás

Azure log abalytycs
- nástroj pro zadávání dotazů na data v Azure monitoru
- včetně analýzy dat

Azure monitor alerts
- posílá upozornění při překročení nějaké nastavené hodnoty
- podmínky posílání jsou na nás
- pro stanovení, koho má kontaktovat jsou action groupy
  - každá skupina obsahu nějaké události, a vždy kontaktuje všechny lidi v dané skupině

application insights
- funkce monitoru pro monitorování webových aplikací
- dokáže sledovat výkon, posílá vlastní požadavky, pri zjištění aktuální stavu aplikace
  - během nižšího provozu




