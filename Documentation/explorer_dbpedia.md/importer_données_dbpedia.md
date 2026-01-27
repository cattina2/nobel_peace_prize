# Explorer DBpedia

### Population de nominés au prix Nobel de la paix:
https://en.wikipedia.org/wiki/List_of_individuals_nominated_for_the_Nobel_Peace_Prize_(1900%E2%80%931999)

### Interface pour requêtes SPARQL 
https://dbpedia.org/sparql

## Requête sur le serveur SPARQL de DBPedia

 PREFIX dbr: <http://dbpedia.org/resource/>
    PREFIX dbo: <http://dbpedia.org/ontology/>
    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

    SELECT DISTINCT ?person_uri (STR(?label) AS ?persName) ?birthYear
    WHERE { 
        dbr:List_of_individuals_nominated_for_the_Nobel_Peace_Prize ?p ?person_uri.
        ?person_uri a dbo:Person;
                dbo:birthDate ?birthDate;
                rdfs:label ?label.
        BIND(xsd:integer(SUBSTR(STR(?birthDate), 1, 4)) AS ?birthYear)
        FILTER ( ?birthYear > 1770
                && LANG(?label) = 'en')
    }
    ORDER BY ?birthYear


## Un nominé au prix Nobel de la paix
Données non-structurées et semi-structurées. Exemple utilisée pour les requêtes à venir.

https://en.wikipedia.org/wiki/Henry_Dunant


