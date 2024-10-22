# I. Le setup
## 2. Marche à suivre
### Prouvez que...
```
[adam@dhcp ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=20.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=20.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=20.7 ms
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2006ms
rtt min/avg/max/mdev = 20.179/20.565/20.788/0.274 ms

[adam@dns ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=20.9 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=21.0 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=19.8 ms
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 19.800/20.564/20.959/0.540 ms

[adam@dhcp ~]$ ping 10.6.2.12
PING 10.6.2.12 (10.6.2.12) 56(84) bytes of data.
64 bytes from 10.6.2.12: icmp_seq=1 ttl=63 time=1.68 ms
64 bytes from 10.6.2.12: icmp_seq=2 ttl=63 time=2.16 ms
64 bytes from 10.6.2.12: icmp_seq=3 ttl=63 time=1.88 ms
--- 10.6.2.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2006ms
rtt min/avg/max/mdev = 1.676/1.902/2.155/0.196 ms
```

# II. LAN clients
## 2. Client
### Prouvez que...
```
adam@client1:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:7d:7f:d9 brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.37/24 brd 10.6.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 42664sec preferred_lft 42664sec
    inet6 fe80::a00:27ff:fe7d:7fd9/64 scope link
       valid_lft forever preferred_lft forever


adam@client1:~$ resolvectl
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 1.1.1.1
       DNS Servers: 1.1.1.1


adam@client1:~$ ip route show
default via 10.6.1.254 dev enp0s3 proto dhcp src 10.6.1.37 metric 100
10.6.1.0/24 dev enp0s3 proto kernel scope link src 10.6.1.37 metric 100


adam@client1:~$ ping google.com
PING google.com (142.250.201.46) 56(84) bytes of data.
64 bytes from mrs08s20-in-f14.1e100.net (142.250.201.46): icmp_seq=1 ttl=111 time=20.3 ms
64 bytes from mrs08s20-in-f14.1e100.net (142.250.201.46): icmp_seq=2 ttl=111 time=20.6 ms
64 bytes from mrs08s20-in-f14.1e100.net (142.250.201.46): icmp_seq=3 ttl=111 time=21.0 ms
64 bytes from mrs08s20-in-f14.1e100.net (142.250.201.46): icmp_seq=4 ttl=111 time=20.5 ms
--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3241ms
rtt min/avg/max/mdev = 20.280/20.610/21.036/0.275 ms       
```

# III. LAN serveurzzzz
# 1. Serveur Web
## 3. Analyse et test
### Déterminer sur quel port écoute le serveur NGINX
```
[adam@web ~]$ sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1696,fd=6),("nginx",pid=1695,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=1696,fd=7),("nginx",pid=1695,fd=7))
```

### Ouvrir ce port dans le firewall
```
[adam@web ~]$ sudo firewall-cmd --permanent --add-port=80/tcp
success
[adam@web ~]$ sudo firewall-cmd --reload
success
```

### Visitez le site web !
```
[adam@web ~]$ curl http://10.6.2.11
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/

      html {
        height: 100%;
        width: 100%;
      }
```


# 2. Serveur DNS
## 4. Analyse du service
### Déterminer sur quel(s) port(s) écoute le service BIND9
```
[adam@dns network-scripts]$ sudo ss -lnpt | grep 53
LISTEN 0      10         127.0.0.1:53        0.0.0.0:*    users:(("named",pid=2400,fd=22))
LISTEN 0      10         10.6.2.12:53        0.0.0.0:*    users:(("named",pid=2400,fd=25))
LISTEN 0      4096       127.0.0.1:953       0.0.0.0:*    users:(("named",pid=2400,fd=28))
LISTEN 0      10             [::1]:53           [::]:*    users:(("named",pid=2400,fd=27))
LISTEN 0      4096           [::1]:953          [::]:*    users:(("named",pid=2400,fd=29))
```

### Ouvrir ce(s) port(s) dans le firewall
```
[adam@dns network-scripts]$ sudo firewall-cmd --permanent --add-port=53/tcp
success
[adam@dns network-scripts]$ sudo firewall-cmd --reload
success
[adam@dns network-scripts]$ sudo firewall-cmd --permanent --add-port=953/tcp
success
[adam@dns network-scripts]$ sudo firewall-cmd --reload
success
```

