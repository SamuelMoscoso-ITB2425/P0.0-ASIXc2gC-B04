# DHCP 

Actualizareos el repositorio de paquetes por si acaso

```bash
sudo apt update
```

Intalamos el servicio de dhcp

---
``` bash
sudo apt install isc-dhcp-server -y
```
---
Lo modificaremos

---
```bash
sudo nano /etc/dhcp/dhcpd.conf
```
Luego configuraremos las interfaces que usara el servidor DHCP

```bash
sudo nano (etc/default/isc-dhcp-server
```

---

Luego reiniciamos el DHCP para poder aplicar la configuración

```bash
sudo systemctl restart isc-dhcp-server
```

Luego habilitaremos y verificaremos el estado

---
```bash
sudo systemctl enable isc-dhcp-server
```
```bash
sudo systemctl status isc-dhcp-server
```
---

Luego deberemos de activar en la tageta de red el dchp como **True** del enp3s0

---
<div align="left"><a href="./creacion_usuarios.md">Página anterior</a></div>
<div align="right"><a href="./comandos_ftp.md">Siguiente página</a></div>
