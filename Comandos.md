# ⚙️ Comandos de Configuración del Proyecto NCC

> **Projecte Intermodular d'Administració de Sistemes Informàtics en Xarxa**  
> Documentación completa de los comandos utilizados en la configuración, despliegue y administración del Proyecto **NCC**.  
> **Autores:** Grupo GNN – ITB · 2025  

---

## 📑 Índice

1. [Configuración de red y NAT](#configuración-de-red-y-nat)  
2. [Creación y gestión de usuarios](#creación-y-gestión-de-usuarios)  
3. [Configuración de claves SSH](#configuración-de-claves-ssh)  
4. [Activación del reenvío de paquetes](#activación-del-reenvío-de-paquetes)  
5. [Configuración de la DMZ](#configuración-de-la-dmz)  
6. [Configuración de MySQL y base de datos](#configuración-de-mysql-y-base-de-datos)  
7. [Resumen general](#resumen-general)  
8. [Conclusión](#conclusión)

---

## 🌐 Configuración de red y NAT

| Comando | Descripción |
|----------|-------------|
| `ip a` | Muestra la configuración de las interfaces de red. |
| `sudo nano /etc/netplan/00-installer-config.yaml` | Edita el archivo de configuración de red. |
| `sudo netplan apply` | Aplica los cambios realizados en la red. |
| `net.ipv4.ip_forward=1` | Activa el reenvío de paquetes IPv4. |
| `sudo sysctl -p` | Aplica los cambios del sistema sin reiniciar. |
| `sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o enp1s0 -j MASQUERADE` | Configura NAT para que los equipos de la red interna salgan a Internet. |
| `sudo apt install iptables-persistent` | Instala el paquete que permite guardar reglas de iptables. |
| `sudo netfilter-persistent save` | Guarda las reglas configuradas de iptables. |

💡 **Importante:**  
Los clientes deben tener configurada como *gateway* la IP del servidor para que el tráfico se enrute correctamente.

---

## 👥 Creación y gestión de usuarios

| Comando | Descripción |
|----------|-------------|
| `sudo useradd -m -s /bin/bash bchecker` | Crea el usuario **bchecker** con su directorio personal. |
| `sudo passwd bchecker` | Asigna la contraseña `bchecker121` al usuario. |
| `echo "bchecker:bchecker121" | sudo chpasswd` | Crea y asigna la contraseña al usuario de forma directa. |
| `sudo mkdir -p /home/bchecker/.ssh` | Crea el directorio `.ssh` dentro del home del usuario. |

💬 **Notas:**
- Este usuario se crea en **todas las máquinas**.  
- Es el usuario común para el acceso a los servicios (`bchecker / bchecker121`).

---

## 🔐 Configuración de claves SSH

### 🔹 Generación y copia de claves

| Comando | Descripción |
|----------|-------------|
| `ssh-keygen -t rsa -b 4096 -C "correo@itb.cat"` | Genera una clave SSH RSA de 4096 bits. |
| `cat ~/.ssh/id_rsa.pub` | Muestra la clave pública generada. |
| `sudo nano /home/bchecker/.ssh/authorized_keys` | Abre el archivo donde se guardan las claves públicas de los clientes. |
| `sudo chown -R bchecker:bchecker /home/bchecker/.ssh` | Cambia la propiedad de la carpeta `.ssh`. |
| `sudo chmod 700 /home/bchecker/.ssh` | Asigna permisos de acceso correctos a la carpeta. |
| `sudo chmod 600 /home/bchecker/.ssh/authorized_keys` | Permite que solo el usuario lea sus claves. |

🧠 **Explicación rápida:**  
1. Los clientes generan sus claves con `ssh-keygen`.  
2. Copian la clave pública al servidor.  
3. El servidor las añade al archivo `authorized_keys` del usuario `bchecker`.  
4. Así se permite acceso por SSH sin contraseña y de forma más segura.

---

## 🚀 Activación del reenvío de paquetes

| Comando | Descripción |
|----------|-------------|
| `sudo nano /etc/sysctl.conf` | Abre el archivo de configuración del sistema. |
| `# net.ipv4.ip_forward = 1` → `net.ipv4.ip_forward = 1` | Activa el reenvío de paquetes IP eliminando el comentario. |
| `sudo sysctl -p` | Aplica los cambios del archivo `sysctl.conf`. |
| `sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o ens1 -j MASQUERADE` | Enruta el tráfico saliente a través del servidor. |

📘 **Motivo:**  
Permite que el servidor funcione como **router**, gestionando el tráfico entre las redes internas y externas.

---

## 🧱 Configuración de la DMZ

> **DMZ (Zona Desmilitarizada)**  
> Área de red destinada a servicios accesibles desde Internet, aislada de la red interna.

| Servicio | Descripción |
|-----------|-------------|
| **Web** | Servidor web público. |
| **BBDD** | Servidor de base de datos del proyecto. |
| **DNS** | Servicio de resolución de nombres. |

💡 **Detalles:**
- La **DMZ** permite que los servidores sean accesibles desde fuera sin comprometer la Intranet.  
- No hay comunicación directa entre la DMZ y la red interna.  
- La seguridad se basa en aislamiento y control de tráfico.

---

## 🗄️ Configuración de MySQL y base de datos

### 🔹 Acceso al monitor de MySQL

```bash
sudo mysql
