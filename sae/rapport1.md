# Configuration réseau

* NOMS : Chamsiddine Abdullah - 
* GROUPE de TP :  GP-1
* X : 122
* SECRET : antares
* IP_CLIENT : 172.20.122.1/24
* IP_DHCP : 172.20.122.2/24
* IP_SMTP : 192.168.0.22/24
* IPs du ROUTEUR : 172.20.122.3/24 et 192.168.0.122/24

**Note**: Le document suivant doit rendre compte de votre plan d’adressage (i.e. la description des différents LAN, de leur interconnexion, des machines avec les IP voire @MAC que vous jugerez pertinentes), de vos tables de routage de CLIENT, ROUTEUR, SMTP et celle (supposée) de DNS, des commandes à réaliser sur CLIENT, ROUTEUR, SMTP, et tout ce qui vous semble nécessaire à la configuration de votre réseau.

## Plan d'adressage


## Tables de routage

**Note**: il existe des générateurs de tables MDhttps://www.tablesgenerator.com/markdown_tables

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

	nslookup smtp122.mail122.com 192.168.0.254


