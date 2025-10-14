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
6. [Documentación y Control de Versiones](#documentación-y-control-de-versiones)
7. [Autores](#autores)

---

## Descripción del Proyecto

Este proyecto consiste en desplegar y documentar una infraestructura multicapa para servicios de red y aplicaciones, simulando el entorno de una organización. El objetivo es integrar varios servicios críticos (Web, SSH, BBDD, DHCP, DNS, FTP) sobre una topología de red segmentada.

---

## Objetivos

- Desplegar una infraestructura de red de 3 segmentos: DMZ, Intranet y NAT.
- Configurar varios servicios: Web Server, SSH, BBDD (MySQL), DHCP, DNS, FTP.
- Importar datos reales de equipamientos educativos de Barcelona en la BBDD.
- Garantizar acceso con usuario común: `bchecker` / `bchecker121`.
- Documentar todo el proceso y mantener el control de versiones con Git.

---

## Requisitos

- **Planificación en Proofhub** con sprints quinzenales (10h cada uno, 3 sprints).
- **Repositorio GitHub** con la nomenclatura: `P0.0-ASIXc2gC-Gnn`
- **Diagrama de arquitectura** de la infraestructura.
- **Configuración y documentación** de todos los servicios, accesibles con usuario `bchecker`.
- **Control de versiones Git** (ADD/PUSH/PULL/CLONE/COMMIT).
- **Documentación Markdown** organizada y clara.

---

## Planificación y Sprints

### Sprint 1: Planificación y infraestructura base

- Definir diagrama de arquitectura.
- Crear repositorio GitHub.
- Configurar acceso SSH por clave pública/privada.
- Documentar control de versiones.
- Desplegar router (R-NCC) y redes.
- Configurar usuarios.
- Instalar y configurar servidor Linux principal.

### Sprint 2: Despliegue de servicios

- Web Server (W-NCC)
- SSH y BBDD (B-NCC, MySQL, carga CSV de equipamientos educativos)
- DHCP, DNS (resolución R-NCC y R), FTP (F-NCC)
- Documentación de servicios

### Sprint 3: Clientes y pruebas

- Desplegar PCs cliente (Linux y Windows)
- Acceso a servicios desde clientes
- Desplegar aplicación de prueba (mostrar contenido MySQL)
- Documentar pruebas y resultados
- Revisión final de documentación

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
        L1[Client Linux 1]
        L2[Client Linux 2]
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

## Documentación y Control de Versiones

- Estructura de documentación en Markdown.
- Guía de comandos Git y despliegues.
- Ejemplos de flujo de trabajo: `git add`, `git commit -m`, `git push`, `git pull`, `git clone`.

---

## Autores

- Grupo: **Gnn** (A/B)
- Participantes:  
  - [Adrià Montero]
  - [Samuel Moscoso]
  - [adrian tamargo]

---

```
> **¡Recuerda mantener actualizada la documentación y los diagramas a medida que avances en los sprints!**
```
