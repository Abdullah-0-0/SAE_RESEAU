# Analyse session SMTP

* NOMS : Chamsiddine Abdullah - Clément Pissot
* GROUPE de TP :  TD-1
* X : 122
* SECRET : antareszetapuppis
* IP_CLIENT : 172.20.122.1/24
* IP_DHCP : 172.20.122.2/24


### a faire:

* capture de trame de la machine client vers le serveur smtp

* y identifier :
- les nom de fammilles
- la valeur de notre secret plus notre X
- le resultat devrait resemblé a: ABDULAH, PISSOT, antareszetapuppis, 122

* expliquer dans les grandes ligne comment on a fait pour crée le le serveur reseaux
* expliquer la capture d'écran

comande faites:

## partie SMTP

* pour crée l'utilisateur :

adduser alice

* rajouter packet:

apt install postfix

dpkg-reconfigure postfix

* configurer le postfix:

aller dans site internet

écrire le configurer comme suivant 
puis apuyer sur continuer

apres avoir terminer la configuration faire la commande 

systemcl reload postfix 

## partier client

* crée utilisateur:

![bvngivu](RESSOURCE/cap1.png)


![bvngivu](RESSOURCE/cap2.png)

![bvngivu](RESSOURCE/cap3.png)

![bvngivu](RESSOURCE/cap4.png)
adduser bob