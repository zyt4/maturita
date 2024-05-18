# Proces zavádění systému, archivace a zálohování v UNIX-like operačních systémech

## Proces zavádění systému

Jádro Linuxu se označuje jako kernel

Plně funkční sytém s kernelem se pak nazývá distribuce

Funkce jádra
- rozhraní mezi HW a SW
- komunikace s HW pomocí ovladačů
- správa systémuvých prostředků
- aktuální verzi zjisítme pomocí `uname -a`

Proces bootování:
1. Zmáčknu tlašítko
2. Spustí se firmware uložený v EEPROM a POST test
3. Načte se Master Boot Record = je to v efi oddílu
4. Spustí se GRUB = pomocník se zaváděním (Boot Manager)
5. GRUB načítá kernel a obraz initrd nebo initramfs = pomáhají při detekci HW
   - načítá moduly a dekce zařízení nutných pro připojení adresářového stromu
6. Inicializace Kernelu
7. Po připojení kořenového adresáře kernel spustí správce systému a služeb
  - sysinit nebo `systemd` (u redhat se jedná o upstart) s PID 1 = ti zajistí uživatelské rozhraní
  - jedná se o deamony, kteří spravují další procesy(deamony) 

Linux podporuje systém runlevelů = umožňuje nastavit, jaké služby se budou v daném levelu spouštět, a v jaké levelu se budeme nacházet

Run levely jsou pro sysinit a jsou definovány v /etc/inittab = pro zpětnou kompatibilitu jsou podporovány i u systemd

|run level|popis|
|:---:|---|
|0|Vypnutí systému, přechodný stav pro rychlé vypnutí sytému|
|2debian/3redhat|Režim multiuser, umož%nuje zavést grafické rozhraní|
|5|Grafické rozhraní|

Pro přepnutí do jiného runlevelu se dá použít: `init N` (N je číslo runlevelu)

Systemd používá paralelní spouštění procesů a dynamickou správu prostředků
- správa prostředí přes `systemctl`

U systemd se používají místo run levelů targety
|runlevel|target|
|---|---|
|0|poweroff target|
|3|multi-user target|
|5|graphical target|


GRUB (the grand unified bootloader)
- existuje ve dvou verzích 1 legaci a 2
- umožňuje měnit chování systému a definovat jádro, systém, který se bude zavádět
- po spouštění se zobrazí obrazovka GRUBu pro výběr systému
- konfigurace probíhá v /etc/default/grub
  - grup_TIMEOUT = doba před automatickým zavedením systému
  - grub_DEFAULT = defaultní systém pro zavedení
  - nakonec je třeba dát `updagte-grub`

## Archivace

Nástroj pro archivaci vytváří z více souborů/adresářů jeden soubor a komprimuje ho pro snadnější přenos

Nástrojů je mnoho, ale na hlavní pro Linux je `tar`(tape archive), který vytváří soubory *.tar*(tarball)

Může používat kompresi

Metody komprese
- gzip *.gz* = jedná se o nejstarší netodu
- bzip2 *.bz2* = průměr
- xz *.xz* = nejnovější a nejlepší

Pokud archiv zkomprimujeme, musíme ho před prací s mín dekompromovat

Archiv uchovává informace jako
- velikost souboru
- datum vytvoření
- poslení zápis
- vlastník

použití
- `f` následně zadáme název výsledného souboru
- `t` vypíše obsah vybraného archivu
- `c` vytvoří archiuv
- `x` extrahuje obsah archivu
- `--gzip` zvolíme metodu komprese
- `-C` zvolíme cíl, kam chceme extrahovat
- na konec pak napíšeme, jaké soubory/složky chceme archivovat oddělené mezerou
  - podle toho, jakou adresaci použijeme, tak budou zapsaný v archivu

```bash
tar cf test.tar.gz --gzip test1 test2
tar ct test.tar.gz
tar xf test.tar.gz -C /kamchci
```

## Vytváření a správa záloh, zálohování dat

Neni jedna správná cesta, záleží co potřebujem a co nám vyhovuje

Máme tři hlavní varianty:
1. záloha celého disku pomocí `dd`
  - jedná se o nástroj, který vytvoří bitovou kopii
  - vhodné u nepřenosných zařízení
  - kopie bude mít stejnou velikost jako originál
  - `if` input file
  - `of` output file
  - `bs` block size
  - `count` počet bloků

```bash
dd if/dev/urandom of=/dev/key bs=1k count=4
```

2. zálohování souborů pomocí tar
   - řešení, pokud je třeba vytvořit kopie jen části souborů
3. synchronizace souborů pomocí `rsync`
   - ideální pro zálohování na síťové disky
   - synchronizace adresářů(jednorázová): `rsync -av <můj adresář> <backup>`
     - `-v` verbose, `-a` ponchá původní atributy, `-z` povolí kompresi, `-c` pužije pro přenos ssh
   - Obnovení je stejný, jen se prohodí zdroj a cíl
   - automatická záloha
     - opužijeme cron(vytvoříme úlohu)
     - z našeho rsync příkazu uděláme *.sh* soubor
     - ten následně dáme do crontabu a nastavíme tam časování
     - `0 * * * * /path/RSYNC_backup.sh`
     
