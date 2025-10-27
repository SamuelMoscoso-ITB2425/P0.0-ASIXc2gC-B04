# ⚙️ Comandos de Configuración del Proyecto NCC

> **Projecte Intermodular d'Administració de Sistemes Informàtics en Xarxa**  
> Documentación completa de los comandos utilizados en la configuración, despliegue y administración del Proyecto **NCC**.  
> **Autores:** Grupo GNN – ITB · 2025  

---

## 📑 Índice

1. [🌐 Configuración de red y NAT](#-configuración-de-red-y-nat)  
2. [👥 Creación y gestión de usuarios](#-creación-y-gestión-de-usuarios)  
3. [🔐 Configuración de claves SSH](#-configuración-de-claves-ssh)  
4. [🧱 Configuración de la DMZ](#-configuración-de-la-dmz)  
5. [🗄️ Configuración de MySQL y base de datos](#️-configuración-de-mysql-y-base-de-datos)  
6. [📦 Carga de datos CSV](#-carga-de-datos-csv)  
7. [📊 Consultas SQL](#-consultas-sql)  
8. [✅ Conclusión](#-conclusión)

---

## 🌐 Configuración de red y NAT

Configura el servidor como router para permitir que la red interna acceda a Internet mediante NAT.

| Comando | Descripción |
|----------|--------------|
| `sudo nano /etc/sysctl.conf` | Abre la configuración del sistema. |
| `net.ipv4.ip_forward=1` | Activa el reenvío de paquetes IP. |
| `sudo sysctl -p` | Aplica los cambios del archivo `sysctl.conf`. |
| `sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o ens1 -j MASQUERADE` | Activa NAT para la red 192.168.50.0/24. |

💡 **Nota:** Los clientes deben tener como *gateway* la IP del servidor para enrutar correctamente el tráfico.

---

## 👥 Creación y gestión de usuarios

Creación del usuario común **bchecker** y configuración de acceso SSH.

```bash
sudo useradd -m bchecker
echo "bchecker:bchecker121" | sudo chpasswd
sudo mkdir -p /home/bchecker/.ssh
sudo nano /home/bchecker/.ssh/authorized_keys
sudo chown -R bchecker:bchecker /home/bchecker/.ssh
sudo chmod 700 /home/bchecker/.ssh
sudo chmod 600 /home/bchecker/.ssh/authorized_keys
```

🧠 **Resumen:**  
Crea el usuario, le asigna contraseña y permisos seguros para conexión SSH.

---

## 🔐 Configuración de claves SSH

Configura acceso sin contraseña mediante claves RSA de 4096 bits.

### 🔹 En el servidor

```bash
ssh-keygen -t rsa -b 4096 -C "adria.montero.7e5@itb.cat"
cat ~/.ssh/id_rsa.pub
```

Ejemplo de clave pública:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC6yVbvTJ/1pZ48h7Gb4k20Yt4Q9M50QLpFLaawDOLQCtkzmYOlqWy1Y...
```

### 🔹 En los clientes

```bash
ssh-keygen -t rsa -b 4096 -C "samuel.moscoso.7e8@itb.cat"
cat ~/.ssh/id_rsa.pub
sudo nano /home/bchecker/.ssh/authorized_keys
sudo chown -R bchecker:bchecker /home/bchecker/.ssh
sudo chmod 700 /home/bchecker/.ssh
sudo chmod 600 /home/bchecker/.ssh/authorized_keys
```

**Flujo general:**
1. El cliente genera su clave.  
2. Envía la clave pública al servidor.  
3. El servidor la añade a `authorized_keys`.  
4. Acceso sin contraseña.

---

## 🧱 Configuración de la DMZ

> **DMZ (Zona Desmilitarizada):** zona para alojar servicios accesibles desde Internet sin exponer la red interna.

| Servicio | Descripción |
|-----------|-------------|
| Web | Servidor HTTP público. |
| BBDD | Servidor MySQL accesible desde fuera. |
| DNS | Resolución de nombres externos. |

💡 **Importante:** La DMZ aísla los servicios públicos del resto de la red para mejorar la seguridad.

---

## 🗄️ Configuración de MySQL y base de datos

Accede al monitor MySQL y crea la base de datos y usuario del proyecto.

```bash
sudo mysql
```

```sql
CREATE DATABASE ncc_db;
CREATE USER 'bchecker'@'%' IDENTIFIED BY 'bchecker121';
GRANT ALL PRIVILEGES ON ncc_db.* TO 'bchecker'@'%';
FLUSH PRIVILEGES;
USE ncc_db;

CREATE TABLE equipaments (
  register_id BIGINT PRIMARY KEY,
  name VARCHAR(255),
  institution_id BIGINT,
  institution_name VARCHAR(255),
  created DATETIME,
  modified DATETIME,
  addresses_roadtype_id INT,
  addresses_roadtype_name VARCHAR(255),
  addresses_road_id INT,
  addresses_road_name VARCHAR(255),
  addresses_start_street_number VARCHAR(50),
  addresses_end_street_number VARCHAR(50),
  addresses_neighborhood_id INT,
  addresses_neighborhood_name VARCHAR(255),
  addresses_district_id INT,
  addresses_district_name VARCHAR(255),
  addresses_zip_code VARCHAR(20),
  addresses_town VARCHAR(255),
  addresses_main_address BOOLEAN,
  addresses_type VARCHAR(50),
  values_id INT,
  values_attribute_id INT,
  values_category VARCHAR(255),
  values_attribute_name VARCHAR(255),
  values_value VARCHAR(255),
  values_outstanding BOOLEAN,
  values_description TEXT,
  secondary_filters_id INT,
  secondary_filters_name VARCHAR(255),
  secondary_filters_fullpath TEXT,
  secondary_filters_tree TEXT,
  secondary_filters_asia_id INT,
  geo_epgs_25831_x DOUBLE,
  geo_epgs_25831_y DOUBLE,
  geo_epgs_4326_lat DOUBLE,
  geo_epgs_4326_lon DOUBLE,
  estimated_dates VARCHAR(255),
  start_date DATE,
  end_date DATE,
  timetable TEXT
);
```

---

## 📦 Carga de datos CSV

Copiar archivo CSV desde cliente al servidor:

```bash
scp /home/isard/Descargas/opendatabcn_llista-equipaments_educacio-csv.csv isard@192.168.50.1:/home/isard/
```

Importar datos en MySQL:

```sql
SET GLOBAL sql_mode='STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

LOAD DATA INFILE '/var/lib/mysql-files/opendatabcn_utf8_nobom.csv'
INTO TABLE equipaments
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(@dummy, name, @institution_id, institution_name, created, modified,
 @addresses_roadtype_id, addresses_roadtype_name, @addresses_road_id, addresses_road_name,
 addresses_start_street_number, addresses_end_street_number, @addresses_neighborhood_id,
 addresses_neighborhood_name, @addresses_district_id, addresses_district_name, addresses_zip_code,
 addresses_town, @addresses_main_address, addresses_type, @values_id, @values_attribute_id,
 values_category, values_attribute_name, values_value, @values_outstanding, values_description,
 @secondary_filters_id, secondary_filters_name, secondary_filters_fullpath, secondary_filters_tree,
 @secondary_filters_asia_id, geo_epgs_25831_x, geo_epgs_25831_y, geo_epgs_4326_lat, geo_epgs_4326_lon,
 estimated_dates, @start_date, @end_date, timetable)
SET addresses_roadtype_id = NULLIF(@addresses_roadtype_id, ''),
    addresses_road_id = NULLIF(@addresses_road_id, ''),
    addresses_neighborhood_id = NULLIF(@addresses_neighborhood_id, ''),
    addresses_district_id = NULLIF(@addresses_district_id, ''),
    values_id = NULLIF(@values_id, ''),
    values_attribute_id = NULLIF(@values_attribute_id, ''),
    secondary_filters_id = NULLIF(@secondary_filters_id, ''),
    secondary_filters_asia_id = NULLIF(@secondary_filters_asia_id, ''),
    institution_id = NULLIF(@institution_id, ''),
    addresses_main_address = CASE WHEN @addresses_main_address='True' THEN 1 WHEN @addresses_main_address='False' THEN 0 ELSE NULL END,
    values_outstanding = CASE WHEN @values_outstanding='True' THEN 1 WHEN @values_outstanding='False' THEN 0 ELSE NULL END,
    start_date = NULLIF(@start_date, ''),
    end_date = NULLIF(@end_date, '');
```

---

## 📊 Consultas SQL

```sql
SELECT COUNT(*) FROM equipaments;
SELECT * FROM equipaments LIMIT 10;
SELECT * FROM equipaments WHERE name LIKE '%Escola%' LIMIT 10;
```

---

## ✅ Conclusión

> Con esta configuración se dispone de un entorno completo:  
> - Servidor con NAT y reenvío activado  
> - Acceso SSH seguro mediante claves  
> - DMZ funcional para servicios públicos  
> - Base de datos MySQL cargada con datos  
>
> 🚀 **Sistema listo para el despliegue del Proyecto NCC**
