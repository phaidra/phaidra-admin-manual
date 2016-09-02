# Configuration of Skosmos and Fuseki

## Configuration of Skosmos

Skosmos can be configured basically in two files, config.inc for setting some general parameters, and vocabularies.ttl is used to configure the vocabularies shown in Skosmos. 

### Config.inc

In config.inc you can set the name of the vocabularies file, change the timeout settings, set interface languages, set the default SPARQL endpoint, and set the SPARQL dialect, etc.

#### SPARQL dialect

if you want to use Jena text index  ```define("DEFAULT_SPARQL_DIALECT", "JenaText");``` ).

#### Interface languages

Regarding the interface languages you have to provide the system locales. E.g.:
```
$LANGUAGES = array(
  'de' => 'de_DE.utf8',
  'en' => 'en_US.utf8',
  'it' => 'it_IT.utf8',
);

```
If the required locales are missing (you can check it with the ```locale -a``` command), you generate it with ```sudo locale-gen language[_country][.charset]```. 
For example ```sudo locale-gen it_IT.utf8```.

It is important, that after generating new locales, Apache should be restarted (on Ubuntu: ```sudo service apache2 restart```, on CENTOS: ```sudo service httpd restart```)  

The translations of Skosmos menus are stored in the folder ```skosmos/resource/translations/``` in skomos_*language*.po and .mo files. If the translation file of a certain language is missing, then instead of the menu items in the added and generated language the text "in_this_language" will appear.

#### feedback address

You can set the default e-mail address to where the the feedback from the user can be sent from the Feedback menu:

```define("FEEDBACK_ADDRESS", "mymail@univie.ac.at");```

#### config.inc in Windows

In Windows you have to edit the config.inc file additionaly as follow:
1. change the TEMPLATE_CACHE setting like this: ```define("TEMPLATE_CACHE", "c:/xampp/tmp/skosmos-template-cache");
2. add this line to the bottom of file:
define("BASE_HREF", "http://localhost/skosmos/");

### vocabularies.ttl

Vocabularies are managed in the RDF store accessed by Skosmos via SPARQL. The available vocabularies are configured in the vocabularies.ttl file that is an RDF file in Turtle syntax.

Each vocabulary is expressed as a skosmos:Vocabulary instance (subclass of void:Dataset). The local name of the instance determines the vocabulary identifier used within Skosmos (e.g. as part of URLs). The vocabulary instance has the following properties: title of vocabulary (in different languages), the URI namespace for vocabulary objects, language(s) and the default language that the vocabulary supports, URI of the SPARQL endpoint containing the vocabulary, and the name of the graph within the SPARQL endpoint containing the data of the individual vocabulary.

In addition to vocabularies, the vocabularies.ttl file also contains a classification for the vocabularies expressed as SKOS. The categorization is used to group the vocabularies shown in the front page of Skosmos. You can also set the content of the About page in about.inc, and add additional boxes to the left or to the right of the front page in left.inc or right.inc.

(See other settings of Skosmos in https://github.com/NatLibFi/Skosmos/wiki/Configuration.)

### favicons

In some istances of Skosmos favicons (favicon.ico or custom-favicon.ico) appears if only you add the following line into the  ```light.twig``` file (in the ```view``` folder) right after the opening ```<head>``` tag:

```<link rel="shortcut icon" href="favicon.ico">```

If you want to use your own custom favicon put a favicon file using the ```custom-favicon.ico``` filename in the root folder of the Skosmos install, then it will be used instead of the default Skosmos favicon.


## Configuration of Fuseki

A Fuseki server can be set up using a configuration file. The configuration is an RDF graph. One graph consists of one server description, with a number of services, and each service offers a number of endpoints over a dataset.

There are some prepared configuration files (e.g. config.ttl) in the Fuseki folder that can be modified.

First of all you have to set the prefixes. In order to enable of skos you have to add to the prfeix session the following line:

```@prefix skos:    <http://www.w3.org/2004/02/skos/core#> .```

Then you can set the services of Fuseki. If you want to run Fuseki as a read-only service, you have to set services as follow:

```
<#service> rdf:type fuseki:Service ;
    fuseki:name                     "/ds" ;   # http://host:port/ds
    fuseki:serviceQuery             "query" ;    # SPARQL query service
    fuseki:serviceReadGraphStore    "data" ;     # SPARQL Graph store protocol (read only)
    fuseki:dataset           <#dataset> ;
```

The jena text enabled configuration file (config-tdb-text.ttl) specifies the directories where Fuseki stores its data. The default locations are /tmp/tdb and /tmp/lucene. To flush the data from Fuseki, simply clear/remove these directories. 

Fuseki stores data in files in a TDB folder. It is also possible to configure Fuseki for in-memory use only, but with a large dataset, this will require a lot of memory. The in-memory use of Fuseki is usually faster.

