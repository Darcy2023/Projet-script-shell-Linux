
Projet de Script Shell qui permettra de faire la supervision de certains services ou deamon Linux.
 
Pour y parvenir, il faut créer un repertoire et un fichier de type script et ouvrir le fichier avec gedit ou vi et ecrire ce script.


## Alarmes des services MySql : 

Le script ci dessous permet de vérifier que les services du serveur de base de données Mysql ont bien demarré, sont en panne ou sont inaccessible.
A chaque fois le test effectué, d'envoyer une alarme OK ou Critical, warning et Unknown à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 

#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin

if ["$(service mysql status)"=~ "start/running"];

then

  echo OK >> yvandarcy12@gmail.com;

elif["$(service mysql status)"=~"stop"];

then

  echo Warning >> yvandarcy12@gmail.com;

elif["$(service mysql status)"=~"failed"];

then

  echo Critical >> yvandarcy12@gmail.com;

else["$(service mysql status)"=~"unreachable"];

then

  echo Unknown >> yvandarcy12@gmail.com;

fi

## Alarme sur les services web apache2

Le script ci dessous permet de vérifier que les services du serveur web apache2 ont bien demarré, sont en panne ou sont inaccessible.
A chaque fois le test effectué, d'envoyer une alarme OK ou Critical, warning et Unknown à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 

#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin
echo le serveur web fonctionne bien?

if ["$(service apache2 status)"=~ "start/running"];

then

  echo OK >> yvandarcy12@gmail.com;

elif["$(service apache2 status)"=~"stop"];

then

  echo Warning >> yvandarcy12@gmail.com;

elif["$(service apache2 status)"=~"failed"];

then

  echo Critical >> yvandarcy12@gmail.com;


else["$(service apache2 status)"=~"unreachable"];

then

  echo Unknown >> yvandarcy12@gmail.com;
fi
## Alarme sur les disques pleins

Le script ci dessous permet de vérifier que les disques du fichier systeme sont pleins?

A chaque fois le test effectué, d'envoyer une alarme OK si les disques sont inférieur ou egal à 50%, warning s'ils sont à 75%  et Critical s'ils sont à 95% et envoyer les alarmes à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 
#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin

echo les disques du systeme de fichier sont pleins


if ["$(df . -h)"=>"95%"];
then
echo disk usage is critical >> yvandarcy12@gmail.com;

elif ["$(df . -h)"=>"75%"];
then
echo disk usage warning >> yvandarcy12@gmail.com;

else ["$(df . -h)"<="50%"];
then
echo disk usage OK >> yvandarcy12@gmail.com;

fi
## Alarme sur le processus qui consomme plus l'espace memoire

Le script ci dessous permet de vérifier le processus qui consomme beaucoup d'espace memoire qui dépasse 100Mo.

A chaque fois le test effectué, d'envoyer une alarme : Attention, un processus consomme un grand espace memoire envoyer à l'adresse mail suivant : yvandarcy12@gmail.com de l'administrateur. 

#! bin/bash

PATH=/usr/sbin:/usr/sbin:/usr/sbin

echo le processus qui consomme un grand espace memoire

if ["$(ps -eo pcpu,pid,user,args --no-headers |\sort -t. -nk1,2 -k4,4 -r |\head -n 1 |\ awk'{print "\t"$1"\t"$2"\t"$3"\t"$4" "$5}')">"100Mo"];
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




