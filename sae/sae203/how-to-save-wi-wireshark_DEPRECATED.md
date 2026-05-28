2025-05-13 Wireshark est une application virtualisée. A ce titre, par défaut, elle n'a pas le droit d'écrire sur le système de fichiers du système qui la virtualise. La note suivante décrivait un process pour récupérer néanmoins une capture. A partir de ce jour, ce n'est plus la procédure à suivre. La DSI a ajouté une règle autorisant l'écriture sur le système de fichiers par Wireshark. Son usage sera donc transparent pour vous. Faire juste "Sauvegarder sous".


# Comment sauvegarder une capture de trames réalisée par Wireshark ? Comment lire une capture ?

Ceux/celles qui ont tenté de sauvegarder leur capture de trames sous Wireshark auront constaté que l'opération n'est pas possible car le système de fichier n'est accessible qu'en lecture.

Pas de panique.

D'abord, il faut savoir que le Wireshark, que vous utilisez via la commande `vm-tcpdump`, est exécuté au travers de l'outil Flatpak qui sert à virtualiser des applications pour les distributions Linux. Virtualiser une application signifie l'exécuter dans un environnement sûr, isolé du reste du système.

Un répertoire est réservé à l'application Wireshark. Il se trouve à cet endroit `/run/user/$UID/.flatpak/org.wireshark.Wireshark/tmp/` avec `$UID` la variable d'environnement contenant votre "User IDentifier".

On peut voir le contenu de ce répertoire en faisant par exemple 

    ls /run/user/$UID/.flatpak/org.wireshark.Wireshark/tmp/

Même si on ne peut exporter une capture, il faut savoir que la capture en cours est écrite dans un fichier temporaire dans ce répertoire. Les fichiers de capture ont l'extension `.pcapng`. Si vous êtes en train d'écouter une interface d'une machine vous devriez voir un fichier dans le répertoire réservé.

Pour connaître le nom du fichier temporaire dont la capture est en cours, regarder dans les messages affichés dans le terminal où vous exécutez la commande `vm-tcpdump` et chercher la ligne qui fait référence à un fichier de capture créé. Quelque chose comme cela : 
<pre>
** (wireshark:2) 15:51:35.808516 [Capture MESSAGE] -- File: "/tmp/wireshark_Standard inputBKJ052.pcapng"
</pre>

Ci-dessous le déroulé théorique complet :

1. Lancer une capture depuis la Silverblue sur l'interface INTERFACE et la machine MACHINE souhaitée

<pre>
vm-tcpdump MACHINE INTERFACE
</pre>

2. Ne pas stopper la capture. Si ce que vous cherchiez à constater est bien présent dans le Wireshark alors faire une copie de ce fichier temporaire dans le répertoire de votre choix, par exemple votre répertoire Documents de Perso sur le réseau. Pour cela, exécuter, depuis une autre fenêtre sous la Silverblue, la commande suivante :

<pre>
cp /run/user/$UID/.flatpak/org.wireshark.Wireshark/tmp/*.pcapng /var/home/$USER/reseau/Perso/Documents/
</pre>

Seulement maintenant vous pouvez arrêter votre capture.

Avec l'explorateur de fichiers *Fichier*, vous pouvez vérifier que le fichier a bien été créé. En double cliquant dessus, vous pourrez le visualiser à nouveau avec Wireshark.

Vous pouvez aussi démarrer Wireshark en ligne de commande depuis la Silverblue avec la commande

	flatpak run org.wireshark.Wireshark
    
Puis chercher votre fichier à lire via "open".
