# Hacer uso del servicio ftp

## Requisitos previos antes de hacer uso del servicio ftp

Comprobar la conexion con el servidor
```bash
ping 192.168.60.1
```

## Saber el usuario y contraseña para poder utilizar el servicio ftp

<p>nombre = bchecker<p>
<p>contraseña = bchecker121<p>

## Para conectarnos al ftp

```bash
ftp 192.168.60.1
```
<p>bchecker<p>
<p>bchecker121<p>

## Algunos comandos del ftp

---
<p>ls    Listar archivos del servidor<p>
<p>cd 'directorio'  Cambiar directorio<p>
<p>pwd  Ver directorio actual<p>
<p>get 'archivo'  Descargar archivo<p>
<p>put 'archivo'   Subir archivo<p>
<p>mget *.txt  Descargar múltiples archivos<p>
<p>mput *.txt   Subir múltiples archivos<p>
<p>delete 'archivo'  Elimnar archivo<p>
<p>mkdir 'archivo'  Elimnar archivo<p>
<p>rmdir 'carpeta'  Elimar carpeta<p>
<p>quit  Cerrar conexión<p>

---
