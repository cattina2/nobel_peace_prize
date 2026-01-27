# Exercices en classe, 1er décembre 2025

Cet exercice sert à trouver soit des livres écrits par les personnes nous intéressant, soit sur les personnes nous intéressant. 
- Utiliser pour cela la population identifiée - nominés au prix Nobel de la paix.

## Ressources liées

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
SELECT ?person ?object
WHERE { 
dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?person.
?person a dbo:Person;
        dbo:birthDate ?birthDate ;
    <http://www.w3.org/2002/07/owl#sameAs> ?object.
    BIND(xsd:integer(SUBSTR(STR(?birthDate), 1, 4)) AS ?birthYear)
    FILTER ( ?birthYear > 1770)
}
LIMIT 100

Permet de voir où l'on peut retrouver les différentes personnes, dans différentes bases de données, par exemple dans DBpedia. Mais l'on retrouve d'autres systèmes d'informations dans laquelle ces personnes sont référencées. 
- p.ex. VIAF 


## Compter les ressources disponibles

Cette requête sert à savoir combien de personnes sont liées pour la population concernée. Donc combien de nominés du prix Nobel de la paix sont trouvables dans les différents systèmes d'informations. 
- VIAF = me donne 3216 personnes liées 

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
SELECT ?addr (COUNT(*) AS ?number)
WHERE { 
dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?person.
?person a dbo:Person;
        dbo:birthDate ?birthDate ;
    owl:sameAs ?object.
    BIND(xsd:integer(SUBSTR(STR(?birthDate), 1, 4)) AS ?birthYear)
    FILTER ( ?birthYear > 1770)
BIND(SUBSTR(STR(?object), 1, 20) as ?addr)
}
GROUP BY ?addr
ORDER BY DESC(?number)

## Exploration du réseau des bibliothèques allemandes

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX owl: <http://www.w3.org/2002/07/owl#>

SELECT ?person ?object
WHERE { 
dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?person.
?person a dbo:Person;
        dbo:birthDate ?birthDate ;
    owl:sameAs ?object.
    BIND(xsd:integer(SUBSTR(STR(?birthDate), 1, 4)) AS ?birthYear)
    FILTER ( ?birthYear > 1770 && CONTAINS(STR(?object), 'gnd'))
}
LIMIT 10

## Vérifier le point d'accès SPARQL de la GND
Pour pouvoir fouiller dans la GND, il faut vérifier que l'URI de DPbedia est toujours correct. IMPORTANT de changer l'outil de requête pour celui de la GND!!!

PREFIX gndo: <https://d-nb.info/standards/elementset/gnd#>
SELECT * WHERE {
?s a gndo:DifferentiatedPerson
}
LIMIT 10

Lien donné par la requête: https://d-nb.info/gnd/100000231
/!\ Il y a pour ce lien un s après http! 

Grâce à cela, il est possible de faire une requête sur le point d'accès de la GND:

## Requête point d'accès GND
Permet de regarder ce que l'on trouve sur les personnes listées dans DPbedia sur GND. C'est la clause "service" qui permet de faire une requête dans DBpedia depuis le point d'accès de la GND. 

PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?person ?gndUri ?p1 ?o1
WHERE {

SERVICE <https://dbpedia.org/sparql> {
SELECT ?person ?gndUri ?object
    WHERE { 
    dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?person.
    ?person a dbo:Person;
            dbo:birthDate ?birthDate ;
        owl:sameAs ?object.
        BIND(xsd:integer(SUBSTR(STR(?birthDate), 1, 4)) AS ?birthYear)
        FILTER ( ?birthYear > 1770 && CONTAINS(STR(?object), 'gnd'))
        BIND( URI(REPLACE(STR(?object), 'http', 'https'))  AS ?gndUri)
    }
    LIMIT 3
}

?gndUri ?p1 ?o1
}

## Compter les propriétés disponibles sur GND
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX owl: <http://www.w3.org/2002/07/owl>
#PREFIX xsd: http://www.w3.org/2001/XMLSchema#

PREFIX owl: <http://www.w3.org/2002/07/owl#>
SELECT ?p1 (COUNT(*) AS ?number) WHERE {
  SERVICE https://dbpedia.org/ sparql> {
    SELECT ?person ?gndUri ?object WHERE {
      dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?person .
      ?person a dbo:Person ;
              dbo:birthDate ?birthDate ;
              owl:sameAs ?object .
      BIND (xsd:integer(SUBSTR(STR(?birthDate),1,4)) AS ?birthYear)
      FILTER (?birthYear > 1770 && CONTAINS(STR(?object),'gnd'))
      BIND (URI(REPLACE(STR(?object), 'http', 'https')) AS ?gndUri)
    }
  }
  ?gndUri ?p1 ?o1
}
GROUP BY ?p1
ORDER BY DESC(?number)

/!\ cette requête de marche pas!


# Wikidata


