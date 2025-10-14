# P0.0-ASIXc2gC-Gnn

> **Projecte Intermodular d'administració de sistemes informàtics en xarxa**  
> Activitat Pràctica P0.0 — Desplegament d’infraestructura

---

## Índice

1. [Descripción del Proyecto](#descripción-del-proyecto)
2. [Objetivos](#objetivos)
3. [Requisitos](#requisitos)
4. [Planificación y Sprints](#planificación-y-sprints)
5. [Arquitectura de la Infraestructura](#arquitectura-de-la-infraestructura)
6. [Desglose de Responsabilidades](#desglose-de-responsabilidades)
7. [Documentación y Control de Versiones](#documentación-y-control-de-versiones)
8. [Autores](#autores)

---

## Descripción del Proyecto

Este proyecto consiste en el despliegue, configuración y documentación de una infraestructura multicapa para servicios de red y aplicaciones, simulando el entorno de una organización real. El objetivo principal es integrar y hacer funcionar servicios críticos (Web, SSH, BBDD, DHCP, DNS, FTP) sobre una topología de red segmentada, así como demostrar su funcionamiento mediante clientes Linux y Windows.

---

## Objetivos

- Desplegar una infraestructura de red segmentada en DMZ, Intranet y NAT.
- Configurar y documentar servicios: Web Server, SSH, BBDD (MySQL), DHCP, DNS, FTP.
- Importar datos reales (CSV de equipamientos educativos de Barcelona) en la BBDD.
- Garantizar acceso con usuario común: `bchecker` / `bchecker121` en todos los sistemas y servicios.
- Documentar todo el proceso y mantener el control de versiones con Git.
- Desplegar y probar la conectividad y acceso desde clientes Linux y Windows.

---

## Requisitos

- **Planificación en Proofhub** con sprints quinzenales (10h cada uno, 3 sprints).
- **Repositorio GitHub** con la nomenclatura: `P0.0-ASIXc2gC-Gnn`.
- **Diagrama de arquitectura** actualizado en la documentación.
- **Configuración y documentación** de todos los servicios, accesibles con usuario `bchecker`.
- **Control de versiones Git** (ADD/PUSH/PULL/CLONE/COMMIT).
- **Documentación Markdown** organizada y clara.
- **Aplicación de prueba** que muestre datos reales desde la BBDD.

---

## Planificación y Sprints

### Sprint 1: Planificación y infraestructura base

- Definir y consensuar el diagrama de arquitectura.
- Crear repositorio GitHub y estructura de documentación.
- Configurar acceso SSH por clave pública/privada.
- Documentar control de versiones y procedimientos.
- Desplegar router (R-NCC) y creación de redes (DMZ, Intranet, NAT).
- Configurar usuarios y permisos necesarios.
- Instalar y configurar el servidor Linux principal.

### Sprint 2: Despliegue de servicios

- Instalación y configuración de Web Server (W-NCC), SSH, BBDD (B-NCC, MySQL), carga de CSV.
- Instalación y configuración de servicios de red: DHCP, DNS (resolución R-NCC y R), FTP (F-NCC).
- Documentación detallada de la configuración de cada servicio.

### Sprint 3: Clientes y pruebas

- Despliegue y configuración de PCs cliente (Linux y Windows).
- Pruebas de acceso y conectividad a todos los servicios desde los clientes.
- Desarrollo y despliegue de aplicación de prueba (visualización de datos MySQL).
- Documentación de pruebas, incidencias y resultados.
- Revisión final de toda la documentación, cierre del proyecto y presentación.

---

## Arquitectura de la Infraestructura

```mermaid
graph TD
    subgraph DMZ
        W[Web Server (W-NCC)]
        F[FTP Server (F-NCC)]
        D[DNS Server]
    end
    subgraph Intranet
        B[BBDD (B-NCC)]
        L1[Client Linux 1 (192.168.50.20)]
        L2[Client Linux 2 (192.168.50.30)]
        Win[Client Windows]
    end
    subgraph NAT
        R[Router (R-NCC)]
    end
    W -- DMZ --> R
    F -- DMZ --> R
    D -- DMZ --> R
    B -- Intranet --> R
    L1 -- Intranet --> R
    L2 -- Intranet --> R
    Win -- Intranet --> R
```

---

## Desglose de Responsabilidades

- **Infraestructura y servicios:**  
  - Configuración del router, redes, y despliegue de servidores (Web, SSH, BBDD, DHCP, DNS, FTP).
- **Clientes y pruebas:**  
  - Instalación y configuración de los equipos cliente (Windows y Linux).
  - Pruebas de acceso, conectividad y funcionalidad de los servicios.
  - Desarrollo de la aplicación de prueba (visualización MySQL).
- **Documentación y control de versiones:**  
  - Mantener el repositorio actualizado.
  - Documentar procedimientos, incidencias y soluciones.
  - Elaborar el árbol de documentación en Markdown y el diagrama de arquitectura.

---

## Documentación y Control de Versiones

- Estructura de documentación en Markdown.
- Guía de comandos Git y despliegues.
- Ejemplos de flujo de trabajo:  
  - `git add`
  - `git commit -m "mensaje"`
  - `git push`
  - `git pull`
  - `git clone`
- Registro de incidencias y soluciones durante el desarrollo.

---

## Autores

- Grupo: **Gnn** (A/B)
- Participantes:  
  - Adrià Montero  
  - Samuel Moscoso  
  - Adrian Tamargo

---

```
