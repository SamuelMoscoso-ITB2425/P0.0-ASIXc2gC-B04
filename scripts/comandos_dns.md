# DNS 

Actualizareos el repositorio de paquetes por si acaso

```bash
sudo apt update
```

Intalamos el servicio de dhcp

---
``` bash
sudo apt install isc-dhcp-server
```
---
Lo modificaremos

---
```bash
sudo nano /etc/dhcp/dhcpd.conf
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
