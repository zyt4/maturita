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



## Virtualizace


