# Configuration of Skosmos and Fuseki

## Settings in vocabularies.ttl 

(move to Getting and setting vocabularies)

* Skosmos relies on hasTopConcept but it is only necessary if you enable the showTopConcepts setting
* The categories have to be defined in vocabularies.ttl file. For the skosmos.dev.finto.fi demo site, there have been defined six categories (loosely based on the UDC top level categories). You need to copy the category definitions to your file as well, or change everything to cat_general 


## Options for starting Fuseki


The --mem option is a shorthand for running Fuseki without a configuration file. (--file is similar too) 
You need a configuration file to use jena-text so you must use 
./fuseki-server --config jena-text-config.ttl 

> What is the difference, if I set and use the FUSEKI_CONF= environment  variable? 
If you use the init script (/etc/init.d/fuseki), then the environment variable is used to set the configuration file. But if you just run Fuseki from the command line, you need to use the --conf option. 

The Getty SPARQL endpoint is not responding in the right way. There seems to be some incompatibility between Skosmos (in practice, the EasyRdf library which is used to perform SPARQL queries) and the Getty SPARQL endpoint. I don't know the reason for this, it would need more investigation. 

I think you'd be better off setting up your own Fuseki SPARQL endpoint, with the jena-text index. Even if you could access the Getty SPARQL endpoint, it would most likely be extremely slow to use it with Skosmos, since it doesn't have a text index that Skosmos could use. 
https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText 
I suggest you set up Fuseki (with jena-text) and then put some much smaller and simpler SKOS dataset there for testing. For example, the STW Thesaurus and/or UNESCO Thesaurus would work for this. Once you get these working, you can then try one of the Getty vocabularies. Though as I've said before, I think it's unlikely that the Getty vocabularies would work well in Skosmos due to their very large size. However, using a small subset could still work. 
We have both the STW and UNESCO thesauri configured on the skosmos.dev.finto.fi demo site. To see how they are configured, you can take a look at the configuration file: 
http://skosmos.dev.finto.fi/vocabularies.ttl 

The STW and UNESCO thesauri are available for download from their respective home pages: 
http://zbw.eu/stw/version/latest/about 
http://skos.um.es/unescothes/ 
---
Okay, so Skosmos is not seeing the triples you have in Fuseki. I'm guessing this could be because of mismatches in named graphs. In a SPARQL triple store there is always a default (unnamed) graph, and there can also be multiple named graphs. If you have put e.g. the UNESCO thesaurus in the default graph and configured Skosmos to use a named graph, or vice versa, then that could explain why you don't see any content. 
I suggest you put the UNESCO thesaurus in a named graph <http://skos.um.es/unescothes/> (that's what we use as graph name). You can upload it to Fuseki with the command line utility s-put that comes with Fuseki, like this: 
./s-put http://localhost:3030/ds/data http://skos.um.es/unescothes/ unescothes.ttl 
(of course the Fuseki web interface can be used to do the same, just make sure you upload to the correct named graph) 
Then you should have the correct named graph in vocabularies.ttl (skosmos:sparqlGraph setting) but you already seem to have that set correctly. 
---
You are probably using the jena-text enabled configuration file with Fuseki. That configuration file specifies directories where Fuseki stores its data. If you have not modified the configuration, those will be /tmp/tdb and /tmp/lucene. You need to clear/remove these directories if you want to flush the Fuseki data. 

Fuseki stores them in files. It is also possible to configure Fuseki for in-memory only, but with a large dataset, that will require a lot of memory. If you want to see how it is done, check out this Fuseki configuration that we use for Skosmos unit tests: https://github.com/NatLibFi/Skosmos/blob/master/tests/fuseki-assembler.ttl 
(tdb:location and text:directory settings are the relevant ones) 

Yes, but then you need to use named graphs. There is only one default graph (with no name), but there can be any number of named graphs on a SPARQL endpoint/dataset. I suggest you use the URI namespaces as graph names. I.e. <http://vocab.getty.edu/tgn/> would store TGN data. 

Why did I receive the Runtime IO Exception? 
Not sure, maybe you have a short execution timeout for PHP scripts that was triggered? See php.ini max_execution_time setting. 

