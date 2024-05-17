# Sledování využití hardwaru a systémových prostředků v UNIX-like operačních systémech

## Monitorování vyuřití prostoru na discích

Pro sledování místa na discích máme dva příkazy: `df`(disk free) a `du` (disk usage)

df
- zobrazí celkové využití disku
- normálně se zobrazuje v Bajtech (pro čitelnější je -h = human readable)
- zobrauí: velikost souborového systému, volný a obsazený prostor, mount pointy...
- pro zobrazení inodes(na které soubory je vázaný): `df -hTi`

du
- používá se převážně pro zjištění, které soubory zabírají nejvíce místa
- použití: `du -h <file>` (pokud dáme složku, zobrazí to využití všech souborů v ní)

## Monitorování využití operační paměti RAM a CPU

Pro sledování využití RAM a CPU je nástroj `top`, jedná se o nástroj v příkazové řádce, který zobrazuje stav systému v reálném čase.

Něco jako správce úloh. Pro lepší čitelhost je nástroj: `htop`

Pro sledování RAM je možné využít i příkaz `free`
- pro čitelný je opět -h nebo můžeme vybrat v jakých jednotkách příslušným písmenen
- zvlášť ukazuje RAM a SWAP
- řekne nám: kolik je volného místa, použitého místa, místa celke...

## Správa procesů

Pro sledování procesů máme nástroj `ps` a `pstree`
- má hodně optionů, podle toho, co chceme
- `-ef` nám zobrazí veškeré běžící procesy

Procesy lze zastavit, přesunou na pozadí...
- k tomu slouží příkaz `kill`
- syntax: `kill <signál/číslo> <proces ID>`

Přehled signálů:
|signál|číslo|popis|
|---|---|---|
|SIGINT|2|Posílá se po stisku CTRL+C, ale procesy ho mohou ingnorovat|
|SIGKILL|9|Bezpodmínečně přeruší proces|
|SIGTSTP|20|Pozastaví činnost a počká, až bude moct pokračovat. Posílá se po stisku CTRL+Z|
|SIGCONT|18|Pokud dostane přákaz 19/20 tak mu říká, aby pokračoval v činnosti na pozadí. Jendá se o příkazy `bg` a `fg`|

Většina událostí je uložena v /var/log, takže pokud se něco stane, můžeme se podívat co a proč.
