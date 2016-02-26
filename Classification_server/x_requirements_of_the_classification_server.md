# Requirements of the Classification server

(source: https://redmine.phaidra.org/redmine/projects/labtech/wiki/Terminology_services)

## General

* it should support classifications and controlled vocabularies
* resolving the URIs of the terms
  * support for multiple languages
  * support for multiple versions
  * should return the list of subterms
  
* Phaidra independent
* no assumptions from Phaidra about the contents (set of classifications can differ on instances, its locally managed)
* no too much development needed
* [some background](https://docs.google.com/presentation/d/1RfvU-vwlAN_slJbziEtGO53BMtzENBe4HLAywnWUPOI/edit#slide=id.gb49f17f5f_0_0)

## Technical
* multiple return formats (XML, JSON, HTML), support for Linked Data (returns SKOS/RDF/XML, eg. http://purl.org/bncf/tid/2062)
* support for some kind of standard import format for vocabularies (e.g. SKOS/RDF)
* sparql endpoint / support for search (needed for Phaidra, at least now we support it)
* it needs to be able to support classifications/vocabularies which do not yet support linked data (do not have URIs)
  * some of the currently used classifications do not have LD/URIs so if terminology services do not support this we would need to use 2 interfaces for Phaidra
* nice to have: it can be able to use external terminology services (eg dewey.info, then we maybe don't have to import it locally)

