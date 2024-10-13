# I. ARP basics
### Avant de continuer...
```
PS C:\Windows\system32> ipconfig /all
Carte réseau sans fil Wi-Fi :
Adresse physique . . . . . . . . . . . : 1C-CE-51-24-C7-33
```

###  Affichez votre table ARP
```
PS C:\Windows\system32> arp -a

Interface : 192.168.56.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.33.70.238 --- 0x14
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```

### Déterminez l'adresse MAC de la passerelle du réseau de l'école
```
PS C:\Windows\system32> arp -a
Interface : 10.33.70.238 --- 0x14
  Adresse Internet      Adresse physique      Type
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
```

### Supprimez la ligne qui concerne la passerelle
```
PS C:\Windows\system32> arp -a

Interface : 192.168.56.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.33.70.238 --- 0x14
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```

### Prouvez que vous avez supprimé la ligne dans la table ARP
```
PS C:\Windows\system32> arp -d 10.33.79.254
PS C:\Windows\system32> arp -a

Interface : 192.168.56.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.33.70.238 --- 0x14
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```

### Wireshark
```
voir arp1.pcap
```

# II. ARP dans un réseau local
## 1. Basics
### ️Déterminer
```
PS C:\Windows\system32> ipconfig /all
Carte réseau sans fil Wi-Fi :

 Adresse physique . . . . . . . . . . . : 1C-CE-51-24-C7-33
 Passerelle par défaut. . . . . . . . . : fe80::fcaa:81ff:fe5d:f164%20
                                       172.20.10.1
```

### ️DIY
```
PS C:\Windows\system32> ipconfig /all
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : MediaTek Wi-Fi 6 MT7921 Wireless LAN Card
   Adresse physique . . . . . . . . . . . : 1C-CE-51-24-C7-33
   DHCP activé. . . . . . . . . . . . . . : Non
   Configuration automatique activée. . . : Oui
   Adresse IPv6. . . . . . . . . . . . . .: 2a01:cb1a:34:4c89:b9ef:45d4:cd1c:3b1f(préféré)
   Adresse IPv6 temporaire . . . . . . . .: 2a01:cb1a:34:4c89:f54d:a2d5:c0bf:51f9(préféré)
   Adresse IPv6 de liaison locale. . . . .: fe80::5cd2:c981:10b1:5928%20(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 172.20.10.4(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.240
   Passerelle par défaut. . . . . . . . . : fe80::fcaa:81ff:fe5d:f164%20
   IAID DHCPv6 . . . . . . . . . . . : 287100497
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2D-6B-70-BC-74-D4-DD-4B-01-36
   Serveurs DNS. . .  . . . . . . . . . . : fe80::fcaa:81ff:fe5d:f164%20
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

### ️Pingz !
```
PS C:\Windows\system32> ping 172.20.10.3

Envoi d’une requête 'Ping'  172.20.10.3 avec 32 octets de données :
Réponse de 172.20.10.3 : octets=32 temps=29 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=7 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=32 ms TTL=128
Réponse de 172.20.10.3 : octets=32 temps=30 ms TTL=128

Statistiques Ping pour 172.20.10.3:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 7ms, Maximum = 32ms, Moyenne = 24ms


PS C:\Windows\system32> ping 172.20.10.6

Envoi d’une requête 'Ping'  172.20.10.6 avec 32 octets de données :
Réponse de 172.20.10.6 : octets=32 temps=13 ms TTL=128
Réponse de 172.20.10.6 : octets=32 temps=31 ms TTL=128
Réponse de 172.20.10.6 : octets=32 temps=31 ms TTL=128
Réponse de 172.20.10.6 : octets=32 temps=32 ms TTL=128

Statistiques Ping pour 172.20.10.6:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 13ms, Maximum = 32ms, Moyenne = 26ms
```

## 2. ARP
### Affichez votre table ARP !
```
PS C:\Windows\system32> arp -a

Interface : 192.168.56.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 172.20.10.4 --- 0x14
  Adresse Internet      Adresse physique      Type
  172.20.10.3           80-19-34-04-ba-b9     dynamique
  172.20.10.6           9c-b6-d0-05-18-db     dynamique
  172.20.10.15          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique  
```

### Capture arp2.pcap
```
PS C:\Windows\system32> arp -a -d
PS C:\Windows\system32> arp -a

Interface : 192.168.56.1 --- 0x11
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 172.20.10.4 --- 0x14
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
```

## 3. Bonus : ARP poisoning
### Empoisonner la table ARP de l'un des membres de votre réseau
J'ai commencé par récuperer l'ip de la passerelle, celle de mon pc portable qui attaque, de mon fixe qui subit, avec leurs adresses mac réspectives

PC ATTAQUANT
```
PS C:\Users\benai> ipconfig /all
Carte réseau sans fil Wi-Fi :
 Adresse physique . . . . . . . . . . . : 1C-CE-51-24-C7-33
 Adresse IPv4. . . . . . . . . . . . . .: 192.168.1.15(préféré)
 Passerelle par défaut. . . . . . . . . : fe80::327c:b2ff:fec2:1466%21
                                       192.168.1.1
```

PC VICTIME
```
PS C:\Users\benai> ipconfig /all
Carte réseau sans fil Wi-Fi :
Adresse physique . . . . . . . . . . . : 40-5B-D8-8D-2D-67
Adresse IPv4. . . . . . . . . . . . . .: 192.168.1.2O(préféré)
```

J'ai récuperer la table ARP de mon PC victime
```
PS C:\Users\benai> arp -a

Interface : 192.168.1.20 --- 0xc
  Adresse Internet      Adresse physique      Type
  192.168.1.1           30-7c-b2-c2-14-66     dynamique
  192.168.1.255         ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```

En fouinant sur Internet j'ai appris qu'on pouvait se servir d'un script pour empoisonner la table ARP de la victime, j'ai pris [celui-ci](https://github.com/davidlares/arp-spoofing) codé en python.

J'ai d'abord installé les bibliothèques python et modifié le script avec l'ip du PC victime
``` 
PS C:\Windows\system32> pip install six
PS C:\Windows\system32> pip install scapy
```

J'ai cd jusqu'au fichier spoofer et je l'ai éxécuté
```
PS C:\Windows\system32> cd C:\Users\benai\Downloads\arp-spoofing-master
PS C:\Users\benai\Downloads\arp-spoofing-master> python .\spoofer.py
```

Si on regarde la capture wireshark spoofer.pcap on voit que mon script spam la table ARP du PC victime en permanence.
Je refais un arp -a sur mon PC victime
```
PS C:\Users\benai> arp -a

Interface : 192.168.1.20 --- 0xc
  Adresse Internet      Adresse physique      Type
  192.168.1.1           1c-ce-51-24-c7-33     dynamique
  192.168.1.15          1c-ce-51-24-c7-33     dynamique
  192.168.1.255         ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```
Ma table ARP est bien empoisonnée, mon adresse mac est reliée à l'adresse du routeur



###  Mettre en place un MITM
avec la capture MITM.pcap on constate que pendant que mon script est en marche, mon PC attaquant intercepte toutes les connexions que mon PC victime fait sur internet et me renvoie les adresses IP de ces sites (ça rame un max part contre).
