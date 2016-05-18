# Available tools for classification

(original source: https://redmine.phaidra.org/redmine/projects/labtech/wiki/Terminology_services#candidates)

##ThManager

http://thmanager.sourceforge.net/

ThManager  is an open source tool for creating and visualizing SKOS RDF vocabularies. ThManager was developed by the Advanced Information Systems Laboratory of the University of Zaragoza. It was implemented in Java using Apache Jena, and facilitates the management of thesauri and other types of controlled vocabularies such as taxonomies or classification schemes. ThManager allows for selecting and filtering the thesauri stored in the local repository. Description of thesauri by means of metadata is in compliance with a Dublin Core based application profile for thesaurus.

ThManager runs on Windows and Unix, and only requires having a Java Virtual Machine installed on the system. The application is multilingual. The application supports out of the box Spanish and English languages, but with little effort other languages can be implemented.

The main features include the visualization of thesaurus concepts (alphabetically, in hierarchical structure, properties of selected concepts), ability to search concepts ("equals", "starts with" and "contains"), editing thesaurus content (creation of concepts, deletion of concepts, and update of concept properties), export of thesauri (including thesaurus metadata) in SKOS format. 

Available vocabularies in ThManager include AGROVOC, DCType, GEMET, ISO639, and UNESCO.

Unfortunately, the latest version of ThManager was launched in 2006, and we cannot expect any updates. Another drawback of ThManager is that it does not provide a SPARQL endpoint for accessing the managed vocabularies.


## **TemaTres**

http://www.vocabularyserver.com/

TemaTres is an open source vocabulary server developed in Argentina. It includes a web application to manage and exploit vocabularies, thesauri, taxonomies and formal representations of knowledge stored in a MySQL database, and provides the created thesauri in SKOS format. TemaTres  requires PHP, MySql MySQL and a HTTP Web server.
TemaTres provides a SPARQL endpoint. Exporting and publishing controlled vocabularies is possible in many metadata schemas (SKOS-Core, Dublin Core, MADS, JSON, etc.). It can import data in SKOS-Core format and has a utility to import thesauri from tabulated text files.
It has an advanced search with search terms suggestions, and a systematic or alphabetic navigation. TemaTres has a special vocabulary harmonization feature where it can find equivalent, no equivalent, and partial terms against other vocabularies.
It supports multilingual thesaurus, multilingual terminology mapping, and includes a multilingual interface. It exposes vocabularies with powerful web services. TemaTres displays terms in multiple deep levels in the same screen. It also provides quality assurance functions (duplicates and free terms, illegal relations). The main drawback of TemaTres is that not all documentation is available in English.

## SKOS Shuttle

https://skosshuttle.ch/

SKOS Shuttle  is a multi-user/multi-tenant online Thesaurus Service developed by Semweb LLC (Switzerland). It supports building, maintaining and operating of SKOS thesauri. SKOS Shuttle allows operating on an internal own RDF repository and on any external SESAME compliant RDF repositories and it easily allows direct editing of RDF statements (triples) without restrictions. 
The user interface is intuitive. SKOS Shuttle also integrates a full REST API to create, manage and navigate thesauri. It accesses securely all information through SSL transported authentication. It provides industrial security (Rights, Groups, User and Project Management) and a smart "Orphan Concept Analysis" together with an assistant for direct concept “deorphanization” without using one single line of SPARQL code. 
With its “systematics assistant”, several base URI’s can be used inside one single thesaurus. RDF Import/Export and whole RDF snapshots are possible in 6 different formats (N3, N-Triples, TRIG, Turtle, NQuads, RDF/XML). 
SKOS Shuttles allows to download/upload a full RDF snapshot preserving versioning of each thesaurus. SKOS language tags can be added/removed “on the fly” while editing the thesaurus, speeding up maintenance tasks. SKOS Shuttle allows quick filtering on any thesaurus language, and also during concepts navigation, this permits to find out missing labels during navigation.
The SKOS Shuttle REST API provides a full range of selections / commands that are embeddable into any application using three output formats: JSON, XML and YAML. The API access requires the same secured authentication as the application to provide online services. 
SKOS Shuttle seems to be a very promising tool. SKOS Shuttle is available as a service and is already being used by several Universities. SKOS Shuttle is not an open source product. Pricing is not yet known but SKOS Shuttle will be provided as a commercial service for small thesauri and as a free service for universities (up to a larger extent).

## PoolParty

https://www.poolparty.biz/

PoolParty is a commercial semantic technology suite, developed by Semantic Web Company that offers solutions to your knowledge organization and content business problems. 
As a semantic middleware, PoolParty enriches your information with metadata and automatically links your business and content assets.
The PoolParty Taxonomy & Thesaurus Manager is a powerful tool to build and maintain your information architecture. The PoolParty thesaurus manager enables practitioners to start their work with limited training. Subject matter experts can model their fields of expertise without IT support.
PoolParty taxonomy management software applies SKOS knowledge graphs. With PoolParty, you can import existing taxonomies and thesauri (e.g. from Excel) and export them in different standard formats. In addition to basic SKOS querying, the API also supports the import of RDF data, SPARQL update and a service to push candidate terms into a thesaurus.

## Protégé

http://protege.stanford.edu/

Protégé  is a free, open-source ontology editor and framework for building intelligent systems. Protégé was developed by the Stanford Center for Biomedical Informatics Research at the Stanford University School of Medicine. Protégé is supported by a strong community of academic, government, and corporate users, who use Protégé to build knowledge-based solutions in areas as diverse as biomedicine, e-commerce, and organizational modelling.
With the web-based ontology development environment of Protégé, called WebProtégé, it is easy to create, upload, modify, and share ontologies for collaborative viewing and editing. The highly configurable user interface provides suitable environment for beginners and experts. Collaboration features abound, including sharing and permissions, threaded notes and discussions, watches and e-mail notifications. RDF/XML, Turtle, OWL/XML, OBO, and other formats are available for ontology upload and download.
Although it is a very good tool, it is too complex for editing and visualizing such a simple model as SKOS, and provides too many options not specifically adapted for the type of relationships used in SKOS.

##**Skosmos**

http://skosmos.org/

Skosmos, developed by the National Library of Finland, is an open source web application for browsing controlled vocabularies. Skosmos was built on the basis of prior development (ONKI, ONKI Light) for developing vocabulary publishing tools in the FinnONTO (2003̄–2012) research initiative from the Semantic Computing Research Group.
Skosmos is a web-based tool for accessing controlled vocabularies used by indexers describing documents, and by users searching for suitable keywords. Vocabularies are accessed via SPARQL endpoints containing SKOS vocabularies.
Skosmos provides a multilingual user interface for browsing vocabularies. The languages currently supported in the user interface are English, Finnish, German, Norwegian, and Swedish. However, vocabularies in any language can be searched, browsed and visualized, as long as proper language tags for labels and documentation properties have been provided in the data. 
Skosmos provides an easy to use REST API for read only access to the vocabulary data. The return format is mostly JSON-LD, but some methods return RDF/XML, Turtle, RDF/JSON with the appropriate MIME type. These methods can be used to publish the vocabulary data as Linked Data. The API can also be used to integrate vocabularies into third party software. For example, the search method can be used to provide autocomplete support and the lookup method can be used to convert term references to concept URIs. [6]
The developers of Skosmos recommend using the Jena Fuseki  triple store with the jena text index for large vocabularies. In addition to using a text index, caching of requests to the SPARQL endpoint with a standard HTTP proxy cache such as Varnish can be used to achieve better performance for repeated queries, such as those used to generate index view. 

The previous version of the software (ONKI Light) was tested by UNIPD (see onki_light_report.pdf).
  
## OCLC Terminology Services 

http://www.oclc.org/research/themes/data-science/termservices.html

Seems to be a closed research project of OCLC with limited outputs. The latest prototype was updated in 2012-11, but *no software downloads are available*. Available outputs are a  [one-page-long overview](http://www.oclc.org/content/dam/research/activities/termservices/resources/termservices-overview.pdf), a [resources webpage](http://tspilot.oclc.org/resources/index.html), where some technical and vocabulary resources are available, as well as some [presentations](http://tspilot.oclc.org/resources/overview.pdf) and [articles](https://journals.tdl.org/jodi/index.php/jodi/article/view/114/113).

## MultiTes Pro

http://www.multites.com/

MultiTes is a Windows based tool to create and manage thesauri, taxonomies and other types of controlled vocabularies. It supports:
* ANSI/NISO standard relationships (USE, UF, BT, NT, RT, SN)
* user-defined relationships, classifications, languages and note/comment fields
* polyhierarchies (i.e. multiple BT's)
* multilingual thesauri
* validation of conflicting relationships
* automatic generation of reciprocal relationships
* complete hierarchy display.
* advanced search by string in term, note content, categories, flags, status and TNR.
* export to different formats, including plain text, XML, SKOS/RDF, RTF, XML, CSV, HTML and user configurable formats
* export to HTML creates a simple web site of your thesaurus.
* import terms, relationships, or fully-featured thesauri from text files or from clipboard

## TermTree 2000 

http://www.termtree.com.au/

TermTree 2000 is a Windows-based tool that uses MS uses Access, SQL Server, or Oracle for data storage. The tool verifies the validity of links as the thesaurus is created and automatically constructs all required reverse relationship links. It can import and export TRIM thesauri (a format used by the Towers Records Information Management system), as well as a defined TermTree 2000 tag format.

## WebChoir 

http://webchoir.com

WebChoir is a family of client-server Web applications that provides different utilities for thesaurus management in multiple DBMS platforms. It is a hierarchical information organizing and searching tool that enables one to create and search varieties of hierarchical subject categories, controlled vocabularies, and taxonomies based on either predefined standards or a user defined structure, and is then exported to an XML-based format.
It has two subsystems:
 * *LinkChoir* that allows indexers to describe information sources using terminology organized in TermChoir.
 * *SeekChoir* is a retrieval system that enables users to browse thesaurus descriptors and their references (broader 
terms, related terms, synonyms, and so on).

## Synaptica 

http://www.synaptica.com

Synaptica is is a client-server Web application that can be installed locally on a client’s intranet or extranet server. Thesaurus data is stored in a SQL server or Oracle database.  The application supports the creation of electronic thesauri in compliance with the ANSI/NISO standard. Synaptica allows the exchange of thesauri in CSV (comma separated values) text format.

## HIVE

http://cci.drexel.edu/mrc/projects/hive/

HIVE (Helping Interdisciplinary Vocabulary Engineering) is a system that assists in indexing of documents. The system is preloaded with multiple SKOS vocabularies.
The web user interface of HIVE provides concept search and browsing facilities as well as an automated
indexing tool which, when given a text document, suggests concepts from a user-selectable subset of the
vocabularies. The main focus of the tool is on the automated indexing aspect, which is implemented using
the KEA++ algorithm. 

## iQvoc

http://iqvoc.net/

iQvoc is a vocabulary management tool that supports editing and publishing of SKOS datasets. Originally was built for the maintenance of German environmental vocabularies, it has since evolved into a general purpose multi-user SKOS and SKOS XL editing tool. For unauthenticated users, it provides search and browsing access to concepts in the vocabulary. In the following, we will only concentrate on the publishing aspect of iQvoc. The system is developed by innoQ
Deutschland GmbH, a commercial company, but available as open source software.

## CATCH

http://www.cs.vu.nl/STITCH/repository/

The CATCH demonstrator is a SKOS-based vocabulary and alignment repository. It consists of middleware providing vocabulary oriented access services and APIs. One of the components is a vocabulary and alignment browser that can be used to search for concepts, for example to support manual indexing of documents. In the following, we concentrate on the browsing tool of CATCH. 
The online demonstrator of the tool was not functional, and the source code is not available for local testing.


