Manual de Administración del Sistema
====================================

Infraestructura Multicapa - Grupo 04
------------------------------------

**Versión:** 2.0 **Fecha:** Noviembre 2025 **Estado:** Producción

* * * * *

Índice
------

1.  [Introducción](https://www.google.com/search?q=%231-introducci%C3%B3n)

2.  [Arquitectura General del Sistema](https://www.google.com/search?q=%232-arquitectura-general-del-sistema)

3.  [Topología y Diseño de Red](https://www.google.com/search?q=%233-topolog%C3%ADa-y-dise%C3%B1o-de-red)

4.  [Infraestructura de Hardware (VMs)](https://www.google.com/search?q=%234-infraestructura-de-hardware-vms)

5.  [Servidor Router](https://www.google.com/search?q=%235-servidor-router-r-n04)

6.  [Servidor Web y FTP](https://www.google.com/search?q=%236-servidor-web-y-ftp-w-n04)

7.  [Servidor Base de Datos](https://www.google.com/search?q=%237-servidor-base-de-datos-b-n04)

8.  [Configuración de la Aplicación Web](https://www.google.com/search?q=%238-configuraci%C3%B3n-de-la-aplicaci%C3%B3n-web)

9.  [Protocolos de Seguridad y Mantenimiento](https://www.google.com/search?q=%239-protocolos-de-seguridad-y-mantenimiento)

10. [Troubleshooting](https://www.google.com/search?q=%2310-troubleshooting)

* * * * *

1\. Introducción
----------------

Este documento detalla la configuración técnica de la infraestructura de red del **Grupo 04**. Esta arquitectura ha sido optimizada para consolidar servicios en **3 servidores principales**, reduciendo la carga de virtualización sin sacrificar seguridad ni funcionalidad.

El sistema integra servicios web, transferencia de archivos segura (SFTP/FTP) y bases de datos relacionales, todo segmentado en dos redes lógicas gestionadas por un router central basado en Linux.

* * * * *

2\. Arquitectura General del Sistema
------------------------------------

### 2.1 Descripción Lógica

La red se divide en dos segmentos para cumplir con las mejores prácticas de seguridad (Defensa en Profundidad):

1.  **Red DMZ (192.168.10.0/24):** Zona de exposición. Aquí reside el servidor que interactúa con el exterior (Web y FTP).

2.  **Red Intranet (192.168.100.0/24):** Zona protegida. Aquí residen los datos sensibles (BBDD) y los clientes internos.

### 2.2 Inventario de Servidores

| Servidor | Hostname | IP Fija | Subred | Funciones Críticas |
| --- | --- | --- | --- | --- |
| **Router** | `Serv Route` | 10.1 / 100.1 | Dual | Gateway, DNS, DHCP, NAT, Firewall |
| **Web + FTP** | `FTP-APACHE2` | 192.168.10.10 | DMZ | Apache2, PHP, vsftpd/SFTP |
| **Datos** | `Serv DB` | 192.168.100.20 | Intranet | MySQL Server (Puerto 3306) |

Exportar a Hojas de cálculo

* * * * *

3\. Topología y Diseño de Red
-----------------------------

### 3.1 Esquema de Direccionamiento

-   **Router:**

    -   Interfaz WAN (Salida Internet): DHCP

    -   Interfaz DMZ (`ens19`): `192.168.10.1`

    -   Interfaz Intranet (`ens20`): `192.168.100.1`

-   **Web/FTP:**

    -   Interfaz DMZ: `192.168.10.10`

    -   Gateway: `192.168.10.1`

-   **BBDD:**

    -   Interfaz Intranet: `192.168.100.20`

    -   Gateway: `192.168.100.1`

### 3.2 Flujo de Datos

1.  **Petición Web:** El router redirige el tráfico HTTP al servidor `Serv Web` en la DMZ.

2.  **Consulta de Datos:** `Serv Web` procesa el PHP y lanza una consulta SQL a `Serv DB` a través del puerto 3306. El router permite este tráfico específico.

3.  **Transferencia de Archivos:** Los administradores suben archivos vía SFTP/FTP directamente a `Serv Web` para actualizar la web.

* * * * *

4\. Infraestructura de Hardware (VMs)
-------------------------------------

Todas las máquinas corren sobre **Ubuntu Server 22.04 LTS**.

### Servidor Routes

-   **RAM:** 2 GB | **CPU:** 2 vCores

-   **Red:** 3 Interfaces (WAN, DMZ, INTRA).

-   **Software:** `isc-dhcp-server`, `bind9`, `iptables-persistent`.

### Servidor de Aplicaciones y Archivos

-   **RAM:** 3 GB | **CPU:** 2 vCores

-   **Red:** 1 Interfaz (DMZ).

-   **Software:** `apache2`, `php8.1`, `php-mysql`, `vsftpd` (o OpenSSH SFTP).

-   **Justificación:** Se han unificado Web y FTP para facilitar que los archivos subidos por FTP estén inmediatamente disponibles en el directorio `/var/www/html`.

### Servidor de Datos

-   **RAM:** 2 GB | **CPU:** 2 vCores

-   **Red:** 1 Interfaz (Intranet).

-   **Software:** `mysql-server`.

* * * * *

5\. Servidor Router
---------------------------

### 5.1 Servicios de Red

#### DHCP (Red Interna 100.X)

El router entrega IPs dinámicas **solo** a la red Intranet.

-   **Archivo:** `/etc/dhcp/dhcpd.conf`

-   **Subnet:** `192.168.100.0` netmask `255.255.255.0`

-   **Rango:** `192.168.100.50` a `192.168.100.150`

-   **Router:** `192.168.100.1`

-   **DNS:** `192.168.100.1`, `8.8.8.8`

#### DNS (Bind9)

Resuelve los nombres internos para que los servidores se encuentren sin usar IPs.

-   **Archivo Zona:** `/etc/bind/db.grup04.com`

-   **Registros A:**

    -   `router` -> `192.168.10.1`

    -   `www` -> `192.168.10.10` (Web)

    -   `ftp` -> `192.168.10.10` (Web/FTP)

    -   `db` -> `192.168.100.30` (Base de Datos)

#### NAT y Enrutamiento

Habilitamos el bit de forwarding en `/etc/sysctl.conf`: `net.ipv4.ip_forward=1`

**Reglas iptables críticas:**

```
# NAT para salida a internet (Interface WAN = eth0, ejemplo)
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Permitir paso de DMZ a Intranet SOLO para MySQL (Puerto 3306)
iptables -A FORWARD -i ens19 -o ens20 -p tcp --dport 3306 -j ACCEPT

```

* * * * *

6\. Servidor Web y FTP 
------------------------------

Este servidor es híbrido. Aloja la web y permite la gestión de ficheros.

### 6.1 Servicio Web (Apache2)

-   **Document Root:** `/var/www/html/`

-   **Configuración:** VirtualHost escuchando en puerto 80.

-   **Módulos:** `mod_rewrite` activo para URLs amigables.

### 6.2 Servicio FTP/SFTP

Hemos configurado **SFTP** (sobre SSH) por ser más seguro que FTP tradicional, usando el puerto 22 o uno alternativo (ej. 2222).

**Configuración `/etc/ssh/sshd_config`:**

```
Subsystem sftp internal-sftp
Match Group sftpusers
    ChrootDirectory /var/www/html
    ForceCommand internal-sftp
    AllowTcpForwarding no

```

*Esto enjaula a los usuarios dentro de la carpeta web, ideal para subir actualizaciones de la página.*

**Usuario de gestión:**

-   User: `webuser`

-   Grupo: `pirineus`

-   Permisos: El usuario es propietario de `/var/www/html` para poder escribir.

* * * * *

7\. Servidor Base de Datos 
----------------------------------

Ubicado en la red **192.168.100.X** para máxima seguridad.

### 7.1 Configuración de Escucha

Por defecto MySQL no permite conexiones externas.

-   **Archivo:** `/etc/mysql/mysql.conf.d/mysqld.cnf`

-   **Directiva:** `bind-address = 0.0.0.0`

-   *Nota:* Esto hace que escuche en todas las interfaces, pero el firewall del Router protege el acceso.

### 7.2 Usuarios y Permisos

Se ha creado un usuario específico para la aplicación web.


```
CREATE USER 'bchecker'@'192.168.10.10' IDENTIFIED BY 'bchecker121';
GRANT SELECT, INSERT, UPDATE ON crud_db.* TO 'bchecker'@'192.168.10.10';
FLUSH PRIVILEGES;

```

> **Importante:** Fíjese que el usuario está restringido a la IP `192.168.10.10` (el servidor web). Si alguien intenta conectar desde otro lado, será rechazado.

* * * * *

8\. Configuración de la Aplicación Web
--------------------------------------

La aplicación PHP alojada en `W-N04` conecta contra la base de datos de la siguiente manera:

**Archivo `db.php`:**

PHP

```
<?php
$servername = "192.168.100.30"; 
$username = "bchecker";
$password = "bchecker121";
$dbname = "equipaments";

// Crear conexión
$conn = new mysqli($servername, $username, $password, $dbname);
?>

```

* * * * *

9\. Protocolos de Seguridad y Mantenimiento
-------------------------------------------

### 9.1 Acceso Administrativo (Backdoor)

Todos los servidores tienen el usuario de mantenimiento obligatorio:

-   **User:** `bchecker`

-   **Pass:** `bchecker121`

### 9.2 Copias de Seguridad

Debido a que la web y los datos están en servidores distintos, el backup debe ser coordinado.

1.  **Dump de BBDD (en Serv DB):** `mysqldump -u root -p crud_db > backup.sql`

2.  **Transferencia:** Se envía el `.sql` al servidor web vía SCP para tener una copia fuera de la Intranet.

* * * * *

10\. Troubleshooting
--------------------

### Problema: La web muestra "Connection Refused"

1.  Hacer ping desde `Serv Web` (10.10) a `B-N04` (100.20).

    -   *Si falla:* Revisar el Router (`Serv Route`), ¿está haciendo forwarding entre las interfaces `10.X` y `100.X`?

2.  Hacer telnet al puerto: `telnet 192.168.100.20 3306`.

    -   *Si falla:* Revisar el `bind-address` en la config de MySQL en `Serv DB`.

### Problema: No puedo subir archivos por FTP/SFTP

1.  Verificar que el servicio SSH/VSFTPD corre en `Serv Web`.

2.  Comprobar permisos de carpeta: El usuario `webadmin` debe tener permisos de escritura (`chmod 755` o `775`) en `/var/www/html`.

### Problema: Clientes Intranet sin Internet

1.  Verificar que tienen IP en el rango `192.168.100.X`.

2.  Verificar que su Gateway es `192.168.100.1`.

3.  Verificar NAT en el Router: `iptables -t nat -L -v`. Debe haber una regla MASQUERADE en la interfaz de salida.

* * * * *

**Documentación Técnica - Grupo 04**
---

<p align="left"><a href="./portadaadmin.md">Página anterior</a></p>
