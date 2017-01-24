# Fedora

The Fedora Repository open source software is a project supported by the DuraSpace not-for-profit organization. The software has its origins in the Flexible Extensible Digital Object Repository Architecture \(i.e., Fedora\) which was originally designed and developed by researchers at Cornell University. Fedora is a robust, modular, open source repository system for the management and dissemination of digital content. It is especially suited for digital libraries and archives, both for access and preservation. It is also used to provide specialized access to very large and complex digital collections of historic and cultural materials as well as scientific data. Fedora has a worldwide installed user base that includes academic and cultural heritage organizations, universities, research institutions, university libraries, national libraries, and government agencies.

## Digital Object Model in Fedora 3.8

Fedora defines a generic digital object model that can be used to persist and deliver the essential characteristics for many kinds of digital content including documents, images, electronic books, multi-media learning objects, datasets, metadata and many others. This digital object model is a fundamental building block of the Content Model Architecture \(CMA\) and all other Fedora-provided functionality.

Fedora uses a "compound digital object" design which aggregates one or more content items into the same digital object. Content items can be of any format and can either be stored locally in the repository, or stored externally and just referenced by the digital object. The Fedora digital object model is simple and flexible so that many different kinds of digital objects can be created, yet the generic nature of the Fedora digital object allows all objects to be managed in a consistent manner in a Fedora repository.

