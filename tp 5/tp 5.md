# I. Setup
### Uniquement avec des commandes, prouvez-que
```
adam@client1:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:44:4c:01 brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.11/24 brd 10.5.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe44:4c01/64 scope li

adam@client2:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:63:d5:d8 brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.12/24 brd 10.5.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe63:d5d8/64 scope link
       valid_lft forever preferred_lft forever

[adam@routeur ~]$ ip a
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:ca:23:76 brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.254/24 brd 10.5.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:feca:2376/64 scope link
       valid_lft forever preferred_lft forever


adam@client1:~$ hostname
client1.tp5.b1

adam@client2:~$ hostname
client2.tp5.b1

[adam@routeur ~]$ hostname
routeur.tp5.b1


adam@client1:~$ ping 10.5.1.12
PING 10.5.1.12 (10.5.1.12) 56(84) bytes of data.
64 bytes from 10.5.1.12: icmp_seq=1 ttl=64 time=2.88 ms
64 bytes from 10.5.1.12: icmp_seq=2 ttl=64 time=1.82 ms
64 bytes from 10.5.1.12: icmp_seq=3 ttl=64 time=1.22 ms
--- 10.5.1.12 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6256ms
rtt min/avg/max/mdev = 1.215/2.051/2.955/0.595 ms

adam@client1:~$ ping 10.5.1.254
PING 10.5.1.254 (10.5.1.254) 56(84) bytes of data.
64 bytes from 10.5.1.254: icmp_seq=1 ttl=64 time=2.80 ms
64 bytes from 10.5.1.254: icmp_seq=2 ttl=64 time=1.69 ms
64 bytes from 10.5.1.254: icmp_seq=3 ttl=64 time=1.22 ms
--- 10.5.1.254 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3579ms
rtt min/avg/max/mdev = 1.219/1.768/2.803/0.620 ms

adam@client2:~$ ping 10.5.1.254
PING 10.5.1.254 (10.5.1.254) 56(84) bytes of data.
64 bytes from 10.5.1.254: icmp_seq=1 ttl=64 time=2.90 ms
64 bytes from 10.5.1.254: icmp_seq=2 ttl=64 time=1.40 ms
64 bytes from 10.5.1.254: icmp_seq=3 ttl=64 time=1.94 ms
--- 10.5.1.254 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 1.398/2.079/2.896/0.619 ms
```

# II. Accès internet pour tous
## 1. Accès internet routeur
### Déjà, prouvez que le routeur a un accès internet
```
[adam@routeur ~]$ ping ynov.com
PING ynov.com (104.26.10.233) 56(84) bytes of data.
64 bytes from 104.26.10.233 (104.26.10.233): icmp_seq=1 ttl=53 time=38.7 ms
64 bytes from 104.26.10.233 (104.26.10.233): icmp_seq=2 ttl=53 time=44.7 ms
64 bytes from 104.26.10.233 (104.26.10.233): icmp_seq=3 ttl=53 time=34.8 ms
^C
--- ynov.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 34.804/39.406/44.678/4.058 ms
```

### Activez le routage
```
[adam@routeur ~]$ sudo firewall-cmd --add-masquerade --permanent
[sudo] password for adam:
success
[adam@routeur ~]$ sudo firewall-cmd --reload
success
```

## 2. Accès internet clients
### Prouvez que les clients ont un accès internet
```
adam@client1:~$ ping ynov.com
PING ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233: icmp_seq=1 ttl=52 time=43.7 ms
64 bytes from 104.26.11.233: icmp_seq=2 ttl=52 time=77.5 ms
64 bytes from 104.26.11.233: icmp_seq=3 ttl=52 time=46.1 ms
--- ynov.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2008ms
rtt min/avg/max/mdev = 43.695/55.748/77.460/15.383 ms


adam@client2:~$ ping ynov.com
PING ynov.com (172.67.74.226) 56(84) bytes of data.
64 bytes from 172.67.74.226: icmp_seq=1 ttl=52 time=43.5 ms
64 bytes from 172.67.74.226: icmp_seq=2 ttl=52 time=115 ms
64 bytes from 172.67.74.226: icmp_seq=3 ttl=52 time=118 ms
64 bytes from 172.67.74.226: icmp_seq=4 ttl=52 time=190 ms
--- ynov.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 43.486/116.740/190.227/51.891 ms
```

### Montrez-moi le contenu final du fichier de configuration de l'interface réseau

```
adam@client2:~$ cat /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [10.5.1.11/24]
      gateway4: 10.5.1.254
      nameservers:
        addresses: [1.1.1.1]
```

