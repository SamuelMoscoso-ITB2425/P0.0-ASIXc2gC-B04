
# Creacion del Usuario

La practica no pide que añadamos en todas las maquina y montemos las aplicaciones con el usuario bchecker 

---
```bash
sudo useradd -m -s /bin/bash bchecker
sudo usermod -aG sudo bchecker
echo "bchecker:bchecker121" | sudo chpasswd
```
---
Aunque salte este mensaje la contraseña (CONTRASEÑA INCORRECTA: De alguna manera, en la contraseña se lee el nombre del usuario) esta se cambiara a la que le hemos puesto con echo

---
<div align="left"><a href="../pages/docu_scripts.md">Página anterior</a></div>
<div align="right"><a href="../scripts/comandos_DHCP.md">Siguiente página</a></div>

