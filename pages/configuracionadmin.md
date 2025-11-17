# 4. Configuración de Dispositivos y Servicios

Esta sección detalla los comandos y pasos clave para configurar y verificar los dispositivos y servicios implementados en la infraestructura.

---

## Router Principal (R-NCC)

Se recomienda utilizar los siguientes comandos para revisar y administrar la configuración del router Cisco que gestiona las VLANs, rutas y demás parámetros de red:

- `show running-config`  
  Visualiza la configuración completa activa en el router.

- `show vlan brief`  
  Muestra un resumen de las VLANs configuradas y sus puertos asignados.

- `show ip route`  
  Muestra la tabla de rutas IP configurada, fundamental para la gestión del tráfico entre subredes.

> **Nota:** Para asegurar la correcta segmentación con la DMZ, es fundamental que el router tenga configuradas las reglas de firewall adecuadas que permitan el acceso restringido a la red interna desde la DMZ, manteniendo así la seguridad.

---

## Servidores Ubuntu

### Apache (Servidor Web)

Verifica el estado y posibles errores del servicio web:

sudo systemctl status apache2
sudo tail -n 50 /var/log/apache2/error.log


### MySQL (Base de Datos)

Comprueba que el servicio de base de datos esté activo y manipula la base `ncc_db`:

sudo systemctl status mysql
mysql -u bchecker -p ncc_db


### DHCP (Asignación dinámica de IP)

Revisa el estado del servicio DHCP responsable de asignar las IPs dinámicas:

sudo systemctl status isc-dhcp-server


### SSH (Acceso remoto seguro)

Asegúrate de que el servicio SSH funcione correctamente para administración remota:

sudo systemctl status ssh

text

---

## Configuraciones PHP

El archivo principal de la aplicación se encuentra en:

`/var/www/html/equipaments.php`

Este script contiene la lógica para la consulta a la base de datos y la presentación estilizada de los datos en forma de tabla, proporcionando una interfaz web funcional para la gestión y visualización de la información.

---

## Navegación

[⬅️ Página Anterior](./arquitecturaadmin.md) | [Siguiente Página ➡️](./administracionadmin.md)
