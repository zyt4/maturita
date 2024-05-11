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



## oprávnění

## práce se souborovými systémy

## pokročilá práce se souborovými systémy

## správa procesů a služeb

## konfigurace služeb: SSH, FTP, DHCP, DNS, NAT

## konfigurace Apache a MariaDB

## Firewall
