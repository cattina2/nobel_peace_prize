# Inspection des objets associés

## Occupation

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?object ?objectLabel (count(*) as ?eff)
WHERE
    {
      {?item wdt:P1411 wd:Q35637}
            UNION
            {?item wdt:P166 wd:Q35637}
      
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1981)# Any instance of a human.
              
          
          ?item wdt:P106 ?object.
       
        ?object rdfs:label ?objectLabel.
        FILTER(LANG(?objectLabel) = 'en')
}  
GROUP BY ?object ?objectLabel 
ORDER BY DESC(?eff)
```

### Occupations les plus fréquentes



## Employeurs

Propriété P108 pour employeur. Permet de donner les organisations ayant employé des membres de ma population.



## Position occupée

