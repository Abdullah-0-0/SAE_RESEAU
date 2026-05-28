# Comment travailler à plusieurs sur un même système ?

Soient deux postes de travail distincts A et B avec l'étudiant Toto connecté sur A et l'étudiant Titi connecté sur B.

Demander aux enseignants de désactiver le pare-feu linux.

Les manipulations suivantes indiquent comment permettre à Titi de se connecter sur la machine de A et, in fine, que Toto et Titi puissent travailler avec les mêmes VM. La machine A hébergera donc les VM.

Sur la machine A, depuis la Silverblue, 
* faire `/usr/sbin/ip a l dev enp0s31f6`
* noter l'adresse IP, nommé ci-dessous `IP_A`

Sur la machine B, depuis la Silverblue
* faire `ssh Toto@IP_A` (attention les majuscules du login E... sont importantes)
* taper `yes` la première fois
* Toto rentre son mot de passe (il ne le dicte pas à Titi)
* Désormais Titi se trouve aussi sur A et peut donc exécuter les commandes `vm-ls, vm-run...`

