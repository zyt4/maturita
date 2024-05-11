# Příkazy pro správu linuxu

## vytváření souborů a složek

vytvoření složek `mkdir`

- možnost `-p` pro vytvoření všechsložek v cestě
- pro vytvoření více složek najednou je dáme do `{}` oddělené `,`
  - pokud to uděláme pro více levelů oddělými `/` tak se vytvoří všechny kombinace
  - `{}` se dají vkládat do sebe, dá se před ně vytknout společná část
  - pokud chceme více čísel, dá se nahradit pomocí `..` bez čárek mezi
```bash
mkdir -p {pokus,nevim}/{test{1..99},testik{0..55}}
```

k vytvoření souboru můžeme použít `nano` a rovnou ho naplnit obsahem

nebo můžme použít `echo "něco" > test.txt` a přesměrovat výpis do souboru

- `>` přepíše obsah
- `>>` připíše obsah na konec
- `<` použije cestu jako zdroj

## archivace

pomocí `tar` a dalších optionů (stačí napsat tar a dár tab, ono nám to nabídne co dál):

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

## správa uživatelů a skupin

vytvoření uživatele `useradd`
nodifikace uživatele `usermod`
zmazání uživatele `userdel`

optiony (username se píše nakonec):

- `-m` make/move hemo directory (default: /home/username)
- `-d path` cesta k adresáři
- `-s shell` určení shellu (/bin/bash)
- `-g group` primární skupina(vlastnictví, kvótování)
- `-G group1,group2` sekundární skupiny
- `--help` zobrazení nápovědy
- `-a` append, připojení při přidání sekundárni skupiny
- `-p` určení hesla
- přidání hesla: `sudo passwd username`
  - vynusení změny hesla `-e` (expire)
  - `-l` uzamkne učet
- expirace účtu pomocí příkazu `sudo chage -E YYYY-MM-DD username`
  - `-l` vypíše informace o účtu

vypsání informací o uživateli:

- `id usrname` řekne nám to id skupna, kde uživatel je
- `/etc/passwd` informace o uživatelích
- `/etc/shadow` uživatelská hesla
- `/etc/groups` informace o skupinách

vytvoření skupiny `groupadd`
nodifikace skupiny `groupmod`
zmazání skupiny `groupdel`

## oprávnění

změna oprávnění `chmod`

r = 4, w = 2, x = 1

změna číslem: napíšeme číselně jaká oprávnění to má mít (pořadí: owner, group, other)

```bash
chmod 764 test.txt
```

další možnost je písmenem (u = user, g = group, o = others, a = all)

`+`přidává, `-` odebírá, `=` přímo nastavuje

dají se nastavovat najednou odělený `,`

```bash
chmod u=rwx,g+w,o-r test.txt
```

změna vlastnictví `chown`

- `chown user:group path` nastaví uživatele i skupinu
- `chown user path` nastaví jen uživatele, skupinu nechá
- `chown :graou path` nastaví jen skupiny, uživatele nechá 

vypání oprávnění `getfacl` nebo `ls -l`

## práce s disky a souborovými systémy

### práce s diskem

rozdělení disku na oddíly: `sudo fdisk /dev/sdb` , disk musí být typ gpt

práce s disky:
- `p` informace o disku (aktuální stav tabulky)1
- `g` vytvoří novou gpt tyble
- `n` vytvoří nový oddíl, pak postupujeme podle promptu
- `w` zapíše změny na disk
- `d` smaže oddíl

### práce se souborovými systémy

vytvoření filesystému: `mkfs.typ_systému disk`
```bash
sudo mkfs.ext4 /dev/sdb2
```

zobrazení filesystému na oddílu:`blkid`

připojení ke složce: `mount co kam`

```bash
sudo mount /dev/sdb2 /mnt/test
```
automatické mountování: píše se do /etc/fstab

```
<systém, buď cesta nebo UUID(blkid),nebo label> <kam to připojím> <filesystem> <možnosti> <dump> <pass>
/dev/sdb3 /mnt/testik ext4 defaults 0 1
LABEL="mujswap" none swap defaults 0 0
```
mountování pak provedeme pomocí `sudo mount -a`

vytvoření swap oddílu: `mkswap`, pak je potřeba aktivovat pomocí swap on

```bash
mkswap /dev/sdb4
swap on

# nebo můžeme použít složku
# nejprve alokujeme místo na disku a pak ho použijeme
fallocate -l 20M /mnt/misto
mkswap /mnt/misto
```

vytvoření popisku: `fsystémlabel`

- ext4 = e4label -L název cesta
- swap = swaplabel -L název cesta

metadata:

zobrazení pomocí `dumpe2fs`
úprava metadat `tune2fs`

## pokročilá práce se souborovými systémy

## správa procesů a služeb

## konfigurace služeb: SSH, FTP, DHCP, DNS, NAT

## konfigurace Apache a MariaDB

## Firewall