# III. Serveur SSH
###  Sur routeur.tp5.b1, déterminer sur quel port écoute le serveur SSH
```
[adam@routeur ~]$ sudo ss -lnpt | grep 22
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=687,fd=3))
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=687,fd=4))
```

### Sur routeur.tp5.b1, vérifier que ce port est bien ouvert
```
[adam@routeur ~]$ cat /etc/services | grep ssh
ssh             22/tcp                          # The Secure Shell (SSH) Protocol
```

# IV. Serveur DHCP
## 3. Rendu attendu
### A. Installation et configuration du serveur DHCP
### Installez et configurez un serveur DHCP sur la machine routeur.tp5.b1
```
[adam@routeur ~]$ sudo dnf -y install dhcp-server


[adam@routeur ~]$ cat /etc/dhcp/dhcpd.conf
cat: /etc/dhcp/dhcpd.conf: Permission denied

[adam@routeur ~]$ sudo cat  /etc/dhcp/dhcpd.conf
subnet 10.5.1.0 netmask 255.255.255.0 {
    range dynamic-bootp 10.5.1.137 10.5.1.237;
    option routers 10.5.1.254;
    option domain-name-servers 1.1.1.1;
}


[adam@routeur ~]$ sudo systemctl start dhcpd

[adam@routeur ~]$ sudo systemctl enable dhcpd
Created symlink /etc/systemd/system/multi-user.target.wants/dhcpd.service → /usr/lib/systemd/system/dhcpd.service.
[adam@routeur ~]$ sudo systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset>
     Active: active (running) since Tue 2024-10-15 16:31:08 CEST; 32s ago
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
   Main PID: 1812 (dhcpd)
     Status: "Dispatching packets..."
      Tasks: 1 (limit: 11100)
     Memory: 4.5M
        CPU: 8ms
     CGroup: /system.slice/dhcpd.service
             └─1812 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd>

Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]:
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]: No subnet declaration for enp0s>
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]: ** Ignoring requests on enp0s3.>
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]:    you want, please write a sub>
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]:    in your dhcpd.conf file for >
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]:    to which interface enp0s3 is>
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]:
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]: Sending on   Socket/fallback/fa>
Oct 15 16:31:08 routeur.tp5.b1 dhcpd[1812]: Server starting service.
Oct 15 16:31:08 routeur.tp5.b1 systemd[1]: Started DHCPv4 Server Daemon.


[adam@routeur ~]$ sudo firewall-cmd --add-service=dhcp --permanent
success

[adam@routeur ~]$ sudo firewall-cmd --reload
success
```
### B. Test avec un nouveau client
### Créez une nouvelle machine client client3.tp5.b1
``` 
adam@client3:~$ hostname
client3.tp5.b1

adam@client3:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:42:c6:bf brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s3
       valid_lft 43059sec preferred_lft 43059sec

adam@client3:~$ ping ynov.com
PING ynov.com (172.67.74.226) 56(84) bytes of data.
64 bytes from 172.67.74.226: icmp_seq=1 ttl=53 time=21.6 ms
64 bytes from 172.67.74.226: icmp_seq=2 ttl=53 time=19.7 ms
64 bytes from 172.67.74.226: icmp_seq=3 ttl=53 time=21.0 ms
--- ynov.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 19.716/20.758/21.560/0.771 ms
```

### C. Consulter le bail DHC
### Consultez le bail DHCP qui a été créé pour notre client
``` 
[adam@routeur ~]$ cd /var/lib/dhcpd
[adam@routeur dhcpd]$ ls
dhcpd6.leases  dhcpd.lease  dhcpd.leases  dhcpd.leases~
[adam@routeur dhcpd]$ cat dhcpd.leases
# The format of this file is documented in the dhcpd.leases(5) manual page.
# This lease file was written by isc-dhcp-4.4.2b1

# authoring-byte-order entry is generated, DO NOT DELETE
authoring-byte-order little-endian;

server-duid "\000\001\000\001.\241HQ\010\000'\312#v";

lease 10.5.1.137 {
  starts 2 2024/10/15 15:36:46;
  ends 3 2024/10/16 03:36:46;
  cltt 2 2024/10/15 15:36:46;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 08:00:27:42:c6:bf;
  uid "\377\3424?>\000\002\000\000\253\021\366\024\344\276\213\342\225\326";
}
```

###  Confirmez qu'il s'agit bien de la bonne adresse MAC
``` 
adam@client3:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:42:c6:bf brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s3
       valid_lft 42622sec preferred_lft 42622sec
```