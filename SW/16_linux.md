# Správa uložišť v UNIX-like operačních systémech

## Správa oddílů a formátování v UNIX-like operačních systémech

Máme dva typy oddílů: MBR a GPT

Pracujeme s násrojem `fdisk`
- disky a oddíly jsou uloženy v /dev/
- syntax `fsdisk <cesta k disku>`
- navigujeme se pomocí písmenek
  - `p` informace o disku (aktuální stav tabulky)1
  - `g` vytvoří novou gpt tyble
  - `n` vytvoří nový oddíl, pak postupujeme podle promptu
  - `w` zapíše změny na disk
  - `d` smaže oddíl

Po vytvoření oddílu je třeba ho naformátovat
- použijeme nástroj `mkfs`
- syntax `mkfs.[souborový systém] <diskový oddíl>`
- podporované systémy
  - Ext2,3,4
  - ReiserFS
  - FAT
  - NTFS (jen okrajově, lepší nepoužívat)

Oddíly typu SWAP
- využívají se k vytvoření místa, pro odkládání operační paměti, pokud je přeplněná
- vytvoření systému: `mkswap <diskový oddíl>`
- pak ho musíme aktivovat: `swapon`
- zrušit se dá pomocí: `swapoff`

## Připojení a používání souborových systémů v UNIX-like operačních systémech

Linux používá jednoduchý adresářový strom, kde každý diskový oddíl je připojený k jednomu bodu(mounting point)

Mounting point = adresář pro přístup k souborovému systému na daném oddílu
mounting = proces připojení konkrétního oddílu k adresáři

K připojení máme příkaz mount
- syntax: `mount <oddíl> <adresář kam>`
- zajímavé optiony
  - AUTO = může se automaticky připojit
  - Noexe = nesmí se spouštěť soubory
  - Po = read only

Odpojení je následně příkaz `umount <cesta k diskovému oddílu nebo složce>`

Připojení síťových souborů
- NFS = UNIX-like (je potřeba nfs-common)
- SMB = Windows (je potřeba smbclient)
- připojení `mount <//ip/složka> <náš adresář pro připojení>`

trvalé mountování se píše do /etc/fstab
```
<systém, buď cesta nebo UUID(blkid),nebo label> <kam to připojím> <filesystem> <možnosti> <dump> <pass>
/dev/sdb3 /mnt/testik ext4 defaults 0 1
LABEL="mujswap" none swap defaults 0 0
```

## Diskové pole RAID a správa svazků na discích LVM

Typy RAID
- 0 = striping (spojí se do většího)
- 1 = mirroring (tvoří se kopie jednoho na druhém)
- 5,6 = striping and parity (spojení do většího s komprimovanou zálohou v každém)

Vytváříme příkazem mdadm
```bash
mdadm --create -v /dev/md1 -l 0 -n 2 /dev/sdb50 /dev/sdc2
```
- `--create` pro vytvoření
- `-v` jak se bude jmenovat výsledný disk
- `-l` úroveň raidu (0,1,5...)
- `-n` kolik se použije disků

Pro automatický raid: `mdadm --detail --scan` to se uloží do /etc/mdadm/mdadm.conf

Formátování pak probíhá jako s klasickým diskem

Náhradnímu disku se říká hot-spare

Pokud mají redundanci, lze poškozený disk nahradit novým: mdadm /dev/md1 --add /dev/sde1

Rozpojení RAIDu
- mdadm --stop /dev/md1 = rozpojí se pole
- mdadm --remove /dev/md1 = odstraní se raid zařízení

Struktura LVM
- fyzický svazek PV = jedná se o diskové oddíly (nafurmátuje se místo souborového systému)
- skupina svazků VG = spojení svazků do jedn= logické skupiny
- logický svazek = vytváří se na VG a odpovídá virtuálnímu diskovému oddílu
  - disky jsou pak uloženy v /dev/mapper

Postup
1. nejprve musíme naformátovat oddíly jako pv: `pvcreate cesta`
2. pak je musíme přidat do jedné skupiny: `vgcreate název pv_cesta`
3. nakonec vytváříme logické svazky: `lvcreate --name="název" --size=100M skupina`
4. s logic volume pak pracujeme stejně jako s každým oddílem
5. Pro změnu velikosti u lv můžeme použít `lvreduce/lvextend`

Zobrazení inforamací:
- pvs/pvdisplay = informace o fyzických svazcích
- vgs/vgdisplay = informace o skupinách svazků
- lvs/lvdisplay = seznam logických svazků

Než se budou dát použít je nejprve třeba je namountovat

Můžeme použít jméno nebo UUID, to zjistíme z příkazu `blkid`
