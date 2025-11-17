# Comandos de Configuración del Proyecto NCC

> **Projecte Intermodular d'Administració de Sistemes Informàtics en Xarxa**  
> Documentación completa de los comandos utilizados en la configuración, despliegue y administración del Proyecto **NCC**.  
> **Autores:** Grupo GNN – ITB · 2025  

---

## Índice

1. [Configuración de red y NAT](#-configuración-de-red-y-nat)  
2. [Creación y gestión de usuarios](#-creación-y-gestión-de-usuarios)  
3. [Configuración de claves SSH](#-configuración-de-claves-ssh)  
4. [Configuración de la DMZ](#-configuración-de-la-dmz)  
5. [Configuración de MySQL y base de datos](#️-configuración-de-mysql-y-base-de-datos)  
6. [Carga de datos CSV](#-carga-de-datos-csv)  
7. [Consultas SQL](#-consultas-sql)  
8. [Conclusión](#-conclusión)

---

## Configuración de red y NAT

Configura el servidor como router para permitir que la red interna acceda a Internet mediante NAT.

| Comando | Descripción |
|----------|--------------|
| `sudo nano /etc/sysctl.conf` | Abre la configuración del sistema. |
| `net.ipv4.ip_forward=1` | Activa el reenvío de paquetes IP. |
| `sudo sysctl -p` | Aplica los cambios del archivo `sysctl.conf`. |
| `sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o ens1 -j MASQUERADE` | Activa NAT para la red 192.168.50.0/24. |

**Nota:** Los clientes deben tener como *gateway* la IP del servidor para enrutar correctamente el tráfico.

---

## Creación y gestión de usuarios

Creación del usuario común **bchecker** y configuración de acceso SSH.

```bash
sudo useradd -m bchecker
echo "bchecker:bchecker121" | sudo chpasswd
sudo mkdir -p /home/bchecker/.ssh
sudo nano /home/bchecker/.ssh/authorized_keys
sudo chown -R bchecker:bchecker /home/bchecker/.ssh
sudo chmod 700 /home/bchecker/.ssh
sudo chmod 600 /home/bchecker/.ssh/authorized_keys
```

**Resumen:**  
Crea el usuario, le asigna contraseña y permisos seguros para conexión SSH.

---

## Configuración de claves SSH

Configura acceso sin contraseña mediante claves RSA de 4096 bits.

### En el servidor

```bash
ssh-keygen -t rsa -b 4096 -C "adria.montero.7e5@itb.cat"
cat ~/.ssh/id_rsa.pub
```

Ejemplo de clave pública:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC6yVbvTJ/1pZ48h7Gb4k20Yt4Q9M50QLpFLaawDOLQCtkzmYOlqWy1Y...
```

### En los clientes

```bash
ssh-keygen -t rsa -b 4096 -C "samuel.moscoso.7e8@itb.cat"
cat ~/.ssh/id_rsa.pub
sudo nano /home/bchecker/.ssh/authorized_keys
sudo chown -R bchecker:bchecker /home/bchecker/.ssh
sudo chmod 700 /home/bchecker/.ssh
sudo chmod 600 /home/bchecker/.ssh/authorized_keys
```

**Flujo general:**
1. El cliente genera su clave.  
2. Envía la clave pública al servidor.  
3. El servidor la añade a `authorized_keys`.  
4. Acceso sin contraseña.

---

## Configuración de la DMZ

> **DMZ (Zona Desmilitarizada):** zona para alojar servicios accesibles desde Internet sin exponer la red interna.

| Servicio | Descripción |
|-----------|-------------|
| Web | Servidor HTTP público. |
| BBDD | Servidor MySQL accesible desde fuera. |
| DNS | Resolución de nombres externos. |

**Importante:** La DMZ aísla los servicios públicos del resto de la red para mejorar la seguridad.

---

## Conclusión

> Con esta configuración se dispone de un entorno completo:  
> - Servidor con NAT y reenvío activado  
> - Acceso SSH seguro mediante claves  
> - DMZ funcional para servicios públicos  
> - Base de datos MySQL cargada con datos  
>
> **Sistema listo para el despliegue del Proyecto NCC**
