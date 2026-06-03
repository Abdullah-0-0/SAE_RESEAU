# Configuration réseau

* NOMS : Chamsiddine Abdullah - Clément Pissot
* GROUPE de TP :  TD-1
* X : 122
* SECRET : antareszetapuppis
* IP_CLIENT : 172.20.122.1/24
* IP_DHCP : 172.20.122.2/24
* IP_SMTP : 192.168.0.22/24(SMTP)
* IPs du ROUTEUR : 172.20.122.3/24 et 192.168.0.122/24

**Note**: Le document suivant doit rendre compte de votre plan d’adressage (i.e. la description des différents LAN, de leur interconnexion, des machines avec les IP voire @MAC que vous jugerez pertinentes), de vos tables de routage de CLIENT, ROUTEUR, SMTP et celle (supposée) de DNS, des commandes à réaliser sur CLIENT, ROUTEUR, SMTP, et tout ce qui vous semble nécessaire à la configuration de votre réseau.

# I) configuration et information du système

## Plan d'adressage (en graphe si possible)

dans la **VLAN122** nous avons :

* **Client**:
	* IP: 172.20.122.1/24
	* Mac : a6:2f:a6:8a:a0:6a
	* Interface : VLAN122@eth2
* **DHCP**: 
	* IP :172.20.122.2/24
	* Mac : b2:85:3f:11:33:8e
	* Interface : VLAN122@eth2
* **ROUTAGE**:
	* IP : 172.20.122.3/24
	* mac : a6:91:8e:18:43:60
	* Interface : VLAN122@eth2

dans le **eth3**  nous avans :

*	ROUTEUR : 
	* IP : 192.168.0.122/24
	* Mac : 6e:22:cf:5e:3a:4e
	* Interface : ETH3
* SMTP :
	* IP : 192.168.0.22/24(SMTP)
	* Mac : d2:57:73:55:76:59
	* Interface : ETH3
	
* DNS :
	* IP : 192.168.0.254/24
	* Mac : 
	* Interface : ETH3

