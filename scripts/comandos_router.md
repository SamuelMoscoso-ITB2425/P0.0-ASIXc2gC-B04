# Router

``bash
sudo nano /etc/netplan/00-installer-config.yaml
sudo netplan apply
net.ipv4.ip_forward=1
sudo sysctl -p
sudo iptables -t nat -A POSTROUTING -s 192.168.50.0/24 -o enp1s0 -j MASQUERADE
sudo apt install iptables-persistent
sudo netfilter-persistent save

``
