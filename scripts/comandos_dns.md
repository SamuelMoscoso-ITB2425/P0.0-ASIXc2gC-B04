# DNS

Actualizamos los repositorios por si acaso
```bash
sudo apt update 
```

Instalamos el bin para los servicios dns@       IN      SOA     ns.B04.local. admin.B04.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.B04.local.
ns      IN      A       192.168.50.1
R-B04   IN      A       192.168.50.2
R       IN      A       192.168.50.3

```bash
sudo apt install bind9 bind9utils bin9-doc dnsutils -y
```

Definimos la zona principal
```bash
sudo nano /etc/bind/named.conf.local
```
Añadiremos esto al archivo named.conf.local

```bash
zone "ncc.local" {
        type master;
        fie "/etc/bind/db.B04.local";
};
```

Copiaremos el archivo db.local y le pondremos el nombre db.ncc.local para luego modificarlo
```bash
sudo cp /etc/bind/db.local /etc/bind/db.B04.local
```
```bash
sudo nano /etc/bind/db.B04.local
```
Lo modificaremos de esta forma
```bash
@       IN      SOA     ns.B04.local. admin.B04.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.B04.local.
ns      IN      A       192.168.50.1
R-B04   IN      A       192.168.50.2
R       IN      A       192.168.50.3
```
---
<div align="left"><a href="./comandos_DHCP.md">Página anterior</a></div>
<div align="right"><a href="./comandos_ftp.md">Siguiente página</a></div>
