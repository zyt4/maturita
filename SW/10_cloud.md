# Popis architektury a základních služeb MIcrosoft Azure

## Hlavní komponenty architektury Azure

Architektura Azure se dělí na dvě části:

**fyzická** infrastruktura
- oblast = datacentra s nízkou latencí propojení
- availibylity zone = fyzicky oddělená centra(zařízení), mají samostatné napájení, internet, budovu...
- region = několik zón propojených dohromady v podbné lokaci, když jedna zóna vypadne, další ji zaskočí
- region pair = propojení dvou regionů pro zvýšení spolehlivosti(hlavně dat)

struktura **pro správu**
- resource = jednotlivé služby, mohou být jen v jedná resource groupě
- resource group = dělí resource, mohou být jen v jednom předplatném
- předplatné = vázané k účtu nebo management groupě, jedná se platební údaje
- management groupy = slouží k hromadné správě předplatných
  - účet se přidělí k managemnt groupě, a tím k předplatný, která tam jsou

## Výpočetní služby Azure

VM = virtuální počítače (IaaS)
- můžeme si vše navolit
- škálovací sada(vytváří vlastní VM, nepracuje s existujícími):
  - slouží k jednoduchému škálování vm
  - škáluje počet VM v závislosti na poptávce(námi nastavený parametr)
  - hlavně big data a kontejnery
- doména dostupnosti
  - VM se dělí do domén dostupnosti, aby např při aktualizaci se jedna aktualizovala a druhá běžela

Azure virtual desktop (PaaS)
- OS Windows s multi user modém
- jednoduše klientský windows bez správy OS
- Remote Desktop Protokol

Azure Containers
- Azure containers v podobě PaaS
- Azure kuberneties (pro správu více kontejnerů)
- kontejner je v podstatě SW balíček, který obsahuje vše pootřebné k tomy, aby nějaký program mohl běžet
  - dá se spustit odkudkoliv, jednoduše se přesouvá

Azure functions
- **bezserverová** architektura
  - řešim pouze že se kód vykoná, nenřešim kde
- platí se pouze za čes, kdy funkce běží
- stateless = vždy se spouští na novo, nepamatuje si předchozí
- statefell = pamatuje si výstup z předchozího spuštění

Azure app services
- slouží pro vytváření a hostování webových aplikací
- dají se generovat api klíče

## Síťové služby Azure

Zajišťují komunikaci mezi prostředky a uživatelem, umí rozšířit on-prem o azure prostředky.

klíčové funkce síťí
- izolace a segmentace = je možné vytvážet izolované sítě a různě je segmentovat
- komunikace s internetem = každý prostředek může mít svou ip
- komunikace mezi prostředky azure
- komunikace s on-prem
  - point2site = klienti se připojí přes VPN do Azure VPN gateway
  - site2site = propojení on-prem gateway s azure gateway pomocí vpn (jeví se to jako by se jenalo a jednu síť)
  - Azure express route = vyhrazené šifrované spojení internetem pro azure
- směrování provozu = můžeme si udělat vlastní routování
  - pomocí protokolu BGP lze routovat i mezi sítěma
  - pravidla se dají nastavit pro zařízení nebo pro celou skupinu (jen packetově a portově)
  - můžeme použít i deditkovný AzureFirewall(funguje i na aplikační vrstvě)
- filtrování síťového provozu = network security group (v podstatě zjednodušený firewall)
- propojení virt sítí = pomocí peeringu(privátní spojení, které nejde do internetu)
- zařízení může mít jak privátní tak i veřejnou IP

VPN
- jedná se o resource v dané síti, zajišťuje šifrované tunelové spojení
- datacentrum k virt. sítím = site2site
- zařízení k síti = point2site
- virt. sítě k sobě = network2network
- dělení:
  - policy based = pro každý tunel se zadá IP a podle toho se posílají a šifrují packety
  - routed based = tunely IPsec mají podobu rozhraní, defunuje se stejně jako v routingu = rozhraní a next hop (převýžně point2site,multisite)
- odolnost:
  - vytvoření dvou instancí (active/standby)
  - dvě instance v režimu active/active = lepší load balancing
  - VPN slouží jako záloha k express routě
  - zone-redundant gateways = nasazení gateway do různých zón dostupnosti

Azure express route
- drahé
- soukromá šifrovaná linka pro spojení s Azure
- pro propojení poboček
- nevyužívá veřejný internet

Azure DNS
- překlad názvů prostřednictvím Azure infrastruktury
- podpora aliasů
- podpora jmen pro privátní virt. sítě

## Služby uložiště Azure storage

Prostředek se nazývá storage account, ten následně obsahuje další dělení, k datům se přistupuje přes url.

redundance uložišť
- LRS = local redundant storage
  - vytvoří tři kopie dané souboru v jené zóně
  - změny jsou synchronizované v real time
  - redundance primárná zóny
- ZRS = zone redundant storage
  - vytvoří tři kopie, každou v jiné zóně dostupnosti
  - změny jsou synchronizované v real time
  - redundance primární zóny
- GRS = geo redundant storage
  - vytvoří LRS jak v primární tak i v sekuntádní lokaci
  - asynchronní synchronizace
- GZRS geo zone redundant storage
  - v primárním regionu udělá ZRS a v sekundárním LRS
  - asynchronní synchronizace

typy ulořišť
- blobs = univerzální (bin, img, txt, csv...)
  - podporují big data(IoT dat)
  - přístup přes url
  - urovně přístupu = HOT(velmi často), COOL(cca jednou za měsíc), ARCHIVE(alepoň jednou za půl roku)
  - podle toho se odvíjí ceny přístupu k datům (čím levnější cena za přístup, tím dražší cena za uložení)
- files = v podstatě emulace sdílených disků
  - podporuje SMB pro windows tak i NFS pro linux
  - lze připojit v cloudu i on-prem
  - pro uložení obsahu sdílené složky do cashe serverů se dá použít azure file sync
- queus = fronty dat a dotozů pro aplikace
  - můžné přistupovat přes http
  - max velikost záznamu je 64KB
  - pro asynchronní předávání dat

 Azure Disks = samostaná resource
 - virtuální disky pro tvorbu VM

migrace dat
- Azure migrate = pomocník migrace dat z on-prem do cloudu
  - jednotná platforma pro celistvou migraci dat
- Azure data box = fyzický přesun velkého oběmu dat(převáží se disk s daty na něm, až 80TB)
- AzCopy = příkaz v příkazovém řádku, funguje jako cp
- Azure storage explorer = grafický nástroj pro správu blobů a souborů
- Azure file sync = synchronizace souborů na všech serverech


