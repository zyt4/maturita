# Sledování a řešení problémů v operačním systému Windows server

## Monitorování činnosti, řešení problémů v eWindows serveru, nástroje

Windows server nabízí několik programů (utilit) pro sledování výkonu

Můžeme se k nim dostat pře ovládací panely(administrative tools), nebo jako moduly do konzole `mmc`

- Event veiwer
  - umožňuje nám procháze záhnamy toho, co se v systému dělo
    - přihlášení, spuštění plikací, změna politik...
  - je faultní část OS
- Sytem information
  - podorbné indforamce o počítači
  - Jak o HW tak i o SW
  - obsahuhje informace o ovladačích
- Performance monitor
  - poskytuje analýzu výkonu počítače
  - sledování výkonu v real time
  - dokáže vytvářet reporty
- Resource monitor
  - zobrazuje zatížení systému a jednotlivých tasků
- Řešení problémů
  - knihovna ITIL (information technology infrastructure library)
  - soubor pravidel a zásad pro správu IT systémů
  - přehled praktit v IT
- Metodika řešení prblémů
  - vyhledat problém
  - izolovat problém
  - vyhodnotit konfiguraci
  - vymyslet řešení
  - implementovat řešení
  - ověřit výsledek
  - zdokumentovat postup
- nástroje řešení problémů
  - správce zařízení
  - systémové informace
  - správce úloh
  - průvodce řešením prpblémů
  - sledování prostřčedků

## Technologie a komponenty používané pro zajištění nepřetržitého provozu serveru

Výpydek serveru znamená ztrátu pro firmu, komplikace pro uživatele

odolnost vůči chybám
- je třeba zajistit které komponenty jsou náchylné k pádu
- minimalizovat riziko pádu

Možnosti podle komponent
- disky
  - RAID (1 a 5) a hotspare
- napájení
  - UPS(přepěťová ochrana)
  - záložní zdroj
- clustery
  - skupina počítačů, teré se chovají jako jeden
- zálohovat pro možnosti zotavení

## Zálohování a možnosti zotavení

Záloha = vytvoření kopie dat v aktuálním stavu, do které se pak můžem vrátit
- rozsah může být od několika souborů po celý systém
- ideálně dělat na trvalá média = HDD, magnetické pásky
- plánování záloh
  - dobré rozdělit soubory a soubory aplikací
  - ideálně dělat mimo provoz, třeba v noci
- typy záloh
  - úplná
  - úplná s přírůstkovými
  - úplná s rozdílovou zálohou
- klonování záloh
  - snaha minimalizovat použité uložiště
  - určuje, jaké zálohy se nachají a jaké se přepíšou
  - nejčastěji se používá GFS sytém: Grandfather > Father > Son (3 verze)
- utilita ve windows je `windows server backup`
- stínová kopie = automatická záloha v naplánovaných časech
- možnosti zotavení serveru
  - instalátor obsahuje nástroj pro opravu v GUI
  - obnova umožňuje: obnovu pomocí bitové zálohy, spustit CLI  
