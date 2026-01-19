# Explorer DBpedia

### Population de nominés au prix Nobel de la paix:
https://en.wikipedia.org/wiki/List_of_individuals_nominated_for_the_Nobel_Peace_Prize_(1900%E2%80%931999)

### Interface pour requêtes SPARQL 
https://dbpedia.org/sparql


## Un nominé au prix Nobel de la paix
Données non-structurées et semi-structurées. Exemple utilisée pour les requêtes à venir.

https://en.wikipedia.org/wiki/Henry_Dunant

### Récupérer les triplets en SPARQL


SELECT *
    WHERE {
      <http://dbpedia.org/resource/Henry_Dunant> ?p ?o
    }

