# Requirements of the Classification server

(source: https://redmine.phaidra.org/redmine/projects/labtech/wiki/Terminology_services)

## General

Generally the Classification Server should
* support classifications and controlled vocabularies
* resolve the URIs of different terms
* support multiple languages (the “official” languages in Phaidra are English, German, Italian and Serbian)
* support for multiple versions of classifications
* return the list of subterms (narrower concepts)
* be Phaidra independent

Phaidra should have no assumptions about the contents, which means that the set of classifications can differ on instances that are locally managed. 
We were looking for solutions that do not require too much development efforts and have lower costs.

* [some background](https://docs.google.com/presentation/d/1RfvU-vwlAN_slJbziEtGO53BMtzENBe4HLAywnWUPOI/edit#slide=id.gb49f17f5f_0_0)

## Technical

Technically the Classification Server should 
* return the terms in multiple formats (such as XML, JSON, RDF, TTL, eg. http://purl.org/bncf/tid/2062)
* supports standard import formats for vocabularies (e.g. SKOS/RDF, TTL, N-TRIPLES)
* support Linked Data (in SKOS/RDF/XML formats)
* provide a SPARQL endpoint
* provide a comprehensive search needed for Phaidra
* support classifications/vocabularies that do not yet support linked data (do not have URIs)
* use external terminology services, e.g. dewey.info, so that we do not necessarily have to import it locally

Some of the currently used classifications do not have LD/URIs so if terminology services do not support this we would need to use 2 interfaces for Phaidra.
