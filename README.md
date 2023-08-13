
Projet de Script Shell qui permettra de faire la supervision de certains services ou deamon Linux.
 
Pour y parvenir, il faut créer un repertoire et un fichier de type script et ouvrir le fichier avec gedit ou vi et ecrire ce script.


## Alarmes des services MySql : 

Le script ci dessous permet de vérifier que les services du serveur de base de données Mysql ont bien demarré, sont en panne ou sont inaccessible.
A chaque fois le test effectué, d'envoyer une alarme OK ou Critical, warning et Unknown à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 

#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin

SMYSQL=sudo service mysql status
if [ $(SMYSQL=~ 'stop')]
then
	$(SMYSQL)>SMYQLS.txt
	echo "Le service MySQL est arrété" |mail -s "Service MySQL arrété" yvandarcy12@gmail.com
elif [ $(SMYSQL)=~'failed' ]
	$(SMYSQL)>SMYQLS.txt
	echo "Le service MySQL ne fonctionne pas" |mail -s "Service MySQL arrété" yvandarcy12@gmail.com
	$(SMYSQL)>SMYQLS.txt
elif [ $(SMYSQL)=~'inactive' ]
	echo "Le service MySQL est inactif" |mail -s "Service MySQL est inactif" yvandarcy12@gmail.com
	$(SMYSQL)>SMYQLS.txt
else [$(SMYSQL)=~'active']	
	echo "Le service MySQL est actif" |mail -s "Service MySQL est actif" yvandarcy12@gmail.com
fi

## Alarme sur les services web apache2

Le script ci dessous permet de vérifier que les services du serveur web apache2 ont bien demarré, sont en panne ou sont inaccessible.
A chaque fois le test effectué, d'envoyer une alarme OK ou Critical, warning et Unknown à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 

#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin
echo le serveur web fonctionne bien?

SAPACHE= systemctl status apache2 | grep 'active'
if [ $(SAPACHE)=~'stop' ]
then
	$(SAPACHE)>SAPACHE.txt
	echo "Le service apache2 est arrété" |mail -s "Service apache arrété" yvandarcy12@gmail.com
elif [ $(SAPACHE)=~'failed' ]
	$(SAPACHE)>SAPACHE.txt
	echo "Le service apache2 ne fonctionne pas" |mail -s "Service apache arrété" yvandarcy12@gmail.com
elif [ $(SAPACHE)=~'inactive' ]
	$(SAPACHE)>SAPACHE.txt
	echo "Le service apache2 est inactif" |mail -s "Service apache est inactif" yvandarcy12@gmail.com
else [$(SAPACHE)=~'active']	
	echo "Le service apache2 est actif" |mail -s "Service MySQL est actif" yvandarcy12@gmail.com
fi

## Alarme sur les disques pleins

Le script ci dessous permet de vérifier que les disques du fichier systeme sont pleins?

A chaque fois le test effectué, d'envoyer une alarme Dique est plein si les disques sont à plus de 80% et envoyer les alarmes à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 
#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin

echo les disques du systeme de fichier sont pleins
DISKUTILISE=$(df -h|awk '{print $3}' )
if [ $DISKUTILISE -gt 80% ]
then
	$DISKUTILISE> disqueplein.txt
	echo "Le disque dur est utilisé à plus de 80 %" |mail -s "Utilisation de disque" yvandarcy12@gmail.com
fi

## Alarme sur le processus qui consomme plus l'espace memoire

Le script ci dessous permet de vérifier le processus qui consomme beaucoup d'espace memoire qui dépasse ou égale 100Mo.

A chaque fois le test effectué, d'envoyer une alarme : Attention, un processus consomme un grand espace memoire envoyer à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 

#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin

echo le processus qui consomme un grand espace memoire

if ["$(ps -eo pcpu,pid,user,args --no-headers |\sort -t. -nk1,2 -k4,4 -r |\head -n 1 |\ awk'{print "\t"$1"\t"$2"\t"$3"\t"$4" "$5}')"-ge "100Mo"];
then
echo Attention, un processus consomme un grand espace memoire >> yvandarcy12@gmail.com;

else
echo le processus consomme normalement;

fi


## Automatisation des taches sur le terminal 

La commande ci dessous :

Crontab -e : permet d'ouvrir l'editeur vim et de programmer le moment de la supervision des services en mentionnant les minutes, l'heure, le jour du mois, les mois et le jour de la semaine suivant le nom du fichier.
ci dessous l'exemple: 

00 07 * * * script.sh 

C'est à dire que chaque jour à 7h du matin, l'administrateur recevra l'etat des services du serveur de base de données Mysql, du serveur web apache2, des disques pleins du système de fichier et du processus qui est gourmand dépassaant 100Mo dans la consommation de l'espace memoire 




