---
layout: post
title: Connection à une instance MySQL depuis une machine tierce
categories: [tech]
tags: [linux, debian, mysql, db, sgbd, remote, connection, acces ]
author: paterne_gaye
comments: true
fullview: false
description: Durant un projet de développement, on a souvent besoin d'accéder à une instance de MySQL installée non pas sur notre machine mais sur un serveur partagé dans le cloud par exemple ou même sur l'intranet. Nous allons nous intérersser à la configuration spécifique pour ce type d'accès distant.
---
# Pré-requis

## Intérêt d'avoir des utilisateurs spécifiques
Quand on installe MySQL, on a un utilisateur par défaut : **root**. Cependant, il est conseillé de créer des utilisateurs spécifiques et de réserver le user root à des taches d'administration. Ainsi, qu'il s'agit d'une application devant accéder à la base de données ou d'un utilisateur humain, nous allons créer un compte utilisateurs spécifique.

## Notion de client MySQL
Il s'agit de tout logiciel nous permettant de manipuler une instance MySQL. On distingue les clients graphiques des outils en "ligne de commande". Dans le cadre de ce tutoriel, nous allons manipuler les deux.

Concernant les outils graphiques nous avons opté pour **MySQL Workbench** fourni par Oracle qui est aussi l'éditeur de MySQL.

## Notion d'accès distant
Il s'agit de toutes les connections qui se produisent depuis une machine autre que celle ou MySQL est installée. On pourrait citer au moins trois use cases récurrents :

* Pendant la phase de développement, les développeur ont besoin d'accéder depuis leur machine à la base de données de test qui et commune à toute l'équipe.
* Pendant la phase d'exploitation, l'équipe support peut avoir accès à la base de données de PROD.
* Au niveau architecture, on peut choisir d'installer la base de données sur un serveur distinct de celui ou est déployé l'application elle-même. Ceci pourrait se justifier entre autres par des problématiques de sécurité.

Techniquement, il est possible d'autoriser des utilisateur, développeurs et service support, de se connecter directement à la machine pour exécuter leurs requêtes. Mais cette solution n'est pas acceptables tant au niveau sécurité que au niveau du confort. En effet, ces utilisateurs souhaitent en général utiliser un client graphique pour consulter leur bases de donner
Pour tous ces cas nous avons besoin que des utilisateurs puissent accéder à MySQL depuis leur machine


# Configuration du serveur

## Autoriser la machine serveur à accepter des requêtes MySQL provenant de machines distantes
MySQL écoute sur le port 3306 par défaut. Il faut donc s'assurer que la machine serveur autorise les requêtes sur ce port ou le port qui aurait été défini dans la configuration.


## Autoriser MySQL à accepter des requêtes provenant de machines distantes
Il nous faut modifier le fichier de config de MySQL :

{% highlight bash %}
sudo vi /etc/mysql/my.cnf
{% endhighlight %}

Il faut ensuite retrouver le paramètre **bind-address** et lui donner la valeur **0.0.0.0**

![image](../../../../assets/media/2017-02-18-mysql-remote-connection/mysql-config-file-bind-adress.png " ")

## Autoriser un utilisateur donnée à accéder à MySQL depuis une machine distante
Avant tout nous allons créer l'utilisateur

{% highlight sql %}
CREATE USER 'paterne_gaye'@'%' IDENTIFIED BY 'mot-de-passe';
{% endhighlight %}

Ensuite nous allons autoriser cet utilisateur à accéder à tous les objets de la base de données depuis n'importe quelle mahine.

{% highlight sql %}
GRANT ALL PRIVILEGES ON *.* TO 'paterne_gaye'@'%' IDENTIFIED BY 'paterne_gaye'  WITH GRANT OPTION;
{% endhighlight %}

Dans le cas ou l'utilisateur dispose d'une IP fixe, par exemple **192.168.2.7** , nous pouvons restreindre l'autorisation à cette IP :

{% highlight sql %}
GRANT ALL PRIVILEGES ON *.* TO 'paterne_gaye'@'192.168.2.7' IDENTIFIED BY 'paterne_gaye'  WITH GRANT OPTION;
{% endhighlight %}

## Accès à MySQL depuis la ligne de commande
MySQL Workbench intègre un client non-graphique MySQL. Pour y avoir accès, il suffit de rajouter le dossier d'installation de MySQL Workbench dans le path.

![image](../../../../assets/media/2017-02-18-mysql-remote-connection/mysql-connection-cmdline.png " ")

## Acces à MySQL depuis une machine distance à l'aide d'un client graphique
Nous pouvons également accéder à notre base de données distante depuis notre machine locale :

![image](../../../../assets/media/2017-02-18-mysql-remote-connection/mysql-connection-reussie.png " ")


