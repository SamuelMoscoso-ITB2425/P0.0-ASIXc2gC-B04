# ⚙️ Comandos de Configuración del Proyecto NCC

> **Projecte Intermodular d'Administració de Sistemes Informàtics en Xarxa**  
> Documentación de comandos utilizados en la configuración y despliegue del proyecto NCC.  
> Autor: **Equipo GNN** · ITB · 2025  

---

## 📑 Índice

1. [Configuración de red y NAT](#configuración-de-red-y-nat)  
2. [Creación y gestión de usuarios](#creación-y-gestión-de-usuarios)  
3. [Configuración de claves SSH](#configuración-de-claves-ssh)  
4. [Activación del reenvío de paquetes](#activación-del-reenvío-de-paquetes)  
5. [Configuración de la DMZ](#configuración-de-la-dmz)  
6. [Configuración de MySQL y base de datos](#configuración-de-mysql-y-base-de-datos)  

---

## 🌐 Configuración de red y NAT

| Comando | Descripción |
|----------|--------------|
| `ip a` | Muestra la configuración de las interfaces de red. |
| `sudo nano /etc/netplan/00-installer-config.yaml` | Edita el archivo principal de configuración de red. |
| `sudo netplan apply` | Aplica los cambios hechos en la configuración de red. |
| `net.ipv4.ip_forward=1` | Habilita el reenvío de paquetes IPv4. |
| `sudo sysctl -p` | Recarga las configuraciones del sistema sin reiniciar. |
| `sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o enp1s0 -j MASQUERADE` | Crea una regla de NAT para permitir el acceso a Internet desde la red interna. |
| `sudo apt install iptables-persistent` | Instala el paquete que guarda reglas de iptables entre reinicios. |
| `sudo netfilter-persistent save` | Guarda permanentemente las reglas configuradas de iptables. |

---

## 👥 Creación y gestión de usuarios

| Comando | Descripción |
|----------|--------------|
| `sudo useradd -m -s /bin/bash bchecker` | Crea el usuario **bchecker** con su directorio personal. |
| `sudo passwd bchecker` | Establece una contraseña al usuario (en este caso `bchecker121`). |
| `echo "bchecker:bchecker121" | sudo chpasswd` | Crea el usuario y cambia la contraseña en una sola línea. |
| `sudo mkdir -p /home/bchecker/.ssh` | Crea el directorio `.ssh` dentro del home del usuario. |

💡 **Nota:** Todos los equipos deben tener configurada la **IP del servidor como gateway** para garantizar la conectividad.

---

## 🔐 Configuración de claves SSH

| Comando | Descripción |
|----------|--------------|
| `ssh-keygen -t rsa -b 4096 -C "correo@itb.cat"` | Genera una clave RSA de 4096 bits con el correo institucional. |
| `cat ~/.ssh/id_rsa.pub` | Muestra la clave pública generada. |
| `sudo nano /home/bchecker/.ssh/authorized_keys` | Abre el archivo donde se añaden las claves públicas de los clientes. |
| `sudo chown -R bchecker:bchecker /home/bchecker/.ssh` | Asigna los permisos correctos de propiedad al usuario. |
| `sudo chmod 700 /home/bchecker/.ssh` | Da permisos de acceso al directorio `.ssh`. |
| `sudo chmod 600 /home/bchecker/.ssh/authorized_keys` | Asegura que solo el usuario pueda leer su archivo de claves. |

🔎 **Explicación rápida:**  
Los clientes (Linux y Windows) generan sus claves públicas y las envían al servidor, que las añade en `authorized_keys`.  
Esto permite conectarse por SSH sin contraseña, aumentando la seguridad.

---

## 🚀 Activación del reenvío de paquetes

| Comando | Descripción |
|----------|--------------|
| `sudo nano /etc/sysctl.conf` | Abre el archivo de configuración del sistema. |
| `net.ipv4.ip_forward = 1` | Activa el reenvío de paquetes IP (elimina el `#` si está comentado). |
| `sudo sysctl -p` | Aplica la configuración actualizada sin reiniciar. |

🔁 **¿Por qué se hace esto?**  
Permite que el servidor actúe como **router** y redireccione tráfico entre redes internas y externas.

---

## 🧱 Configuración de la DMZ

> **DMZ (Zona Desmilitarizada)**  
> Espacio intermedio que permite acceso externo (Internet) a servicios públicos sin comprometer la red local.

| Componente | Función |
|-------------|----------|
| **Web Server (W)** | Aloja las páginas accesibles desde Internet. |
| **BBDD (B)** | Contiene la base de datos del sistema. |
| **DNS** | Gestiona las resoluciones de nombres. |

💡 Los servidores ubicados en la DMZ **no tienen acceso directo a la red interna**, aumentando la seguridad.

---

## 🗄️ Configuración de MySQL y base de datos

```bash
sudo mysql

