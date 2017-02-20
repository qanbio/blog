---
layout: post
title: Installation de mysql en ligne de commande sur une machine Linux
categories: [tech]
tags: [mysql, linux, debian,db,sgbd,acces, installation, setup]
author: crepin_fadjo
comments: true
fullview: false
description: Ce post est un aide-mémoire pour l'installation en ligne de commande de apache
---
## mysql
>MySQL est un système de gestion de bases de données relationnelles (SGBDR). Il est distribué sous une double licence GPL et propriétaire. Il fait partie des logiciels de gestion de base de données les plus utilisés au monde2, autant par le grand public (applications web principalement) que par des professionnels, en concurrence avec Oracle, Informix et Microsoft SQL Server.

Source : [wikipedia](https://fr.wikipedia.org/wiki/MySQL)

### Installation de mysql

Avant toute installation sur linux,il faut mettre à jour le package avec la commande

{% highlight bash %}
sudo apt-get update
{% endhighlight bash %}

Maintenant on peut installer mysql avec la commande

{% highlight bash %}
sudo apt-get install mysql
{% endhighlight bash %}

Lors de l’installation un mot de passe vous sera demandé pour le compte d’administration de MySQL, il est impératif pour des raisons de sécurité d’en spécifier un.

### Configuration
Pour en savoir plus sur la configuration se référer à cet [article](http://blog.qanbio.com/tech/2017/02/18/mysql-remote-connection.html)

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