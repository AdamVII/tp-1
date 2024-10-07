# I. Simplest LAN
## 1. Quelques pings**Prouvez que votre configuration est effective
###  Prouvez que votre configuration est effective

```
PS C:\Windows\system32> Get-NetIPAddress -InterfaceAlias Ethernet


IPAddress         : fe80::43ee:8de1:9f1c:f588%4
InterfaceIndex    : 4
InterfaceAlias    : Ethernet
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 10.67.69.132
InterfaceIndex    : 4
InterfaceAlias    : Ethernet
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore
```

### Tester que votre LAN + votre adressage IP est fonctionnel
```
PS C:\Windows\system32> ping 10.67.69.133

Envoi d’une requête 'Ping'  10.67.69.133 avec 32 octets de données :
Réponse de 10.67.69.133 : octets=32 temps=4 ms TTL=128
Réponse de 10.67.69.133 : octets=32 temps=2 ms TTL=128
Réponse de 10.67.69.133 : octets=32 temps=2 ms TTL=128
Réponse de 10.67.69.133 : octets=32 temps=2 ms TTL=128

Statistiques Ping pour 10.67.69.133:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 2ms, Maximum = 4ms, Moyenne = 2ms
```

### Capture de ping
```
voir ping.pcap
```

# II. Utilisation des ports
## 1. Animal numérique
### Sur le PC serveur
```
PS C:\Windows\system32> cd "C:\Users\benai\Downloads\netcat-win32-1.11\netcat-1.11"
PS C:\Users\benai\Downloads\netcat-win32-1.11\netcat-1.11> .\nc64.exe -l -p 9999
test
test2
```

### Sur le PC serveur toujours
```
PS C:\Users\benai\Downloads\netcat-win32-1.11\netcat-1.11> netstat -a -b -n

Connexions actives

  Proto  Adresse locale         Adresse distante       État
  TCP    0.0.0.0:9999           0.0.0.0:0              LISTENING
  [nc64.exe]
```

#### Utilisez une commande qui permet de voir la connexion en cours
```
PS C:\Users\benai\Downloads\netcat-win32-1.11\netcat-1.11> netstat -b -n

Connexions actives

  Proto  Adresse locale         Adresse distante       État
  TCP    10.33.70.238:9999      10.33.68.254:29873     ESTABLISHED
```

###  Faites une capture Wireshark complète d'un échange
```
voir netcat1.pcap
```

## Inversez les rôles
### Sur le PC client
```
PS C:\Users\benai\Downloads\netcat-win32-1.11\netcat-1.11> .\nc64.exe 10.67.69.133 9999
parle moi
coucou
```

### Utilisez une commande qui permet de voir la connexion en cours
```
PS C:\Users\benai\Downloads\netcat-win32-1.11\netcat-1.11> netstat -b -n
Connexions actives

  Proto  Adresse locale         Adresse distante       État
TCP    10.67.69.132:27021     10.67.69.133:9999      ESTABLISHED
 [nc64.exe]
```

### Faites une capture Wireshark complète d'un échange

```
voir netcat2.pcap
```

# III. Analyse de vos applications usuelles
## 1. Serveur web
### Utilisez Wireshark pour capturer du trafic HTTP
```
voir http.pcap
```

## 2. Autres services
### Pour les 5 applications
**commande netstat**
```
PS C:\Users\benai\Downloads\netcat-win32-1.11\netcat-1.11> netstat -a -b -n

chrome:
 TCP    10.33.70.238:28600     162.19.57.18:443       ESTABLISHED
 [chrome.exe]

deezer:
TCP    10.33.70.238:28668     78.40.123.97:443       ESTABLISHED
 [Deezer.exe]

discord:
TCP    10.33.70.238:16559     162.159.136.234:443    ESTABLISHED
 [Discord.exe]

Outlook:
 TCP    10.33.70.238:29717     13.69.109.130:443      ESTABLISHED
 [OUTLOOK.EXE]

client riot:  
 TCP    10.33.70.238:28323     104.16.56.40:443       ESTABLISHED
 [RiotClientServices.exe]
```

**capture WireShark**
```
voir service1.pcap, service2pcap, service3.pcap, service4.pcap, service5.pcap
```
