# DNS

Actualizamos los repositorios por si acaso
```bash
sudo apt update 
```

Instalamos el bin para los servicios dns
```bash
sudo apt install bin9 bind9utils bin9-doc dnsutils -y
```

Definimos la zona principal
```bash
sudo nano /etc/bind/named.conf.local
```
Añadiremos esto al archivo named.conf.local

```bash
zone "ncc.local" {
        type master;
        fie "/etc/bind/db.ncc.local";
};
```

Copiaremos el archivo db.local y le pondremos el nombre db.ncc.local para luego modificarlo
```bash
sudo cp /etc/bind/db.local /etc/bind/db.B04.local
```
```bash
sudo nano /etc/bind/db.ncc.local
```

