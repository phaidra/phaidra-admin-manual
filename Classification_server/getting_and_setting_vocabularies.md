# Getting and setting vocabularies

(source: https://redmine.phaidra.org/redmine/projects/labtech/wiki/Skosmos#vocabularies)

## Getting vocabularies (if you want to use certain vocabularies locally)

(e.g. Putting "COAR - Resource Type Vocabulary" or "Getty TGN vocabulary" into Skosmos)

1. Create a new directory called vocabularies in Skosmos folder (c:\xampp\htdocs\skosmos)
2. Open a browser and go to the vocabulary that you want to use (e.g. COAR - Resource Type Vocabulary or Getty TGN voabulary
3. Download (save) the vocabulary as an RDF file (in XML, Turtle, or N-Triples syntax) (e.g. resource_types.xml or tgn_7011179.rdf) into c:\xampp\htdocs\skosmos\vocabularies

## Skosify the downloaded vocabularies (optional)

(Check if the downloaded files (e.g. resource_types.xml or tgn_7011179.rdf) is in correct SKOS format using the Skosify tool.)

Online version of the Skosify tool used to be available here: https://code.google.com/p/skosify/, and use as follows
1. Select the vocabulary to be checked as input
2. Leave the default options as they are
3. Click on the Process button
After successful process you will get a Processed vocabulary that you can download and rename (e.g. as checked_resource_types.xml or checked_tgn_7011179.rdf) into the folder c:\xampp\htdocs\skosmos\vocabularies
You can find a recent versions of the Skosify tool here 
Unfortunately there is currently no recent online version available. 
Skosify requires Python (2.x or 3.x) and the rdflib library. It should run fine on Windows after those installed.


## Setting vocabularies

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
* The uriSpace setting is not crucial for Skosmos (???). When you represent your data as RDF, the best practice is to coin a new URI namespace for your data set. Then use that as the value of the uriSpace setting. 

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

## Individual vocabularies

(to be aligned)

### Getty

They have the "explicit" set and the "full" set (aka Total Exports). With the "explicit" set, which is smaller, you will need  to configure Fuseki to use inference so that the data store can infer the missing triples. With the full set this is not needed, but in turn the data set is much larger so you may have difficulties loading it (I wouldn't try loading that through Fuseki, but it could work with tdbloader as I explained in a previous message). 

full set to download:
http://vocab.getty.edu/doc/#Total_Exports
This file includes all statements (explicit and inferred) of all independent entities.  It's a concatenation of the Per-Entity Exports in NTriples format. Because it includes all required Inference, you can load it to any repository (even one without RDFS reasoning):
1.      Load the External Ontologies (SKOS, SKOS-XL, ISO 25964): links are provided in that section. The purpose is to get descriptions of properties, associative relations, etc.
2.      Load the GVP Ontology from http://vocab.getty.edu/ontology.rdf
3.      If your repository supports Subproperty/Inverse/Transitive reasoning:
·        If you want to eliminate the struck-out properties in Reduced SKOS Inference (like we do), execute the SPARQL updates given in Reduced SKOS Inference.
·        If you don't, these properties will be inferred, which will add 2-3x more statements to your repository.
4.      Load the export files:
·        http://vocab.getty.edu/dataset/aat/full.zip: 120 Mb zipped, 1871 Mb unzipped
·        http://vocab.getty.edu/dataset/tgn/full.zip: 340 Mb zipped, 5300 Mb unzipped
·        http://vocab.getty.edu/dataset/ulan/full.zip: TODO Mb zipped, TODO Mb unzipped
AAT
TGN
ULAN
AATOut_Full.nt     : subjects
AATOut_Contribs.nt : contribs
AATOut_Sources.nt  : sources
TGNOut_Full.nt     : subj, places
TGNOut_Contribs.nt : contribs
TGNOut_Sources.nt  : sources
ULANOut_Full.nt     : subj, agents
ULANOut_Contribs.nt : contribs
ULANOut_Sources.nt  : sources
·        Since the *Out_Full.nt files are very large, they may require some special data loading tool for your repository.

1) set up inferencing or 
With the "explicit" set, which is smaller, you will need  to configure Fuseki to use inference so that the data store can infer the missing triples. As for configuring inference in Fuseki, you will have to do some research (a search for "fuseki inference" at least gets you some hits). There is some documentation ( https://jena.apache.org/documentation/assembler/assembler-howto.html ) about configuring inference in Jena assembler files (such as the Fuseki configuration file). If that doesn't help, you can asks on the Jena users' mailing list or on forums such as StackOverflow
2) use the "full" dataset with tdbloader. 
tdbloader is part of the Jena distribution which you will need to download separately. A tutorial for using tdbloader with the text index is here: 
https://jena.apache.org/documentation/query/text-query.html#building-a-text-index 
(Jena Assemblers are recipes (written in RDF) for constructing RDF stores:

https://jena.apache.org/documentation/assembler/index.html
https://jena.apache.org/documentation/assembler/assembler-howto.html

You will use a set of RDF vocabularies to write RDF describing how you want TDB to ingest your data. That will be your assembler file.)
Building a TDBB dataset and Text Index
The index and the dataset can be built using command line tools in two steps: first load the RDF data, second create an index from the existing RDF dataset.

Step 1 - Building a TDB dataset
Build the TDB dataset:
java -cp $FUSEKI_HOME/fuseki-server.jar tdb.tdbloader --tdb=assembler_file data_file
java -cp /fuseki-server.jar tdb.tdbloader --tdb=jena-text-config.ttl --graph=http://vocab.getty.edu/aat/  ./vocabularies/getty_aat/full/ontology.rdf
using the copy of TDB included with Fuseki.
You can use the same .ttl file  ((/var/www/skosmos/jena-fuseki1-1.3.0/jena-text-config.ttl))
that you used for the Fuseki database and text index configuration, i.e. the one documented here:
https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText#configuration
Alternatively, use one of the TDB utilities tdbloader or tdbloader2 which are better for bulk loading:
$JENA_HOME/bin/tdbloader --loc=directory  data_file
(((Has been Jena already installed separately on phaidraentwl ? ))) 
::
Step 2 - Build the Text Index
You can then build the text index with the jena.textindexer tool:

java -cp $FUSEKI_HOME/fuseki-server.jar jena.textindexer --desc=assembler_file
(java -cp ./fuseki-server.jar jena.textindexer --desc=jena-text-config.ttl)
Because a Fuseki assembler description can have several datasets descriptions, and several text indexes, it may be necessary to extract a single dataset and index description into a separate assembler file for use in loading.

Updating the index

If you allow updates to the dataset through Fuseki, the configured index will automatically be updated on every modification. This means that you do not have to run the above mentioned jena.textindexer after updates, only when you want to rebuild the index from scratch.

#### Problems with Getty

The Getty SPARQL endpoint is not responding in the right way. There seems to be some incompatibility between Skosmos (in practice, the EasyRdf library which is used to perform SPARQL queries) and the Getty SPARQL endpoint. 

It is better to set up your own Fuseki SPARQL endpoint, with the jena-text index. Even if you could access the Getty SPARQL endpoint, it would most likely be extremely slow to use it with Skosmos, since it doesn't have a text index that Skosmos could use.

I think it's unlikely that the Getty vocabularies would work well in Skosmos due to their very large size. 

### COAR Resource Type Vocabulary

### ÖFOS

ÖFOS - 2012

You need to represent your classification using the SKOS data model. For some guidance, see 
the SKOS Primer [1] and introductory articles [2,3]. An excellent general introduction to RDF and Linked Data is given for example in the Linked Data book [4] available for free online. For specifics on what aspects of SKOS and other RDF vocabularies Skosmos supports, see the wiki page Data Model [5]. 
[1] https://www.w3.org/TR/skos-primer/ 
[2] http://www.dataversity.net/introduction-to-skos/ 
[3] http://www.ala.org/alcts/resources/z687/skos 
[4] http://linkeddatabook.com/ 
[5] https://github.com/NatLibFi/Skosmos/wiki/Data-Model 

### Examples

(to be deleted)

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