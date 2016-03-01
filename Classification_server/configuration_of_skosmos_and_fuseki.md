(to be edited!!!)

# Configuration of Skosmos and Fuseki


## Checking the jena-text configuration

1. You have this line in config.inc: 
define("DEFAULT_SPARQL_DIALECT", "JenaText"); 

2. You are using a Fuseki assembler configuration with jena-text, i.e. the one from here: 
https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText 

3. You have built the jena-text index using jena.textindexer: 
http://jena.staging.apache.org/documentation/query/text-query.html#step-2-build-the-text-index 

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

(to be aligned)

Short execution timeout for PHP scripts can trigger Runtime IO Exception. See php.ini max_execution_time setting. 

 If there is more data than Skosmos is made to handle, so some queries can take a very long time. Based on the error message I think you have a 60 second timeout somewhere, either in Apache (TimeOut directive) (/etc/httpd/conf/snippets/timeout.conf: Timeout 60 -> 600)
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

## Memory problems by uploading files to Fuseki

The Fuseki file upload handling is not very good at processing large files. It will load them first into memory, only then to the on-disk TDB database (and also the Lucene/jena-text index in your case). It can  to run out of memory on the first step ("OutOfMemoryError: java heap space" is a typical error message when this happens). 
You can try giving Fuseki more memory. See for some tips:

**https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning** 

If you give it several GB it should be able to handle a 400MB file upload just fine, though it might take a while and you may want to restart Fuseki afterwards to free some memory. 
