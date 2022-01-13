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

> Pourquoi MongoDB et pas MySQL ?

Contrairement à MySQL, MongoDB n'est pas un système relationnel. Cela signifie que les données n'ont pas la contrainte d'être unique.Une certaine donnée peut donc être stockée à plusieurs reprises.

Une base de données MySQL contient des clées primaires et des clées secondaires qui permettent de créer des liens entre les différentes tables de la base de données. Cela permet donc l'unicité des données (https://sql.sh/cours/create-table/primary-key). Dans le cadre d'une base de données MongoDB, les documents possèdent bien une clé unique ("_id" : ObjectId("xxxx")) mais elle sert uniquement a identifier de manière unique la donnée en question, et pas à établir des relations. (https://askcodez.com/lid-de-la-collection-de-longueur-dans-mongodb.html)

MongoDB est un SGBD (Système de Gestion de Base de Données) orienté document, ce qui n'est pas le cas de MySQL
Un avantage certain de MongoDB face à MySQL est que les données sont stockées sous forme de documents au format JSON dans des collections, alors que pour MySQL, les données sont stockées sous forme de lignes dans des tableaux. Dans un tableau de données MySQL, toutes les lignes de données ont la même composition, donc le même nombre de colonnes, alors que dans une collection MongoDB, chaque document peut avoir une composition différente.  

## Le BigData

Il y a énormément de domaines qui se spécialise dans la gestion de grosse quantité de données. Les principaux sont la politique, la recherche scientifique, la communication et le marketing.
MongoDB est parfaitement adapté à la gestion de grandes quantités de données

> Hadoop, autre alternative NoSQL

Hadoop est une solution opensource appartenant à Apache. Hadoop offre un espace de stockage de données massif, et ce pour tous types de données. Hadoop est réputé pour son immense puissance de traitement de données et pour la prise en charge d'une quantité de tâches virtuellement illimitée. Hadoop était la principale plateforme de BigData en 2018 (selon https://www.lebigdata.fr/hadoop).

## Les fonctionnalités avancée de MongoDB

Les fonctionnalités avancées de MongoDB sont un énorme avantage car elle permettent de réaliser des analyses poussées. Grâce à ses analyses en condition réelles, un ciblage et des changements dans les restaurants pourront être effectués afin de développer les restaurants. Ces analyses pourront également servir à faire des statistiques.

Afin de réduire toujours plus le temps de retours des données suite à une requête, nous pouvons utiliser l'indexation des données.
L'indexation correspond à réaliser un tableu d'éléments triés selon un champs du document. Par exemple, si nous voulons indexer la collection "Clients" sur le champs "Firstname", cela permettra en cas de recherche d'un client en fonction de son nom de famille de trouver directement dans quels documents de la collection le nom de famille recherché est présent, sans avoir à parcourir touts les documents les uns après les autres. Cela mène à un gain de performance considérable sur nos requêtes.
![indexation](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/index.png?raw=true)


## Création de la base de données

Pour commencer, nous avons créé la base de données "Restaurants" puis nous nous y sommes connecté avec la commande 
> use Restaurants

Nous avons ensuite créée une collection "restaurants", puis une collection "clients"
> db.createCollection("Restaurants", { collection: {locale:"fr"}})

> db.createCollection("Clients", { collection: {locale:"fr"}})

![CreateCollection](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetCreateCollectionRestaurant.PNG?raw=true)

Il a ensuite fallu intégrer des données dans ces collections, par exemple, nous avons intégrer des clients :
![CreateCollection](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetCreateCollectionClient.PNG?raw=true)

La commande insertMany nous retourne le résultat suivant :
![ReturnInsertMany](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetCreateCollectionClientResponse.PNG?raw=true)
Nous voyons donc que 6 clients ont bien été entrés dans la collection Clients. Nous pouvons par ailleurs avoir leur _id.

## Les requêtes GéoSpaciales

Les requêtes GéoSpaciales sont très utiles pour faire des analyses en fonction d'un zone géographique par exemple.
Une requêtes GéoSpaciale permet de trouver des éléments de la base de données inclus dans un polygones, mais aussi de trouver des éléments de la BDD proches d'un point géographique donné.

Avant toutes choses, il faut créer un index de type "2dsphere" sur le champs localisation de la collection "Restaurants". Cet index va nous permettre de récupérer les documents compris dans une zone circulaire définie (grâce aux coordonnées des restaurants)
![CreateIndex](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetCreateIndex2dsphere.png?raw=true)

Nous pouvons vérifier que l'index est bien créé dans Atlas
![CreateIndex](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetCreateIndex2dsphere2.png?raw=true)

Nous pouvons donc maintenant effectuer une requête GéoSpaciale. Par exemple, nous voulons récupérer les restaurants se trouvant à moins de 10 km d'un lieu nommé "opéra".
![RequeteGéoSpaciale](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetNearSphereResult1.png?raw=true)
Nous ponvonc don voir qu'il y a un seul restaurant se trouvant à une distance de 10km maximum de l'"opéra"

Maintenant, si nous augmentons la distance maximale à 100 km:
![RequeteGéoSpaciale](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetNearSphereResult2.png?raw=true)
Il y a maintenant le restaurant "Roanne Restaurant" qui apparait. Il n'apparaissait pas avant car il est à une distance supérieure à 10km de l'"opéra".

Ces deux requêtes montrent bien que nous pouvons retourner tous les restaurants se trouvant dans une certaine zone géographique

## L'aggrégation

L'aggrégation permet de regrouper les données de plusieurs documents afin de retourner un résultat.
La pipeline d'aggrégation est une aggrégation faite aavec un enchainement de plusieurs étapes.
L'aggrégation sera bien utile dans notre projet, elle permettra par exemple de trouver les clients qui sont allés dans un certain restaurant donné. A savoir que l'aggrégation peut également être combinée à une requête GéoSpaciale.

Voici une requête utilisant l'aggrégation :
![Aggrégation](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetAggregateRequestWithResponse.png?raw=true)
Cette requête nous permet de retourner le nom et prénom ("firstname" "lastname") des clients ayant déjà été dans le restaurant n°1 ("visit.restaurantId").

Nous allons maintenant combiner l'aggrégation avec une requête GéoSpaciale :
![Aggrégation](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projectCombineGeoSWithAggregation.png?raw=true)
Cette requête permet dans un premier temps de récupérer la distance entre l'"opéra", point défini en fonction de ces coordonnées, et les restaurants. L'aggrégation va ensuite filtrer tous les restaurants afin de ne retourner uniquement les restaurants se trouvant dans la ville("city") de Lyon.

Nous allons maintenant exécuter la même requete mais dans la collection "Clients":
![Aggrégation](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetClientsGeoNearResult1.png?raw=true)
Cette requête retourne donc tous les clients habitant à Lyon("localisation.city"), ainsi que la distance("distance.estimation") de leur domicile par rapport à l'opéra.

## La méthode explain

La méthode explain sert à retourner à la fin du résultat de requête des informations sur l'éxécution de certaines commande (aggregate(), count(), distinct(), find(), findAndModify(), delete(), mapReduce() et update()).
Parmis ces informations, elle retourne par exemple le temps d'éxécution d'une requête. Ce retour peut être très utile dans le cadre de l'optimisation de requêtes.

Voici une requête utilisant la méthode explain :
![Explain](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/projetMethodExplain3.png?raw=true)
On peut voir que énormément d'information sont retournés.

# TP Requêtes

### 1. Ecrivez une requête MongoDB pour afficher tous les documents dans les restaurants de la collection 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP1.png?raw=true)

### 2. Ecrivez une requête MongoDB pour afficher les champs restaurant_id, name, borough et cuisine pour tous les documents de la collection restaurant.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP2.png?raw=true)

### 3. Ecrivez une requête MongoDB pour afficher les champs restaurant_id, name, borough et cuisine, mais exclure le champ _id pour tous les documents de la collection restaurant. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP3.png?raw=true)

### 4. Ecrivez une requête MongoDB pour afficher les champs restaurant_id, nom, arrondissement et code postal, mais excluez le champ _id pour tous les documents de la collection restaurant.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP4.png?raw=true)

### 5. Ecrivez une requête MongoDB pour afficher tous les restaurants qui sont dans l'arrondissement du Bronx. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP5.png?raw=true)

### 6. Ecrivez une requête MongoDB pour afficher les 5 premiers restaurants qui se trouvent dans le quartier du Bronx. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP6.png?raw=true)

### 7. Ecrivez une requête MongoDB pour afficher les 5 restaurants suivants après avoir sauté les 5 premiers qui sont dans le quartier du Bronx.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP7.png?raw=true)

### 8. Ecrivez une requête MongoDB pour trouver les restaurants qui ont obtenu un score supérieur à 90. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP8.png?raw=true)

### 9. Ecrivez une requête MongoDB pour trouver les restaurants qui ont obtenu un score supérieur à 80 mais inférieur à 100. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP9.png?raw=true)

### 10. Ecrivez une requête MongoDB pour trouver les restaurants qui se situent dans une latitude inférieure à -95,754168.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP10.png?raw=true)

### 11. Ecrivez une requête MongoDB pour trouver les restaurants qui ne préparent pas de cuisine américaine et dont la note est supérieure à 70 et la latitude inférieure à -65.754168
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP11.png?raw=true)

