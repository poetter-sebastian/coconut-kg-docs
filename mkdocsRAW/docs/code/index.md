```python

from SPARQLWrapper import SPARQLWrapper, JSON

sparql = SPARQLWrapper(
    "http://coconut-kg.aksw.org/sparql"
)
sparql.setReturnFormat(JSON)

sparql.setQuery(
    """
    PREFIX coco:  <http://coconut-kg.aksw.org/ontology#>
    PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    PREFIX owl:  <http://www.w3.org/2002/07/owl#>
    PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

    SELECT DISTINCT ?formula ?name ?weight ?smile WHERE {
        ?compound a coco:Compound .
        ?compound coco:molecularFormula ?formula .
        ?compound coco:name ?name .

        ?compound coco:hasDescriptors ?descriptor .
        ?descriptor coco:isMolecular ?mdescriptor .
        ?mdescriptor coco:molecularWeight ?weight .

        ?compound coco:isIdentifiedBy ?unique .
        ?unique coco:smiles ?smile .}

    LIMIT 10
    """
)

try:
    ret = sparql.queryAndConvert()

    for r in ret["results"]["bindings"]:
        print(r)
except Exception as e:
    print(e)

```