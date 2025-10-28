
# Creacion del Usuario

La practica no pide que añadamos en todas las maquina y montemos las aplicaciones con el usuario bchecker 

---
```bash
sudo useradd -m -s /bin/bash bchecker
sudo usermod -aG sudo bchecker
echo "bchecker:bchecker121" | sudo chpasswd
```
---
Aunque salte este mensaje la contraseña (Poner mensaje) esta se cambiara a la que le hemos puesto con echo
