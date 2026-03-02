##Exercices du 23.02.2026

URI d'une personne nominée au choix sur Wikidata: <https://www.wikidata.org/wiki/Q193009>

Regarder quelle est la propriété qui associe les personnes à notre objet: ici, qui a été marié à la personne choisie, avec le nom de l'époux en anglais:

PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?spouse ?spouseLabel 
WHERE {
   wd:Q193009 wdt:P26 ?spouse.
   ?spouse rdfs:label ?spouseLabel.
   FILTER (LANG(?spouseLabel) = "en")
}


