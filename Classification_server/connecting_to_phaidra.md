# Connecting Phaidra to the Classification Server 

Phaidra will require the Classification Server when the user ingests new items and wants to add metadata to this from a controlled vocabulary, as well as when the user searches for some documents classified with some metadata from a controlled vocabulary and wants to display or resolve them.

The connection between Phaidra and the Classification Server will be realised using the REST API of Skosmos and/or wit the REST-style SPARQL Queries of Jena Fuseki. These are read-only interfaces over HTTP to the data stored in the Classification Server, where requests can be built in the URL. The returned data is in UTF-8 encoded JSON-LD format.

## REST API of Skosmos

Skosmos provides a REST-style API and Linked Data access to the underlying vocabulary data. The REST API is a read-only interface to the data stored in the Classification Server. 

The REST URLs must begin with ```/rest/v1``` prefix. Most of the methods return the data as UTF-8 encoded JSON-LD, served using the application/json MIME type. The data consists of a single JSON object which includes JSON-LD context information (in the @context field) and one or more fields which contain the actual data. 

E.g.:

Request:

```
http://skosmos.phaidra.org/rest/v1/oefos02/label?uri=http://phaidra.univien.ac.at/voc_9/1331&lang=en
```

Answer:

```
{"@context":{"skos":"http:\/\/www.w3.org\/2004\/02\/skos\/core#","uri":"@id","type":"@type","prefLabel":"skos:prefLabel", "@language":"en"}, "uri":"http:\/\/phaidra.univien.ac.at\/voc_9\/1331", "prefLabel":"Accounting"}
```

## REST-style SPARQL Query of Jena Fuseki

Fuseki provides REST-style SPARQL HTTP Update, SPARQL Query, and SPARQL Update using the SPARQL protocol over HTTP.

E.g.:

SPARQL Query:

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#> 
SELECT ?prefLabel
WHERE {
<http://phaidra.univien.ac.at/voc_9/1331> skos:prefLabel ?prefLabel
FILTER ( lang(?prefLabel) = "en" ) }
```

The SPARQL Query must be URL-encoded like this:
```
http://fuseki.phaidra.org:3030/ds/sparql?query=PREFIX+skos%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2004%2F02%2Fskos%2Fcore%23%3E+%0D%0ASELECT+%3FprefLabel%0D%0AWHERE+%7B%0D%0A%3Chttp%3A%2F%2Fphaidra.univien.ac.at%2Fvoc_9%2F1331%3E+skos%3AprefLabel+%3FprefLabel%0D%0AFILTER+%28+lang%28%3FprefLabel%29+%3D+%22en%22+%29+%7D%0D%0A

```

Then the answer from the Fuseki server can be like this:
```
{ "head": { "vars": [ "prefLabel" ] } , "results": { "bindings": [ { "prefLabel": { "type": "literal" , "xml:lang": "en" , "value": "Accounting" } } ] } }
```