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

# Partie SMTP

## I) crée l'utilisateur :

1) créer utilisateur **alice** dans la vm `SMTP`:

    ````bash 
    adduser alice
    ````
## II) install / configuration les paquets necessaire
1)  rajouter packet necessaire :

    ````bash 
    #install le paquet
    apt install postfix
    #puis le configue
    dpkg-reconfigure postfix
    ````



2) Configurer le `postfix`:

    * aller dans la partie `site internet`
    puis le configure de cette manier la en suivant les different etapes.

        *  rajouter le nom du courrier du système `mail122.com`.
    ![bvngivu](RESSOURCE/cap1.png)
        * rajouter d'adresse `mail122.com` sur la liste des domaines.
        ![bvngivu](RESSOURCE/cap2.png)
        * rajouter le réseaux internes :
        ![bvngivu](RESSOURCE/cap3.png)
        * et finir par selection le protocols `ipv4`.
        ![bvngivu](RESSOURCE/cap4.png)
3) mettre en route le SMTP :
    * après avoir terminer la configuration faire la commande `systemcl reload postfix`.

        ```` bash 
        systemcl reload postfix 
        ````

## partier client

* crée utilisateur:








adduser bob