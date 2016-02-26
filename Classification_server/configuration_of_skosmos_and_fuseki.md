# Configuration of Skosmos and Fuseki

## Settings in vocabularies.ttl 

(move to Getting and setting vocabularies)

* Skosmos relies on hasTopConcept but it is only necessary if you enable the showTopConcepts setting
* The categories have to be defined in vocabularies.ttl file. For the skosmos.dev.finto.fi demo site, there have been defined six categories (loosely based on the UDC top level categories). You need to copy the category definitions to your file as well, or change everything to cat_general 
* It is not recommended to use fullAlphabeticalIndex for large vocabularies

## Options for starting Fuseki

(move to Start Fuseki and Skosmos)

* The --mem option is a shorthand for running Fuseki without a configuration file (--file is similar too)
*  If you need a configuration file to use jena-text so you must use

  **./fuseki-server --config jena-text-config.ttl **

* If you use the init script (/etc/init.d/fuseki), then the FUSEKI_CONF= environment variable is used to set the configuration file. But if you just run Fuseki from the command line, you need to use the --conf option. 

## Named Graph in SPARQL triple store

In a SPARQL triple store there is always a default (unnamed) graph, and there can also be multiple named graphs. 

There is only one default graph (with no name), but there can be any number of named graphs on a SPARQL endpoint/dataset. I suggest you use the URI namespaces as graph names. I.e. <http://vocab.getty.edu/tgn/> would store TGN data. 

If you have put e.g. the UNESCO thesaurus in the default graph and configured Skosmos to use a named graph, or vice versa, then that could explain why you don't see any content.

Put the UNESCO thesaurus in a named graph <http://skos.um.es/unescothes/> (that's what we use as graph name). You can upload it to Fuseki with the command line utility s-put that comes with Fuseki, like this: 

**./s-put http://localhost:3030/ds/data http://skos.um.es/unescothes/ unescothes.ttl 
**

(of course the Fuseki web interface can be used to do the same, just make sure you upload to the correct named graph) 

Then you should have the correct named graph in vocabularies.ttl (skosmos:sparqlGraph setting) 

## Data files of Fuseki

The jena-text enabled configuration file specifies directories where Fuseki stores its data. If you have not modified the configuration, those will be /tmp/tdb and /tmp/lucene. You need to clear/remove these directories if you want to flush the Fuseki data. 

Fuseki stores them in files. It is also possible to configure Fuseki for in-memory only, but with a large dataset, that will require a lot of memory. If you want to see how it is done, check out this Fuseki configuration that we use for Skosmos unit tests:

https://github.com/NatLibFi/Skosmos/blob/master/tests/fuseki-assembler.ttl

(tdb:location and text:directory settings are the relevant ones) 

## Timeout settings

Short execution timeout for PHP scripts can trigger Runtime IO Exception. See php.ini max_execution_time setting. 


## Memory problems by uploading files to Fuseki

The Fuseki file upload handling is not very good at processing large files. It will load them first into memory, only then to the on-disk TDB database (and also the Lucene/jena-text index in your case). It can  to run out of memory on the first step ("OutOfMemoryError: java heap space" is a typical error message when this happens). 
You can try giving Fuseki more memory. See for some tips:

**https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning** 

If you give it several GB it should be able to handle a 400MB file upload just fine, though it might take a while and you may want to restart Fuseki afterwards to free some memory. 

## Uploading files to Fuseki

(move to Getting and setting vocabularies)

### Using the web interface of Fuskei

#### To the default graph

(to be completed)

#### To a named graph

(to be completed)

### From the command line in Fuseki's folder

#### On-line (when Fuseki is running)
  
* use s-put if you want to add single data file, like this e.g.: 

**./s-put http://localhost:3030/ds/data http://skos.um.es/unescothes/ unescothes.ttl 
**

(of course the Fuseki web interface can be used to do the same, just make sure you upload to the correct named graph) 

Note: s-put does - it clears the graph first. It may happen, that you overwrite the previous data when loading a new file.

Then you should have the correct named graph in vocabularies.ttl (skosmos:sparqlGraph setting) 
* s-post if you want to add new data files to existing data without clearing it
  

#### Off-line (when Fuseki is not running)
  
If there are memory problems by uploading (several, large) files to Fuseki, it is worth to use offline loading up the data. This means shutting down Fuseki (since only one process can use the TDB at the same time) and 

* using the tdbloader command line utilities to create the TDB and load the RDF data. 

(to be completed)

* then you will still need to generate the text index as a separate step. A short tutorial of this is included in the jena-text documentation

https://jena.apache.org/documentation/query/text-query.html#building-a-text-index 


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


---
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


--- 
X