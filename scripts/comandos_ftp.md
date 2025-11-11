# FTP

```bash
sudo apt-get update
sudo apt-get install vsftpd
sudo hostnamectl set-hostname F-NCC
sudo nano /etc/vsftpd.conf
```bash
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
chroot_local_user=YES
allow_writeable_chroot=YES
```
```bash
sudo systemctl restart vsftpd
sudo systemctl enable vsftpd
sudo systemctl status vsftpd
isard@ClnProject1:~$ ftp 192.168.50.1
Connected to 192.168.50.1.
220 (vsFTPd 3.0.5)
Name (192.168.50.1:isard): isard
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||30049|)
150 Here comes the directory listing.
-rwxr-xr-x    1 0        0           19394 Oct 07 14:37 instalador.sh
-rw-rw-r--    1 1000     1000      2220488 Oct 27 14:56 opendatabcn_llista-equipaments_educacio-csv.csv
226 Directory send OK.

```
