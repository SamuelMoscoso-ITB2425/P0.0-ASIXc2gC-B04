<!-- 1. OBJETIVOS DEL PROYECTO -->

# 1. Objetivos del Proyecto

> Desplegar una infraestructura multicapa, segura y eficiente, para ofrecer servicios integrados en un entorno profesional.  
> El objetivo es favorecer la conectividad, la administración y la demostración de buenas prácticas IT.

---

## Metas generales

- Implementar **dos servidores físicos** que alberguen servicios críticos (web, base de datos, DNS, DHCP, FTP, monitorización).
- Segmentar la red en **DMZ**, **Intranet** y **NAT** para maximizar la seguridad y el control del tráfico.
- Documentar todo el proceso en **Markdown** y alojarlo en un **repositorio GitHub** gestionando versiones y accesos.

---

## Objetivos específicos

| Fase                    | Objetivo                                                         | Resultado esperado         |
|-------------------------|------------------------------------------------------------------|---------------------------|
| Diseño de topología     | Planificar y dibujar la arquitectura física y lógica             | Diagrama profesional      |
| Despliegue de servicios | Instalar y configurar servicios integrados en los servidores     | Infraestructura funcional |
| Clientes                | Integrar dos estaciones, una Windows y una Linux                 | Acceso validado           |
| Seguridad               | Implementar autenticación y segmentación correcta                | Red segura y aislada      |
| Documentación           | Redactar todo el proceso en Markdown y almacenar en GitHub       | Manual técnico completo   |
| Validación              | Mostrar evidencias mediante logs, pruebas y aplicación demo      | Proceso auditable         |

---

## Ejemplo de topología del proyecto

![Diagrama de red y servicios](../img/1topologia.png)

La topología muestra una red dividida en tres zonas. Internet se conecta al router principal, que recibe la conexión y la pasa al router central, el cual controla toda la red. La LAN (verde) es la red interna segura con un PC Windows, un PC Linux y un servidor de base de datos y DHCP, todos conectados a un switch y al router central. La DMZ (rojo) es una zona semiprotegida con servidores DNS, FTP y web, conectados a su propio switch y separados de la LAN. El router central controla el acceso entre LAN, DMZ e Internet, separando los equipos internos de los servicios públicos y protegiendo la red.

---

<p align="left"><a href="./indice_proyecto.md">Página anterior</a></p>
<p align="right"><a href="./requisitos.md">Siguiente página</a></p>
