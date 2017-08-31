---
layout: post
title: Mise en place d'un serveur RapidPro sur un serveur debian
categories: [tech]
tags: [linux, debian, rapidpro]
author: 
comments: true
fullview: false
description: Ce post est un aide-mémoire pour la mise en place du serveur rapidpro sur un serveur debian 
---

# Qu'est ce que RapidPro?
RapidPro est une plateforme de gestion de flux d’information, en temps réel, proposée par l’UNICEF. La plateforme a été développée en 2014, en collaboration avec la société rwandaise de logiciels Nyaruka.


# Mise en place de RapidPro
La mise en place d’un serveur RapidPro suppose le déploiement d’une application Web complexe intégrant des bases de données PostgreSQL et Redis ainsi que plusieurs outils de développement côté serveur tels que NodeJS. Dans ce qui va suivre, nous allons successivement installer ces différents composants, y compris l’environnement de développement Python. 

Créer un utilisateur temba avec les droits d’administrateur
Installation de NodeJS et ses modules
Installation de Redis 
Installation de Less
Installation de CoffeeScript
Installation de libmagic-dev
Installation de PostgreSQL
Récupérer le code source de RapidPro
Construire l’environnement virtuel

## Créer l’utilisateur ‘temba’ sur la machine
Créer l’utilisateur temba:
{% highlight bash %}
sudo adduser temba
{% endhighlight %}

Ajouter l’utilisateur temba au groupe d’administrateur sudo:
{% highlight bash %}
sudo usermod -aG sudo temba
{% highlight bash %}

Basculer vers le compte de l’utilisateur temba :
{% highlight bash %}
su - temba
{% highlight bash %}

![image](../../../../assets/media/2017-08-31-rapidpro-installation-serveur-debian/creer_utilisateur_temba .png " ")


## Installation de NodeJS
## Installer NVM
Télécharger le script d’installation et l'exécuter 
{% highlight bash %}
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
{% endhighlight %}

Se déconnecter et se reconnecter pour forcer l’initialisation de l’environnement  et se reconnecter
{% highlight bash %}
exit 
su - temba
{% endhighlight %}

Vérifier que la commande nvm fonctionne correctement
{% highlight bash %}
nvm current
{% endhighlight %}

![image](../../../../assets/media/2017-08-31-rapidpro-installation-serveur-debian/intaller_mvn.png " ")

## Installation de NodeJS

Installer le patch le plus récent de la version 6 (v6.*.*), au moment de l'écriture de cet article, il s’agit de la v6.11.2
{% highlight bash %}
nvm install v6
{% endhighlight %}


Vérifier le contenu du script téléchargé en demandant à nouveau à NVM la version courante de Node installée :
{% highlight bash %}
nvm current
{% endhighlight %}

Vérifier que notre installation de Node fonctionne correctement
{% highlight bash %}
node -v
{% endhighlight %}

![image](../../../../assets/media/2017-08-31-rapidpro-installation-serveur-debian/installer_node.png " ")

Vérifier que dans le PATH il y a /home/temba/.nvm/versions/node/v6.11.2/bin/

{% highlight bash %}
sudo ln -sf /home/temba/.nvm/versions/node/v6.11.2/bin/node /usr/bin/node
{% endhighlight %}

![image](../../../../assets/media/2017-08-31-rapidpro-installation-serveur-debian/verifier_path_apres_node_install.png " ")


## Installation de Redis

Nous allons maintenant installer Redis, un Système de gestion de base de données “mémoire” de type “clés - valeurs” .

Installation de Redis
{% highlight bash %}
sudo apt-get update 
sudo apt-get install redis-server
{% endhighlight %}

Vérifier que Redis s’est bien installé
{% highlight bash %}
redis-server -v
{% endhighlight %}

 Vérifier la version du client, nécessaire pour se connecter au serveur Redis
 {% highlight bash %}
 redis-cli -v
 {% endhighlight %}


Démarrer le serveur Redis et faire un ping pour vérifier que le serveur est en marche. En retour le serveur vous renvoie un PONG
{% highlight bash %}
redis-server
redis-cli ping
 {% endhighlight %}

## Installation de Less
Less un langage au dessus de CSS qui permet de rajouter notamment des fonctions et des opérateurs au CSS. Less est utilisé dans la Webapp RapidPro. Nous allons donc installer le compilateur LESSC qui permettra de convertir les fichiers .less en format .css

Installer le compilateur par npm
{% highlight bash %}
npm install -g less
{% endhighlight %}

