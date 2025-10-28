
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
```
---