## 5. Tests manuels
###  Effectuez des requêtes DNS manuellement depuis le serveur DNS lui-même dans un premier temps
```
[adam@dns network-scripts]$ dig web.tp6.b1 @10.6.2.12
; <<>> DiG 9.16.23-RH <<>> web.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 59685
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 6436d4819654ed2d01000000670fdc77fcdf7313572b7b3e (good)
;; QUESTION SECTION:
;web.tp6.b1.                    IN      A
;; AUTHORITY SECTION:
.                       10800   IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2024101600 1800 900 604800 86400
;; Query time: 1360 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:32:07 CEST 2024
;; MSG SIZE  rcvd: 142

[adam@dns network-scripts]$ dig dns.tp6.b1 @10.6.2.12
; <<>> DiG 9.16.23-RH <<>> dns.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 22217
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 183a407baf9b08f901000000670fdc7f4fcfb73302d156b2 (good)
;; QUESTION SECTION:
;dns.tp6.b1.                    IN      A
;; AUTHORITY SECTION:
.                       10800   IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2024101600 1800 900 604800 86400
;; Query time: 173 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:32:15 CEST 2024
;; MSG SIZE  rcvd: 142

[adam@dns network-scripts]$ dig ynov.com @10.6.2.12
; <<>> DiG 9.16.23-RH <<>> ynov.com @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 64231
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: f7e432ead0dcdc0d01000000670fdc879c5d8421ecc154f0 (good)
;; QUESTION SECTION:
;ynov.com.                      IN      A
;; ANSWER SECTION:
ynov.com.               300     IN      A       104.26.10.233
ynov.com.               300     IN      A       172.67.74.226
ynov.com.               300     IN      A       104.26.11.233
;; Query time: 275 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:32:23 CEST 2024
;; MSG SIZE  rcvd: 113

[adam@dns network-scripts]$ dig -x 10.6.2.11 @10.6.2.12
; <<>> DiG 9.16.23-RH <<>> -x 10.6.2.11 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 55019
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 4dd767842844c99601000000670fdc8f15568541ffe49b39 (good)
;; QUESTION SECTION:
;11.2.6.10.in-addr.arpa.                IN      PTR
;; AUTHORITY SECTION:
10.IN-ADDR.ARPA.        86400   IN      SOA     10.IN-ADDR.ARPA. . 0 28800 7200 604800 86400
;; Query time: 0 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:32:31 CEST 2024
;; MSG SIZE  rcvd: 129

[adam@dns network-scripts]$ dig -x 10.6.2.12 @10.6.2.12
; <<>> DiG 9.16.23-RH <<>> -x 10.6.2.12 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 10313
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 236c4bbb1b0d125101000000670fdc945ebb7bf48c394517 (good)
;; QUESTION SECTION:
;12.2.6.10.in-addr.arpa.                IN      PTR
;; AUTHORITY SECTION:
10.IN-ADDR.ARPA.        86400   IN      SOA     10.IN-ADDR.ARPA. . 0 28800 7200 604800 86400
;; Query time: 0 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:32:36 CEST 2024
;; MSG SIZE  rcvd: 129
```

### Effectuez une requête DNS manuellement depuis client1.tp6.b1
```
adam@client1:/etc/netplan$ dig web.tp6.b1 @10.6.2.12

;; ANSWER SECTION:
web.tp6.b1.             86400   IN      A       10.6.2.11

;; Query time: 1 msec
;; SERVER: 10.6.2.12#53(10.6.2.12) (UDP)
;; WHEN: Mon Oct 21 15:18:46 CEST 2024
;; MSG SIZE  rcvd: 83
```

### Capturez une requête DNS et la réponse de votre serveur
```
adam@client1:~/Desktop$ sudo tcpdump -w toto2.pcap -i enp0s3
tcpdump: listening on enp0s3, link-type EN10MB (Ethernet), snapshot length 262144 bytes
384 packets captured
388 packets received by filter
0 packets dropped by kernel


voir toto2.pcap
```


## 3. Serveur DHCP
### Créez un nouveau client client2.tp6.b1 vitefé
```
adam@client2:~$ resolvectl
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 10.6.2.12
       DNS Servers: 10.6.2.12
```

```
adam@client2:~$ sudo curl http://web.tp6.b1
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/
```

```
je peux visiter http://web.tp6.b1 depuis client2
```