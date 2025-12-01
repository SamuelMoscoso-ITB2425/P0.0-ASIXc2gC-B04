# Router

Configuramos la targeta de red con esta configuración

```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: no
      addresses:
        - 192.168.1.1/24
      nameservers:
        addresses: [8.8.8.8, 192.168.1.1]
    enp2s0:
      dhcp4: no
      addresses:
        - 10.0.0.1/24
    enp3s0:
      dhcp4: no
      addresses:
        - 172.16.0.1/24
```

Modificamos el archivo sysctl.conf para el renvio de paquetes

```bash
sudo nano /etc/sysctl.conf
```
Descomenta net.ipv4.ip_forward=1

```bash
sudo iptables -t nat -A POSTROUTING -o eth2 -j MASQUERADE
sudo iptables -A FORWARD -i eth1 -o eth2 -j ACCEPT
sudo iptables -A FORWARD -i eth2 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
```


```bash
sudo nano /etc/netplan/00-installer-config.yaml
sudo netplan apply
net.ipv4.ip_forward=1
sudo sysctl -p
sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o enp1s0 -j MASQUERADE
sudo apt install iptables-persistent
sudo netfilter-persistent save
```
---
<div align="left"><a href="./comandos_ftp.md">Página anterior</a></div>
<div align="right"><a href="./comandos_BBDD.md">Siguiente página</a></div>
