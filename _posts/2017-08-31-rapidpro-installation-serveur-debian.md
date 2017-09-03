---
layout: post
title: Mise en place d'un serveur RapidPro sur un serveur debian
categories: [tech]
tags: [linux, debian, rapidpro]
author: ginette_hounkponou
comments: true
fullview: false
description: Dans ce billet, nous allons installer pas à pas l'application RapidPro sur une machine Linux Débian 9
---

### Problématique
La mise en place d’un serveur RapidPro suppose le déploiement d’une application  intégrant divers composants dont :
 * des bases de données PostgreSQL et Redis 
 
 * une application web développée à l'aide
   
    ** du framework Django (Python)
    
    ** d'outils JavaScript, notamment NodeJS, Less, CoffeeScript
    
Dans ce qui va suivre, nous allons successivement installer ces différents composants. 


### Création d'un utilisateur Linux 
RapidPro requiert la création d’un utilisateur temba avec des droits d’administrateur

Créer l’utilisateur temba:
sudo adduser temba

Ajouter l’utilisateur temba au groupe d’administrateur sudo:
{% highlight bash %}
sudo usermod -aG sudo temba
{% endhighlight %}

Basculer vers le compte de l’utilisateur temba :
{% highlight bash %}
su - temba
{% endhighlight %}