### 12. Ecrivez une requête MongoDB pour trouver les restaurants qui ne préparent pas de cuisine américaine, qui ont obtenu une note supérieure à 70 et qui sont situés à une longitude inférieure à -65.754168.
Note : Faites cette requête sans utiliser l'opérateur $and. 

![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP12.png?raw=true)

### 13. Ecrivez une requête MongoDB pour trouver les restaurants qui ne préparent pas de cuisine 'américaine' et ont obtenu une note 'A' et qui n'appartiennent pas à l'arrondissement de Brooklyn. Le document doit être affiché en fonction de la cuisine par ordre décroissant. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP13.png?raw=true)

### 14. Écrivez une requête MongoDB pour trouver l'identifiant du restaurant, le nom, l'arrondissement et la cuisine pour les restaurants qui contiennent 'Wil' dans les trois premières lettres de leur nom. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP14.png?raw=true)

### 15. Écrivez une requête MongoDB pour trouver l'identifiant, le nom, l'arrondissement et la cuisine des restaurants dont le nom contient trois lettres 'ces'. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP15.png?raw=true)

### 16. Ecrivez une requête MongoDB pour trouver l'Id, le nom, l'arrondissement et la cuisine des restaurants qui contiennent "Reg" comme trois lettres dans leur nom.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP16.png?raw=true)

