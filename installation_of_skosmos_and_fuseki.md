# Installation and configuration of Skosmos and Fuseki

## On skosmos.phaidra.org

(to be added) 

## On a Windows 7 machine 

Installation on Windows 7 (Professional 64 bit, Service Pack 1, Intel Core i7-56000 CPU, 2.6 GHz, 16 GB RAM) using Java 1.8 (jre1.8.0_40), with installed XAMPP (xampp-win32-1-8-3-4-VC11) environment.

### 1. Install XAMPP

(Apache web server of XAMPP is required in order to run PHP.)

1. Download XAMPP (e.g. XAMPP Windows 5.6.3, xampp-win32-1-8-3-4-VC11-installer.exe from http://en.softonic.com/s/download-xampp-64-bit-windows-7-free-downloads) (Don't be surprised, that it looks like a win32 version, direct 64-bit version is not available, but it will not be a problem.)
2. Run xampp-win32-1-8-3-4-VC11-installer.exe with the default options (install XAMPP into the folder C:\xampp)
(with the installation of XAMPP we have installed Apache and PHP, as its is required for Skosmos)

### 2. Get Skosmos

1. Download Skosmos current master branch from https://github.com/NatLibFi/Skosmos.
2. Unzip Skosmos-master.zip to a skosmos subfolder under c:\xampp\htdocs folder (which should the default DocumentRoot for XAMPP.)
Result: c:\xampp\htdocs\skosmos contains index.php and other files and folders.
(Cloning the code from GitHub would be better, but will be tested in the next turn only.)

### 3. Install Git

(Git is required for running the Dependency Manager in the next step.)

1. Download git form https://git-scm.com/download/win
2. Run Git-2.5.3-64-bit.exe (options: Use Git and optional Unix tools from the Windows Command Prompt; Checkout Windows-style, commit Unix-style line endings; Use Windows' default console window)
3. Restart your computer to make the changes in the PATH environment variable in effect

### 4. Install Dependency Manager for PHP

(Skosmos requires a number of PHP libraries, which will be installed using Composer.)

1. Download the Windows installer of Composer (Composer-Setup.exe) from https://getcomposer.org/doc/00-intro.md#installation-windows.
2. Run Composer-Setup.exe
3. Open command prompt and cd into the skosmos directory (c:\xampp\htdocs\skosmos)
4. Run from this folder the command (this will install the dependencies): composer install --no-dev
(Successful result: lot of components will be installed, lock file will be written, and finally autoload file will be generated.)

If you download a new version of Skosmos (in case of by starting Skosmos you receive the error message: "Error: Dependencies managed by Composer missing. Please run "php composer.phar install"), that requires to set up PHP dependencies again.

### 5. Install Jena Fuseki (later with jena-text extension)

(Jena Fuseki is a SPARQL server and RDF triple store which is the recommended backend for Skosmos. 
The jena-text extension can be used for faster text search.)

1. Download the latest Fuseki distribution jena-fuseki-*.zip from http://jena.apache.org/download/#apache-jena-fuseki
2. Unpack the downloaded file (apache-jena-fuseki-2.3.0.zip) to c:\apache-jena-fuseki-2.3.0

(More details on installing Fuseki with the jena-text extension can be seen at https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText)



### 6. Configure Skosmos

Create config.inc file in the Skosmos directory (c:\xampp\htdocs\skosmos). There are example files named config.inc.dist that you can rename/copy as starting point.

Edit the config.inc file:
1. change the TEMPLATE_CACHE setting like this: define("TEMPLATE_CACHE", "c:/xampp/tmp/skosmos-template-cache");
2. check the default SPARQL endpoint setting and change that to match your SPARQL endpoint: 
define("DEFAULT_ENDPOINT", "http://localhost:3030/ds/sparql");
3. add this line to the bottom of file:
define("BASE_HREF", "http://localhost/skosmos/");

(See other settings of Skosmos in https://github.com/NatLibFi/Skosmos/wiki/Configuration.)


### 7. Configure PHP (optional)

The default PHP configuration provided by your distribution is probably fine for Skosmos, but you may want to check php.ini anyway. Here are some things to check:

* Make sure you have the date.timezone setting configured, otherwise you may get errors displaying date values.
* If you use vocabularies with potentially a large number of triples, you may need to adjust the memory_limit setting. The default is usually 128M but the recommended setting is 256M.

## Getting and setting vocabularies

### 1. Getting vocabularies (if you want to use certain vocabularies locally)

(e.g. Putting "COAR - Resource Type Vocabulary" or "Getty TGN vocabulary" into Skosmos)

1. Create a new directory called vocabularies in Skosmos folder (c:\xampp\htdocs\skosmos)
2. Open a browser and go to the vocabulary that you want to use (e.g. COAR - Resource Type Vocabulary or Getty TGN voabulary
3. Download (save) the vocabulary as an RDF file (in XML, Turtle, or N-Triples syntax) (e.g. resource_types.xml or tgn_7011179.rdf) into c:\xampp\htdocs\skosmos\vocabularies

### 2. Skosify the downloaded vocabularies (optional)

(Check if the downloaded files (e.g. resource_types.xml or tgn_7011179.rdf) is in correct SKOS format using the Skosify tool.)

Online version of the Skosify tool used to be available here: https://code.google.com/p/skosify/, and use as follows
1. Select the vocabulary to be checked as input
2. Leave the default options as they are
3. Click on the Process button
After successful process you will get a Processed vocabulary that you can download and rename (e.g. as checked_resource_types.xml or checked_tgn_7011179.rdf) into the folder c:\xampp\htdocs\skosmos\vocabularies
You can find a recent versions of the Skosify tool here 
Unfortunately there is currently no recent online version available. 
Skosify requires Python (2.x or 3.x) and the rdflib library. It should run fine on Windows after those installed.


### 3. Setting vocabularies

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

#### Examples

##### For "COAR-Resource Type Vocabulary" set the properties as follow:

dc:title "COAR - Resource Type Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://purl.org/coar/resource_type/";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/ds/sparql> ;
.

##### For "getty TGN vocabulary" set the properties as follow:

dc:title "getty TGN Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://vocab.getty.edu/tgn/7011179";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/> ;
.

(See more details of configuring the vocabularies.ttl here: https://github.com/NatLibFi/Skosmos/wiki/Vocabularies )