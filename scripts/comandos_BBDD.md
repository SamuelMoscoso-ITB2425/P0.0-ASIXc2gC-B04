
# BBDD

Instalaremos el MySQL para poder tener nuestra BBDD y le assignaremos el usuario bchecker

Antes de empezar se recomienda tener todo actualiza con
```bash
sudo apt update
sudo apt upgrade
```

Instalacion del mysql 

---
```bash
sudo apt install mysql-server -y
```
---
Nos meteremos al mysql con root y crearemos el usuario

---
```bash
sudo mysql -u root
```
```bash
CREATE USER 'bchecker'@'%' IDENTIFIED BY 'bchecker121';
```
---
Por si quieres comprobar los usuarios creados en mysql

---
```bash
SELECT user,host FROM mysql.user;
```
---

---

Nos instalamos el CSV el qual queremos añadir en la BBDD 

---
```bash
sudo wget https://opendata-ajuntament.barcelona.cat/data/dataset/f36b60f2-9541-4d08-b0f9-b0a9313fab3d/resource/29d9ff10-6892-4f16-9012-d5c4997857e7/download
sudo mv /home/bchecker/download /home/bchecker/var/lib/mysql-files/
```

---

Cracion de la base de datos 

```bash
CREATE DATABASE B_N04;
GRANT ALL PRIVILEGES ON B_N04.* TO 'bchecker'@'%';
FLUSH PRIVILEGES;
```
```bash
USE B_N04
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
```bash
SET GLOBAL sql_mode='STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';
```

```bash
LOAD DATA INFILE 'download'
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
addresses_main_address = CASE WHEN @addresses_main_address = 'True' THEN 1
WHEN @addresses_main_address = 'False' THEN 0
ELSE NULL END,
values_outstanding = CASE WHEN @values_outstanding = 'True' THEN 1
WHEN @values_outstanding = 'False' THEN 0
ELSE NULL END,
start_date = NULLIF(@start_date, ''),
end_date = NULLIF(@end_date, '');
```
---
<div align="left"><a href="./comandos_apache.md">Página anterior</a></div>
