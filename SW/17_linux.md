# Správa uživatelů a oprávnění v UNIX-like systémech

## Správa uživatelů a skupin, oprávnění v UNIX-like, atributy souborů

Pro přidání uživatele máme příkaz `useradd`, `usermod`, `userdel`

Přidáním uživatele se automaticky vytvoří
- domovský adresář = defaultně /home/username
- skryté soubory v adresáři, kde se nachází proměnné prostředí
  - .bash_logout
  - .bash_profile
  - .hashrc
- mail spool = soubor pro ukládání a práci s maily
- skupina se stejným jménem jako uživatel
- informace o účtu se uloží do /etc/passwd
  - [username]:[x]:[UID]:[GID]:[Comment]:[Home directory]:[Default shell]
  - kde x znamená, že je chráněn heslem, to je v /etc/shadow
- informace o skupině se uloží do /etc/groups
  - [group name]:[group password]:[GID]:[Members]

Uživatelský účet se dá modifikovat při vytváření i potom
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

**Oprávnění souborů**

První znak označuje typ souboru
- `-` = standartní soubor
- `d` = adresář
- `l` = symbolický odkaz na jiný soubor
- `c` = zařízení typu char (např terminál)
- `b` = zyřízení typu blok (disk)

zbylých 9 znaků jsou oprávnění
![alt text](https://github.com/zyt4/maturita/blob/22c5737f54f510067624a3dff279d53bc7c27f2f/obrazky/permissions.png)

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

speciální oprávnění
- SETUID = spustitelný soubor se automaticky spustí s oprávněním majitele
  - při vytváření se před normální zápos oprávnění ještě pridá číslo 4
  - ve výpisu pak na pozici user exercute místo klasického "x" tam je "s"
- SETGID = umožňuje spustit pod oprávněním skupiny
  - při vytváření se před normální zápos oprávnění ještě pridá číslo 2
  - ve výpisu se pak projevuje znakem "s" na pozici group execute
- Sticky bit = u souborů se ignoruje, u adresářů zabraňuje nevlastníkům přejmenovat soubory(root je vyjímka)
  - při vytváření se před normální zápos oprávnění ještě pridá číslo 1
  - pak se projevuje znakem "t" v části other

Další speciální oprávnění se dají nastavit pomocí `chattr` = zabraňuje přejmenování, přesunu, smazání
- `+i` = immutable = nejde nijak upravovat (ani pro roota)
- `+a` = append = lze pouze přidávat další obsah

Vypsat se dají pomocí `lsattr`(list attribute)

## Příkaz sudo a jeho konfigurace

Dává nám přístup k root účtu a jeho oprávněním příkaz `su`

Sudo umožňuje uživatelům dělat věci s root oprávněním

Použití se nastavuje v /etc/sudoers. Nejdůležitější části
- root ALL=(ALL) ALL
  - root může všechno
- user ALL=NOPASSWD: ALL
  - user může vše a nemusí zadávat heslo
- Defaults secure path = "/esr/bin..."
  - definuje adresáře, kde se dá sudo použít

Naše sudo oprávnění zobrazíme pomocí `sudo -l`

## ACL a používání kvód u souborových systémů typu ext

Umožňuje podrobnější přidělení oprávnění k souborům a složkám

ACL jsou podporování pouze systémy, které jsou mounted s volbou acl

typy acl
- access acl = lze použít na soubory i adresáře
- default acl = pouze pro adresáře

Ověření aktuálního nastavení přes: `getfacl <soubor>`

Nastavení acl: `setfacl -m u:username:rw <soubor>`

Přidělení defaultních pomocí d: `setfacl -m d:o:r <soubor>`

odebrání všech oprávnění: `setfacl -b <soubor>`

Diskové kvóty nám umožňují limitovat místo, které uživatel může využít.

V /etc/fstab je potřeba změnit options na `usrquota,grpquota`

Pak musíme vytvořit databázi kvót: `quotacheck -cug cesta_k_souboru` (Create User Group)

přidání kvót:
- `edquota -u username` pro uživatele
- `edquota -ut` mění dobu překročení soft limitu pro všechny
- `edquota -g group` pro skupinu
- `edquota -gt` mění dobu překročení soft limitu
- `quotaon cesta` zapne kvóty

Editor:
- blocks = místo (100M,250M...)
- inodes = soubory (2000,3650...)
- soft = lze překročit na čas (jakmile čas vyprší, už nemůžeme přidávat, ale data tam zůstanou)
- hard = nelze překročit
- time = slovy (minutes,hours,days...)




