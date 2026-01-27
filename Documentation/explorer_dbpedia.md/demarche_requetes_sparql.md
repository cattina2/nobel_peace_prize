# Exploration de DBpedia: exercice en classe

Pour cette exploration, je vais utiliser comme point de départ  la liste Wikipédia suivante:

<https://en.wikipedia.org/wiki/List_of_individuals_nominated_for_the_Nobel_Peace_Prize_(1900%E2%80%931999)>

La fiche de nominé pour le prix de Nobel de la paix que je vais utiliser comme exemple est celle de Henry Dunant:

<https://en.wikipedia.org/wiki/Henry_Dunant>

Je dois utiliser pour l'exploration de DBpedia l'éditeur de commandes suivantes:

<https://dbpedia.org/sparql>

Pour se faire, je vais tenter d'appliquer les commandes proposées par le professeur dans cet éditeur en les adaptant à ma population et à ma liste de personnes. 

## Tentative de compréhension des informations

Triplets => ont la structure suivante: sujet-prédicat-objet. Donc l'individu s (pour sujet) a une propriété "rdf:type" ou en anglais "is a".
- Donc possibilité d'écrire dbo:Person
  = ceci revient à dire que x est une personne 

Il faut maintenant prendre notre population choisie - nominés au prix Nobel de la paix pour moi - et la liste correspondante en anglais. 

Voici l'url de mon personnage choisi:
- https://en.wikipedia.org/wiki/Henry_Dunant
Pour accéder à DBPedia, il faut y ajouter la chose suivante: http://dbpedia.org/resource/
Ce qui donne: http://dbpedia.org/resource/Henry_Dunant URL IRI => ceci est la pk d'Henry Dunant dans la base de données graphe. 

## Utilisation de commandes DBPedia 

Maintenant, nous pouvons utiliser cette commande-ci:

PREFIX dbr: <http://dbpedia.org/resource/>
  SELECT *
WHERE 
{
  dbr:Henry_Dunant ?p ?o
}

Cette commande permet de trouver les triplets ayant comme sujet Henry Dunant. L'idée est maintenant d'aller chercher l'entier de notre population sans devoir utiliser manuellement cette commande pour chacun de nos sujets. 

### Compter le nombre de personnes totales dans DBPedia

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dbo: <http://dbpedia.org/ontology/>

SELECT (COUNT(*))
WHERE 
{
  ?person rdf:type dbo:Person 
}

Selon cette requête, il y a 2642367 sur DBPedia. 

### Compter le nombre de personnes dans ma liste 

Cette première commande permet d'afficher dix personnes provenant de ma liste. 

PREFIX dbr: <http://dbpedia.org/resource/>
  PREFIX dbo: <http://dbpedia.org/ontology/>
  SELECT DISTINCT ?p ?o1 
  WHERE { 
    dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?o1.
    ?o1 a dbo:Person.
    }
  LIMIT 10

    
En modifiant cette requête, il est possible de compter le nombre de personnes dans la liste. Pour ce faire, il faut enlever la limite (LIMIT 10) et modifier la ligne de commande SELECT afin qu'elle affiche: SELECT(COUNT(*)as?eff) : 

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
SELECT (COUNT(*)as?eff)
WHERE { 
  dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?o1.
  ?o1 a dbo:Person.
  }

Ma liste contient ainsi 456 personnes le 24 novembre 2025. 

Après les avoir compté, nous voulons voir les propriétés sortantes:

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
SELECT ?p1 (COUNT(*) as ?eff)
WHERE { 
dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?o1.
?o1 a dbo:Person;
    ?p1 ?o2.
  }
GROUP BY ?p1
ORDER BY DESC(?eff)

Cette requête me permet de dégager les propriétés pouvant m'être utiles pour la suite. Elle est donc primordiale. 

### Requête "award"

PREFIX dbr: <http://dbpedia.org/resource/>
  SELECT *
WHERE 
{
  dbr:Henry_Dunant <http://dbpedia.org/ontology/award>?award
}

Ceci me donne qu'Henry Dunant a reçu le prix Nobel de la paix. 

Il est possible de compter le nombre "d'award" pour ma population grâce à la requête suivante:

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
SELECT ?award (COUNT(*)as ?eff)
WHERE { 
dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?o1.
?o1 a dbo:Person;
    dbo:award ?award
  }
GROUP BY ?award
ORDER BY DESC(?eff)

