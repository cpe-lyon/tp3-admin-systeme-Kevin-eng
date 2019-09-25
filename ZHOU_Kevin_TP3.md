# Exercice 1. Commandes de base
**Commencez par mettre à jour votre système avec les commandes vues dans le cours.**

*On va mettre à jour le système avec la commande : sudo apt full-upgrade*

**1. Quels sont les 5 derniers paquets installés sur votre machine ?**

*La commande qui va permettre de visualiser les derniers paquet installés est : "cat dpkg.log | tail -5"*

**2. Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !).
Comment explique-t-on la (petite) différence de comptage ?**

*La commande avec dpkg : kz@serveur:/var/log$ dpkg -l | grep "^ii" | wc -l*

*514*


*La commande avec apt :  kz@serveur:/var/log$ apt list --installed | wc -l*

*WARNING: apt does not have a stable CLI interface. Use with caution in scripts.*

*515*

*APT importe plus d'informations que DPKG*

**3. Combien de paquets sont disponibles en téléchargement ?**

*kz@serveur:/var/log$ apt list | wc -l*

*WARNING: apt does not have a stable CLI interface. Use with caution in scripts.*

*64118*

**4. Créer un alias “maj” qui met à jour le système**

*On va aller ajouter lalias dans le .bashrc à l'aide de la commande : " nano .bashrc*

*Puis à l'intérieur alias maj ="sudo apt full-upgrade"*

*Pour que notre commande soit prise en compte, nous devons nous reconnecter*

**5. A quoi sert le paquet fortunes ? Installez-le.**

*Il s’agit d’un programme affichant des épigrammes, connues sous le nom de cookies fortune, choisies aléatoirement à partir de fichiers fortune.*

*On va utiliser la commande :*

*sudo apt install fortunes*

**6. Quels paquets proposent de jouer au sudoku ?**

*"apt search sudoku" va permettre de trouver le paquets*

*On va utiliser la commande:*

*kz@serveur:~$ sudo apt install sudoku*

**7. Lister les derniers paquets installés explicitement avec la commande apt install**

*kz@serveur:~$ grep "apt install" /var/log/apt/history.log*

*Commandline: apt install openssh-server*

*Commandline: apt install fortunes*

*Commandline: apt install sudoku*


# Exercice 2.
**A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule
commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des
commandes utiles) ? Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension
.sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.**

*which ls va permettre de localiser la commande*

*kz@serveur:~$ which -a  ls | xargs dpkg -S 2> /dev/null*

*coreutils: /bin/ls*

*Script :*

which -a  $1 | xargs dpkg -S 2> /dev/null

*On peut transformer le script en commande :*

kz@serveur:~/script$ chmod +x origine_commande

kz@serveur:~/script$ sudo cp origine_commande /usr/bin/

[sudo] password for kz:

kz@serveur:~/script$ origine_commande ls

coreutils: /bin/ls

# Exercice 3.
**Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package
spécifié dans cette commande.**

#!/bin/bash

(dpkg -l | grep "$1" ) 1>/dev/null && echo "INSTALLÉ" || echo "NON INSTALLÉ"

*TEST :*

kz@serveur:~/script$ installer coreutils

INSTALLÉ

kz@serveur:~/script$ installer ezrz

NON INSTALLÉ

# Exercice 4.
**Lister les programmes livrés avec coreutils. A quoi sert la commande ’[’ et comment aﬀicher ce qu’elle retourne?**

kz@serveur:~$ dpkg -L coreutils

/.

/bin

/bin/cat

/bin/chgrp

/bin/chmod

/bin/chown

/bin/cp

/bin/date

..

# Exercice 5.aptitude 
**Installez le paquet emacs à l’aide de la version graphique d’aptitude.**

 *On va installer aptitue avec "sudo apt install aptitude"*

*On lance aptitude en ecrivant : aptitude*

*On va aller dans les paquets non installé, ensuite /emacs pour recherche, puis appuyer sur n jusqu'a le trouver.La touche + va permettre de selectionner les packages à installer. Finir en appuyant sur g pour lancer l'installation*

# Exercice 6. Installation d’un paquet par PPA 

**Certains logiciels ne figurent pas dans les dépôts oﬀiciels.C’est le cas par exemple de la version ”oﬀicielle” de Java depuis qu’elle est développée par Oracle. Dans ces cas, on peut parfois se tourner vers un ”dépôt personnel” ou PPA. 1. Installer la version Oracle de Java (avec l’ajout des PPA) sudo add-apt-repository ppa:linuxuprising/java sudo apt update sudo apt install oracle-java11-installer 2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il?**


kz@serveur:~$ sudo add-apt-repository ppa:linuxuprising/java

[sudo] password for kz:

kz@serveur:~$ sudo apt update

sudo apt install oracle-java11-installer-local

*Ensuite, on observe qu'un nouveau fichier a été crée :*

kz@serveur:/etc/apt$ ls

apt.conf.d   preferences.d  sources.list.curtin.old  
sources.list.save

auth.conf.d  sources.list   sources.list.d          

trusted.gpg.d

*Le nouveau fichier qui a été crée contient un lien vers une plateforme de mise à jour afin de faire la maintenance de la version d'oracle installé. Il va se connecter sur le lien puis rentrer dans disco puis main.*

deb-src http://ppa.launchpad.net/linuxuprising/java/ubuntu disco main
