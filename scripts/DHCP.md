# DHCP

```bash
sudo apt-get update
sudo apt-get install isc-dhcp-server
isard@ServerProject:~$ sudo nano /etc/dhcp/dhcpd.conf
ption domain-name "ncc.local";
option domain-name-servers 192.168.50.1;

default-lease-time 600;
max-lease-time 7200;

authoritative;

subnet 192.168.50.0 netmask 255.255.255.0 {
    range 192.168.50.100 192.168.50.200;
    option routers 192.168.50.1;
    option broadcast-address 192.168.50.255;
}
isard@ServerProject:~$ sudo nano /etc/default/isc-dhcp-server
INTERFACESv4="enp3s0"
INTERFACESv6=""
isard@ServerProject:~$ sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
Synchronizing state of isc-dhcp-server.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable isc-dhcp-server
isard@ServerProject:~$ sudo systemctl status isc-dhcp-server
● isc-dhcp-server.service - ISC DHCP IPv4 server
     Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2025-10-27 16:45:34 CET; 7s ago
       Docs: man:dhcpd(8)
   Main PID: 5792 (dhcpd)
      Tasks: 4 (limit: 6968)
     Memory: 4.5M
        CPU: 25ms
     CGroup: /system.slice/isc-dhcp-server.service
             └─5792 dhcpd -user dhcpd -group dhcpd -f -4 -pf /run/dhcp-server/dhcpd.pid -cf /etc/dhcp/dhcpd.conf enp3s0

Oct 27 16:45:34 ServerProject dhcpd[5792]: PID file: /run/dhcp-server/dhcpd.pid
Oct 27 16:45:34 ServerProject dhcpd[5792]: Wrote 2 leases to leases file.
Oct 27 16:45:34 ServerProject sh[5792]: Wrote 2 leases to leases file.
Oct 27 16:45:34 ServerProject dhcpd[5792]: Listening on LPF/enp3s0/52:54:00:41:11:99/192.168.50.0/24
Oct 27 16:45:34 ServerProject sh[5792]: Listening on LPF/enp3s0/52:54:00:41:11:99/192.168.50.0/24
Oct 27 16:45:34 ServerProject sh[5792]: Sending on   LPF/enp3s0/52:54:00:41:11:99/192.168.50.0/24
Oct 27 16:45:34 ServerProject sh[5792]: Sending on   Socket/fallback/fallback-net
Oct 27 16:45:34 ServerProject dhcpd[5792]: Sending on   LPF/enp3s0/52:54:00:41:11:99/192.168.50.0/24
Oct 27 16:45:34 ServerProject dhcpd[5792]: Sending on   Socket/fallback/fallback-net
Oct 27 16:45:34 ServerProject dhcpd[5792]: Server starting service.
isard@ServerProject:~$ 



sudo nano /etc/netplan/01-network-manager-all.yaml
network:
  version: 2
  ethernets:
    enp3s0:
      dhcp4: true             # Habilita DHCP per a aquesta interfície interna
    enp1s0:
      dhcp4: true             # Conserva o ajusta segons la teva xarxa WAN/NAT
    enp2s0:
      dhcp4: false
      addresses: [192.168.60.11/24]
      gateway4: 192.168.60.2
      nameservers:
        addresses: [8.8.8.8, 192.168.60.1]

sudo netplan apply

ard@ClnProject1:~$ ip a show enp3s0
4: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:16:aa:98 brd ff:ff:ff:ff:ff:ff
    inet 192.168.50.102/24 metric 100 brd 192.168.50.255 scope global dynamic enp3s0
       valid_lft 521sec preferred_lft 521sec
    inet6 fe80::5054:ff:fe16:aa98/64 scope link 
       valid_lft forever preferred_lft forever
isard@ClnProject1:~$ dig @192.168.50.1 R-NCC.ncc.local

; <<>> DiG 9.18.39-0ubuntu0.22.04.1-Ubuntu <<>> @192.168.50.1 R-NCC.ncc.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 62927
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 44ee0f5a521f80c20100000068ff94cfb0b235491ff769f1 (good)
;; QUESTION SECTION:
;R-NCC.ncc.local.		IN	A

;; ANSWER SECTION:
R-NCC.ncc.local.	604800	IN	A	192.168.50.2

;; Query time: 3 msec
;; SERVER: 192.168.50.1#53(192.168.50.1) (UDP)
;; WHEN: Mon Oct 27 16:50:39 CET 2025
;; MSG SIZE  rcvd: 88

```
