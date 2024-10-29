# II. Serveur Web
# 1. HTTP
## B. Configuration
### Lister les ports en écoute sur la machine
```
[adam@web ~]$ sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1497,fd=6),("nginx",pid=1496,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=1497,fd=7),("nginx",pid=1496,fd=7))
```

### Ouvrir le port dans le firewall de la machine
```
[adam@web ~]$ sudo firewall-cmd --permanent --add-port=80/tcp
success
[adam@web ~]$ sudo firewall-cmd --reload
success
```

## C. Tests client
### Vérifier que ça a pris effet
```
[adam@client1 ~]$ ping sitedefou.tp7.b1
PING sitedefou.tp7.b1 (10.7.1.11) 56(84) bytes of data.
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=1 ttl=64 time=1.50 ms
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=2 ttl=64 time=1.14 ms
--- sitedefou.tp7.b1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 1.136/1.317/1.498/0.181 ms


[adam@client1 ~]$ curl http://sitedefou.tp7.b1
meow !
```

## D. Analyze
### Capture tcp_http.pcap
```
voir tcp_http.pcap
```

### Voir la connexion établie
```
[adam@client1 ~]$ ss

tcp          ESTAB        0              52                                       10.7.1.101:ssh                       10.7.1.1:4254
```


# 2. On rajoute un S
## A. Config
### Lister les ports en écoute sur la machine
```
[adam@web ~]$ sudo ss -lnpt | grep 443
LISTEN 0      511        10.7.1.11:443       0.0.0.0:*    users:(("nginx",pid=1253,fd=6),("nginx",pid=1252,fd=6))
```

### Gérer le firewall
```
[adam@web ~]$ sudo firewall-cmd --permanent --add-port=443/tcp
success
[adam@web ~]$ sudo firewall-cmd --reload
success
[adam@web ~]$ sudo firewall-cmd --permanent --remove-port=80/tcp
success
[adam@web ~]$ sudo firewall-cmd --reload
success
```

## B. Test test test analyyyze
### Capture tcp_https.pcap
```

```


# III. Serveur VPN
# 1. Install et conf Wireguard
### Prouvez que vous avez bien une nouvelle carte réseau wg0
```
[adam@vpn ~]$ ip a

4: wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN group default qlen 1000
    link/none
    inet 10.7.200.1/24 scope global wg0
       valid_lft forever preferred_lft forever
```

### Déterminer sur quel port écoute Wireguard
```
[adam@vpn ~]$ sudo ss -lnpu | grep 51820
UNCONN 0      0            0.0.0.0:51820      0.0.0.0:*
UNCONN 0      0               [::]:51820         [::]:*
```

### Ouvrez ce port dans le firewall
```
[adam@vpn ~]$ sudo firewall-cmd --permanent --add-port=51820/tcp
success
[adam@vpn ~]$ sudo firewall-cmd --reload
success
```


# 3. Proofs
### Ping ping ping !
```
[adam@client1 ~]$ ping 10.7.200.1
PING 10.7.200.1 (10.7.200.1) 56(84) bytes of data.
64 bytes from 10.7.200.1: icmp_seq=1 ttl=64 time=2.69 ms
64 bytes from 10.7.200.1: icmp_seq=2 ttl=64 time=2.22 ms
64 bytes from 10.7.200.1: icmp_seq=3 ttl=64 time=2.35 ms
--- 10.7.200.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2006ms
rtt min/avg/max/mdev = 2.221/2.419/2.690/0.198 ms
```

### Capture ping1_vpn.pcap
```
voir ping1_vpn.pcap
```

### Capture ping2_vpn.pcap
```
voir ping2_vpn.pcap
```

### Prouvez que vous avez toujours un accès internet
```
[adam@client1 ~]$ sudo traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  _gateway (10.7.200.1)  2.711 ms  3.127 ms  6.733 ms
 2  * * *
 3  10.0.2.2 (10.0.2.2)  15.081 ms  15.272 ms  15.309 ms
 4  10.0.2.2 (10.0.2.2)  48.317 ms  47.390 ms  46.690 ms
```


# 4. Private service
###  Visitez le service Web à travers le VPN
```
[adam@client1 ~]$ curl https://sitedefou.tp7.b1
curl: (60) SSL certificate problem: self-signed certificate
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.

[adam@client1 ~]$ curl https://sitedefou.tp7.b1 -k
meow !
```