![img plage d'adresse](RESSOURCE_RAPPORT/IP%20%20172.20.122.124.png)



## Tables de routage

**Note**: il existe des générateurs de tables MDhttps://www.tablesgenerator.com/markdown_tables

## pour le **CLIENT**(172.20.122.1) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |VLAN122|        |
|172.20.122.3/24(ROUTEUR)  |VLAN122|        |
|192.168.0.22/24(SMTP) |VLAN122|172.20.122.3|
|192.168.0.254(DNS)|VLAN122|172.20.122.3|


## pour le **DHCP**(172.20.122.2) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.1/24(CLIENT)  |VLAN122|        |
|172.20.122.3/24(ROUTEUR)  |VLAN122|        |
|192.168.0.22/24(SMTP) |VLAN122|172.20.122.3|
|192.168.0.254(DNS)|VLAN122|172.20.122.3|

## pour le **ROUTEUR**(192.168.0.122 |172.20.122.3 ) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |VLAN122|        |
|172.20.122.1/24(CLIENT)  |VLAN122|        |
|192.168.0.22/24(SMTP) |ETH3||
|192.168.0.254(DNS)|ETH3||

## pour le **SMTP**(192.168.0.22) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |ETH3| 192.168.0.122|
|172.20.122.1(Client)|ETH3|192.168.0.122|
|192.168.0.122/24(ROUTEUR)  |ETH3|        |
|192.168.0.254(DNS)|ETH3||

## pour le **DNS**(192.168.0.254) :
| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |ETH3| 192.168.0.122|
|172.20.122.1(Client)|ETH3|192.168.0.122|
|192.168.0.122/24(ROUTEUR)  |ETH3|        |
|192.168.0.22(SMTP)|ETH3||


## Commandes de configuration
### 1) Commandes sur la machine CLIENT



```bash
# Afficher la configuration réseau actuelle
ip a

# Créer l'interface VLAN 122 sur l'interface eth2
ip link add link eth2 name VLAN122 type vlan id 122

# Activer l'interface physique eth2
ip link set up dev eth2

# Activer l'interface VLAN122
ip link set up dev VLAN122

# Attribuer l'adresse IP au CLIENT dans le VLAN
ip a add 172.20.122.1/24 dev VLAN122

#Ajouter la route vers le SMTP
ip route add 192.168.0.22 via 172.20.122.3 dev VLAN122

# pour générer le fichier log du DHCP
chmod +x log
./log > CLIENT.log

# demander au DNS l’adresse IP associée au nom smtp122.mail122.com
nslookup smtp122.mail122.com 192.168.0.254
```
### 2)  Commandes sur la machine DHCP :
``` bash
# Afficher la configuration réseau actuelle
ip a

# Créer l'interface VLAN 122 sur eth2
ip link add link eth2 name VLAN122 type vlan id 122

# Activer l'interface physique eth2
ip link set up dev eth2

# Activer l'interface VLAN122
ip link set up dev VLAN122

# Attribuer l'adresse IP à la machine DHCP
ip a add 172.20.122.2/24 dev VLAN122
# Ajouter la route vers le SMTP
ip route add 192.168.0.22 via 172.20.122.3 dev VLAN122

# pour genere le fichier log du DHCP
chmod +x log
./log > DHCP.log
```
### 3) Commandes sur la machine ROUTEUR :
``` bash
# Afficher la configuration réseau actuelle
ip a

# Créer l'interface VLAN 122 sur eth2
ip link add link eth2 name VLAN122 type vlan id 122

# Activer l'interface physique eth2
ip link set up dev eth2

# Activer l'interface VLAN122
ip link set up dev VLAN122

# Attribuer l'adresse IP du ROUTEUR côté VLAN
ip a add 172.20.122.3/24 dev VLAN122

# Activer l'interface eth3 côté LAN
ip link set up dev eth3

# Attribuer l'adresse IP du ROUTEUR côté LAN
ip a add 192.168.0.122/24 dev eth3

#demande au dsn l'adresse ip qui est associe a cette adresse mail:
host smtp122.mail122.com 192.168.0.254

# rajoute une resolution sur le reseau pour la demande DNS
nano /etc/resolv.conf
nameserver 192.168.0.254 
```
![capture du resolv.conf](RESSOURCE_RAPPORT/resolv_conf.png "capture du resolv.conf")
``` bash
# pour genere le fichier log du ROUTEUR
chmod +x log
./log > ROUTEUR.log
```


### 4)Commandes sur la machine SMTP:
``` bash 
# Afficher la configuration réseau actuelle
ip a

# Activer l'interface eth3
ip link set up dev eth3

# Attribuer l'adresse IP au serveur SMTP
ip a add 192.168.0.22/24 dev eth3

# Ajouter une route vers le VLAN en passant par le ROUTEUR
ip route add 172.20.122.0/24 via 192.168.0.122 dev eth3

#Ajouter une route vers le CLIENT
ip route add 172.20.122.1 via 192.168.0.122 dev eth3

#Ajouter une route vers le DHCP
ip route add 172.20.122.2 via 192.168.0.122 dev eth3


```







Le deuxième message est la réponse de la machine SMTP, qui passe également par le même VLAN virtuel.
# II) Analyse capture de trames - VLAN

La capture `vlan.pcapng` montre une communication entre le CLIENT et DHCP dans le VLAN 122. Toutes les trames analysées portent le tag VLAN 122. Cela confirme que les échanges passent bien par le VLAN.



Les trames 13 à 16 correspondent au protocole ARP. Elles aident les deux machines à associer les adresses IP aux adresses MAC.
##  1) Capture de la trame du VLAN allant du client vers le DHCP ainsi que son explication

Veuillez trouver ci-joint le fichier `vlan.pcapng`.

Voici la capture du VLAN entre notre machine cliente et notre machine DHCP.
On peut y voir deux adresses IP qui communiquent successivement :

* **172.20.122.1**, qui correspond à la machine cliente ;
* **172.20.122.2**, qui correspond au serveur DHCP.

Le contenu de ces échanges est de type *ping* et passe par notre VLAN ayant l’identifiant **122**.

en effet,
La capture `vlan.pcapng` montre comment la machine CLIENT et la machine DHCP communiquent au sein du VLAN 122. Le CLIENT, avec l’adresse IP 172.20.122.1, échange avec le serveur DHCP, dont l’adresse IP est 172.20.122.2.

Les trames 1 à 12 montrent des échanges ICMP. Ces échanges correspondent à un test de connectivité réalisé avec la commande ping. Le CLIENT envoie des messages ICMP Echo Request vers le serveur DHCP, puis le serveur DHCP répond avec des messages ICMP Echo Reply. Cela signifie que les paquets envoyés par le CLIENT atteignent bien la machine DHCP et que les réponses reviennent correctement vers le CLIENT.

Les trames 13 à 16 correspondent au protocole ARP. Ce protocole permet aux machines de connaître l’adresse MAC associée à une adresse IP. Dans un réseau local ou dans un VLAN, une machine ne peut pas envoyer directement une trame Ethernet uniquement avec une adresse IP : elle doit aussi connaître l’adresse MAC de la machine destinataire. Les trames ARP observées montrent donc que le CLIENT et le DHCP identifient correctement leurs adresses MAC respectives.

Les trames 17 et 18 montrent un dernier échange ICMP réussi. Cela confirme que la communication entre les deux machines est stable et fonctionnelle.
## 3) Visualisation de la résolution DNS via le routeur depuis le client
## Analyse capture de trames - DNS

La capture `dns.pcapng` montre que la machine CLIENT arrive bien à interroger le serveur DNS `192.168.0.254`. Le CLIENT possède l’adresse IP `172.20.122.1`. Comme le serveur DNS se trouve sur un autre réseau, le CLIENT doit passer par le ROUTEUR, dont l’adresse côté VLAN est `172.20.122.3`.

Dans la trame 1, le CLIENT envoie une requête DNS au serveur `192.168.0.254`. Il cherche à connaître l’adresse IP associée au nom `smtp122.mail122.com`. Dans la trame 2, le serveur DNS répond. Il indique que `smtp122.mail122.com` renvoie vers `machine122.mail122.com`, puis donne son adresse IPv4 : `192.168.0.22`.

Les trames 3 et 4 montrent une autre recherche DNS, cette fois pour savoir s’il existe une adresse IPv6 associée à `machine122.mail122.com`. Le serveur DNS répond, mais aucune adresse IPv6 n’est fournie. Ce n’est pas un problème ici, car l’adresse IPv4 du serveur SMTP a déjà été trouvée dans la trame 2.

Les trames 5 et 6 montrent ensuite un échange ARP entre le CLIENT et le ROUTEUR. Le CLIENT cherche l’adresse MAC du ROUTEUR `172.20.122.3` pour pouvoir lui envoyer les paquets destinés au réseau `192.168.0.0/24`.

## 4) Démonstration du routage entre la machine cliente et le serveur SMTP

La capture `routage_1.pcapng` a été réalisée côté VLAN, entre la machine CLIENT et le ROUTEUR. Elle montre une communication entre le CLIENT `172.20.122.1` et le serveur SMTP `192.168.0.22`. Ces deux machines appartiennent à deux réseaux différents : le CLIENT est dans le VLAN 122, alors que le serveur SMTP est dans le LAN `192.168.0.0/24`. La communication doit donc passer par le ROUTEUR.

Les trames 1 à 12 correspondent à des échanges ICMP générés par la commande `ping`. Les trames impaires sont des requêtes ICMP Echo Request envoyées par le CLIENT vers `192.168.0.22`. Les trames paires sont les réponses ICMP Echo Reply renvoyées par le serveur SMTP vers le CLIENT. Le fait que chaque requête reçoive une réponse montre que le routage fonctionne dans les deux sens.

Les trames 13 à 16 correspondent au protocole ARP. Elles montrent que le CLIENT `172.20.122.1` et le ROUTEUR `172.20.122.3` s’identifient au niveau Ethernet grâce à leurs adresses MAC. Même si le CLIENT veut joindre l’adresse IP `192.168.0.22`, la trame Ethernet est envoyée vers l’adresse MAC du ROUTEUR, car le serveur SMTP se trouve sur un autre réseau.


La capture `routage_2.pcapng` a été réalisée côté LAN, sur le réseau `192.168.0.0/24`. Elle permet d’observer la communication entre le ROUTEUR et la machine SMTP `192.168.0.22`.

Dans les trames 1 à 12, on observe des échanges ICMP. Ces trames correspondent à un test de ping entre SMTP `192.168.0.22` et le CLIENT `172.20.122.1`. Le CLIENT n’est pas dans le même réseau que SMTP : il se trouve dans le VLAN `172.20.122.0/24`. Le fait que SMTP reçoive des réponses montre donc que les paquets passent bien par le ROUTEUR.

Les trames 13 à 16 montrent des échanges ARP entre SMTP et le ROUTEUR côté LAN `192.168.0.122`. Ces échanges permettent de retrouver les adresses MAC nécessaires pour envoyer les trames Ethernet sur le LAN. 

Les trames 17 à 22 confirment que les échanges ICMP continuent correctement.