If you want to run Fuseki using a TDB dataset for RDF storage and query (see https://jena.apache.org/documentation/tdb/), you have to create a folder in Fuseki folder (e.g.: /var/www/fuseki/tdb) where the dataset will be stored. 

You have to set the locations of the tdb folder in the configuration file of fuseki (e.g. ```config-tdb-text.ttl```), as follow:

```
<#dataset> rdf:type      tdb:DatasetTDB ;
    tdb:location "DB" ;
    tdb:unionDefaultGraph true ;
    .
``` 

### Using text index

The jena text extension can be used for faster text search, and Skosmos simply needs to have a text index to work with vocabularies of medium to large size. The limit is a few thousand concepts, depending on the performance of the endpoint / triple store and how much latency is acceptable to the users.

If you want to use indexed test search applying Apache Lucene, then you have to create a folder (e.g.: ```/var/www/fuseki/lucene```) where you want to keep the Lucene index.

You have to set the locations of the lucene folder in the configuration file of fuseki (e.g. ```config-tdb-text.ttl```), as follow:

```
<#indexLucene> a text:TextIndexLucene ;
    text:directory <file:/var/www/fuseki/lucene> ;
    text:entityMap <#entMap> ;
    .
``` 
If you start fuseki in the TDB with ```./fuseki-server --config config.ttl```then it will run using text index.

If you start fuseki in the memory with ```./fuseki-server --update --mem /ds```, then there is no text indexing by default.

It is also possible to use in-memory TDB and text index, but you need a Fuseki configuration file (config.ttl) with special "file names" that are actually in-memory:
* for tdb: ```tdb:location "--mem--" ; ```
* for jena-text: ```text:directory "mem"; ```

By default Skosmos uses text index, but it can be switched off by 
* either setting DEFAULT_SPARQL_DIALECT to "Generic" (instead of "JenaText") in config.inc (this affects all 
vocabularies):
```define("DEFAULT_SPARQL_DIALECT", "Generic")```;
* or setting ```skosmos:sparqlDialect "Generic"``` (instead of "JenaText") for just the certain vocabularies in vocabularies.ttl.

## Timeout settings

If there is more data than Skosmos is made to handle, so some queries can take very long time. 
The slow queries are probably the statistical queries (number of concepts per type, number of labels per language) as well as the alphabetical index. 

Short execution timeout for PHP scripts can trigger Runtime IO Exception.

### PHP and Apache timeout settings

See php.ini max_execution_time and Apache's TimeOut directive the settings. 
It is highly recommended to find this setting and change it to a higher value (say to 5 or 10 minutes):  

* /etc/httpd/conf/snippets/timeout.conf: Timeout 60 -> 600)
* time.ini  
  * max_execution_time=30 -> 600
  * max_input_time = 60 -> 600
  * default_socket_timeout = 60 -> 600
  * mysql.connect_timeout = 60 -> 600
  * mssql.timeout = 60 -> 600
  * ja:context [ ja:cxtName "arq:queryTimeout" ;  ja:cxtValue "1000" ] ;

### Skosmos timeout setting

Skosmos also has a HTTP_TIMEOUT setting in config.inc, that should only be used for external URI requests, not for regular SPARQL queries, but there may be unknown side-effects:

```define("HTTP_TIMEOUT", 600); // external HTTP request timeout in seconds```


### EasyRdf HTTP client timeout

The EasyRdf HTTP client has a default timeout of 10 seconds. It is also recommended to change this value. 
It is set in /var/www/skosmos/vendor/easyrdf/easyrdf/lib/EasyRdf/Http/Client.php near the top of the class definition. (If you change this then an update of EasyRdf might revert your changes later on)

### Checking the browsers' timing out. 

It is suggested to change the timeout value from the browsers where you are planning to access Skosmos.

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

### Varnish

It is worth setting up **Varnish** (first_byte_timeout and between_bytes_timeout) in between Skosmos/Apache and Fuseki. It doesn't help with the first request, but subsequent ones will be answered very quickly from the cache. The statistical queries are always the same so they can be cached very well. 
See https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning#http-caching 

## Memory problems by uploading files to Fuseki

The Fuseki file upload handling is not very good at processing large files. It will load them first into memory, only then to the on-disk TDB database (and also the Lucene/jena-text index in your case). It can  to run out of memory on the first step ("OutOfMemoryError: java heap space" is a typical error message when this happens). 
You can try giving Fuseki more memory. See for some tips:

**https://github.com/NatLibFi/Skosmos/wiki/FusekiTuning** 

For example Setting JVM heap to 16 GB:
**export JVM_ARGS=-Xmx16000M**

If you give it several GB it should be able to handle a 400MB file upload just fine, though it might take a while and you may want to restart Fuseki afterwards to free some memory. 