First of all, reading about SPARQL is recommended, especially dataset management and named graphs. There are some good books such as "Learning SPARQL", and of course the specifications are available though maybe not the best introductory material. 
The Fuseki documentation is highly recommended: 
https://jena.apache.org/documentation/serving_data/ 
---
The Fuseki file upload handling is not very good at processing large files. It will load them first into memory, only then to the on-disk TDB database (and also the Lucene/jena-text index in your case). It seems to run out of memory on the first step ("OutOfMemoryError: java heap space" is a typical error message when this happens). 
You can try giving Fuseki more memory. See https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning for some tips. If you give it several GB it should be able to handle a 400MB file upload just fine, though it might take a while and you may want to restart Fuseki afterwards to free some memory. 
If that doesn't help, you will have to use offline loading of the data. This means shutting down Fuseki (since only one process can use the TDB at the same time) and using the tdbloader command line utilities to create the TDB and load the RDF data. Then you will still need to generate the text index as a separate step. A short tutorial of this is included in the jena-text documentation: https://jena.apache.org/documentation/query/text-query.html#building-a-text-index 
You can also try asking for help on the Jena users mailing list, see 
https://jena.apache.org/help_and_support/ 
---
I wouldn't recommend using fullAlphabeticalIndex for such a large vocabulary, but that's not the reason for your problems. 
I wonder whether your data is OK. You say you've loaded everything into the graph, but have you checked what's actually in there? For example these SPARQL queries that you could execute in the Fuseki user interface could help to see if there are any problems: 
1. The amount of triples in the graph 
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { ?s ?p ?o } 
} 

///
SELECT (COUNT(*) AS ?count) { ?s ?p ?o } 
///
This should be a large number, maybe 10 or 20 times the number of concepts. 
2. The number of SKOS concepts 
PREFIX skos: <http://www.w3.org/2004/02/skos/core#> 
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { 
     ?s a skos:Concept 
   } 
} 
3. The number of SKOS prefLabels 
PREFIX skos: <http://www.w3.org/2004/02/skos/core#> 
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { 
     ?s skos:prefLabel ?label 
   } 
} 
Is it possible that while loading the data from several files, you have overwritten the previous data when loading a new file? That's what s-put does - it clears the graph first. You should use s-post if you want to add to existing data without clearing it. 
---
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

---
It seems you've succeeded loading the data properly. The problem sounds like a timeout to me. There is simply a lot of data in AAT, more than Skosmos is made to handle, so some queries can take a very long time. Based on the error message I think you have a 60 second timeout somewhere, either in Apache (TimeOut directive) (/etc/httpd/conf/snippets/timeout.conf: Timeout 60 -> 600)
, PHP (max_execution_time setting) 
time.ini  max_execution_time=30 -> 600
max_input_time = 60 -> 600
default_socket_timeout = 60 -> 600
mysql.connect_timeout = 60 -> 600
mssql.timeout = 60 -> 600


///<#dataset> rdf:type      tdb:DatasetTDB ;
 ...
    # Query timeout on this dataset (1s, 1000 milliseconds)
    ja:context [ ja:cxtName "arq:queryTimeout" ;  ja:cxtValue "1000" ] ;///
or Varnish (first_byte_timeout and between_bytes_timeout). I suggest finding the setting and changing it to a higher value (say 5 or 10 minutes). 
The slow queries are probably the statistical queries (number of concepts per type, number of labels per language) as well as the alphabetical index. For the statistical queries, we are making them optional in Skosmos 1.5 but this is not yet implemented, see https://github.com/NatLibFi/Skosmos/issues/413 

I hope you've set up Varnish in between Skosmos/Apache and Fuseki? You definitely should. It doesn't help with the first request, but subsequent ones will be answered very quickly from the cache. The statistical queries are always the same so they can be cached very well. 
See https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning#http-caching 
---

Okay, maybe it's the browser timing out. You could also check the Apache error.log in case there are warnings or errors there that could indicate why the SPARQL query is terminated. This time it happened at almost exactly 50 seconds, which also looks like some kind of a timeout. 
Chrome Timout setting:
You can't. Google has been ignoring requests to implement this feature for over six years, so I wouldn't hold your breath.
IE Timeout setting:
Click Start, click Run, type regedit, and then click OK.
Locate and then click the following key in the registry: HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\InternetSettings
On the Edit menu, point to New, and then click DWORD Value.
Type KeepAliveTimeout, and then press ENTER.
On the Edit menu, click Modify.
Type the appropriate time-out value (in milliseconds), and then click OK. For example, to set the time-out value to two minutes, type 120000.
Restart Internet Explorer.
Firefox Timout setting:
There are two ways to do this. You can extend your timeout, or you can totally disable timeout. Depending on the way you use firefox, either may be helpful. If you want to extend your timeout, type about:config in your search bar on the top. From there it will take you to a list of preferences. There is a search bar. Type "Timeout" into it. The top two are disable timeout and the "count timeout". If you want to disable timeout, simply double click the "enabletimeout" on the top. This will change the value to false. If you would like to change the value of how long it takes to timeout, double click the "CountTimeout" and enter in your new value.

