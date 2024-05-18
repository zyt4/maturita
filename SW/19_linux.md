# Konfigurace a správa služby FTP a webového serveru, virtualizace v UNIX-like systémech

## FTP

Jedná se o protokol pro přenos souborů mezi serverem a klientem

Balíček se nazývá `vsftpd` (very secure FTP deamon)

Pro připojení používá klienta FTP

Konfigurace probíhá v /etc/vsftpd/vsftpd.conf:
- Povolení anonymního přístupu
```
anonymous_enable=YES
no_anon_password=YES
anon_root=/storage/ftp # jedná se o adresář, kam se připojí
```
- Povolení přístupu pouze pro čtení
```
write_enable=NO
```
- Lze povolit přihláše přes místní účty
```
local_enable=YES
```
- Povolení přístupu ke svým domovským adresářům
```
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chrootlist # list adresářů a příslučných uživatelů
```
- Omezení šířky pásma
```
anon_max_rate=10240
local_max_rate=20480
max_per_ip=5
```
- Omezení portů
```
pasv_enable=YES
pasv_max_port=15500
pasv_min_port=15000
```
- nastavení banneru
```
ftpd_banner = text
```

Je pořeba ještě povolit porty na firewallu, aby nám neblokoval komunikaci.

## WEB

Pokud se jedná o webový server máme víc možností(nginx), ale my se budeme bavit o apache.

Podporuje víc webů na jednom serveru.

Balíček se jmenuje `apache2`, hodí se i proxy `squid3` a `squidguard` pro vytváření blacklistů...

Konfigurace má víc kroků:

1. vezmem stránky z /etc/apache2/sites-available a uděláme kopie:

```
sudo cp 000default.conf maturitaformalita.cz.conf #http
sudo cp defaultsssl.conf maturitaformalita.cz.ssl.conf # https
```
> v nich se pak upravuje nastavení stránek

2. Provedeme obecnou konfigurace stránky

http konf:
```
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/mysite/maturitaformalita.cz # kde jsou stránkové soubory
        ServerName www.maturitaformalita.cz #url
	   #Redirect / https://www.maturitaformalita.cz #možné přesměrování
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

https konf
```
<VirtualHost *:443>
        ServerName www.maturitaformalita.cz # url
        ServerAdmin webmaster@localhost
        DocumentRoot /var/mysite/maturitaformalita.cz #kde jsou soubory

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        SSLEngine on

        SSLCertificateFile      /etc/ssl/certs/maturitaformalita.cz.crt #certifikáty
        SSLCertificateKeyFile   /etc/ssl/private/maturitaformalita.cz.key

        <FilesMatch "\.(?:cgi|shtml|phtml|php)$">
                SSLOptions +StdEnvVars
        </FilesMatch>
        <Directory /usr/lib/cgi-bin>
                SSLOptions +StdEnvVars
        </Directory>

</VirtualHost>
```

3. U SSL je třeba vygenerovat ssl certifikáty:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```

ssl je třeba povolit pomocí `sudo a2enmod ssl`

4. Nové stránky je potřeba také povolit: `sudo a2ensite stránka` 

a zakázat default stránku `sudo a2dissite 000-default.conf`, aby nám nedělala potíže

5. Nakonec je třeba povolit náš adresář v souboru /etc/apache2.conf na cca 170 řádku

## Virtualizace a kontejnery

Pokud chceme virtualizovat, tak máme dvě možnosti: VM a kontejnery

VM
- OS na hlavním PC je host, systém na VM je guest
- je potřeba povolit virtualizaci CPU
- balíčky jsou: `gemu gemu-kvm virt-manager`
- pro vytvoření VM potřebujem obraz OS
- nakonec ho jen vytvořit
```
virt-install --name=centos7cm --ram=1024 --vcpus=1 ... --disk path=/var/lib/libvirt/images/centos7vm.dist,size=8
```

Příkazy pro práci s VM
- vypsání našich VM: `virsh --list all`
- inforamce o VM: `virs dominfo <název VM>`
- nastavení VM: `virsh edit <název VM>`
- zastavení: `virsh shutdown <název VM>`

Kontejnerizace
- používá se platforma docker
- kontejner je v podstatě SW balíček všeho, co daný program potřebuje
- běží nezávisle na tom, kde se nachází
- jednodušší správa oproti VM
- běží bez nutnosti instalace aplikace (pokud chci MariaDB je jednodušší docker než instalace)

Stačí stáhnout docker
- po jeho spuštění příkaz je `docker`
- kontejenry teď už jen stačí stáhnout a spustit
