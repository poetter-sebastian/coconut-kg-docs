# Querying COCONUT[KG]

COCONUT[KG] allows a variety of queries on a large number of natural products.
You can query the COCONUT[KG] online with a SPARQL endpoint.


## SPARQL endpoint

At [http://coconut-kg.aksw.org/sparql](http://coconut-kg.aksw.org/sparql) you'll find a SPARQL endpoint.
Both back-end and front-end are provided by OpenLink Virtuoso.
The back-end serves a SPARQL engine.
In the front-end we find a HTTP/SPARQL server with nginx overlay.

*NOTE: Before using the SPARQL endpoint we recommend to read this documentation first*


## SPARQL endpoint details

### Endpoint type

The SPARQL enpoint is provided by the [OpenLink Software Virtuoso](https://virtuoso.openlinksw.com)


### Rates and limitations

You can make a limited number of connections. The settings can be seen below:

            ResultSetMaxRows           = 10000
            MaxQueryExecutionTime      =   600  (seconds)
            MaxQueryCostEstimationTime =   400  (seconds)
            Connection limit           =    10  (parallel connections per IP address

**ATTENTION**: *The result size is currently limited to 1000 rows. This way partial results are displayed as complete ones and there is no HTTP error.*

## SPARQL example queries
The following query gives you formula, name, weight	and smile of a compound:
```
PREFIX coco:  <http://coconut-kg.aksw.org/ontology#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

SELECT DISTINCT ?formula, ?name, ?weight, ?smile WHERE {
  ?compound a coco:Compound . 
  ?compound coco:molecularFormula ?formula .
  ?compound coco:name ?name .

  ?compound coco:hasMolecularProperties ?properties .
  ?properties coco:molecularWeight ?weight .

  ?compound coco:isIdentifiedBy ?unique .
  ?unique coco:smiles ?smile .
} 
LIMIT 10
```
The following query gives you formula and weight of a compound where the weight is between 320.00 and 320.20:
```
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

SELECT DISTINCT ?formula ?weight  WHERE {
  ?compound a coco:Compound . 
  ?compound coco:molecularFormula ?formula .
  ?compound coco:name ?name .

  ?compound coco:hasMolecularProperties ?properties .
  ?properties coco:molecularWeight ?weight .

  ?compound coco:isIdentifiedBy ?unique .
  ?unique coco:smiles ?smile .

FILTER (?weight > 320.00 && ?weight < 320.20) .
} 
```
