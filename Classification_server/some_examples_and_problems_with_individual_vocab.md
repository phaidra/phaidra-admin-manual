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


COAR Resource Type Vocabulary  defines concepts to identify the genre of a resource. Such resources, like publications, research data, audio and video objects, are typically deposited in institutional and thematic repositories or published in journals.

The main problem with COAR is that it only represents labels using SKOS-XL properties. Skosmos doesn't support SKOS-XL currently. 

Unfortunately, the remote SPARQL endpoint of COAR ([http://vocabularies.coar-repositories.org/sparql/repositories/coar](http://vocabularies.coar-repositories.org/sparql/repositories/coar)) cannot be used either, because the COAR endpoint data currently is not SKOS Core, but SKOS-XL. 

Since we wanted to use COAR data in our Classification Server, we have converted to SKOS Core labels using both owlart converter and SPARQL Update Queries (see Getting and setting vocabularies). 

Another problem was with the COAR Resource Type vocabulary that the imported SKOS-XL version doesn't contain the skos:narrower tag, just the skos:broader, which is not enough for Skosmos. To solve the problem, we used the Skosify tool (see Getting and setting vocabularies), which checks the dependencies, and pairs the broader and narrower concepts. But Skosify creates skos:narrower as a pair of skos:broadMatch, wich is not correct. 
Therefore we had to apply some tricks (renaming skos:broadMatch before skosifying and naming back afterwards).

COAR Resource Type Vocabulary contains the dct:created data in xsd:http://www.w3.org/2001/XMLSchema#dateTime format (e.g. "2015-06-03T20:06:48Z"^^xsd:dateTime), that should be displayed in the "New" tab of Skosmos. Unfortunately the current version (1.8) can not handle this format, only the xsd:date (e.g. "2015-06-03"^^xsd:date). Therefore we had to transform the data form xsd:dateTime format (e.g. "2015-06-03T20:06:48Z") to xsd:date format (e.g. "2015-06-03") and apply ^^xsd:date instead of ^^xsd:dateTime.

### ÖFOS

ÖFOS is the Austrian version of the Field of Science and Technology Classification (FOS 2007), maintained by Statistics Austria. The Austrian classification scheme for branches of science (1-character and 2-character) is a further development modified for Austrian data.
ÖFOS can be downloaded in PDF and CSV format, but neither in SKOS structure (in xml/rdf, turtle or N-Triples) format, nor Linked Open Data through a SPARQL Endpoint is available.
Since we have received it directly from Statistics Austria in Excel format, the simplest way of converting it to SKOS was using VBA macros. These macros simply reads the content of the Excel file, extend them with the appropriate RDF and SKOS labels, and writes the to the desired xml/rdf or ttl format.
