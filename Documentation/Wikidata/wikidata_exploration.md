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