### 17. Ecrivez une requête MongoDB pour trouver les restaurants qui appartiennent à l'arrondissement du Bronx et qui préparent des plats américains ou chinois.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP17.png?raw=true)

### 18. Ecrivez une requête MongoDB pour trouver l'Id, le nom, l'arrondissement et la cuisine des restaurants qui appartiennent à l'arrondissement de Staten Island ou Queens ou Bronx ou Brooklyn.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP18.png?raw=true)

### 19. Ecrivez une requête MongoDB pour trouver l'Id, le nom, l'arrondissement et la cuisine des restaurants qui n'appartiennent pas à l'arrondissement de Staten Island ou Queens ou Bronx ou Brooklyn.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP19.png?raw=true)

### 20. Ecrivez une requête MongoDB pour trouver l'Id, le nom, l'arrondissement et la cuisine des restaurants qui ont obtenu un score inférieur à 10.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP20.png?raw=true)

### 21. Ecrivez une requête MongoDB pour trouver l'identifiant, le nom, l'arrondissement et la cuisine des restaurants qui ont préparé des plats autres que "américains" et "chinois" ou dont le nom commence par la lettre "Wil". 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP21.png?raw=true)

### 22. Ecrivez une requête MongoDB pour trouver l'Id du restaurant, le nom et les notes pour les restaurants qui ont obtenu la note "A" et un score de 11 sur la date : ISODate "2014-08-11T00:00:00Z" parmi de nombreuses dates d'enquête... 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP22.png?raw=true)

### 23. Ecrivez une requête MongoDB pour trouver l'Id du restaurant, le nom et les notes pour les restaurants dont le 2ème élément du tableau des notes contient la note "A" et le score 9 à la date ISOD "2014-08-11T00:00:00Z". 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP23.png?raw=true)

### 24. Ecrivez une requête MongoDB pour trouver l'identifiant du restaurant, le nom, l'adresse et l'emplacement géographique des restaurants pour lesquels le deuxième élément du tableau coord contient une valeur supérieure à 42 et inférieure à 52... 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP24.png?raw=true)

### 25. Ecrivez une requête MongoDB pour classer le nom des restaurants dans l'ordre croissant avec toutes les colonnes.
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP25.png?raw=true)

### 26. Ecrivez une requête MongoDB pour classer le nom des restaurants en ordre décroissant avec toutes les colonnes. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP26.png?raw=true)

### 27. Ecrivez une requête MongoDB pour classer le nom de la cuisine par ordre croissant et pour cette même cuisine, l'arrondissement doit être classé par ordre décroissant. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP27.png?raw=true)

### 28. Ecrivez une requête MongoDB pour savoir si toutes les adresses contiennent la rue ou pas.
Je n'ai pas réussi à créer cette requête

### 29. Ecrivez une requête MongoDB qui sélectionnera tous les documents de la collection restaurants où la valeur du champ coord est Double. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP29.png?raw=true)

### 30. Ecrivez une requête MongoDB qui sélectionnera l'Id, le nom et les notes des restaurants qui renvoient 0 comme reste après avoir divisé le score par 7. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP30.png?raw=true)

### 31. Écrivez une requête MongoDB pour trouver le nom du restaurant, l'arrondissement, la longitude, l'attitude et la cuisine pour les restaurants dont le nom contient trois lettres 'mon'. 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP31.png?raw=true)

### 32. Écrivez une requête MongoDB pour trouver le nom du restaurant, le quartier, la longitude, la latitude et la cuisine des restaurants dont le nom contient trois lettres "Mad". 
![TP1](https://github.com/NicolasISI/MongoDB-Rapport/blob/master/image/TP32.png?raw=true)

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



