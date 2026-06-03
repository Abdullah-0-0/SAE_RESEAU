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

# comment on si est pris:

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

        ````bash 
        systemcl reload postfix 
        ````

# Partie CLIENT

## I) Creation de l'utilsateur dans la VM `CLIENT`:

1) créer l'utilisateur bob  
    ````bash
    adduser bob
    ````
## II) Envoie de mail :

### dans la VM `CLIENT`.

1) créer une connexion avec le `SMTP` avec la commande `netcat`:

    ````bash 
    nc smtp122.mail122.com smtp
    ````
2) démarrer une session SMTP avec la commende `ehlo`.
    ````bash
    ehlo smtp122.mail122.com
    ````
3) et ensuite precise le expediteur dans notre cas **`bob`**
    ````bash 
    mail from: bob@mail122.com
    ````
4) puis le precise le destinateur dans notre cas **`alice`**.
    ````bash
    rcpt to: alice@mail122.com
    ````
5) puis a ce moment la on peut ecrire le contenu de notre menace avec `data`.
    ````bash
    data
    #puis ecrire le contenu.
    CHAMSIDDINE,PISSOT,antareszetapuppis,122
    # et finir par un point pour "dire que on n'a fini"
    .
    ````
6) et on finit par **``quit``**
    ````bash 
    quit
    ````


### III) Verification de l'envoie du mail.

Pour verifier que le mail a etais mien envoyer.

on va aller regard au pres de `SMTP`.
en regard le contenu de `/var/mail/alice`

````bash 
cat /var/mail/alice
````
    



#  capture de trame:




on peut voir deux machine comuniquer avec les adresse IP suivante:

* 172.20.122.1 qui corespond a la machine client
* 192.168.0.22 qui corespond a la machine SMTP

la première ligne est probablement la plus diférents des autres.
elle est envoyer par la machine client vers l'adresse 192.168.0.254 qui corespond au DNS
et est un protocole DNS (qui est le seul de la trame).
il permet le transpore d'un message pour l'adresse smtp122.mail122.com.
puis commence l'envoi du mail en lui meme.
il va passer par localhost.ad.iut-nantes.univ-nantes.prive.
on aprend que le message viens de l'adresse mail bob@mail122.com et a été recu par l'adresse alice@mail122.com.
l'entièreter des tramee envoyer sont de protocole TCP et SMTP.
