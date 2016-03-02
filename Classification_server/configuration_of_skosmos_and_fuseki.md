# Configuration of Skosmos and Fuseki

## Checking the jena-text configuration

1. You have this line in config.inc: 
define("DEFAULT_SPARQL_DIALECT", "JenaText"); 

2. You are using a Fuseki assembler configuration with jena-text, i.e. the one from here: 
https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText 

3. You have built the jena-text index using jena.textindexer: 
http://jena.staging.apache.org/documentation/query/text-query.html#step-2-build-the-text-index 

## Data files of Fuseki

Fuseki stores data in files. It is also possible to configure Fuseki for in-memory only, but with a large dataset, that will require a lot of memory. 

The jena-text enabled configuration file specifies directories where Fuseki stores its data. If you have not modified the configuration, those will be /tmp/tdb and /tmp/lucene. You need to clear/remove these directories if you want to flush the Fuseki data. 


For Skosmos unit tests this configuration was used:
https://github.com/NatLibFi/Skosmos/blob/master/tests/fuseki-assembler.ttl
where **tdb:location** and **text:directory** settings are the relevant ones. 

## Timeout settings

If there is more data than Skosmos is made to handle, so some queries can take a very long time. Short execution timeout for PHP scripts can trigger Runtime IO Exception.

See php.ini max_execution_time either in Apache (TimeOut directive) the setting. I suggest finding the setting and changing it to a higher value (say 5 or 10 minutes):  

* /etc/httpd/conf/snippets/timeout.conf: Timeout 60 -> 600)
* time.ini  
  * max_execution_time=30 -> 600
  * max_input_time = 60 -> 600
  * default_socket_timeout = 60 -> 600
  * mysql.connect_timeout = 60 -> 600
  * mssql.timeout = 60 -> 600
  * ja:context [ ja:cxtName "arq:queryTimeout" ;  ja:cxtValue "1000" ] ;

The slow queries are probably the statistical queries (number of concepts per type, number of labels per language) as well as the alphabetical index. (The statistical queries will be optimized in Skosmos 1.5 but this is not yet implemented, see https://github.com/NatLibFi/Skosmos/issues/413 )

It is worth setting up **Varnish** (first_byte_timeout and between_bytes_timeout) in between Skosmos/Apache and Fuseki. It doesn't help with the first request, but subsequent ones will be answered very quickly from the cache. The statistical queries are always the same so they can be cached very well. 
See https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning#http-caching 


### Checking the browsers' timing out. 

#### IE Timeout setting

1. Click Start, click Run, type regedit, and then click OK.
2. Locate and then click the following key in the registry: HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\InternetSettings
3. On the Edit menu, point to New, and then click DWORD Value.
4. Type KeepAliveTimeout, and then press ENTER.
5. On the Edit menu, click Modify.
6. Type the appropriate time-out value (in milliseconds), and then click OK. For example, to set the time-out value to two minutes, type 120000.
7. Restart Internet Explorer.

#### Firefox Timout setting

There are two ways to do this. You can extend your timeout, or you can totally disable timeout. Depending on the way you use Firefox, either may be helpful. 

1. Type about:config in your search bar on the top. 
2. From there it will take you to a list of preferences. 
3. There is a search bar. Type "Timeout" into it. 
4. The top two are disable timeout and the "count timeout". 
  * If you want to disable timeout, simply double click the "enabletimeout" on the top. This will change the value to false. 
  * If you would like to change the value of how long it takes to timeout, double click the "CountTimeout" and enter in your new value.

#### Chrome Timeout setting

You can't change the timeout setting of Google Chrome. 
Google has been ignoring requests to implement this feature for over six years.


---
Skosmos has a HTTP_TIMEOUT setting in config.inc, you could see if increasing that to a large value helps. It should only be used for external URI requests, not for regular SPARQL queries, but there may be unknown side-effects. 
Also the EasyRdf HTTP client has a default timeout of 10 seconds. You could see if editing it helps. It is set in /var/www/skosmos/vendor/easyrdf/easyrdf/lib/EasyRdf/Http/Client.php near the top of the class definition. (If you change this then an update of EasyRdf might revert your changes later on, but at least we know where the timeout is being set) 

## Memory problems by uploading files to Fuseki

The Fuseki file upload handling is not very good at processing large files. It will load them first into memory, only then to the on-disk TDB database (and also the Lucene/jena-text index in your case). It can  to run out of memory on the first step ("OutOfMemoryError: java heap space" is a typical error message when this happens). 
You can try giving Fuseki more memory. See for some tips:

**https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning** 


If you give it several GB it should be able to handle a 400MB file upload just fine, though it might take a while and you may want to restart Fuseki afterwards to free some memory. 
