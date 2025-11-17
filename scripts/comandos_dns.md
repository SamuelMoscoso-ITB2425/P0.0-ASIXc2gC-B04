# DNS 

```bash
sudo apt update
```

``` bash
sudo apt install isc-dhcp-server
```

```bash
sudo nano /etc/dhcp/dhcpd.conf
```


Luego reiniciamos el DHCP para poder aplicar la configuración

```bash
sudo systemctl restart isc-dhcp-server
```

Luego habilitaremos y verificaremos el estado

```bash
sudo systemctl enable isc-dhcp-server
```
```bash
sudo systemctl status isc-dhcp-server
```
