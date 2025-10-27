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
| `ip a` | Muestra la configuración de todas las interfaces de red. |
| `sudo nano /etc/netplan/00-installer-config.yaml` | Abre el archivo de configuración de red para editarlo. |
| `sudo netplan apply` | Aplica los cambios realizados en la configuración de red. |
| `net.ipv4.ip_forward=1` | Activa el reenvío de paquetes IPv4 (permite que el servidor actúe como router). |
| `sudo sysctl -p` | Recarga las configuraciones del sistema sin reiniciar. |
| `sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o enp1s0 -j MASQUERADE` | Aplica una regla de NAT que permite a la red interna salir a Internet. |
| `sudo apt install iptables-persistent` | Instala la herramienta que guarda las reglas de iptables al reiniciar. |
| `sudo netfilter-persistent save` | Guarda las reglas actuales de iptables de forma permanente. |

💡 **Consejo:** Los usuarios deben tener como *gateway* la IP del servidor para enrutar correctamente su tráfico.

---

## 👥 Creación y gestión de usuarios

| Comando | Descripción |
|----------|-------------|
| `sudo useradd -m -s /bin/bash bchecker` | Crea el usuario **bchecker** con su carpeta personal. |
| `sudo passwd bchecker` | Asigna la contraseña del usuario (`bchecker121`). |
| `echo "bchecker:bchecker121" | sudo chpasswd` | Crea y asigna la contraseña al usuario directamente. |
| `sudo mkdir -p /home/bchecker/.ssh` | Crea el directorio `.ssh` del usuario si no existe. |

🔧 **Notas adicionales:**
- Este usuario se crea en **todas las máquinas** del proyecto.
- Se utiliza como usuario común de acceso y gestión (`bchecker / bchecker121`).

---

## 🔐 Configuración de claves SSH

### 🔹 Generación y copia de claves

| Comando | Descripción |
|----------|-------------|
| `ssh-keygen -t rsa -b 4096 -C "correo@itb.cat"` | Genera una nueva clave SSH RSA de 4096 bits. |
| `cat ~/.ssh/id_rsa.pub` | Muestra la clave pública generada. |
| `sudo nano /home/bchecker/.ssh/authorized_keys` | Abre el archivo para añadir las claves públicas de los clientes. |
| `sudo chown -R bchecker:bchecker /home/bchecker/.ssh` | Cambia la propiedad del directorio `.ssh` al usuario bchecker. |
| `sudo chmod 700 /home/bchecker/.ssh` | Asigna permisos de acceso correctos al directorio. |
| `sudo chmod 600 /home/bchecker/.ssh/authorized_keys` | Asegura que solo el usuario tenga acceso a sus claves. |

🧠 **Proceso resumido:**
1. Los clientes (Linux/Windows) generan su clave con `ssh-keygen`.
2. Copian su clave pública y la envían al servidor.
3. El servidor las añade al archivo `authorized_keys` del usuario `bchecker`.
4. Con esto se puede acceder sin contraseña y de forma más segura.

---

## 🚀 Activación del reenvío de paquetes

| Comando | Descripción |
|----------|-------------|
| `sudo nano /etc/sysctl.conf` | Abre el archivo de configuración del sistema. |
| `# net.ipv4.ip_forward = 1` → `net.ipv4.ip_forward = 1` | Se elimina el `#` para activar el reenvío IPv4. |
| `sudo sysctl -p` | Aplica los cambios del archivo sysctl inmediatamente. |
| `sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o ens1 -j MASQUERADE` | Configura la red NAT para enrutar todo el tráfico saliente. |

📘 **Explicación:**  
Esto permite que el servidor funcione como **router**, redirigiendo tráfico entre redes internas y externas (Intranet ↔ Internet).

---

## 🧱 Configuración de la DMZ

> **DMZ (Zona Desmilitarizada):**  
> Segmento de red que aloja servicios accesibles desde Internet, protegiendo la red interna.

| Elemento | Descripción |
|-----------|-------------|
| **Web** | Servidor accesible desde Internet (HTTP/HTTPS). |
| **BBDD** | Base de datos del sistema (MySQL). |
| **DNS** | Resolución de nombres de dominio. |

🔐 **Notas:**
- Los servidores en la DMZ **no pueden acceder directamente a la red interna**.  
- Su función es servir recursos públicos (web, DNS, FTP, etc.) sin exponer información sensible.

---

## 🗄️ Configuración de MySQL y base de datos

### 🔹 Acceso a MySQL

```bash
sudo mysql
