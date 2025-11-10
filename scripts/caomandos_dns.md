# DNS 

```bash
sudo apt-get update
sudo apt-get install bind9 bind9utils bind9-doc dnsutils
sudo nano /etc/bind/named.conf.local
one "ncc.local" {
    type master;
    file "/etc/bind/db.ncc.local";
};
sudo nano /etc/bind/db.ncc.local
 GNU nano 6.2                        /etc/bind/db.ncc.local                                 
$TTL    604800
@       IN      SOA     ns.ncc.local. admin.ncc.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.ncc.local.
ns      IN      A       192.168.50.1
R-NCC   IN      A       192.168.50.2
R       IN      A       192.168.50.3

isard@ClnProject1:~$ dig @192.168.50.1 R-NCC.ncc.local

; <<>> DiG 9.18.39-0ubuntu0.22.04.1-Ubuntu <<>> @192.168.50.1 R-NCC.ncc.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 49171
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 1856a8368f034bf50100000068ff964178e585080d65ecae (good)
;; QUESTION SECTION:
;R-NCC.ncc.local.		IN	A

;; ANSWER SECTION:
R-NCC.ncc.local.	604800	IN	A	192.168.50.2

;; Query time: 0 msec
;; SERVER: 192.168.50.1#53(192.168.50.1) (UDP)
;; WHEN: Mon Oct 27 16:56:49 CET 2025
;; MSG SIZE  rcvd: 88

isard@ClnProject1:~$ 
isard@ServerProject:~$ dig @localhost R-NCC.ncc.local

; <<>> DiG 9.18.39-0ubuntu0.22.04.2-Ubuntu <<>> @localhost R-NCC.ncc.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 56860
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 3de4994da60238200100000068ff96117b9282b04e848cc0 (good)
;; QUESTION SECTION:
;R-NCC.ncc.local.		IN	A

;; ANSWER SECTION:
R-NCC.ncc.local.	604800	IN	A	192.168.50.2

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(localhost) (UDP)
;; WHEN: Mon Oct 27 16:56:01 CET 2025
;; MSG SIZE  rcvd: 88

```