---
Skosmos has a HTTP_TIMEOUT setting in config.inc, you could see if increasing that to a large value helps. It should only be used for external URI requests, not for regular SPARQL queries, but there may be unknown side-effects. 
Also the EasyRdf HTTP client has a default timeout of 10 seconds. You could see if editing it helps. It is set in /var/www/skosmos/vendor/easyrdf/easyrdf/lib/EasyRdf/Http/Client.php near the top of the class definition. (If you change this then an update of EasyRdf might revert your changes later on, but at least we know where the timeout is being set) 
The search for single letter error "No vocabularies on the server!" is strange. Do you get it directly or only after a delay? Have you configured jena-text properly? Please check the following: 
1. You have this line in config.inc: 
define("DEFAULT_SPARQL_DIALECT", "JenaText"); 
2. You are using a Fuseki assembler configuration with jena-text, i.e. the one from here: 
https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText 
3. You have built the jena-text index using jena.textindexer: 
http://jena.staging.apache.org/documentation/query/text-query.html#step-2-build-the-text-index 
Please also try searching for a whole word that you know exists in AAT, for example "architecture", not just a single letter. 
---
1. Hierarchy. Try setting "skosmos:showTopConcepts true" in your vocabularies.ttl file. That should display the top level hierarchy - assuming that the AAT data contains the skos:hasTopConcept and/or skos:topConceptOf relationships that are necessary for this to work. 
2. Group index. This is meant for thematic groups, often represented as skos:Collection or iso-thes:ConceptGroup. I'm not sure whether the AAT has these at all and in that case how they are represented in RDF. If you don't need this tab, simply drop the skosmos:groupClass setting from vocabularies.ttl. 
---
The lack of a text index would most likely prevent any actual use of Skosmos with the Getty endpoint. Skosmos simply needs to have a text index to work with vocabularies of medium to large size. The limit is perhaps a few thousand concepts, depending on the performance of the endpoint / triple store and how much delay users are willing to accept, but Getty vocabularies have many more than that. 

---
ÖFOS - 2012
You need to represent your classification using the SKOS data model. For some guidance, see the SKOS Primer [1] and introductory articles [2,3]. An excellent general introduction to RDF and Linked Data is given for example in the Linked Data book [4] available for free online. For specifics on what aspects of SKOS and other RDF vocabularies Skosmos supports, see the wiki page Data Model [5]. 
[1] https://www.w3.org/TR/skos-primer/ 
[2] http://www.dataversity.net/introduction-to-skos/ 
[3] http://www.ala.org/alcts/resources/z687/skos 
[4] http://linkeddatabook.com/ 
[5] https://github.com/NatLibFi/Skosmos/wiki/Data-Model 
The uriSpace setting is not crucial for Skosmos, this is the least of your worries. When you represent your data as RDF, the best practice is to coin a new URI namespace for your data set (see the Linked Data book for some guidance). Then use that as the value of the uriSpace setting. 

--- 
COAR Resource Type Vocabulary

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
start-fuseki-in-mem
load checked_resource_types.xml into the default graph
(COAR and it seems to me that it only represents labels using SKOS XL properties. Skosmos doesn't support SKOS XL currently (see issue #109 in the GitHub Issues section, e.g. https://github.com/NatLibFi/Skosmos/issues/109 ), so you will first need to convert the data to SKOS Core labels ("plain SKOS"). 
One tool that can do this for you is here: http://art.uniroma2.it/owlart/documentation/utilities.jsf#skos_utilities )
...or perhaps an easier way would be to simply load the data into Fuseki and then execute this SPARQL Update query that converts SKOS XL labels into SKOS Core labels: 
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
 The remote endpoint of COAR (http://vocabularies.coar-repositories.org/sparql/repositories/coar) can not be used. Skosmos requires the data to follow SKOS Core. The COAR endpoint data currently is not SKOS Core. 

---
Upgrading Skosmos
(https://github.com/NatLibFi/Skosmos/wiki/Upgrading)
make a backup of your current installation, especially config.inc and vocabularies.ttl  
(make a backup of fuseki files except tdb and lucene)    
upgrade your code to the new version: 
a) download a new version form https://github.com/NatLibFi/Skosmos/releases and replace the files or
b) switch to the new version tag/branch using e.g. git checkout tags/v1.4 or git checkout v1.4-maintenance
migrate (copy) config.inc and vocabularies.ttl to your new installation if necessary
update Composer just in case: php composer.phar self-update   
update dependencies: php composer.phar update --no-dev   
restart Apache (this will clear gettext and APC caches - reloading is not enough!)
Version specific notes
Skosmos 1.4 requires Jena Fuseki 1.3.0 or 2.3.0 if you use the JenaText index (NOTE: Fuseki 1.3.1 and 2.3.1 have a bug which affects Skosmos so they are not recommended). 
You will also need to change the Fuseki configuration and rebuild the text index.
 See the InstallFusekiJenaText page for details about the new text index configuration (you will need to add text:storeValues, text:uidField and text:langField settings). 
Note that these versions of Jena Fuseki require a Java 8 environment, so you may need to upgrade that first. 
$ java -version
openjdk version "1.8.0_65"
OpenJDK Runtime Environment (build 1.8.0_65-b17)
OpenJDK 64-Bit Server VM (build 25.65-b01, mixed mode) 
