# Getting and setting vocabularies


## 1. Getting vocabularies (if you want to use certain vocabularies locally)

(e.g. Putting "COAR - Resource Type Vocabulary" or "Getty TGN vocabulary" into Skosmos)

1. Create a new directory called vocabularies in Skosmos folder (c:\xampp\htdocs\skosmos)
2. Open a browser and go to the vocabulary that you want to use (e.g. COAR - Resource Type Vocabulary or Getty TGN voabulary
3. Download (save) the vocabulary as an RDF file (in XML, Turtle, or N-Triples syntax) (e.g. resource_types.xml or tgn_7011179.rdf) into c:\xampp\htdocs\skosmos\vocabularies

## 2. Skosify the downloaded vocabularies (optional)

(Check if the downloaded files (e.g. resource_types.xml or tgn_7011179.rdf) is in correct SKOS format using the Skosify tool.)

Online version of the Skosify tool used to be available here: https://code.google.com/p/skosify/, and use as follows
1. Select the vocabulary to be checked as input
2. Leave the default options as they are
3. Click on the Process button
After successful process you will get a Processed vocabulary that you can download and rename (e.g. as checked_resource_types.xml or checked_tgn_7011179.rdf) into the folder c:\xampp\htdocs\skosmos\vocabularies
You can find a recent versions of the Skosify tool here 
Unfortunately there is currently no recent online version available. 
Skosify requires Python (2.x or 3.x) and the rdflib library. It should run fine on Windows after those installed.


## 3. Setting vocabularies

(The vocabularies to show in Skosmos are configured in the file `vocabularies.ttl` which is an RDF file in Turtle syntax.)

1. create a vocabularies.ttl file in the Skosmos directory (c:\xampp\htdocs\skosmos). There are example files named vocabularies.ttl.dist that you can rename/copy to vovabularies.ttl as starting point. In this file
2. create a new session starting with the line: 
:id a skosmos:Vocabulary, void:Dataset ;
where id is just an identifier. It will be used in the URL after /skosmos/.
3. set the following required parameters:


* the title of the vocabulary in different languages:
dc:title " title_of_the_vocabulary "@ language ; 
where language can be: en, fi, sv, ?
* the category of the vocabulary:
dc:subject : category ;
where category can be: cat_general
* the URI namespace for vocabulary objects (these my not overlap):
void:uriSpace " URI_namespace ";
* the language(s) that the vocabulary supports:
skosmos:language " language ", " language ", ...
where language can be: en, fi, sv, ?
* the default language of the vocabulary, if the vocabulary supports multiple languages:
skosmos:defaultLanguage " language " 
where language can be: en, fi, sv, ?
* the URI of the SPARQL endpoint containing this vocabulary 
void:sparqlEndpoint < URI_SPARQL_endpoint > ;
URI_SPARQL_endpoint can be http://localhost:3030/ if you want to use the vocabulary locally, or the URL of the SPARQL endpoint of the remote vocabulary


You can set the following optional parameters:
* if you want to enable the Hierarchy tab showing top-level concepts on the vocabulary home page:
skosmos:showTopConcepts "true";
* if you do not want Skosmos to query the mapping concept URIs for labels if they haven't been found at the configured SPARQL endpoint:
skosmos:loadExternalResources "false";
* class of resources to display as concept groups (typical values are skos:Collection or isothes:ConceptGroup):
skosmos:groupClass isothes:ConceptGroup ;
* class of resources to display as arrays (subdivisions) of sibling concepts (typical values are skos:Collection or isothes:ThesaurusArray):
skosmos:arrayClass isothes:ThesaurusArray ;
* URI of the main skos:ConceptScheme instance of this vocabulary. Should be specified if the vocabulary contains multiple skos:ConceptScheme instances.
skosmos:mainConceptScheme < _main_Concept_Scheme_URI_ > .
* if the vocabulary is relatively small (eg. 100 concepts) you can show the alphabetical index with all the concepts instead of showing only the concepts of one letter at a time.
skosmos:fullAlphabeticalIndex "true"

### Examples

#### For "COAR-Resource Type Vocabulary" set the properties as follow:

dc:title "COAR - Resource Type Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://purl.org/coar/resource_type/";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/ds/sparql> ;
.

#### For "getty TGN vocabulary" set the properties as follow:

dc:title "getty TGN Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://vocab.getty.edu/tgn/7011179";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/> ;
.

(See more details of configuring the vocabularies.ttl here: https://github.com/NatLibFi/Skosmos/wiki/Vocabularies )