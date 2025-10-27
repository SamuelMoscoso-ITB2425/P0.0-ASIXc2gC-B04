# ⚙️ Comandos de Configuración del Proyecto NCC

> **Projecte Intermodular d'Administració de Sistemes Informàtics en Xarxa**  
> Documentación completa de los comandos utilizados en la configuración, despliegue y administración del Proyecto **NCC**.  
> **Autores:** Grupo GNN – ITB · 2025  

---

## 👥 Creación y gestión de usuarios

Creación del usuario común y configuración de acceso seguro.

```bash
sudo useradd -m -s /bin/bash bchecker
sudo usermod sudo bchecker
echo "bchecker:bchecker121" | sudo chpasswd
```
---

🧠 **Explicación:**  
Crea el usuario `bchecker`, define su contraseña y configura la autenticación SSH con permisos seguros.

---

## 🔐 Configuración de claves SSH

Generación de claves en el servidor y clientes, y configuración de acceso sin contraseña.

### En el servidor

```bash
ssh-keygen -t rsa -b 4096 -C "adria.montero.7e5@itb.cat"
cat ~/.ssh/id_rsa.pub
```

Ejemplo de clave pública:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC6yVbvTJ/1pZ48h7Gb4k20Yt4Q9M50QLpFLaawDOLQCtkzmYOlqWy1Y/EsXT0pQO3vJ/EGeBWICFpM4dUBoAGzAk8j0SW+0kMqk284+1HNJK7P0nBeM8sBLzyeUSVayRbvgB20DbMKAsSA0Tlqk+F1dL36ZjVQjnDDVxUjwvY7jFpGOexAX/X03lt62jXwMsWXy4eEjCFSnq+TMI6sSEIAXY9j84cVrEQefHJnMbC9P3gjDXgX+QNA7Gdcwi9NdKkrSQjwWThNxY/aa7PLcJiFvJJ9ANVRovFL9liFajeaQZ6vD9ZXJ5XCY21fbfLPjqIwvoFgqSdoH6yWSKZdzDKoNv9Kbac9cvVcTfT+pVXE5PQ5bgm3ibVkzqZoJXdnjK2YN64bw0aj1ky3AW0rkeGPOYMITQHDV7B5fyX6q6yQQJcyNq5ZKICt0KlLYTwVRSY4gB5TZTFMTpEQGrNht7Sg42FtkYFH8oSHxH3MYfkRhtJxSPoAHGrWfNdXaRwQ5XNtCLzKpBoHYHzCsinoq3B8Re7+kBQuexoj5eyp8WHIGBVuP8l0qHO7PCv8El9ft3u8kO8/rei4GgaMp7T3XF1JE2Xkot14DLrPVoJVVBppN/0+MFqoSbBpW1+ghmVbNHyDQt2UKbeoCNVVBhS0r7W6nx7S7MUws04YKfeZFAaGHw== adria.montero.7e5@itb.cat
```

### En los clientes

```bash
ssh-keygen -t rsa -b 4096 -C "samuel.moscoso.7e8@itb.cat"
cat ~/.ssh/id_rsa.pub
sudo nano /home/bchecker/.ssh/authorized_keys
sudo chown -R bchecker:bchecker /home/bchecker/.ssh
sudo chmod 700 /home/bchecker/.ssh
sudo chmod 600 /home/bchecker/.ssh/authorized_keys
```

**Flujo de trabajo:**
1. Los clientes generan sus claves.
2. Copian la clave pública al servidor.
3. El servidor la agrega a `authorized_keys`.
4. Los accesos se realizan sin contraseña.

---

## 🌐 Configuración de red y NAT

Configuración del servidor como router/NAT para permitir la salida a Internet desde la red interna.

```bash
sudo nano /etc/sysctl.conf
# Quitar el comentario en la siguiente línea:
# net.ipv4.ip_forward = 1  →  net.ipv4.ip_forward = 1
sudo sysctl -p
sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o ens1 -j MASQUERADE
```

💡 **Explicación corta:**  
Activa el reenvío de paquetes y aplica una regla de NAT para que los equipos de la red `192.168.50.0/24` salgan a Internet a través del servidor.

---

## 🌐 Configuración adicional de red

```bash
ip a
sudo nano /etc/netplan/00-installer-config.yaml
sudo netplan apply
net.ipv4.ip_forward=1
sudo sysctl -p
sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o enp1s0 -j MASQUERADE
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

💡 Los clientes deben tener como *gateway* la IP del servidor.

---

## 🧱 Configuración de la DMZ

> **DMZ (Zona Desmilitarizada)**  
> Espacio para alojar servicios públicos (web, base de datos y DNS) sin comprometer la red interna.

- Web  
- BBDD  
- DNS  

---

## 🗄️ Configuración de MySQL y base de datos

Conexión al monitor y creación de la base de datos y usuario.

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
LOAD DATA INFILE '/var/lib/mysql-files/opendatabcn_utf8_nobom.csv' ...
```

---

## 📊 Consultas SQL

```sql
SELECT COUNT(*) FROM equipaments;
SELECT * FROM equipaments LIMIT 10;
SELECT * FROM equipaments WHERE name LIKE '%Escola%' LIMIT 10;
```

---

## 🧩 Conclusión

> Con esta configuración se dispone de un entorno completo:  
> - Servidor con NAT y reenvío activado  
> - Acceso SSH seguro mediante claves  
> - DMZ funcional para servicios públicos  
> - Base de datos MySQL cargada con datos  
>  
> ✅ Sistema listo para el despliegue del **Proyecto NCC**.