The Fedora digital object model is defined in XML schema language \(see The [Fedora Object XML](http://www.fedora.info/definitions/1/0/foxml1-0.xsd) - FOXML\).

The basic components of a Fedora digital object are:

* **PID:**
  A persistent, unique identifier for the object.
* **Object Properties:**
  A set of system-defined descriptive properties that are necessary to manage and track the object in the repository.
* **Datastream\(s\):**
  The element in a Fedora digital object that represents a content item.

![](/assets/DOModel.png)

### Datastreams

A Datastream is the element of a Fedora digital object that represents a content item. A Fedora digital object can have one or more Datastreams. Each Datastream records useful attributes about the content it represents such as the MIME-type \(for Web compatibility\) and, optionally, the URI identifying the content's format \(from a format registry\). The content represented by a Datastream is treated as an opaque bit stream; it is up to the user to determine how to interpret the content \(i.e. data or metadata\). The content can either be stored internally in the Fedora repository, or stored remotely \(in which case Fedora holds a pointer to the content in the form of a URL\).

Each Datastream is given a Datastream Identifier which is unique within the digital object's scope. Fedora reserves four Datastream Identifiers for its use, "DC", "AUDIT", "RELS-EXT" and "RELS-INT". Every Fedora digital object has one "DC" \(Dublin Core\) Datastream by default which is used to contain metadata about the object \(and will be created automatically if one is not provided\). Fedora also maintains a special Datastream, "AUDIT", that records an audit trail of all changes made to the object, and can not be edited since only the system controls it. The "RELS-EXT" Datastream is primarily used to provide a consistent place to describe relationships to other digital objects, and the "RELS-INT" datastream is used to describe internal relationships from digital object datastreams. In addition, a Fedora digital object may contain any number of custom Datastreams to represent user-defined content.

![](/assets/DOExample.png)

The basic properties that the Fedora object model defines for a Datastream are as follows:

* **Datastream Identifier:**
  an identifier for the datastream that is unique within the digital object \(but not necessarily globally unique\)
* **State:**
  the Datastream's state: Active, Inactive, or Deleted
* **Created Date:**
  the date/time that the Datastream was created \(assigned by the repository service\)
* **Modified Date:**
  the date/time that the Datastream was modified \(assigned by the repository service\)
* **Versionable:**
  an indicator \(true/false\) as to whether the repository service should version the Datastream \(by default the repository versions all Datastreams\)
* **Label:**
  a descriptive label for the Datastream
* **MIME Type:**
  the MIME type of the Datastream \(required\)
* **Format Identifier:**
  an optional format identifier for the Datastream such as emerging schemes like PRONOM \(a web-based technical registry to support digital preservation services, developed by The National Archives of the United Kingdom\) and the Global Digital Format Registry \(GDRF\)
* **Alternate Identifiers:**
  one or more alternate identifiers for the Datastream \(such identifiers could be local identifiers or global identifiers such as Handles or DOI\)
* **Checksum:**
  an integrity stamp for the Datastream content which can be calculated using one of many standard algorithms \(MD5, SHA-1, etc.\)
* **Bytestream Content:**
  the content \(as a stream resource\) represented or encapsulated by the Datastream \(such as a document, digital image, video, metadata record\)
* **Control Group:**
  the approach used by the Datastream to represent or encapsulate the content as one of four types or control groups \(Internal XML Content, Managed Content, Externally Referenced Content, Redirect Referenced Content\)

### Four Types of Fedora Digital Objects

Although every Fedora digital object conforms to the Fedora object model, as described above, there are four distinct types of Fedora digital objects that can be stored in a Fedora repository. The distinction between these four types is fundamental to how the Fedora repository system works. In Fedora, there are objects that store digital content entities, objects that store service descriptions, objects used to deploy services, and objects used to organize other objects.

#### Data Object

In Fedora, a Data object is the type of object used to represent a digital content entity. Data objects are what we normally think of when we imagine a repository storing digital collections. Data objects can represent such varied entities as images, books, electronic texts, learning objects, publications, datasets, and many other entities. One or more Datastreams are used to represent the parts of the digital content. **A Datastream is an XML element that describes the raw content \(a bitstream or external content\).**

#### Service Definition Object

In Fedora, a Service Definition object or `SDef` is a special type of control object used to store a model of a Service. A Service contains an integrated set of Operations that a Data object supports. In object-oriented programming terms, the `SDef` defines an "interface" which lists the operations that are supported but does not define exactly how each operation is performed. This is also similar to approaches used in Web \(REST\) programming and in SOAP Web services.

#### Service Deployment Object

The Service Deployment object or `SDep` is a special type of control object that describes how a specific repository will deliver the Service Operations described in a `SDef` for a class of Data objects described in a `CModel`. The `SDep` is not executable code but instead it contains information that tells the Fedora repository how and where to execute the function that the `SDep` represents.

#### Content Model Object

The Content Model object or `CModel` is a new specialized control object introduced as part of the CMA. It acts as a container for the Content Model document which is a formal model that characterizes a class of digital objects. It can also provide a model of the relationships which are permitted, excluded, or required between groups of digital objects. All digital objects in Fedora including Data, SDef, SDep, and CModel objects are organized into classes by the CModel object.

## Metadata design patterns in Fedora 4

Fedora 4 presents metadata designers with radical new capabilities, as well as new responsibilities. The main point is that Fedora 4 uses RDF structure.

### Blank nodes

Fedora 4 offers very limited support for the use of blank nodes in metadata. While no response from the API will contain blank nodes, it is possible to send a request to the API containing blank nodes.

### Ordering

RDF, as a graph, is inherently unordered, and this can lead to difficulty when forms of description that presuppose ordering are translated into it. The Fedora community is trying several methods for constructing order in a graph.

## An architecture for complex objects and their relationships

Link: [https://arxiv.org/ftp/cs/papers/0501/0501012.pdf](https://arxiv.org/ftp/cs/papers/0501/0501012.pdf)

\(The article was written when the actual release of Fedora was version 2.0, which includes the semantic web integration that is important for us.\)

This work uniquely integrates advanced content management with semantic web technology. It supports the representation of rich information networks, where the nodes are complex digital objects combining data and metadata with web services and the edges are ontology-based relationships among these digital objects.

The most familiar example is the need to express well-known management relationships among digital resources such as the organization of items in a collection and structural relationships such as the part-whole relationships between individual articles and a journal. While the relationships among digital objects in these familiar applications are mainly hierarchical, we are working with other applications where the relationships are more graph-like.

There are a number of schemes for representing these relationships such as conventional relational databases and formalisms like conceptual graphs, the products of the semantic web initiative such as RDFS, OWL, and highly scalable triple-stores provide extensible open-source solutions for representation, manipulation, and querying these knowledge networks.

By providing both a model for digital objects and repository services to manage them, Fedora is distinguished from work focused on defining and promoting standard XML formats for representing and transmitting complex objects \(e.g., METS, MPEG-21 DIDL, IEEE LOM\). However, Fedora is compatible with these efforts since it has the ability to ingest and export digital objects that are encoded in such XML transmission formats . This allows Fedora to comfortably coexist in the archival framework defined by OAIS.

As a service-based architecture for complex digital objects, Fedora has some commonality with the aDORe architecture developed at the Los Alamos National Laboratory research library. The aDORe system provides a standards-based repository for managing and accessing complex digital objects. Objects are encoded in XML using DIDL and a limited set of object relationships can be expressed using RDF. Object dissemination services are available via OAI-PMH and OpenURL.

### Fedora model for complex objects

The Fedora object model supports the expression of many kinds of complex objects, including documents, images, electronic books, multi-media learning objects, datasets, computer programs, and other compound information entities. Fedora supports aggregation of any combination of media types into complex objects, and allows the association of services with objects that produce dynamic or computed content. The Fedora model also allows the assertion of relationships among objects so that a set of related Fedora objects can represent the items in a managed collection, the components of a structural object like the chapters of a book, or a set of resources that share common characteristics \(defined by semantic relationships\).

Fedora defines a powerful object model for expressing this variety of complex content and their relationships. This object model can be understood from two perspectives:

1. The _representational_ perspective defines a simplified abstraction for understanding Fedora objects, where each object is modeled as a uniquely identified resource projecting one or more views, or representations. From this perspective the internal structure of a digital object is opaque; however, relationships among objects are observable.

2. The _functional_ perspective reveals the object components that underlie the representational perspective and provides the basis for understanding how the Fedora object model relates to the management services exposed in the Fedora repository architecture.

#### Representational View

The representational perspective of the Fedora object model asserts that each digital object can disseminate one or more representations of itself, and that each object can be related to one or more other objects. A familiar example of digital object with multiple representations is a document or image where the content is available in multiple formats. All digital objects, and their individual representations, are identified with Uniform Resource Identifiers \(URIs\). This choice frees the architecture from ties with any identifier resolution system \(e.g., the Handle System\).

This perspective hides complexity and exposes only the access points to content stored in a Fedora repository. The figure below depicts the representational view of three inter-related Fedora objects. The diagram shows a directed graph, where the larger nodes are digital objects, and the smaller nodes are representations of the digital objects. These nodes are linked by two types of arcs – relationship arcs connect digital objects, and representation arcs connect digital objects to their respective representations. **This graph can be expressed as RDF, stored in a triple store,** and queried.

![](/assets/Representational View of Fedora Objects small.PNG)

Each digital object in the diagram has at least one representation, related to its originating  
 digital object by a “hasRep” arc. For example, the node labeled  
 info:fedora/demo:11 is an image digital object with four representations, identified by  
 their respective URIs:

* Dublin Core record, identified as info:fedora/demo:11/DC

* High-resolution image, identified as info:fedora/demo:11/HIGH

* Thumbnail image, identified as info:fedora/demo:11/THUMB

* Image with zoom/pan utility, as info:fedora/demo:11/bdef:2/ZPAN

This figure also demonstrates an example of inter-object relationships. In this example,  
 the node labeled info:fedora/demo:10 is a “collection” with two “items”, the  
 nodes labeled info:fedora/demo:11 and info:fedora/demo:12. These collection-item  
 relationships are expressed by the “hasMember“ arc that emanates from the collection  
object. The inverse “isMemberOf” relationships are not shown in the diagram for  
 simplification.

This simple representational view forms the basis of Fedora’s REST-based access service \(i.e., API-A-LITE\), whereby digital object URIs and representation URIs can be easily converted to service request URLs upon Fedora repositories.

#### Functional View I - Datastreams

While the representational perspective of the Fedora object model provides a simple, access-oriented overlay for digital resources and collections, the functional perspective provides a view of the core underlying data model for Fedora.

In its simplest form a Fedora object is an aggregation of content items, where each content item maps to a representation. The Fedora object model defines a component known as a datastream to represent a content item. A datastream component either encapsulates bytestream content internally or references it externally. In either case that content may be in any media type.

The figure below shows the digital object \(info:fedora/ demo:11\) as an aggregation of datastreams and the one-to-one correspondence of those datastreams to the representations of the digital object that are exposed to accessing clients. In this simple case, each representation of a Fedora object is a simple transcription of the content that lies behind a datastream component.

![](/assets/Fedora Object with PID, Properties, and Datastreams.PNG)

As seen in the above diagram, a digital object has a unique identifier \(PID\) and a set of key descriptive properties. Each datastream contains information necessary to manage a content item in a Fedora repository. These are stored as properties of the datastream as shown in the figure below.

![](/assets/Properties of a Datastream Component.PNG)

Three datastream properties deserve special attention. The FORMAT\_URI refines the media type definition and anticipates the emergence of a global digital format registry such as the GDFR. CONTROLGROUP defines whether the datastream represents either local or remote content. Datastreams with a control group of “Managed” are internal content bytestreams that are under the direct custodianship of the Fedora repository. Datastreams whose control group is “External” or “Redirected” represent content that is stored outside the repository. These datastreams have a CONTENT\_LOCAT property that is a URL pointing to a service point outside the repository that is responsible for providing the content. The ability to create digital objects that aggregate locally managed content with external content is a powerful feature of Fedora, and is useful in a variety of contexts. A good example of a hybrid local/remote object is an educational object where local content is an instructor’s syllabus, lecture notes, and exams, and remote content are primary resources included by-reference from other sites.

#### Functional View II - Disseminators

In addition to the representations described in the previous section, which are direct transcriptions of datastreams, the Fedora object model enables the definition of virtual representations of a digital object. A virtual representation, also known as a dissemination, is a view of an object that is produced by a service operation \(i.e., a method invocation\) that can take as input one or more of the datastreams of the respective digital object. As such, it is a means to deliver dynamic or computed content from a Fedora object.

t.b.c.

#### Functional View III – Object Integrity Components

The Fedora object model defines several metadata entities that pertain to managing the integrity of digital objects. These entities are the object’s relationship metadata, access control policy, and audit trail. To keep the Fedora model simple and consistent, integrity entities are modeled as datastream components with reserved identifiers. As such, the integrity entities are stored like other datastreams, however the Fedora Repository system recognizes them as special and asserts constraints over how they are created and modified. Figure 6 depicts these integrity-oriented entities as special datastreams in a digital object, identified as Relations, Policy, and Audit Trail.

t.b.c.

### Relationships in Fedora

The Fedora object model can be abstractly viewed as a directed graph, consisting of internal arcs that relate digital objects nodes to their representation nodes and external arcs between digital objects.

The Fedora Resource Index module allows storage and query of the graph. This module builds on RDF \(Resource Description Framework\)  primitives.  The Fedora system defines a base relationship ontology that, in the fashion of any RDF properties, can co-exist with domain-specific ontologies from other namespaces. Each digital object’s relationships to other digital objects are expressed in RDF/XML \[20\] within a reserved datastream in the object. The Resource Index is a relationship graph over all digital objects in the repository that is derived by merging the relationships implied by the Fedora object model itself with the relationships explicitly stated in an object’s relationship datastream. The triples representing this graph are then stored in a triple-store providing the capability for searching over the graph.

The combination of representing explicit relationships as RDF/XML in a datastream of a digital object and then mapping them to the Kowari triple store offers the “best of both worlds”. The explicit representation provides the basis for exporting, transporting, and archiving of the digital objects with their asserted relationships to other objects.  The mapping to \(Kowari\) triples provides a graph-based index of an entire repository and the basis for high-performance queries over the relationships. An  added advantage of the dual representation is that the entire triple store can be rebuilt by importing and parsing the XML-based digital objects.

#### Representing object-to-object relationships

Fedora decomposes the structural units \(e.g.  book composed of chapters and diaries consisting of entries\) into separate digital objects. The units can then be reused in a variety of structural compositions. In addition, this lays the basis for expressing other types of non-structural relationships among digital objects such as:

* The organization of individual resources into larger collection units, for the purpose of management, \(OAI-PMH\) harvesting, user browsing, and other uses.
* The relationships among bibliographic entities.

* Semantic relationships among resources such as their relevance to state educational standards or curricula in an educational digital library like the National Science Digital Library.

* Modeling more complex forms of network overlays over the resources in a content repository such as citation links, link structure, friend of a friend, etc.

All of these relationships, including structural relationships, should be expressible both within individual digital objects and among multiple digital objects. For example breaking the components of a structural entity, such as the chapters of a book, into separate digital objects provides the flexibility for reuse of those individual components into other structural units. This is even more important for the other forms of relationships. For example, a single resource may be part of multiple collections or may be relevant for multiple state standards.

The expression of arbitrary, inter-object relationships in Fedora is enabled by a reserved datastream known as the Relations datastream. This datastream allows for a restricted subset of RDF/XML where the subject of each statement must be the digital object within which the datastream is defined.

Since predicates from any vocabulary can be used in Relations, the repository  
manager has considerable flexibility in the kinds of relationships that can be asserted.  
The code segment below shows an example Relations datastream in a Fedora digital object identified  
by the URI, `info:fedora/demo:11`. The RDF/XML refers to three different relationship  
vocabularies \(hypothetical for the purpose of this example\) and asserts the following  
relationships:  
• demo:11 is a member of the collection represented by the object demo:10,  
• demo:11 fulfills the state educational standard represented by the object  
demo:Standard5,  
• demo:11 is a manifestation of the expression represented by the object  
demo:Expression2.

```
<rdf:RDF
   xmlns:rdf ="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:nsdl=”http://nsdl.org/std#”
   xmlns:rel="http://example.org/rel#"
   xmlns:frbr="http://example.org/frbr#">
     <rdf:Description rdf:about="info:fedora/demo:11">
       <rel:isMemberOf rdf:resource="info:fedora/demo:10"/>
       <std:fulfillsStandard rdf:resource="info:fedora/demo:Standard5"/>
       <frbr:isManifestionOf rdf:resource=
             "info:fedora/demo:Expression2"/>
     </rdf:Description>  
</rdf:RDF>
```

#### Object representations and properties in the Resource Index

A Fedora digital object consists of a number of core components such as datastreams and disseminators, which bind to BDefs and BMechs. In addition each Fedora digital object has system metadata or properties. The architecture provides a system-defined ontology to represent the relationships among these core components. For example, the relationships of an object to its representations is expressed using the `<fedora-model:disseminates `predicate as shown in the triple below:

```
<info:fedora/demo:11>
 <fedora-model:disseminates>
 <info:fedora/demo:11/HIGH> 
```

In addition to these relationships, the system-defined ontology also represents object data properties whose range contains date and boolean datatypes, as shown in the triple below:

```
<info:fedora/demo:11/HIGH>
 <fedora-view:lastModifiedDate>
 "2004-12-12T00:22:00"^^xsd:dateTime 
```

 Unlike the relationships expressed in the Relations datastream, these relationships are not explicitly asserted within the digital object. Instead they are derived from the structure of the object itself and mapped into the Resource Index, alongside the relationships represented in the Relations datastreams.

####  Storing and querying the relationship graph

All these relationships – the relationships explicitly stated in the Relations datastream, the relationships implied by the object structure, and the data relationships contained in the object properties – are stored in the Resource Index. This index is automatically updated by the repository service whenever an object structure is modified or its Relations datastream is changed.

The Resource Index handles queries over these relationships. The combination of all relationships into a single graph, and the automated management of that combined graph, enables a powerful and flexible service model. External services may issue queries combining relationships from different name spaces, since they are all RDF properties.  The example below shows a query listing all the representations of all objects that are members of a particular collection.

```
select $dissemination
from <#ri>
where ($object <fedora-view:disseminates> $dissemination)
 and $object <rel:isMemberOf> <demo:10> 
```

 An early design goal of the Resource Index was to allow the use of different triplestores and thus permit the Fedora repository administrator to choose the most appropriate underlying store. To that end, the Resource Index employs a triplestore API similar in spirit to JDBC, to provide a consistent update and query interface to a  variety of triplestores. Extensive testing of both query performance time and query language features ultimately led to the selection of Kowari as the default triplestore.

 The RDF query results naturally take the form of rows of key-value pairs, again similar to the result sets returned by a SQL query. However, it is often useful to work with a sub-graph or a constructed graph based on the original. To this end, the query API may also return triples instead of tuples.

####  Using the relationship graph

The Resource Index interface is exposed in a REST architectural style to provide a stateless query interface that accepts queries by value or by reference. The service has been implemented with an eye toward eventual conformance to the W3C Data Access Working Group's SPARQL protocol for RDF, as it matures.

One example of a service exploiting the Resource Index is the OAI Provider Service that exposes metadata about resources in a Fedora repository. This OAI Provider Service is quite flexible in that it can be configured to allow harvesting not only of static metadata formats, but those that are dynamically produced via service-based disseminations of Fedora objects.

An example of the interaction of this service with the Resource Index is as follows. An external OAI harvester requests qualified Dublin Core records for a particular set of resources from the repository. The OAI Provider service processes this by issuing the query to the Resource Index listed below. This query effectively requests “all disseminations of qualified Dublin Core records of resources that are members of the collection identified as ‘demo:10’”. The significance of requesting disseminations is that the Dublin Core records may not statically exist as datastreams within the object, but they may be derived from another metadata format such as MARC.

```
select $member $collection $dissemination
from <#ri>
where $member <rel:isMemberOf> <info:fedora/demo:10>
 and $member <rel:isMemberOf> $collection
 and $member <rel:isMemberOf> $dissemination
 and $member <fedora-view:disseminates> $dissemination
 and $dissemination <fedora-view:disseminationType>
<info:fedora/*/bdef:OAI/getQualifiedDC> 
```

 The Resource Index query would return the tuples shown below that can provide the basis of an OAI response.



