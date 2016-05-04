# Available tools for classification

(source: https://redmine.phaidra.org/redmine/projects/labtech/wiki/Terminology_services#candidates)

##**ONKI**

* Finnish Ontology Library Service for a collection of several ontologies and vocabularies
* Implementation: Onki Light (now Skosmos)
  * With very nice REST API, build on top of php and[ Apache Jena](http://jena.apache.org/) (a free and open source Java framework for building Semantic Web and Linked Data applications).
  * Open source - MIT license.
  * The software was tested by unipd, report is attached (onki_light_report.pdf).
  * Currently, this software is used also by FAO/AGROVOC

## **TemaTres** GPUv2 

* [wiki](http://r020.com.ar/tematres/wiki/doku.php) in spanish with a lot of information
* [Demo here](http://vogon.cab.unipd.it/tematres/) in Padua (is not) available (login: yuri.carrer@unipd.it password: tematres )
* TemaTres is an open source vocabulary server, web application to manage and exploit vocabularies, thesauri, taxonomies and formal representations of knowledge, that are stored in a MySQL database, and provides the created thesauri in SKOS (or Zthes) format
* Require PHP, MySql and HTTP Web server.
* Web application for management formal representations of knowledge, thesauri, taxonomies and multilingual vocabularies
  * one can export and publish controlled vocabulary and in many metadata schemas (SKOS-COre, Dublin Core, MADS, JSON, etc.)
* latest version (TemaTres 2.0) was modified at 2015-08-26, can be downloaded from [here](http://sourceforge.net/projects/tematres/files/latest/download)

Main features:
* SPARQL endpoint (SPARQL Protocol and RDF Query Language)
* Meta-terms: define facets, collections or arrays of terms
* Support for multilingual thesaurus
* Expose vocabularies with powerful web services
* Search terms suggestion (did you mean...?)
* Display terms in multiple deep levels in the same screen
* Search expansion
* Vocabulary harmonization features: equivalent, no equivalent and partial terms with other vocabularies
* Relationship between terms (BT/NT, USE/UF, RT)
* No limits to number of terms, alternative labels, levels of hierarchy, etc
* Systematic or alphabetic navigation
* Search
* Complete export in XML format (Zthes, TopicMaps, MADS, Dublin Core,VDEX, BS 8723, SiteMap, SQL)
* Complete export in RDF format (Skos-Core)
* Complete export in txt
* Scope notes, Historical and Bibliographical notes
* User management
* Terms and user supervision
* Duplicates terms control
* Free terms control
* Quality assurance functions (Duplicates and free terms, ilegal relations)
* Multilingual interface
* Easy Install
* Utility to import thesauri from tabulated textfiles
* Unique code for each term
* "Edit in place" features for terms and codes.
* Terminology mapping, multilingual terminology mapping
* Term reports for editors
* Workflow: candidate, accepted and rejected terms
* Allow to create user-defined relationships
* Relationships between terms and web entities
* Allow to define published and hidden labels
* Export to WXP (WordPress XML)
* Import and export data in Skos-core

## SKOS Shuttle
SKOS Shuttle supports you in building, maintaining and operating taxonomies, thesauri and ontologies. It allows operating on basically any external cloud based RDF SESAME compliant repositories and editing of Triples directly. SKOS Shuttle integrates a REST API to create, manage, navigate thesauri. It uses internal and also external RDF repositories (e.g. blazegraph), and accesses securely all information through SSL/DIGEST. It provides comprehensive security (Rights, Groups, User and Project Management) and a very smart Orphan Concept Analysis and direct “deorphanization” without SPARQL. RDF Import/Exportis possible in 6 different formats (…).
Systematic Assistant helps you with different descriptor systems.
The SKOS Shuttle API provides a full REST API that can be embedded into any application. The API requires authentication to provide online services.


## PoolParty

PoolParty [https://www.poolparty.biz/] is a commercial world-class semantic technology suite that offers sharply focused solutions to your knowledge organization and content business. 
As a semantic middleware, PoolParty enriches your information with valuable metadata and links your business and content assets automatically.
The PoolParty Taxonomy & Thesaurus Manager is a world-class tool to build and maintain your information architecture. It differentiates from other market solutions for taxonomy management through powerful capabilities like corpus analysis that can be extended gradually.
The PoolParty thesaurus manager enables practitioners to start their work with limited training. Subject matter experts can model their fields of expertise without IT support.
PoolParty taxonomy management software applies SKOS, knowledge graphs and APIs. With PoolParty, you can import existing taxonomies and thesauri (e.g. from Excel) and export them in different standard formats.

## GINCO (CeCiLL v2 license)

* GINCO is a free software developed by the French Ministry of Culture and Communication for managing vocabularies. 
* It implements the principles defined in the ISO standard on "Thesauri and interoperability with other vocabularies" particularly on "Thesauri for information retrieval".
* From [this GitHub repository](https://github.com/culturecommunication/ginco) can be downloaded, and could be tested.
* The given links to the [Installation](http://culturecommunication.github.io/ginco/doc/INSTALL.md) and [Demo guides](http://culturecommunication.github.io/ginco/doc/VM_INSTALL.md) are not working.

## OCLC Terminology Services 

* seems to be a finished research project with limited outputs
* latest prototype was updated in 2012-11, but *no software downloads are available*
* available outputs are:
  *[ one-page-long overview](http://www.oclc.org/content/dam/research/activities/termservices/resources/termservices-overview.pdf)
  * [resources webpage](http://tspilot.oclc.org/resources/index.html), where some technical and vocabulary resources are available
  * some [presentations](http://tspilot.oclc.org/resources/overview.pdf) and [articles](https://journals.tdl.org/jodi/index.php/jodi/article/view/114/113)

## MultiTes

* is a Windows based tool that provides
 * support for ANSI/NISO relationships
 * user defined relationships and comment fields for an unlimited number of thesauri (both monolingual and multilingual)

## TermTree 2000 

* Windows-based tool
* uses Access, SQL Server, or Oracle for data storage.
* it can import and export
  * TRIM thesauri (a format used by the Towers Records Information Management system),
  * a defined TermTree 2000 tag format.

## WebChoir 

* is a family of client-server Web applications
* that provides different utilities for thesaurus management in multiple DBMS platforms,
* is a hierarchical information organizing and searching tool that enables one to create and search varieties of hierarchical subject categories, controlled vocabularies, and taxonomies based on either predefined standards or a 
user defined structure, and is then exported to an XML-based format.
* it has two subsystems:
  * *LinkChoir* that allows indexers to describe information sources using terminology organized in TermChoir.
  * *SeekChoir* is a retrieval system that enables users to browse thesaurus descriptors and their references (broader 
terms, related terms, synonyms, and so on).

## Synaptica 

* is a client-server Web application
* that can be installed locally on a client’s intranet or extranet server,
* thesaurus data is stored in a SQL server or Oracle database,
* the application supports the creation of electronic thesauri in compliance with the ANSI/NISO standard,
* the application allows the exchange of thesauri in CSV (comma separated values) text format.

## **ThManager**

* is an Open Source Tool, for creating and visualizing SKOS RDF vocabularies
* Windows executable, source, manuals, etc. can be downloaded from [here](http://sourceforge.net/projects/thmanager/)
* the latest version of ThManager was launched in 2006, so it was not tested in a Windows 7 environment
* developed by the Advanced Information Systems Laboratory of the University of Zaragoza
* implemented in Java using [Apache Jena](http://jena.apache.org/) API
* facilitates the management of thesauri and other types of controlled vocabularies, such as taxonomies or classification schemes
* Multi-platform (Windows, Unix) with the minimum requirement of having installed a Java virtual machine
* Multilingual (developed following the Java internationalization methodology), Spanish and English versions are available, but with little effort, other languages could be supported
* Selection and filtering of the thesauri stored in the local repository
* Description of thesauri by means of metadata is in compliance with a Dublin Core based application profile for thesaurus
* Main features:
  * Visualization of thesaurus concepts (alphabetically, in hierarchical structure, properties of selected concepts)
  * Search of concepts ("equals", "starts with" and "contains")
  * Edition of thesaurus content (creation of concepts, deletion of concepts, and update of concept properties)
  * Export of thesauri (including thesaurus metadata) according to SKOS format
  * Extraction of related concepts in WordNet

* Available vocabularies in ThManager:
  * AGROVOC
  * DCType
  * GEMET
  * ISO639
  * UNESCO
  * etc.

* ThManager does not provide any SPARQL endpoint!

## Protégé

(see [protégé](http://protege.stanford.edu/))

Protégé  is a free, open-source ontology editor and framework  for building intelligent systems. 
Protégé is supported by a strong community of academic, government, and corporate users, who use Protégé to build knowledge-based solutions in areas as diverse as biomedicine, e-commerce, and organizational modelling.
WebProtégé is an ontology development environment for the Web that makes it easy to create, upload, modify, and share ontologies for collaborative viewing and editing. WebProtégé fully supports the latest OWL 2 Web Ontology Language. The highly configurable user interface creates the perfect environment for beginners and experts alike. Collaboration features abound, including sharing and permissions, threaded notes and discussions, watches and email notifications. RDF/XML, Turtle, OWL/XML, OBO, and other formats available for ontology upload and download.

* too complex for editing and visualizing such a simple model as SKOS.
* provides too many options not specifically adapted to the type of relations in SKOS.

## SKOS Play

(to be added)

## HIVE

HIVE (Helping Interdisciplinary Vocabulary Engineering) is a system that assists in indexing of documents
(Greenberg et al., 2011; White et al., 2013). The system is pre­loaded with multiple SKOS vocabularies.
The web user interface of HIVE provides concept search and browsing facilities as well as an automated
indexing tool which, when given a text document, suggests concepts from a user­selectable subset of the
vocabularies. The main focus of the tool is on the automated indexing aspect, which is implemented using
the KEA++ algorithm (Medelyan, 2009). 

## iQvoc

iQvoc is a vocabulary management tool that supports editing and publishing of SKOS datasets (Bandholtz et al., 2010; Bandholtz et al., 2011). Originally built for the maintenance of German environmental vocabularies, it has since evolved into a general purpose multi­user SKOS and SKOS XL editing tool. For
unauthenticated users, it provides search and browsing access to concepts in the vocabulary. In the
following, we will only concentrate on the publishing aspect of iQvoc. The system is developed by innoQ
Deutschland GmbH, a commercial company, but available as open source software.

## CATCH

The CATCH demonstrator (van der Meij, 2010) is a SKOS­based vocabulary and alignment repository. It
consists of middleware providing vocabulary­oriented access services and APIs. One of the components is
a vocabulary and alignment browser that can be used to search for concepts, for example to support manual indexing of documents. In the following, we concentrate on the browsing tool of CATCH. 
* the online demonstrator of the tool was not functional, and the source code is not available for local testing


## OpenSKOS
