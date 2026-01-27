# Importer des données de DBPedia

### Population de nominés au prix Nobel de la paix:
https://en.wikipedia.org/wiki/List_of_individuals_nominated_for_the_Nobel_Peace_Prize_(1900%E2%80%931999)

### Interface pour requêtes SPARQL 
https://dbpedia.org/sparql

## Requête sur le serveur SPARQL de DBPedia, importer des personnes

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


Une fois ces données importées en CSV et placées dans la base de données DBeaver au sein de la table dbp_liste_personnes, nécessité de vider la table person ainsi que de remettre sa séquence à 0 avec les requêtes SQLite suivantes:

   DELETE FROM "person" ;

    UPDATE SQLITE_SEQUENCE
    SET seq = 0
    WHERE name ='person';

Ceci fait, il est maintenant nécessaire de contrôler la liste de personnes importées pour la présence de doublons grâce à la requête suivante:

    SELECT person_uri
    FROM dbp_liste_personnes lp 
    GROUP BY person_uri
    HAVING COUNT(*) > 1 ;

Dans le cas de mon projet, les personnes suivantes semblent présenter des doublons:
- George Saint-Paul
- Gregers Gram
- Harry Willis Miller

Nécessité donc de grouper ces doublons avec de n'avoir qu'une seule ligne par URI avec la commande suivante:

SELECT max(birthYear), person_uri, max(persname),
            FROM dbp_liste_personnes lp
            GROUP BY person_uri;


Une fois ceci fait, il est possible d'importer les données de la table dbp_liste_personnes vers la table person:

    INSERT INTO person (birth_year, dbpedia_uri, name, import_metadata)
        SELECT birthYear, person_uri, persname, "Importé le 27 janvier 2026 depuis le résultat d'une requête SPARQL sur DBPedia, cf. astronomers/DBpedia_importer_dans_base_personnelle"
        FROM dbp_liste_personnes lp ;  
 

Et les données pour la table person ont été importées.