![Ajout de l'utilisteur Linux *temba*](../../../../assets/media/2017-08-31-rapidpro-installation-serveur-debian/adduser_temba.png "Ajout de l'utilisteur Linux *temba*")

### Installation de NodeJS
Nous allons d'abord installer l'utilitaire *NVM* qui permet de gérer facilement plusieurs versions de NodeJS. 

Dans un premier temps, nous allons télécharger le script d’installation de NVM et l'exécuter 

{% highlight bash %}
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
{% endhighlight %}
![Installer NVM](../../../../assets/media/2017-08-31-rapidpro-installation-serveur-debian/installer_nvm.png "Installer NVM")


Se déconnecter et se reconnecter pour forcer l’initialisation de l’environnement  et se reconnecter

{% highlight bash %}
exit 
su - temba
{% endhighlight %}

Vérifier que la commande nvm fonctionne correctement

{% highlight bash %}
nvm current
{% endhighlight %}


Installation de NodeJS
Installer le patch le plus récent de la version 6 (v6.*.*), au moment de l'écriture de cet article, il s’agit de la v6.11.2

{% highlight bash %}
nvm install v6
{% endhighlight %}
![Installer Node](../../../../assets/media/2017-08-31-rapidpro-installation-serveur-debian/installer_node.png "Installer NVM")

Vérifier le contenu du script téléchargé en demandant à nouveau à NVM la version courante de Node installée :

{% highlight bash %}
nvm current
{% endhighlight %}

Vérifier que notre installation de Node fonctionne correctement
{% highlight bash %}
node -v
{% endhighlight %}


Vérifier que dans le PATH il y a /home/temba/.nvm/versions/node/v6.11.2/bin/

{% highlight bash %}
echo $PATH
{% endhighlight %}

{% highlight bash %}
sudo ln -sf /home/temba/.nvm/versions/node/v6.11.2/bin/node /usr/bin/node
{% endhighlight %}


### Titre 2
texte ici

### Autres tires
texte ici

### Conclusion 
texte ici



{% highlight bash %}

{% endhighlight %}


{% highlight bash %}

{% endhighlight %}

======================================================================================================================================================================



Installation de Redis
Nous allons maintenant installer Redis, un Système de gestion de base de données “mémoire” de type “clés - valeurs” .

Installation de Redis
sudo apt-get update 
sudo apt-get install redis-server

Vérifier que Redis s’est bien installé
redis-server -v


 Vérifier la version du client, nécessaire pour se connecter au serveur Redis
redis-cli -v

Démarrer le serveur Redis et faire un ping pour vérifier que le serveur est en marche. En retour le serveur vous renvoie un PONG
redis-server
redis-cli ping





Installation de Less
Less un langage au dessus de CSS qui permet de rajouter notamment des fonctions et des opérateurs au CSS. Less est utilisé dans la Webapp RapidPro. Nous allons donc installer le compilateur LESSC qui permettra de convertir les fichiers .less en format .css

Installer le compilateur par npm
npm install -g less



Vérifier que le compilateur Lessc est bien installé
which lessc



sudo ln -sf /home/temba/.nvm/versions/node/v6.11.2/bin/lessc /usr/bin/lessc


Installation de CoffeeScript
A présent, nous allons installer les outils de développement web côté serveur. Il s’agit de CoffeeScript un langage au dessus de JavaScript qui apporte un peu de sucre syntaxique au langage.

Installer CoffeeScript
npm install -g coffeescript

Une fois l’installation terminée, les commandes ‘coffee’ et ‘cake’, vont automatiquement explorer le répertoire personnel de l’utilisateur temba (~) pour trouver la version de CoffeeScript qui est installée en local.



sudo ln -sf /home/temba/.nvm/versions/node/v6.11.2/bin/coffee /usr/bin/coffee


Installation de Bower
 Bower est un gestionnaire de packages semblable à npm, mais  en plus des dépendances Node, il gère les composants (html, css, js, etc...) front-end des webapp. Il faut installer bower avec le module bower.

npm install bower -g

Installation de PostgreSQL

Nous allons commencer par installer le Système de gestion de base de données PostgreSQL ainsi que son plugin PostGIS qui permet de manipuler les données géospatiales. 

Identifier le nom de code de votre distribution linux. Ce nom nous servira à l’étape 2.,  pour préciser la source où sera récupéré Postgresql
lsb_release -c

           Stretch est le nom de code de développement de Debian 9.

Créer le fichier /etc/apt/sources.list.d/pgdg.list         
sudo touch /etc/apt/sources.list.d/pgdg.list 

Ouvrir le fichier /etc/apt/sources.list.d/pgdg.list  et rajouter la ligne suivante
deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main
         
 
Importer la clé du dépôt de Postgresql
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -



Mettre à jour la liste de paquets :
sudo apt-get update

Installer PostgreSQL et PostGis : 
sudo apt-get install postgresql postgis

Vérifier que Postgresql est bien installé :
which psql 
psql --version


Vérifier que l’utilisateur postgres a été créé :
Par défaut un nouvel utilisateur postgresql est créé après l’installation de PostgreSQL. Il faut tout de même vérifier que l’utilisateur postgres a été créé par défaut
sudo -i -u postgres



Créer l’utilisateur temba pour postgres :
Grace à l’utilisateur Linux postgres avec lequel nous sommes connectés maintenant, nous avons accès à un ensemble d'utilitaires du SGBD Postgres. A l’aide de l’utilitaire “createuser” nous allons créer un user Postgres nommé temba et correspondant au user Linux temba. Le mot de passe de cet utilisateur temba dans Postgres sera temba.
createuser temba -P -d
L’option “-d” va permettre au nouvel utilisateur de créer des bases de données tandis que l’option “-P” va nous forcer à définir un mot de passe.

Créer la base de donnée temba
createdb temba

Lancer la console Postgres
 psql

A noter que comme nous étions logués dans Linux en tant que postgres, la commande précédente nous connecte dans Postgres en tant que postgres.  

Dans Postgres, donner au user temba les droits de superuser
ALTER USER temba WITH SUPERUSER;

Dans la console Postgres, attribuer les privilèges de ‘super user’ à l’utilisateur postgresql nommé temba. Cela lui permettra d’ajouter des extensions à sa base de données




Se deconnecter de la console Postgres

Ensuite, il faut se déconnecter de la base de données avec la commande:
\q

Se logger dans Linux en tant que temba
su - temba
Passer à la console de Postgresql avec la commande :
psql.

Vérifier que le user temba est connecté 
\conninfo



Activer les extensions Postgis pour la base de données temba avec les commandes respectives :
create extension postgis;
create extension postgis_topology;
create extension hstore;


Déconnectez-vous de la base de données (\q) et allez dans le dossier personnel du compte temba (cd ~)



Il s’agit de récupérer le projet RapidPro depuis Github vers votre serveur, avec l’outil Git. Git est un 
 Récupérer le code source de RapidProsystème de contrôle de version pour suivre les changements dans les fichiers informatiques et coordonner le travail sur ces fichiers parmi plusieurs personnes. 

Installation de git
Installer git sur le serveur s’il n’est pas disponible : 
sudo apt-get install git -y 
L’option y permet de répondre automatiquement ‘yes’ à toutes les confirmations interactives

Récupérer le code source

Récupérer le dépôt de RapidPro sur le serveur  
 git clone https://github.com/rapidpro/rapidpro.git



Construire l’environnement virtuel
Lorsqu’on parcourt le projet Rapidpro, on trouve divers fichiers.
cd ~/rapidpro 
ls -la

Nous allons d’abord créer un lien symbolique  /home/temba/rapidpro/temba/settings.py  qui va pointer vers le fichier de configuration  /home/temba/rapidpro/temba/settings.py.dev : 
sudo ln -s  /home/temba/rapidpro/temba/settings.py.dev /home/temba/rapidpro/temba/settings.py
 Installation de Python et ses paquets
Rapidpro est écrit en Python. Vous devriez toujours utiliser un environnement virtuel pour exécuter votre application RapidPro. Les dépendances nécessaire pour RapidPro se trouvent dans pip-freeze.txt.
Installation de Python et de Pip
(une mise à jour du système (Debian) suffit pour nous avoir Python à jour sur la machine)
sudo apt-get update
sudo apt-get -y upgrade
Vérifier que l’installation est bien effectuée
python  --version
Installer python-pip
sudo apt-get install python-pip
Vérifier que pip est installé par défaut avec Python. 
apt-cache show python-pip



Installer et démarrer l’outil virtualenv
Virtualenv est un outil pour créer des environnements Python isolés. Virtualenv crée un dossier contenant tous les exécutables nécessaires pour utiliser les paquets dont un projet Python aurait besoin.

Voici la commande pour installer Virtualenv
pip install virtualenv


Démarrer l’outil virtualenv, pour récupérer ses dépendances et l’activer
sudo /usr/bin/easy_install virtualenv

Vérifier que la commande est opérationnelle
virtualenv --version





Créer l’environnement virtuel 
Exécutez la commande sudo virtualenv env
Cette commande va créer un dossier dans le répertoire actif, pour contenir les fichiers exécutables Python et une copie de la bibliothèque pip, laquelle vous pouvez utiliser pour installer d'autres paquets.
Le nom de l’environnement ainsi créé est env

Activer l’environnement virtuel du projet
source env/bin/activate
L’activation de l’environnement fera apparaître le nom de l’environnement entre parenthèses au début du prompt 


Désormais, tout paquet que vous installerez depuis l’environnement, avec pip sera placé dans le dossier env, isolé de l'installation globale Python.

Installer les composants supplémentaires 
sudo apt-get install build-essential libssl-dev libffi-dev python-dev
 
Finaliser installation env virtuel
Dans le projet de Rapidpro, se trouve le fichier pip-freeze.txt. Ce fichier contient la liste des dépendances utiles pour finaliser la mise en place de l’environnement virtuel.

Faite la commande suivante pour restaurer l’environnement env et importe toutes les librairies nécessaire pour RapidPro, y compris DJANGO.

sudo pip install -r pip-freeze.txt

Vérifier que Django est bien installé :
django-admin --version

Installer les scripts Bower pour Rapidpro
A la racine du projet Rapidpro, il y a un fichier bower.json. Comme son nom l’indique, ce fichier sera exploité par l’outil Bower pour télécharger les dépendances Node du projet et gérer les composants frontend.
Charger les  dépendances listées dans le fichier bower.js avec la commande :
bower install

Modifier les configurations d’accès à la webapp  
Il faut indiquer à Django l’IP du serveur sur lequel sera déployé Rapidpro
sudo vi  /home/temba/rapidpro/temba/settings.py.dev
















Migration et démarrage de Rapidpro
Vous devriez maintenant pouvoir exécuter toutes les migrations et initialiser votre serveur de développement Rapidpro. Ce processus inclut : la migration, l’initialisation de tous les groupes d'utilisateurs et les autorisations. Le processus peut durer jusqu'à plusieurs minutes. Ce vaut le coup de patienter.

Migrer la webapp et synchroniser la base de données
sudo python manage.py migrate

Démarrer le serveur
sudo python manage.py runserver 0.0.0.0:8000
Vérifier que Rapidpro est accessible pour les clients web
Taper l’adresse publique de votre serveur avec le port 8000 





====sources

https://rapidpro.github.io/rapidpro/docs/development/
https://travis-ci.org/rapidpro/rapidpro/builds/124720331
https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-debian-8
http://docs.python-guide.org/en/latest/dev/virtualenvs/
https://www.tqhosting.com/kb/446/How-to-install-PostgreSQL-95-on-Debian-8-Jessie.html
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-9-4-on-debian-8


export PATH=/home/temba/.nvm/versions/node/v6.11.2/bin:$PATH

https://askubuntu.com/questions/370368/after-installing-coffee-script-the-coffee-command-is-found-but-does-nothing




