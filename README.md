# MongoDB-Rapport
# Projet 

## Travail demandé
Une chaine de restaurants souhaite récupérer des données clients afin de capitaliser sur un flux de personnes sans cesse grandissant (plusieurs dizaine de milliers de clients par semaine dans toute la France).

Fournissez un projet d'application centré sur un stockage des données avec Mongo DB.

Le projet dans son ensemble ne sera pas forcément terminé cette semaine, en revanche toutes les interactions avec la base de données doivent être documentées et testées. Et toutes les questions posées devront être traitées.

## Introduction

Pour répondre au besoin demandé, nous avons décidé de mettre en place une application qui sera installé sur les bornes des restaurants. Un client pourra se connecter à son compte grâce à son numéro de téléphone.
Plus un client viendra dans un restaurant, plus le système lui posera des questions ciblées.
Les points de fidélité seront accumulé en fonction des réponses à leur question posées par le système, mais aussi à chaque connection du client dans un restaurant. Afin d'éviter les abuts, un client pourra se connecter une seule fois toutes les 5h dasn un même restaurant.

## Technologies utilisées

Pour développer ce projet, nous avons retenu 2 technologies, qui sont maîtrisées et connues de toute l'équipe du projet.
Pour la partie Backend, création de l'API, nous allons utiliser NodeJS.
En ce qui concernet le front, et donc l'interface graphique, nous utiliserons React

## MongoDB et notre projet

MongoDB est une base de donnée de documents qui est réputée pour sa simplicité de développement et de mise à l'échelle d'une application. (https://docs.mongodb.com/manual/)

Nous avons choisi d'utiliser MongoDB car c'est un système de base de données non relationnel et qu'il est extrenement performant pour la gestion de grosse quantité de données (BigData)

Pour notre projet, la base de données a été créée à l'aide de MongoDB Atlas. Le logiciel Compass est utilisé pour administrer et visualiser la base de données.
La connection à nos bases de données à été sécurisé avec une whitelist d'IP via Atlas. De plus, nous avons mis en place un compte utilisateur par personne afin de pouvoir limiter toute fraude.

## Création de la base de données

Pour commencer, nous avons créé la base de données "Restaurants" puis nous nous y sommes connecté avec la commande 
> use Restaurants

Nous avons ensuite créée une collection "restaurants", puis une collection "clients"
> db.createCollection("Restaurants", { collection: {locale:"fr"}})
> db.createCollection("Clients", { collection: {locale:"fr"}})
![CreateCollection](https://github.com/NicolasISI/MongoDB-Rapport/blob/main/image/TP1.png?raw=true)

Il a ensuite fallu intégrer des données dans ces collections, par exemple, nous avons intégrer des clients :





## Requêtes GéoSpatiales :

### Expliquez au sein de votre rendu l’utilité de telles requêtes. Discuter de la structure des données GeoJson.
Les requêtes géospatiales permettent d’analyser des données en fonction d’un zone géographique.

### Illustrez votre réponse avec un exemple (l’usage de MongoDB Charts est optionnel mais apprécié)
Avec MongoDB Chart, nous avons la possibilité de créer des tableaux, graphiques, cartes et bien d’autre afin de visualiser la donnée sans interagir directement avec la base de données.
Cela nous permet de réaliser des études, bilans ou encore inventaire

### Comment ce type de données peut intervenir dans projet de la semaine ?
Ce type de données peut nous permettre de nous apercevoir si dans une zone géographique données si un restaurant est manquant, ce qui permettrai ensuite de cibler les endroits où ouvrir un nouveau restaurant serait judicieux.
Ce type de requête permettra également de comparer deux restaurant afin de savoir pourquoi dans un même secteur géographique un restaurant fonctionne plus qu’un autre.
Enfin, cela permettrait également de réaliser des statiques par secteur géographique.

### Proposer une ou plusieurs applications de requêtes géospatiale et illustrer au moins l’une des propositions avec un exemple (Screenshot attendu)



