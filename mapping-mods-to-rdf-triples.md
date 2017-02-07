The mapping of MODS to RDF triples is probably the way we'd use to transfer metadata to Fedora 4. Mapping UWmetadata directly might be too complicated, but we might map UWmetadata to MODS and then use the existing mappings from MODS to RDF. 

Also, we have some objects with MODS anyway.

# Mapping MODS to RDF triples

## MODS (Metadata Object Description Schema)

MODS is an XML schema for a bibliographic element set that may be used for a variety of purposes, and particularly for library applications. It is a derivative of the MARC 21 bibliographic format (MAchine-Readable Cataloging) and as such includes a subset of MARC fields, using language-based tags rather than numeric ones. Definitions (semantics) of MODS elements may be found in the appropriate section.

### XML Structures

By using the XML schema language, MODS defines main elements, child elements (i.e. subelements), and attributes of elements and subelements. Content of elements are included in the lowest level elements so as to avoid "mixed content", which is when some elements contain character data interspersed with child elements. For instance, if <titleInfo> contains subelements for <title>, <partNumber>, <partName>, then <titleInfo> is only a container tag to include the more specific elements <title>, <partNumber>, <partName> and does not contain any content data. (A "container" tag is one that it used only as an element that binds together child elements, but contains no data other than tags.)

Attributes may be associated with elements at any level and are defined with the element with which they are associated. They serve to modify the element. Common attributes that occur throughout the schema include: type, encoding, and authority.

## MODS RDF Ontology

MODS RDF is an RDF ontology for MODS. As MODS is an XML schema for a bibliographic element set,   MODS RDF is an expression of that element set in RDF.

MODS/RDF is modeled as an OWL ontology. 

## MODS XML to RDF

MODS RDF may be used to create born-RDF MODS, or it may be used to create an RDF description corresponding to an existing MODS XML record.

http://www.loc.gov/standards/mods/modsrdf/primer-2.html

### Top Level Triple

The top level RDF triple for a MODS RDF description is of the form:

`{MODS resource}`    `is-a`    `#ModsResource`

Where `{MODS resource}` is the resource described.

`#ModsResource` is a blank node and becomes the subject for subsequent triples corresponding to top-level MODS elements.  

`{MODS resource}`  is of the form:
`http://www.loc.gov/mods/rdf/v1#{someIdentifier}` 

The MODS XML to RDF stylesheet assigns a value to `{someIdentifier}` as follows:

If the XML record includes an identifier with type ‘modsRDFIdentifier’, the value of that identifier is used.
Otherwise, the value ‘MODS123456’ is assigned.

For example if the record includes:


```
<identifier type=’modsRDFIdentifier’>xyz</identifier>
```

then the top-level triple will be:

```
http://www.loc.gov/mods/rdf/v1#xyz is-a    #ModsResource
```


In XML:


```
<modsrdf:ModsResource rdf:about="http://www.loc.gov/mods/rdf/v1#MODS123456"> 
```

if the record does not include an identifier with `type=’modsRDFIdentifier’` then the top-level triple will be:

```
http://www.loc.gov/mods/rdf/v1#123456  is-a    #ModsResource
```

A user of the stylesheet wishing to ensure that a meaningful identifier will be used for this triple should insert an identifier with `type=’modsRDFIdentifier’` into the record.

### Converting of Top-level MODS Elements

#### MODS `<abstract>`

The following MODS record:

```
<mods>
     <abstract> based on a novel by a  man named Lear </abstract>
</mods>
```
Results in the following RDF description:

```
<rdf:RDF>
     <modsrdf:ModsResource rdf:about="http://www.loc.gov/mods/rdf/v1#MODS123456">
                 <modsrdf:abstract> based on a novel by a man named Lear</modsrdf:abstract>
     </modsrdf:ModsResource>
</rdf:RDF>

```
#### MODS `<classification>`

The following XML:

```
<classification authority="lcc">HE380.8</classification>
<classification authority="xyz">abc.xyz</classification>
```
Results in the following RDF:



```
<class:lcc>HE380.8</class:lcc>
<!-- -->
<modsrdf:classificationGroup>
   <ClassificationGroup>
      <classificationGroupScheme>xyz</classificationGroupScheme>
      <classificationGroupValue>abc.xyz</classificationGroupValue>
   </ClassificationGroup>
</modsrdf:classificationGroup>
```

#### MODS `<identifier>`

The following XML:

```
<identifier type="isbn">0-937383-18-X</identifier>
<identifier type="lccn">##2001336783</identifier>
<identifier type="local">xjz123</identifier>
```
Results in the following RDF:

```
<identifier:isbn>0-937383-18-X</identifier:isbn>
<identifier:lccn>##2001336783</identifier:lccn>
<modsrdf:identifierGroup>
                <IdentifierGroup>
                                <identifierGroupType>local</identifierGroupType>
                                <identifierGroupValue>xjz123</identifierGroupValue>
                </IdentifierGroup>          
</modsrdf:identifierGroup>
```

#### MODS `<language>`

The following XML:

```
<languageTerm authority="iso639-2b" >eng</languageTerm>
                                <languageTerm>english</languageTerm>
```
Results in the following RDF:



```
<LanguageOfResource rdf:resource="http://id.loc.gov/vocabulary/language#eng"/>
<LanguageOfResource >english</LanguageOfResource>
```
# Mapping uwmetadata to MODS

## UWmetadata 

PHAIDRA metadata includes also locally developed metadata (e.g., UWmetadata, crosswalked to DC and some other
metadata solutions) implemented as minimum information needed to fulfill requirements of different designated communities (Digital Humanities Centers,OAPEN Foundation, OpenAIRE, e- Infrastructures Austria, EUROPEANA Libraries,
local CRIS, institutional repositories). 

_E.g. t.b.c._