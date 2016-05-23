# Some examples and problems with individual vocabularies

In this chapter we are going to describe some examples for individual vocabularies that we are using in our Classification Server, that show typical problems and solutions.

## Getty

Getty vocabularies  contain structured terminology for art and other cultural, archival and bibliographic materials. They provide authoritative information for cataloguers and researchers, and can be used to enhance access to databases and web sites.

Therefore, we have tried to upload Getty vocabularies to our own local Fuseki SPARQL endpoint with the jena text index. But unfortunately Getty vocabularies do not work well in Skosmos due to their very large size.

There are two sets of each Getty vocabulary, the "explicit" set and the "full" set (Total Exports). With the "explicit" set, which is smaller, we had to  configure Fuseki to use inference so that the data store can infer the missing triples. With the full set this is not needed, but the data set is much larger so we had difficulties loading it. We could finally upload the full set of Getty’s vocabularies using the tdbloader utility.

### Full set 

(You can to download the full set of Getty's vocabularies from here: [http://vocab.getty.edu/doc/#Total_Exports](http://vocab.getty.edu/doc/#Total_Exports))

The downloaded export file includes all statements (explicit and inferred) of all independent entities.  It's a concatenation of the Per-Entity Exports in NTriples format. Because it includes all required Inference, you can load it to any repository (even one without RDFS reasoning).

1. Load the External Ontologies (**SKOS**, SKOS-XL, ISO 25964) from http://vocab.getty.edu/doc/#External_Ontologies. The purpose is to get descriptions of properties, associative relations, etc.
2. Load the GVP Ontology from http://vocab.getty.edu/ontology.rdf
3. If your repository supports Subproperty/Inverse/Transitive reasoning:
If you want to eliminate the struck-out properties in Reduced SKOS Inference (like we do), execute the SPARQL updates given in Reduced SKOS Inference.
If you don't, these properties will be inferred, which will add 2-3x more statements to your repository.
4. Load the export files:
  * http://vocab.getty.edu/dataset/aat/full.zip: 120 Mb zipped, 1871 Mb unzipped
  * http://vocab.getty.edu/dataset/tgn/full.zip: 340 Mb zipped, 5300 Mb unzipped
  * http://vocab.getty.edu/dataset/ulan/full.zip: TODO Mb zipped, TODO Mb unzipped

| **AAT** | **TGN** | **ULAN** |
| -- | -- | -- |
| AATOut_Full.nt     : subjects | TGNOut_Full.nt     : subj, places | ULANOut_Full.nt     : subj, agents |
| AATOut_Contribs.nt : contribs | TGNOut_Contribs.nt : contribs     | ULANOut_Contribs.nt : contribs     |
| AATOut_Sources.nt  : sources  | TGNOut_Sources.nt  : sources      | ULANOut_Sources.nt  : sources      |

Since the *Out_Full.nt files are very large, they require using the tdbloader:

```
java -cp /fuseki-server.jar tdb.tdbloader --tdb=jena-text-config.ttl --graph=http://vocab.getty.edu/aat/  ./vocabularies/getty_aat/full/ontology.rdf
...
```
### Explicit set

With the "explicit" set, which is smaller, you will need  to configure Fuseki to use inference so that the data store can infer the missing triples. 
(For configuring inference in Jena Fuseki see https://jena.apache.org/documentation/assembler/assembler-howto.html)  

#### Problems with Getty

Getty has its own SPARQL endpoint, but it is not responding in the right way. There seems to be some incompatibility between Skosmos (in practice, the EasyRdf library which is used to perform SPARQL queries) and the Getty SPARQL endpoint.

Even if we could access the Getty SPARQL endpoint, it would most likely be extremely slow to use it with Skosmos, since it doesn't have a text index that Skosmos could use. The lack of a text index would most likely prevent any actual use of Skosmos with the Getty endpoint.

The lack of a text index would most likely prevent any actual use of Skosmos with the Getty endpoint. 

Skosmos needs to have a text index to work with vocabularies of medium to large size. The limit is perhaps a few thousand concepts, depending on the performance of the endpoint / triple store and how much delay is acceptable by the users. 

### COAR Resource Type Vocabulary


COAR only represents labels using SKOS XL properties. Skosmos doesn't support SKOS XL currently (see issue #109 in the GitHub Issues section, e.g. https://github.com/NatLibFi/Skosmos/issues/109 ).

The remote endpoint of COAR (http://vocabularies.coar-repositories.org/sparql/repositories/coar) can not be used. Skosmos requires the data to follow SKOS Core. The COAR endpoint data currently is not SKOS Core. 

If you want to use COAR data locally, so you will first need to convert the data to SKOS Core labels ("plain SKOS"). One tool that can do this for you is here: http://art.uniroma2.it/owlart/documentation/utilities.jsf#skos_utilities

#### 1. Configure in vocabularies.ttl:

     :coarrt a skosmos:Vocabulary, void:Dataset ;
        dc:title "COAR Resource Type Vocabulary - PL - load dataset first!"@en ;
        dc:subject :cat_general ;
        void:uriSpace "http://purl.org/coar/resource_type/";
        skosmos:language "en";
        skosmos:showTopConcepts "true";
        skosmos:loadExternalResources "true";
        skosmos:fullAlphabeticalIndex "true";
        #skosmos:arrayClass isothes:ThesaurusArray ;
        void:sparqlEndpoint <http://localhost:3030/ds/sparql>;
        #skosmos:sparqlGraph <http://purl.org/coar/resource_type/>;
        .
#### 2. Start Fuseki
#### 3. Load COAR Resource Type Vocabulary
If you have it in SKOS XL format only, then execute this SPARQL Update query that converts SKOS XL labels into SKOS Core labels:

    PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
    PREFIX skos:   <http://www.w3.org/2004/02/skos/core#> 
    PREFIX skosxl: <http://www.w3.org/2008/05/skos-xl#> 
    INSERT { 
       ?c skos:prefLabel ?pref . 
       ?c skos:altLabel ?alt . 
       ?c skos:hiddenLabel ?hidden . 
       ?c skos:definition ?def . 
    } 
    WHERE { 
       { ?c skosxl:prefLabel/skosxl:literalForm ?pref } 
       UNION 
       { ?c skosxl:altLabel/skosxl:literalForm ?alt } 
       UNION 
       { ?c skosxl:hiddenLabel/skosxl:literalForm ?hidden } 
      UNION 
       { ?c skos:definition/rdf:value ?def }  
     } 

### ÖFOS - 2012

(to be edited)

You need to represent your classification using the SKOS data model. For some guidance, see 
the SKOS Primer [1] and introductory articles [2,3]. An excellent general introduction to RDF and Linked Data is given for example in the Linked Data book [4] available for free online. For specifics on what aspects of SKOS and other RDF vocabularies Skosmos supports, see the wiki page Data Model [5]. 
[1] https://www.w3.org/TR/skos-primer/ 
[2] http://www.dataversity.net/introduction-to-skos/ 
[3] http://www.ala.org/alcts/resources/z687/skos 
[4] http://linkeddatabook.com/ 
[5] https://github.com/NatLibFi/Skosmos/wiki/Data-Model 

### Examples

(to be edited and completed)

#### For "COAR-Resource Type Vocabulary" set the properties as follow:

dc:title "COAR - Resource Type Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://purl.org/coar/resource_type/";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/ds/sparql> ;
.

#### For sample record of "Getty TGN vocabulary" set the properties as follow:

dc:title "Getty TGN Vocabulary"@en ;
dc:subject :cat_general ;
void:uriSpace "http://vocab.getty.edu/tgn/7011179";
skosmos:language "en";
void:sparqlEndpoint <http://localhost:3030/> ;
.

