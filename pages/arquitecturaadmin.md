# Arquitectura de la Infraestructura

La infraestructura desplegada está diseñada para proporcionar una solución de red multicapa segura, escalable y eficiente, integrada por varios componentes clave que interactúan para soportar los servicios y aplicaciones de la organización.

---

## Componentes Principales

- **Router Principal:**  
  Configurado con interfaces específicas para la DMZ (zona desmilitarizada) e Intranet interna, lo que permite segmentar el tráfico y aumentar la seguridad. Se aplican políticas de NAT y filtrado para controlar el acceso entre redes.

- **VLANs (Redes Virtuales Locales):**  
  Se utilizan para segregar el tráfico de red, manteniendo separados los recursos críticos y limitando el alcance de posibles amenazas.

- **Servidor Web (Apache con PHP):**  
  Hospeda la aplicación web diseñada para interactuar con la base de datos, facilitando la gestión y visualización en tiempo real de la información.

- **Servidor de Base de Datos MySQL (`ncc_db`):**  
  Almacena de forma segura y estructurada los datos de la aplicación, asegurando integridad y disponibilidad.

- **Servicios DHCP y DNS:**  
  Proveen asignación dinámica de direcciones IP a los clientes y resolución rápida de nombres dentro de la red, mejorando la eficiencia de las comunicaciones.

- **Servicios SSH y FTP:**  
  Permiten la administración remota segura y la transferencia de archivos entre los diferentes sistemas involucrados.

---

## Representación Visual

![Arquitectura de Red Multicapa](../img/1topologia.png)

*Figura: Diagrama conceptual de la arquitectura de la infraestructura, mostrando la segmentación VLAN, los servicios desplegados y la interconexión entre dispositivos.*

---

## Navegación

[⬅️ Página Anterior](./descripcionadmin.md) | [Siguiente Página ➡️](./configuracionadmin.md)
