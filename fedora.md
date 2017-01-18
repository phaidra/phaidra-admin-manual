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

