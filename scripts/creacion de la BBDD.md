
# BBDD

Instalaremos el MySQL para poder tener nuestra BBDD y le assignaremos el usuario bchecker

```bash
sudo apt install mysql-server
sudo mysql -u root
CREATE USER 'bchecker'@'localhost' IDENTIFIED BY 'bchecker121';

```

Nos instalamos el CSV el qual queremos añadir en la BBDD 

---
```bash
sudo wget https://opendata-ajuntament.barcelona.cat/data/dataset/f36b60f2-9541-4d08-b0f9-b0a9313fab3d/resource/29d9ff10-6892-4f16-9012-d5c4997857e7/download
```
---
