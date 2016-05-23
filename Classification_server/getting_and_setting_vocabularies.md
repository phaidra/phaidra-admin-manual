# Getting and setting vocabularies

The basic usage of our Classification Server is to store the classifications locally (if its access time is acceptable), and we also provide the links to the remote SPARQL endpoints of the classifications, if they are available.
If you want to use certain vocabularies locally, you have to get it in the right format, and upload it to the local SPARQL server, that is to Jena Fuseki.

## Downloading and converting vocabularies 

Vocabularies can be downloaded from the original dataset provider (e.g. from Getty, COAR, Statistics Austria, etc.), or in case of a small dataset, it can be created manually. The vocabulary needs to be expressed using SKOS Core representation in order to publish it via Skosmos, but SKOS-XL representation or even files in Excel can be also easily converted to SKOS Core. 

### Downloading vocabularies in Windows

(source: https://redmine.phaidra.org/redmine/projects/labtech/wiki/Skosmos#vocabularies)

1. Create a new directory called vocabularies in Skosmos folder (c:\xampp\htdocs\skosmos)
2. Open a browser and go to the vocabulary that you want to use (e.g. COAR - Resource Type Vocabulary or Getty TGN voabulary
3. Download (save) the vocabulary as an RDF file (in XML, Turtle, or N-Triples syntax) (e.g. resource_types.xml or tgn_7011179.rdf) into c:\xampp\htdocs\skosmos\vocabularies

### Converting SKOS-XL to plain SKOS Core fomat using owlart

For the SKOS-XL to SKOS Core conversion you can use for example the owlart (https://bitbucket.org/art-uniroma2/owlart/downloads) converter.

1. Download library 'owlart-2.3-dist.zip' from https://bitbucket.org/art-uniroma2/owlart/downloads

2. copy inputfile e.g. 'skosxl.xml' into workbench/rdf_files
3. create in 'workbench/output' directory output file e.g. 'resultSkos.rdf'
4. make config file in workbench/config dir e.g. workbench/config/skosxl2skos.config and set 'baseuri', 'namespace', and 'defaultScheme' according to your project settings.
Here is an example of the config file:
<code>
###ModelFactory Implementation: required
modelFactoryImplClassName=it.uniroma2.art.owlart.sesame2impl.factory.ARTModelFactorySesame2Impl

###modelConfigClass Implementation: not required
modelConfigClass=it.uniroma2.art.owlart.sesame2impl.models.conf.Sesame2NonPersistentInMemoryModelConfiguration

###modelConfigFile : not required in Sesame2, though we have to disable inference, which is set to true by default
modelConfigFile=workbench/config/model_sesame-noinference.config

modelDataDir=workbench/storage

baseuri=http\://purl.org/coar/resource_type/
namespace=http\://phaidra.org/
defaultScheme=http\://phaidra.org/
</code>
5. run library with dependencies in project root directory(dir above workbench directory):
5.1. windows:

java -classpath ".;dependency\commons-codec-1.2.jar;dependency\commons-httpclient-3.1.jar;dependency\commons-io-2.4.jar;dependency\commons-logging-1.0.4.jar;dependency\guava-15.0.jar;dependency\jackson-core-2.4.0.jar;dependency\log4j-1.2.16.jar;dependency\opencsv-2.0.jar;dependency\owlart-api-2.3.jar;dependency\owlart-sesame2impl-1.3.jar;dependency\sesame-onejar-2.7.10.jar;dependency\slf4j-api-1.6.1.jar;dependency\slf4j-log4j12-1.6.1.jar" it.uniroma2.art.owlart.utilities.transform.SKOSXL2SKOSConverter workbench\config\skosxl2skos.config workbench\rdf_files\skosxl.xml workbench\output\resultSkos.rdf false

5.2. linux:

java -classpath ".:dependency/commons-codec-1.2.jar:dependency/commons-httpclient-3.1.jar:dependency/commons-io-2.4.jar:dependency/commons-logging-1.0.4.jar:dependency/guava-15.0.jar:dependency/jackson-core-2.4.0.jar:dependency/log4j-1.2.16.jar:dependency/opencsv-2.0.jar:dependency/owlart-api-2.3.jar:dependency/owlart-sesame2impl-1.3.jar:dependency/sesame-onejar-2.7.10.jar:dependency/slf4j-api-1.6.1.jar:dependency/slf4j-log4j12-1.6.1.jar" it.uniroma2.art.owlart.utilities.transform.SKOSXL2SKOSConverter workbench/config/skosxl2skos.config workbench/rdf_files/skosxl.xml workbench/output/resultSkos.rdf false

6. Result skos file is in workbench/output/resultSkos.rdf
7. tested in java version "1.8.0_72"

### Converting SKOS-XL to plain SKOS Core fomat using SPARQL Update query

You can also convert SKOS-XL labels to SKOS Core labels by executing this SPARQL Update query:

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX skos:   <http://www.w3.org/2004/02/skos/core#> 
PREFIX skosxl: <http://www.w3.org/2008/05/skos-xl#> 
INSERT { 
   ?c skos:prefLabel ?pref . 
   ?c skos:altLabel ?alt . 
   ?c skos:hiddenLabel ?hidden . 
   ?c skos:definition ?def . 
} 
WHERE { 
   { ?c skosxl:prefLabel/skosxl:literalForm ?pref } 
   UNION 
   { ?c skosxl:altLabel/skosxl:literalForm ?alt } 
   UNION 
   { ?c skosxl:hiddenLabel/skosxl:literalForm ?hidden } 
  UNION 
   { ?c skos:definition/rdf:value ?def }  
 } 
 ```

### Skosify the downloaded vocabularies (optional)

When the SKOS file was downloaded from external resources or it has been converted from other formats, it is recommended that you pre-process your vocabularies using a SKOS proofing tool, like Skosify . This will ensure, e.g., that the broader/narrower relations work in both directions, and that related relationships are symmetric. Skosify will report and try to correct lots of potential problems in SKOS vocabularies. It can also be used to convert non-SKOS RDF data into SKOS. 

An online version of the Skosify tool used to be available here: https://code.google.com/p/skosify/, and use as follows
1. Select the vocabulary to be checked as input
2. Leave the default options as they are
3. Click on the Process button
After successful conversion process you will get a processed vocabulary that you can download and rename (e.g. as checked_resource_types.xml) into a local folder.

The offline Skosify tool requires Python (2.x or 3.x) and the rdflib library. It should run fine even on Windows after installing them.

## Uploading files to Fuseki

If you want to use Skosmos for accessing classifications in your local SPARQL triple store you have to upload the datasets to Fuseki. First, you have to consider if Fuseki will run either in memory or in a predefined folder, usually called TDB. 
You also have to consider that in a SPARQL triple store there is always a default (unnamed) graph, and there can also be multiple named graphs. 
You can upload datasets to Fuseki online, when Fuseki is running, or offline, when Fuseki is not running.

### Running Fuseki in the memory

If you run Fuseki in memory, then all uploads and updates (if you allow that) will be temporary.

### Runing Fuseki in the TDB

If you run Fuseki in the TDB, then the uploads and updates will remain there even if we exit from Fuseki and restart it.

### Named and default graph in SPARQL triple store

In a SPARQL triple store there is a default (unnamed) graph, and there can be multiple named graphs. In other words, there is only one default graph (with no name), but there can be any number of named graphs on a SPARQL endpoint/dataset. The URI namespaces can be used as graph names. E.g. http://vocab.getty.edu/tgn/ would store Getty's TGN data.



### Uploading files online

To upload online you can use the control panel of the web interface of Fuseki or you can use command line instructions. 

When uploading datasets online to Fuseki through its control panel, you can set the Graph to “default” or provide a graph name.


If you use a graph name when uploading a dataset to Fuseki, you have to make sure of giving the same graph name to this dataset in skosmos:sparqlGraph (e.g. http://vocab.getty.edu/tgn/) by setting Skomos vocabularies in vocabularies.ttl.

### Using the web interface of Fuseki

1. start Fuseki server (see [Using Skosmos with Fuseki](Classification_server/using_skosmos_with_fuseki.md))
2. open Fuseki's web interface on [http://skosmos.phaidra.org:3030/](http://skosmos.phaidra.org:3030/)
3. Select Server Management / Control Panel
4. Select Dataset: /ds
5. File upload / Choose Files

#### To the default graph

1. Graph: default
2. Upload

#### To a named graph

1. Graph: *write here the graph name according to the skosmos:sparqlGraph parameter in vocabularies.ttl*
2. Upload

### From the command line in Fuseki's folder on Linux

#### On-line 

1. Start Fuseki server (with text index) from its directory:
  * **./fuseki-server --config jena-text-config.ttl**
2. use the command line utility s-put or s-post, that comes with Fuseki:
  * use **s-put** if you want to add single data file:
    * ./s-put *SPARQL-server* *Graph-name* *data-to-be-uploaded*
    * e.g.: ./s-put http://localhost:3030/ds/data http://skos.um.es/unescothes/ unescothes.ttl 

    Note: s-put does - it clears the graph first. It may happen, that you overwrite the previous data when loading a new file.

  * use **s-post** if you want to add new data files to existing data without clearing it:
    * ./s-post *SPARQL-server* *Graph-name* *data-to-be-uploaded*
    * e.g.: ./s-post http://localhost:3030/ds/data http://vocab.getty.edu/aat/ ./vocabularies/skos.rdf

### Uploading files offline

To upload offline you can upload datasets directly to the TDB. 

*** itt tartok ***
#### Off-line 

(when Fuseki is not running)
  
If there are memory problems by uploading (several, large) files to Fuseki, it is worth to use offline loading up the data. You can do it with tdbloader, which is part of the Jena distribution which you will need to download separately. A tutorial for using tdbloader with the text index is here: 
https://jena.apache.org/documentation/query/text-query.html#building-a-text-index 
Jena Assemblers are recipes (written in RDF) for constructing RDF stores:
https://jena.apache.org/documentation/assembler/index.html
https://jena.apache.org/documentation/assembler/assembler-howto.html

You will use a set of RDF vocabularies to write RDF describing how you want TDB to ingest your data. That will be your assembler file.

**Building a TDBB dataset and Text Index**

The index and the dataset can be built using command line tools in two steps: first load the RDF data, second create an index from the existing RDF dataset.

*Step 1 - Building a TDB dataset*

Build the TDB dataset:
**java -cp $FUSEKI_HOME/fuseki-server.jar tdb.tdbloader --tdb=assembler_file data_file**

using the copy of TDB included with Fuseki.
You can use the same .ttl file (/var/www/skosmos/jena-fuseki1-1.3.0/jena-text-config.ttl)
that you used for the Fuseki database and text index configuration, i.e. the one documented here:
https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText#configuration
Alternatively, use one of the TDB utilities tdbloader or tdbloader2 which are better for bulk loading:
$JENA_HOME/bin/tdbloader --loc=directory  data_file

*Step 2 - Build the Text Index*

You can then build the text index with the jena.textindexer tool:

**java -cp ./fuseki-server.jar jena.textindexer --desc=jena-text-config.ttl**

(java -cp $FUSEKI_HOME/fuseki-server.jar jena.textindexer --desc=assembler_file)

Because a Fuseki assembler description can have several datasets descriptions, and several text indexes, it may be necessary to extract a single dataset and index description into a separate assembler file for use in loading.

*Updating the index*

If you allow updates to the dataset through Fuseki, the configured index will automatically be updated on every modification. This means that you do not have to run the above mentioned jena.textindexer after updates, only when you want to rebuild the index from scratch.

Using the tbdloader you have to shut down Fuseki (since only one process can use the TDB at the same time) and 

1. <a name="tbdloader"></a>Using the tdbloader command line utilities in Fuseki's folder to create the TDB and load the RDF data.  
  **$java -cp ./fuseki-server.jar tdb.tdbloader --tdb=jena-text-config.ttl --graph=http://vocab.getty.edu/aat/ ./vocabularies/ontology.rdf**
  where 
    * jena-text-config.ttl is a configuration file (to be described)
    *  --graph=http://vocab.getty.edu/aat/ is the named graph according to the skosmos:sparqlGraph parameter in vocabularies.ttl

2. then you will still need to generate the text index as a separate step. A short tutorial of this is included in the jena-text documentation

  https://jena.apache.org/documentation/query/text-query.html#building-a-text-index 


### Adding vocabularies to Fuseki server on Windows

In order to add vocabularies to the Fuseki server use the control panel of the Fuseki server from a browser: http://localhost:3030/

1. click on the "add data" button in the middle
2. on the next page (Upload files) click on the "+ select files" button in the middle
3. select the vocabulary (e.g. checked_resource_types.xml or checked_tgn_7011179.rdf) file from the c:\xampp\htdocs\skosmos\vocabularies dictionary
4. click on the "upload now" button


## Checking data in Fuseki server

These SPARQL queries that you could execute in the Fuseki user interface could help to see if there are any problems: 

### 1. The amount of triples 

* in named graph 

SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { ?s ?p ?o } 
} 

* in default graph 

SELECT (COUNT(*) AS ?count) { ?s ?p ?o } 

This should be a large number, maybe 10 or 20 times the number of concepts. 

### 2. The number of SKOS concepts 

PREFIX skos: <http://www.w3.org/2004/02/skos/core#> 
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { 
     ?s a skos:Concept 
   } 
}

### 3. The number of SKOS prefLabels 

PREFIX skos: <http://www.w3.org/2004/02/skos/core#> 
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { 
     ?s skos:prefLabel ?label 
   } 
}


## Setting vocabularies

(The vocabularies to show in Skosmos are configured in the file `vocabularies.ttl` which is an RDF file in Turtle syntax.)

1. create a vocabularies.ttl file in the Skosmos directory (c:\xampp\htdocs\skosmos). There are example files named vocabularies.ttl.dist that you can rename/copy to vovabularies.ttl as starting point. In this file
2. create a new session starting with the line: 
:id a skosmos:Vocabulary, void:Dataset ;
where id is just an identifier. It will be used in the URL after /skosmos/.
3. set the following required parameters:

  * the title of the vocabulary in different languages: **dc:title "title_of_the_vocabulary"@ language ;** where language can be: en, fi, sv, etc.
  * the category of the vocabulary: **dc:subject : category ;** where category can be: **cat_general**. The categories have to be defined in vocabularies.ttl file. For the skosmos.dev.finto.fi demo site, there have been defined six categories (loosely based on the UDC top level categories). You need to copy the category definitions to your file as well, or change everything to cat_general.
  * the URI namespace for vocabulary objects (these may not overlap): **void:uriSpace " URI_namespace ";** When you represent your data as RDF, the best practice is to coin a new URI namespace for your data set. Then use that as the value of the uriSpace setting. 
  * the language(s) that the vocabulary supports: **skosmos:language " language ", " language ", ... ;** where language can be: en, fi, sv, etc.
  * the default language of the vocabulary, if the vocabulary supports multiple languages: **skosmos:defaultLanguage " language ";**  where language can be: en, fi, sv, ?
  * the URI of the SPARQL endpoint containing this vocabulary **void:sparqlEndpoint < URI_SPARQL_endpoint > ;** URI_SPARQL_endpoint can be http://localhost:3030/ if you want to use the vocabulary locally, or the URL of the SPARQL endpoint of the remote vocabulary
  * Skosmos relies on **hasTopConcept** but it is only necessary if you enable the **showTopConcepts** setting. Setting **skosmos:showTopConcepts true** should display the top level hierarchy - assuming that the dataset contains the **skos:hasTopConcept** and/or **skos:topConceptOf** relationships that are necessary for this to work. If you want to enable the *Hierarchy tab* showing top-level concepts on the vocabulary home page: **skosmos:showTopConcepts** "true";
  * *Group index* is meant for thematic groupsClass of resources to display as concept groups,  or as arrays (subdivisions) of sibling concepts (typical values are **skos:Collection** or **isothes:ConceptGroup**): **skosmos:groupClass isothes:ConceptGroup ;** If you don't need this tab, simply drop the **skosmos:groupClass** setting.
  * if you do not want Skosmos to query the mapping concept URIs for labels if they haven't been found at the configured SPARQL endpoint: **skosmos:loadExternalResources "false";**
  * URI of the main **skos:ConceptScheme** (instance of the current vocabulary) should be specified if the vocabulary contains multiple **skos:ConceptScheme** instances **skosmos:mainConceptScheme < _main_Concept_Scheme_URI_ >**.
  * if the vocabulary is relatively small (eg. 100 concepts) you can show the alphabetical index with all the concepts instead of showing only the concepts of one letter at a time: **skosmos:fullAlphabeticalIndex "true";**. It is not recommended to use **fullAlphabeticalIndex** for large vocabularies

(See more details of configuring the vocabularies.ttl here: https://github.com/NatLibFi/Skosmos/wiki/Vocabularies)

## Individual vocabularies

(to be aligned, edited and completed)

### Getty

There are two sets of each Getty vocabulary, the "explicit" set and the "full" set (Total Exports). With the "explicit" set, which is smaller, you will need  to configure Fuseki to use inference so that the data store can infer the missing triples. With the full set this is not needed, but in turn the data set is much larger so you may have difficulties loading it (I wouldn't try loading that through Fuseki, but it could work with tdbloader (see [here](#off-line)). 

### Full set 

(You can to download the full set of Getty's vocabularies from here: [http://vocab.getty.edu/doc/#Total_Exports](http://vocab.getty.edu/doc/#Total_Exports)

The dowloaded export file includes all statements (explicit and inferred) of all independent entities.  It's a concatenation of the Per-Entity Exports in NTriples format. Because it includes all required Inference, you can load it to any repository (even one without RDFS reasoning).

1. Load the External Ontologies (**SKOS**, SKOS-XL, ISO 25964) from http://vocab.getty.edu/doc/#External_Ontologies. The purpose is to get descriptions of properties, associative relations, etc.
2. Load the GVP Ontology from http://vocab.getty.edu/ontology.rdf
3. If your repository supports Subproperty/Inverse/Transitive reasoning:
If you want to eliminate the struck-out properties in Reduced SKOS Inference (like we do), execute the SPARQL updates given in Reduced SKOS Inference.
If you don't, these properties will be inferred, which will add 2-3x more statements to your repository.
4. Load the export files:
  * http://vocab.getty.edu/dataset/aat/full.zip: 120 Mb zipped, 1871 Mb unzipped
  * http://vocab.getty.edu/dataset/tgn/full.zip: 340 Mb zipped, 5300 Mb unzipped
  * http://vocab.getty.edu/dataset/ulan/full.zip: TODO Mb zipped, TODO Mb unzipped

| **AAT** | **TGN** | **ULAN** |
| -- | -- | -- |
| AATOut_Full.nt     : subjects | TGNOut_Full.nt     : subj, places | ULANOut_Full.nt     : subj, agents |
| AATOut_Contribs.nt : contribs | TGNOut_Contribs.nt : contribs     | ULANOut_Contribs.nt : contribs     |
| AATOut_Sources.nt  : sources  | TGNOut_Sources.nt  : sources      | ULANOut_Sources.nt  : sources      |

Since the *Out_Full.nt files are very large, they require using the tdbloader:

**java -cp /fuseki-server.jar tdb.tdbloader --tdb=jena-text-config.ttl --graph=http://vocab.getty.edu/aat/  ./vocabularies/getty_aat/full/ontology.rdf**
...

(to be continued)

### Explicit set

With the "explicit" set, which is smaller, you will need  to configure Fuseki to use inference so that the data store can infer the missing triples. As for configuring inference in Fuseki, you will have to do some research (a search for "fuseki inference" at least gets you some hits). There is some documentation (https://jena.apache.org/documentation/assembler/assembler-howto.html) about configuring inference in Jena assembler files (such as the Fuseki configuration file). 

#### Problems with Getty

The Getty SPARQL endpoint is not responding in the right way. There seems to be some incompatibility between Skosmos (in practice, the EasyRdf library which is used to perform SPARQL queries) and the Getty SPARQL endpoint. 

It is better to set up your own Fuseki SPARQL endpoint, with the jena-text index. Even if you could access the Getty SPARQL endpoint, it would most likely be extremely slow to use it with Skosmos, since it doesn't have a text index that Skosmos could use.

I think it's unlikely that the Getty vocabularies would work well in Skosmos due to their very large size. 

The lack of a text index would most likely prevent any actual use of Skosmos with the Getty endpoint. Skosmos simply needs to have a text index to work with vocabularies of medium to large size. The limit is perhaps a few thousand concepts, depending on the performance of the endpoint / triple store and how much delay users are willing to accept, but Getty vocabularies have many more than that. 

### COAR Resource Type Vocabulary

COAR only represents labels using SKOS XL properties. Skosmos doesn't support SKOS XL currently (see issue #109 in the GitHub Issues section, e.g. https://github.com/NatLibFi/Skosmos/issues/109 ).

The remote endpoint of COAR (http://vocabularies.coar-repositories.org/sparql/repositories/coar) can not be used. Skosmos requires the data to follow SKOS Core. The COAR endpoint data currently is not SKOS Core. 

If you want to use COAR data locally, so you will first need to convert the data to SKOS Core labels ("plain SKOS"). One tool that can do this for you is here: http://art.uniroma2.it/owlart/documentation/utilities.jsf#skos_utilities

#### 1. Configure in vocabularies.ttl:

     :coarrt a skosmos:Vocabulary, void:Dataset ;
        dc:title "COAR Resource Type Vocabulary - PL - load dataset first!"@en ;
        dc:subject :cat_general ;
        void:uriSpace "http://purl.org/coar/resource_type/";
        skosmos:language "en";
        skosmos:showTopConcepts "true";
        skosmos:loadExternalResources "true";
        skosmos:fullAlphabeticalIndex "true";
        #skosmos:arrayClass isothes:ThesaurusArray ;
        void:sparqlEndpoint <http://localhost:3030/ds/sparql>;
        #skosmos:sparqlGraph <http://purl.org/coar/resource_type/>;
        .
#### 2. Start Fuseki
#### 3. Load COAR Resource Type Vocabulary
If you have it in SKOS XL format only, then execute this SPARQL Update query that converts SKOS XL labels into SKOS Core labels:

    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
    PREFIX skos:   <http://www.w3.org/2004/02/skos/core#> 
    PREFIX skosxl: <http://www.w3.org/2008/05/skos-xl#> 
    INSERT { 
       ?c skos:prefLabel ?pref . 
       ?c skos:altLabel ?alt . 
       ?c skos:hiddenLabel ?hidden . 
       ?c skos:definition ?def . 
    } 
    WHERE { 
       { ?c skosxl:prefLabel/skosxl:literalForm ?pref } 
       UNION 
       { ?c skosxl:altLabel/skosxl:literalForm ?alt } 
       UNION 
       { ?c skosxl:hiddenLabel/skosxl:literalForm ?hidden } 
      UNION 
       { ?c skos:definition/rdf:value ?def }  
     } 

### ÖFOS - 2012

(to be edited)

You need to represent your classification using the SKOS data model. For some guidance, see 
the SKOS Primer [1] and introductory articles [2,3]. An excellent general introduction to RDF and Linked Data is given for example in the Linked Data book [4] available for free online. For specifics on what aspects of SKOS and other RDF vocabularies Skosmos supports, see the wiki page Data Model [5]. 
[1] https://www.w3.org/TR/skos-primer/ 
[2] http://www.dataversity.net/introduction-to-skos/ 
[3] http://www.ala.org/alcts/resources/z687/skos 
[4] http://linkeddatabook.com/ 
[5] https://github.com/NatLibFi/Skosmos/wiki/Data-Model 

### Examples

(to be edited and completed)

#### For "COAR-Resource Type Vocabulary" set the properties as follow:

dc:title "COAR - Resource Type Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://purl.org/coar/resource_type/";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/ds/sparql> ;
.

#### For sample record of "Getty TGN vocabulary" set the properties as follow:

dc:title "Getty TGN Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://vocab.getty.edu/tgn/7011179";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/> ;
.