Vérifier que le compilateur Lessc est bien installé
{% highlight bash %}
which lessc
{% endhighlight %}

{% highlight bash %}
sudo ln -sf /home/temba/.nvm/versions/node/v6.11.2/bin/lessc /usr/bin/lessc
{% endhighlight %}

## Installation de CoffeeScript
A présent, nous allons installer les outils de développement web côté serveur. Il s’agit de CoffeeScript un langage au dessus de JavaScript qui apporte un peu de sucre syntaxique au langage.

Installer CoffeeScript
{% highlight bash %}
npm install -g coffeescript
{% endhighlight %}

Une fois l’installation terminée, les commandes ‘coffee’ et ‘cake’, vont automatiquement explorer le répertoire personnel de l’utilisateur temba (~) pour trouver la version de CoffeeScript qui est installée en local.

{% highlight bash %}
sudo ln -sf /home/temba/.nvm/versions/node/v6.11.2/bin/coffee /usr/bin/coffee
{% endhighlight %}

## Installation de Bower

 Bower est un gestionnaire de packages semblable à npm, mais  en plus des dépendances Node, il gère les composants (html, css, js, etc...) front-end des webapp. Il faut installer bower avec le module bower.

{% highlight bash %}
npm install bower -g
{% endhighlight %}

## Installation de PostgreSQL

Nous allons commencer par installer le Système de gestion de base de données PostgreSQL ainsi que son plugin PostGIS qui permet de manipuler les données géospatiales. 

Identifier le nom de code de votre distribution linux. Ce nom nous servira à l’étape 2.,  pour préciser la source où sera récupéré Postgresql

{% highlight bash %}
lsb_release -c
{% endhighlight %}

Stretch est le nom de code de développement de Debian 9.

Créer le fichier /etc/apt/sources.list.d/pgdg.list

{% highlight bash %}
sudo touch /etc/apt/sources.list.d/pgdg.list 
{% endhighlight %}

Ouvrir le fichier /etc/apt/sources.list.d/pgdg.list  et rajouter la ligne suivante

{% highlight bash %}
deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main
{% endhighlight %}


Importer la clé du dépôt de Postgresql

{% highlight bash %}
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
{% endhighlight %}


Mettre à jour la liste de paquets :
{% highlight bash %}
sudo apt-get update
{% endhighlight %}

Installer PostgreSQL et PostGis : 
{% highlight bash %}
sudo apt-get install postgresql postgis
{% endhighlight %}

Vérifier que Postgresql est bien installé :
{% highlight bash %}
which psql 
psql --version
{% endhighlight %}

Vérifier que l’utilisateur postgres a été créé :
Par défaut un nouvel utilisateur postgresql est créé après l’installation de PostgreSQL. Il faut tout de même vérifier que l’utilisateur postgres a été créé par défaut
{% highlight bash %}
sudo -i -u postgres
{% endhighlight %}


Créer l’utilisateur temba pour postgres :
Grace à l’utilisateur Linux postgres avec lequel nous sommes connectés maintenant, nous avons accès à un ensemble d'utilitaires du SGBD Postgres. A l’aide de l’utilitaire “createuser” nous allons créer un user Postgres nommé temba et correspondant au user Linux temba. Le mot de passe de cet utilisateur temba dans Postgres sera temba.

{% highlight bash %}
createuser temba -P -d
{% endhighlight %}

L’option “-d” va permettre au nouvel utilisateur de créer des bases de données tandis que l’option “-P” va nous forcer à définir un mot de passe.

Créer la base de donnée temba
{% highlight bash %}
createdb temba
{% endhighlight %}

Lancer la console Postgres
{% highlight bash %}
 psql
{% endhighlight %}

A noter que comme nous étions logués dans Linux en tant que postgres, la commande précédente nous connecte dans Postgres en tant que postgres.  

Dans Postgres, donner au user temba les droits de superuser
{% highlight bash %}
ALTER USER temba WITH SUPERUSER;
{% endhighlight %}

Dans la console Postgres, attribuer les privilèges de ‘super user’ à l’utilisateur postgresql nommé temba. Cela lui permettra d’ajouter des extensions à sa base de données




Se deconnecter de la console Postgres

Ensuite, il faut se déconnecter de la base de données avec la commande:
{% highlight bash %}
\q
{% endhighlight %}

Se logger dans Linux en tant que temba

{% highlight bash %}
su - temba
{% endhighlight %}

Passer à la console de Postgresql avec la commande :
{% highlight bash %}
psql.
{% endhighlight %}

Vérifier que le user temba est connecté 
{% highlight bash %}
\conninfo
{% endhighlight %}


Activer les extensions Postgis pour la base de données temba avec les commandes respectives :
{% highlight bash %}
create extension postgis;
create extension postgis_topology;
create extension hstore;
{% endhighlight %}

Déconnectez-vous de la base de données 
{% highlight bash %}
(\q) 
{% endhighlight %}

et allez dans le dossier personnel du compte temba 
{% highlight bash %}
(cd ~)
{% endhighlight %}

## Récupérer le code source de RapidPro

Il s’agit de récupérer le projet RapidPro depuis Github vers votre serveur, avec l’outil Git. Git est un système de contrôle de version pour suivre les changements dans les fichiers informatiques et coordonner le travail sur ces fichiers parmi plusieurs personnes. 

Installation de git
Installer git sur le serveur s’il n’est pas disponible : 

{% highlight bash %}
sudo apt-get install git -y 
{% endhighlight %}

L’option y permet de répondre automatiquement ‘yes’ à toutes les confirmations interactives

Récupérer le code source

Récupérer le dépôt de RapidPro sur le serveur  
{% highlight bash %}
 git clone https://github.com/rapidpro/rapidpro.git
{% endhighlight %}


Construire l’environnement virtuel
Lorsqu’on parcourt le projet Rapidpro, on trouve divers fichiers.

{% highlight bash %}
cd ~/rapidpro 
ls -la
{% endhighlight %}

Nous allons d’abord créer un lien symbolique  /home/temba/rapidpro/temba/settings.py  qui va pointer vers le 
fichier de configuration  /home/temba/rapidpro/temba/settings.py.dev : 

{% highlight bash %}
sudo ln -s  /home/temba/rapidpro/temba/settings.py.dev /home/temba/rapidpro/temba/settings.py
{% endhighlight %}

## Installation de Python et ses paquets

Rapidpro est écrit en Python. Vous devriez toujours utiliser un environnement virtuel pour exécuter votre application RapidPro. Les dépendances nécessaire pour RapidPro se trouvent dans pip-freeze.txt.

Installation de Python et de Pip
(une mise à jour du système (Debian) suffit pour nous avoir Python à jour sur la machine)

{% highlight bash %}
sudo apt-get update
sudo apt-get -y upgrade
{% endhighlight %}

Vérifier que l’installation est bien effectuée

{% highlight bash %}
python  --version
{% endhighlight %}

Installer python-pip

{% highlight bash %}
sudo apt-get install python-pip
{% endhighlight %}

Vérifier que pip est installé par défaut avec Python. 
{% highlight bash %}
apt-cache show python-pip
{% endhighlight %}

## Installer et démarrer l’outil virtualenv
Virtualenv est un outil pour créer des environnements Python isolés. Virtualenv crée un dossier contenant tous les exécutables nécessaires pour utiliser les paquets dont un projet Python aurait besoin.

Voici la commande pour installer Virtualenv
pip install virtualenv
{% endhighlight %}

Démarrer l’outil virtualenv, pour récupérer ses dépendances et l’activer
sudo /usr/bin/easy_install virtualenv
{% endhighlight %}

Vérifier que la commande est opérationnelle
virtualenv --version
{% endhighlight %}


Créer l’environnement virtuel 
Exécutez la commande 

sudo virtualenv env
{% endhighlight %}

Cette commande va créer un dossier dans le répertoire actif, pour contenir les fichiers exécutables Python et une copie de la bibliothèque pip, laquelle vous pouvez utiliser pour installer d'autres paquets.
Le nom de l’environnement ainsi créé est env

Activer l’environnement virtuel du projet

source env/bin/activate
{% endhighlight %}

L’activation de l’environnement fera apparaître le nom de l’environnement entre parenthèses au début du prompt 


Désormais, tout paquet que vous installerez depuis l’environnement, avec pip sera placé dans le dossier env, isolé de l'installation globale Python.

Installer les composants supplémentaires 

sudo apt-get install build-essential libssl-dev libffi-dev python-dev
{% endhighlight %}

Finaliser installation env virtuel
Dans le projet de Rapidpro, se trouve le fichier pip-freeze.txt. Ce fichier contient la liste des dépendances utiles pour finaliser la mise en place de l’environnement virtuel.

Faite la commande suivante pour restaurer l’environnement env et importe toutes les librairies nécessaire pour RapidPro, y compris DJANGO.

sudo pip install -r pip-freeze.txt
{% endhighlight %}

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
