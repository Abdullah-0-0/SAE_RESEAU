# Comment copier des fichiers entre la Silverblue et une VM ? 

Utile pour récupérer le programme `log`

Il existe au moins deux moyens : 1) soit en utilisant un protocole sécurisé de transfert de fichier (`sftp`), 2) soit en utilisant une commande de copie sécure de fichiers (`scp`).

Les deux moyens requièrent que vous récupériez l'adresse IP de la VM configurée sur son interface `host0` ; pour la suite, nous désignerons celle-ci par `ip_sur_host0`. 

La mise en oeuvre de (1) est la plus simple. Votre explorateur de fichiers *Fichier* connait le protocole `sftp`.

Dans l'application *Fichier*, 
- faire `Ctrl+l` (ou bien cliquer sur le champ qui indique le chemin courant), puis saisir la commande suivante `sftp://root@ip_sur_host0` et faire `Entrée`. 
- Un message *Échec de la vérification d'identité* apparaîtra, faire `Se connecter malgré tout`. Et voilà. 
- Travailler depuis le répertoire `/root`.

La mise en oeuvre de (2) n'est pas compliquée non plus. La commande `pwd` exécuté depuis la Silverblue ou la VM nous apprend le chemin du répertoire dans lequel on se trouve. Au démarrage, vous devez vous trouver respectivement dans `/var/home/$USER/` sur la Silverblue et `/root` sur la VM. Supposons l'existence du fichier `/root/fichier_sur_vm.txt` sur votre VM et du fichier `/var/home/$USER/fichier_sur_silverblue.txt` sur la Silverblue. 
Que cela soit pour copier depuis la Silverblue vers la VM ou depuis la VM vers la Silverblue, nous allons à chaque fois exécuter une commande depuis la Silverblue. La commande en question est `scp SOURCE CIBLE` avec `SOURCE` et `CIBLE` des chemins potentiellement distants.

Pour copier de la VM vers la Silverblue alors depuis la Silverblue 
- faire `scp root@ip_sur_host0:/root/fichier_sur_vm.txt /var/home/$USER/` ou tout simplement `scp root@ip_sur_host0:fichier_sur_vm.txt .`

Pour copier de la Silverblue vers la VM alors depuis la Silverblue 
- faire `scp /var/home/$USER/fichier_sur_silverblue.txt root@ip_sur_host0:/root/`  ou tout simplement `scp fichier_sur_silverblue.txt root@ip_sur_host0:`
