# Hacer uso del servicio ftp

## Requisitos previos antes de hacer uso del servicio ftp

Comprobar la conexion con el servidor
```bash
ping 192.168.50.1
```

## Saber el usuario y contraseña para poder utilizar el servicio ftp

nombre = 
contraseña =

## Para conectarnos al ftp

```bash
ftp 192.168.50.1
```
nombre
contraseña

## Algunos comandos del ftp
---

| ls             | Listar archivos del servidor |
|cd directorio   | Cambiar directorio |
|pwd             | Ver directorio actual |
|get archivo     | Descargar archivo |
|put archivo     | Subir archivo |
|mget *.txt      | Descargar múltiples archivos |
|mput *.txt      | Subir múltiples archivos |
|delete archivo  | Eliminar archivo |
|mkdir carpeta   | Crear carpeta | 
|rmdir carpeta   | Eliminar carpeta |
|quit            | Cerrar conexión |

---
| [1. Objetivos del Proyecto](./objetivos.md)              | Qué se busca y ámbito del despliegue                         |
| [2. Requisitos y Alcance](./requisitos.md)              | Qué se debe cumplir y el alcance técnico                     |
| [3. Planificación y Roles](./planificacion.md)          | Organización del equipo y reparto de tareas                  |
| [4. Despliegue de Infraestructura](./despliegue.md)    | Montaje real y virtualización de redes y servicios           |
| [5. Documentación y Control de Versiones](./documentacion.md)     | Estructura del repositorio, uso de Markdown y GitHub         |

---



