# FTP

```bash
sudo apt update
```
```bash
sudo apt install vsftpd -y
```
sudo hostnamectl set-hostname F-NCC

```bash
sudo nano /etc/vsftpd.conf
```
Modificaremos estos apartados
```bash
listen=YES
listen_ipv6=NO
```
Descomentaremos estos
```bash
write_enable=YES
local_umask=022
chroot_local_user=YES
```
```bash
allow_writeable_chroot=YES
```
```bash
sudo systemctl restart vsftpd
```
```bash
sudo systemctl status vsftpd
```
```bash
ftp <tu IP>
<el nombre de la cuenta>
<la contraseña de la cuenta>
```
Para salir del ftp deberas de poner exit en el terminal
```bash
exit
```
