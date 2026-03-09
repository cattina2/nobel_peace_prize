# Documentation about SPARQL queries performed in Wikidata

### SPARQL endpoints

* Official SPARQL Endpoint: https://query.wikidata.org
* [QLever Wikidata](https://qlever.dev/wikidata)
  * This is an alternative Wikidata Query service which is much faster (because it uses the [QLever triplestore technology](https://github.com/ad-freiburg/qlever)) but the data is not necessarily up to date.

### Documentation about Queries in Wikidata

* [SPARQL query service/queries](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries): documentation.
* Library Carpentry Wikidata: [Introduction to querying](https://librarycarpentry.github.io/lc-wikidata/05-intro_to_querying.html#top)
* [Structure of statements in Wikidata](https://www.wikidata.org/wiki/Help:Statements)
* Query builder: https://query.wikidata.org/querybuilder/?uselang=en
* [Wikidata Dates](https://www.wikidata.org/wiki/Help:Dates)

### SPARQL Tutorial and Standard (with examples)

* [Wikidata SPARQL Tutorial](https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial)
* [W3C SPARQL Standard (and tutorial)](https://www.w3.org/TR/sparql11-query/)

## Insepction of available information

The objective of this stage of the workflow is to analyse the available information to answer our research questions about the population in our prosopography.

To begin with, we select a few people from the population chosen for the research - Nobel Peace Prize nominees - and inspect their entries in Wikidata. This allows us to identify the properties that will enable us to find the population and determine what information is available.

* [Fridtjof Nansen](https://www.wikidata.org/wiki/Q72292)
    * On this page or 'card' we can inspect the RDF triples available about this person in the Wikidata knowledge graph.
    * Carefullyl inspect the properties ‘employer’ and ‘position held’
         * Cf. the ['card' of the same person in DBpedia](https://dbpedia.org/page/Fridtjof_Nansen)

* Woodrow Wilson

### URI vs. URL

* URI → &lt;http://www.wikidata.org/entity/Q72292&gt;
* URL → https://www.wikidata.org/wiki/Q72292


## Querying Wikidata to find the population

For Nobel Peace Prize nominees, the following properties appear to be an effective way of identifying the population:

* [nominated for](https://m.wikidata.org/wiki/Property:P1411)
* [award received](https://m.wikidata.org/wiki/Property:P166)

Pour trouver une entité sur le Wikidara Query Service, presser la combinaison ctrl + espace derrière le wd. ou wdt.

### Number of persons with 'nominated for' and/or 'award received' as Nobel Peace Prize

Figures as of February 28, 2026

```
SELECT (COUNT(*) as ?eff)
WHERE {

    ## we use this clause in order to get only humans and not
    ## pages or any other entity wrongly associated to the occupation or field of work

    ?item wdt:P31 wd:Q5;  # Any instance of a human.
  
          wdt:P1411 wd:Q35637  # nominee 443

    # wdt:P166 wd:Q35637 # awarded 112

}  
```

### Combine 'nominated for' with 'award received'

We use here the **UNION** clause which allows to express an **OR** condition and merge two populations.

#### Nominated for the Nobel Peace Prize

555 as of February 28, 2026.

```
SELECT (COUNT(*) as ?eff)
WHERE {
    ?item wdt:P31 wd:Q5.
    {?item wdt:P1411 wd:Q35637}
    UNION
    {?item wdt:P166 wd:Q35637}    
}  
```

#### Awarded the Nobel Peace Prize

112 as of February 28, 2026.

?item représente une variable. Il s'agit du sujet, dans la forme sujet-prédicat-objet. 

```
SELECT (COUNT(*) as ?eff)
WHERE {
    ?item wdt:P31 wd:Q5.
    {?item wdt:P166 wd:Q35637}
    UNION
    {?item wdt:P1511 wd:Q35637}    
}
```

#### Both sub-populations

555 as of February 28, 2026.

But it is the sum of the two, so a perosn could appear more then once - and do appear more then once, as the people awarded the Nobel Prize were nominated for it.

```
SELECT (COUNT(*) as ?eff)
WHERE {
    ?item wdt:P31 wd:Q5.
    {?item wdt:P1411 wd:Q35637}
    UNION
    {?item wdt:P166 wd:Q35637}    
}  
```

## Actual number of people

504 as of March 02, 2026. 


```
SELECT (COUNT(*) as ?eff)
WHERE {
    ### subquery adding the distinct clause
    {
        SELECT DISTINCT ?item
        WHERE {
        ?item wdt:P31 wd:Q5;  # Any instance of a human.
        {?item wdt:P1411 wd:Q35637}
        UNION
        {?item wdt:P166 wd:Q35637}  
        }
    }
}
```

## Add a filter on the birth year

500 on March 2nd

```
SELECT (COUNT(*) as ?eff)
WHERE
    {
    ### subquery adding the distinct clause
        {
        SELECT DISTINCT ?item
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1990)# Any instance of a human.
           {?item wdt:P1411 wd:Q35637}
            UNION
            {?item wdt:P166 wd:Q35637}
            }
        }  
    }
```

## Inspect individuals

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?item ?itemLabel ?year
WHERE {
    {
  
          {?item wdt:P1411 wd:Q35637}
          UNION
          {?item wdt:P166 wd:Q35637}  
    }  
    ?item wdt:P31 wd:Q5;  # Any instance of a human.
            wdt:P569 ?birthDate.
  BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1981)
  
    ### Two ways of getting labels
    # SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }

    ## This is useful for query from external tool
    ?item rdfs:label ?itemLabel.
    FILTER(LANG(?itemLabel) = 'en')
    }  
LIMIT 100
```

## Count population with English labels

498 as of March 2nd 2026.

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT (COUNT(*) as ?eff)
WHERE
    {
    ### subquery adding the distinct clause
        {
        SELECT DISTINCT ?item ?itemLabel ?year
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1981)# Any instance of a human.
           {?item wdt:P1411 wd:Q35637}
          UNION
          {?item wdt:P166 wd:Q35637}  
        ?item rdfs:label ?itemLabel.
        FILTER(LANG(?itemLabel) = 'en')
            }
        }  
    } 
```

## Number of individuals without English label

3 as of March 2nd 2026.

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT (COUNT(*) as ?eff)
WHERE
    {
    ### sous requête qui ajoute la clause distinct
        {
        SELECT DISTINCT ?item ?itemLabel ?year
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1981)# Any instance of a human.
            {?item wdt:P1411 wd:Q35637}
            UNION
            {?item wdt:P166 wd:Q35637}  
        MINUS {?item rdfs:label ?itemLabel.
            FILTER(LANG(?itemLabel) = 'en')
            }
            }
        }  
    }

```
### Inspecter ces personnes

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?item ?year (group_concat(?iso_lang ; separator = ',') as ?langs) (max(?itemLabel) as ?maxLabel)
WHERE
    { 
       { SELECT DISTINCT ?item ?year
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1990)# Any instance of a human.
             {?item wdt:P1411 wd:Q35637}
            UNION
            {?item wdt:P166 wd:Q35637}  
        MINUS {?item rdfs:label ?itemLabel.
            FILTER(LANG(?itemLabel) = 'en')
            }
            }
        }  
  
        ?item rdfs:label ?itemLabel. 
		BIND(LANG(?itemLabel) as ?iso_lang)
  
       }
	   GROUP BY ?item ?year
	   ORDER BY ?item
	   LIMIT 100
```

## List of the available properties and their numbers

### Outgoing
Ce sont les plus intéressantes dans notre cas. 

Exécuter les requêtes plus volumineuses dans QLever!

Se référer à [cette page](proprietes_population.md) pour la liste de propriétés résultant de cette requête.

```
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?p ?propLabel ?eff ('' as ?notes)
WHERE {
{
    SELECT DISTINCT  ?p  (count(*) as ?eff)
    WHERE {
        ?item wdt:P31 wd:Q5; 
             wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1981)# Any instance of a human.
            {?item wdt:P1411 wd:Q35637}
            UNION
            {?item wdt:P166 wd:Q35637}.
			?item ?p ?o.
        }
		GROUP BY ?p
    }

    ## we need this construct to get the label of the property
    ## properties are also entities in Wikidata,
    ## but only in the entities' namespace

    ?prop wikibase:directClaim ?p .
    ?prop rdfs:label ?propLabel.
    FILTER(LANG(?propLabel) = 'en')
    }  
ORDER BY DESC(?eff) 
```

### Incoming 

The relevant incoming properties can be found [here](proprietes_entrantes_population.md)

```
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX wikibase: <http://wikiba.se/ontology#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?p ?propLabel ?eff  ('' as ?notes)
WHERE {
{
    SELECT DISTINCT  ?p  (count(*) as ?eff)
    WHERE {
        ?item wdt:P31 wd:Q5; 
             wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 1981)# Any instance of a human.
            {?item wdt:P1411 wd:Q35637}
            UNION
            {?item wdt:P166 wd:Q35637}.

            ## inversed triple
			?s ?p ?item.
        }
		GROUP BY ?p
    }
    ?prop wikibase:directClaim ?p .

    ?prop rdfs:label ?propLabel.
        FILTER(LANG(?propLabel) = 'en')
    }  
ORDER BY DESC(?eff) 
```


