# Configuration réseau

* NOMS : Chamsiddine Abdullah - Clément Pissot
* GROUPE de TP :  TD-1
* X : 122
* SECRET : antareszetapuppis
* IP_CLIENT : 172.20.122.1/24
* IP_DHCP : 172.20.122.2/24
* IP_SMTP : 192.168.0.22/24
* IPs du ROUTEUR : 172.20.122.3/24 et 192.168.0.122/24

**Note**: Le document suivant doit rendre compte de votre plan d’adressage (i.e. la description des différents LAN, de leur interconnexion, des machines avec les IP voire @MAC que vous jugerez pertinentes), de vos tables de routage de CLIENT, ROUTEUR, SMTP et celle (supposée) de DNS, des commandes à réaliser sur CLIENT, ROUTEUR, SMTP, et tout ce qui vous semble nécessaire à la configuration de votre réseau.

# 1) configuration et information du système

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
	* IP : 192.168.0.22/24
	* Mac : d2:57:73:55:76:59
	* Interface : ETH3
	
* DNS :
	* IP : 192.168.0.254/24
	* Mac : 
	* Interface : ETH3



## Tables de routage

**Note**: il existe des générateurs de tables MDhttps://www.tablesgenerator.com/markdown_tables

pour le **CLIENT**(172.20.122.1) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |VLAN122|        |
|172.20.122.3/24()  |VLAN122|        |
|192.168.0.22/24 |ETH3|172.20.122.3|
|192.168.0.254|ETH3|172.20.122.3|

pour le **CLIENT**(172.20.122.1) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |VLAN122|        |
|172.20.122.3/24()  |VLAN122|        |
|192.168.0.22/24 |ETH3|172.20.122.3|
|192.168.0.254|ETH3|172.20.122.3|

pour le **CLIENT**(172.20.122.1) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |VLAN122|        |
|172.20.122.3/24()  |VLAN122|        |
|192.168.0.22/24 |ETH3|172.20.122.3|
|192.168.0.254|ETH3|172.20.122.3|

pour le **CLIENT**(172.20.122.1) :

| **destination** | **iface** | **gw** |
|-----------------|-----------|--------|
|172.20.122.2/24(DHCP)  |VLAN122|        |
|172.20.122.3/24()  |VLAN122|        |
|192.168.0.22/24 |ETH3|172.20.122.3|
|192.168.0.254|ETH3|172.20.122.3|


## Commandes de configuration

Connaître sa configuration réseau 

    ip a 
pour créer la route entre le CLIENT et le SMTP: 
	
	ip r a 192.168.0.22 via 172.20.122.3 dev VLAN122
	
	ip r a 172.20.122.2 via 192.168.0.122 dev eth3
<<<<<<< HEAD
=======
demande de renseignement concernant cette adresse mail faite par le client :
>>>>>>> refs/remotes/origin/main

	nslookup smtp122.mail122.com 192.168.0.254

demande au dsn l'adresse ip qui est associe a cette adresse mail:

	host smtp122.mail122.com 192.168.0.254
donne le droit d'executer le fichier log :

	chmod +x log
executer le log depuis la VM CLIENT:

	./log > CLIENT.log
<<<<<<< HEAD
	
=======

active les interfacts:

	ip link set up dev eth2
	ip link set up dev VLAN122
	ip link set up dev eth3

accede a la VM depuis l'explorateur de fichier:

	sftp://root@172.21.180.146/

modifiaction de configuration de resolv pour que il demande d'abord au DNS:

>>>>>>> refs/remotes/origin/main
	nano /etc/resolv.conf
	nameserver 192.168.0.254
![capture du resolv.conf](RESSOURCE_RAPPORT/resolv_conf.png "capture du resolv.conf")
Creer la vlan :

	ip link a link eth2 name VLAN122 type vlan id 122

	
	
Pour active l'interface:

	ip link set up dev eth2	

# 2) la capture de la trame du vlan allant du client vers le dhcp ainsi que son expliquation

veullez trouver ci joint le fichier vlan_entre_client_et_DHCP.pcapng

voici la capture de la vlan entre notre machine client et notre machine DHCP.
on peut y voir deux adresse ip qui se réponde successivement.
172.20.122.1 qui corespond a la machine client et 172.20.122.1


# 3) visualisation de la résolution DNS via routeru vers client 

veuillez trouver si joint le fichier dns_via_routeur_depuis_client.pcapng

voici la capture de la résolution DNS via Routeur depuis la machine client.
on peut y voir les communication entre deux machines.
l'ip 172.20.122.1 corespond a notre machine client et l'ip 192.168.0.254 qui corespond au DNS
les quatre premier signaux coresponds au protocole DNS 
le premier corespond a une demande pour l'utilisateur sous le nom de smtp122.mail122.com
la duexième est la reponse du DNS qui signifie qu'il reconait desormais smtp122.mail122.com sous le nom machine122.mail122.com et enregistre qu'il peut l'apeler a l'adresse ip 192.168.0.22 (qui corespond a l'SMTP)
la troisième est la machine client qui fait une demande sous son nouveau nom
la quatrieme est la réponse de DNS qui indique que machine122.mail122.com est desormais reconnu comme l'utilisateur principale de machine client

# 4) demonstration du routage entre la machine client et le smtp

(on place la trame ici) 

explication ici (les condition dans lesquelle on été prise les capture et sur quelle machne etc, et se que l'on voit )

# 5) resulata de la commande ./log > MACHINE.log sur les diférente machines

### Client:



### DHCP:



### Routeur:



### SMTP: