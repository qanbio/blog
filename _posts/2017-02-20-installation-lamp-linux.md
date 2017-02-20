---
layout: post
title: Installation de LAMP en ligne de commande sur une machine Linux
categories: [tech]
tags: [java, linux, debian, installation, setup,apache mysql,php]
author: crepin_fadjo
comments: true
fullview: false
description: Ce post est un aide-mémoire pour l'installation en ligne de commande de LAMP
---
## Installation
Avant de commencer mettre à jour la distribution de debian avec la commande
{% highlight shell %}
apt-get update && apt-get upgrade
{% endhighlight shell %}

L’ensemble peut s’installer de façon basique.Pour ce fait exécuter la commande:

{% highlight shell %}
sudo apt-get install apache2 libapache2-mod-php5 mysql-server php5-mysql php5-gd phpmyadmin
{% endhighlight %}


Cette commande ajoutera toutes les dépendances requises et installera donc un système LAMP de base. On y a ajouté phpMyAdmin qui permettra de gérer les bases de données de MySQL en ligne.
Lors de l’installation un mot de passe vous sera demandé pour le compte d’administration de MySQL, il est impératif pour des raisons de sécurité d’en spécifier un.
Il vous sera ensuite demandé quel serveur web à reconfigurer pour exécuter phpMyAdmin, choisissez apache2. Il est également demandé s’il faut configurer la base de données de phpmyadmin avec dbconfig-common, répondez Oui puis un mot de passe administrateur qui sert à créer la base de phpMyAdmin, il faut mettre le même que l’on a inséré lors de la configuration de MySQL.

## Tester le bon déroulement de l’installation
Afin de tester si tout s’est bien passé vous devez connaître votre adresse ip

### Tester si apache est disponible
Avant de tester si apache est disponible on va modifier son port de démarage et mettre la valeur voulu.
Pour ce fait exécuter la commande suivante:

{% highlight bash %}
sudo vi /etc/apache2/port.conf
{% endhighlight bash %}

![configuration port apache](../../../../assets/media/2017-02-20-installation-lamp-linux/port.PNG "configuration port apache")

Redemarrer apache
{% highlight bash %}
sudo /etc/init.d/apache2 restart
{% endhighlight bash %}

Maintenant nous pouvons tester apache en allant dans un navigateur et en allant sur  l’url suivante

{% highlight bash %}
<adresse IP>:<port>
{% endhighlight bash %}

![verification fonctionnement apache](../../../../assets/media/2017-02-20-installation-lamp-linux/apache.PNG "verification fonctionnement apache")

### Tester PHP

Créons un fichier de test

{% highlight bash %}
sudo sh -c 'echo "<?php phpinfo();?>" > /var/www/html/phpinfo.php'
{% endhighlight bash %}


Maintenant ouvrez un navigateur et aller sur l’url suivante:

{% highlight bash %}
<adresse IP>:<port>/phpinfo.php
{% endhighlight bash %}

Vous devez avoir ceci

![verification fonctionnement php](../../../../assets/media/2017-02-20-installation-lamp-linux/php.PNG "verification fonctionnement php")


## Tester Mysql

Connectez vous à mysql avec la commande

{% highlight bash %}
Mysql -u “root” -p “password”
{% endhighlight bash %}

Créer d’autres users pour qu’ils puissent accéder à mysql.Ici on en a créé 3 : paterne, fabrice et crépin
{% highlight bash %}
GRANT ALL ON *.*  TO 'paterne'@'%' IDENTIFIED BY 'paterne';
GRANT ALL ON *.*  TO 'fabrice'@'%' IDENTIFIED BY 'fabrice';
GRANT ALL ON *.*  TO 'crepin'@'%' IDENTIFIED BY 'crepin';
{% endhighlight bash %}

##  Tester phpmyadmin


Ouvrez le fichier /etc/apache2/apache2.conf avec les droits root, soit :
{% highlight bash %}
sudo gedit /etc/apache2/apache2.conf
{% endhighlight bash %}
Ajoutez la ligne suivante à la toute fin du fichier :
{% highlight bash %}
Include /etc/phpmyadmin/apache.conf
{% endhighlight bash %}
Ensuite, redémarrez votre serveur Apache avec la commande suivante :
{% highlight bash %}
sudo /etc/init.d/apache2 restart
{% endhighlight bash %}
Après avoir fait ces étapes, vous devriez être capable d'accéder à votre outil de gestion de base de données phpMyAdmin via :

{% highlight bash %}
<adresse IP>:<port>/phpmyadmin
{% endhighlight bash %}

![verification fonctionnement phpmyadmin](../../../../assets/media/2017-02-20-installation-lamp-linux/phpmyadmin.PNG "verification fonctionnement phpmyadmin")


