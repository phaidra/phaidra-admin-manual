# Current Phaidra-related service of Skosmos

##General classifications
###Thesauri

| **Name** | **uriSpace and sparqlGraph** | **sparqlEndpoint / Source to download** |**Format** | **Text-index / TDB Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| **EuroVoc (4.2)**| http://eurovoc.europa.eu/ |  http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes  |  online ![](Images/tick.png) |
| | | http://eurovoc.europa.eu/http://open-data.europa.eu/en/data/dataset/eurovoc/ resource/8e0272e9-d12a-4e78-9fe7-8e8ceb5535d8 |.rdf |  900 MB | local ![](Images/tick.png) |
| **Getty Art & Architecture Thesaurus ** |http://vocab.getty.edu/aat/ | http://vocab.getty.edu/sparql | SKOS | *No* | online ![](Images/delete.png) |
| | | http://vocab.getty.edu/dataset/aat/full.zip | .nt | 5,9 GB | local ![](Images/tick.png)|  
| **Getty Thesaurus of Geographic Names** | http://vocab.getty.edu/tgn/ | http://vocab.getty.edu/sparql | SKOS | *No* | online ![](Images/delete.png)| 
| | | http://vocab.getty.edu/dataset/tgn/full.zip | .nt |69 GB | (local) ![](Images/tick.png) |
| **Getty Union List of Artist Names** | http://vocab.getty.edu/ulan/ | http://vocab.getty.edu/sparql | SKOS | *No* | online ![](Images/delete.png)| 
| | | http://vocab.getty.edu/dataset/ulan/full.zip | .nt |16 GB | (local) ![](Images/tick.png) |
|  **UNESCO Thesaurus** | http://skos.um.es/unescothes/ | http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes | online ![](Images/tick.png) |
|  |   | http://skos.um.es/unescothes/ |  .rdf| 320 MB  | local ![](Images/tick.png) |
| **STW Thesaurus for Economics** | http://zbw.eu/stw/ | http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes  | online ![](Images/tick.png) |
| | | http://zbw.eu/stw/version/latest/about | .rdf / .nt / .ttl | ![](Images/question_mark.png) |  (local) ![](Images/tick.png)  |
| **AGROVOC - Multilingual agricultural thesaurus** | http://aims.fao.org/aos/agrovoc/ | http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes | online ![](Images/tick.png) |
| **GND Subject Categories** | http://d-nb.info/standards/vocab/gnd/gnd-sc | https://github.com/jneubert/ swdskos/tree/master/var/2016-02/src |  rdf |  ![](Images/question_mark.png) | local ![](Images/tick.png)|

###Taxonomies

| **Name** | **uriSpace and sparqlGraph** | **sparqlEndpoint / Source to download** |**Format** | **Text-index / TDB Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| **ÖFOS 2012 - Statistik Austria** | http://www.statistik.at/ | doesn't exist | -  | - | online ![](Images/delete.png)  | 
|  |  | http://www.statistik.at/ KDBWeb/kdb_DownloadsAnzeigen.do?KDBtoken=ignore | CSV, PDF, XLS | 900 MB  | local ![](Images/tick.png)  |

###Vocabularies

| **Name** | **uriSpace and sparqlGraph** | **sparqlEndpoint / Source to download** |**Format** | **Text-index / TDB Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| **COAR - Resource Type Vocabulary** |  http://purl.org/coar/resource_type/ | http://vocabularies.coar-repositories.org/sparql/repositories/coar | *SKOS-XL* | ![](Images/question_mark.png) | online ![](Images/delete.png) |
| | | http://vocabularies.coar-repositories.org/pubby/page/ resource_type.xml | SKOS-XL | 60 MB  | local ![](Images/tick.png) |


## Phaidra's classifications
### Phaidra's general classifications - local

| **Name** | **uriSpace and sparqlGraph** | ** Source** |**Format** | **Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| **The ACM Computing Classification System [1998 Version]**| http://www.acm.org/about/class/1998 | Phaidra's DB | MySQL | 1,2 MB | ![](Images/tick.png) | 
| **Physics and Astronomy Classification Scheme** | https://www.aip.org/pacs | Phaidra's DB | MySQL | 4,6 MB | ![](Images/tick.png) |
| **Basisklassifikation** | http://www.gbv.de/verbundbibliotheken/Basisklassifikation | Phaidra's DB | MySQL | 1,7 MB | ![](Images/tick.png) |
| **Dewey Decimal Classification** | http://www.oclc.org/dewey/ | Phaidra's DB | MySQL | 1,3 MB | ![](Images/tick.png) |
| **BIC Standard Subject Qualifiers** | http://www.bic.org.uk/bic-standard-subject-categories/ | Phaidra's DB | MySQL | 533 KB | ![](Images/tick.png) |
| **BIC Standard Subject Categories** | http://www.bic.org.uk/bic-standard-subject-qualifiers/ | Phaidra's DB | MySQL | 1,6 MB | ![](Images/tick.png) | 
| **ÖFOS 2002** | http://www.statistik.at/2002 | Phaidra's DB | MySQL | 1,3 MB | ![](Images/tick.png) | 

### Phaidra's controlled vocabularies - local

| **Name** | **uriSpace and sparqlGraph** | ** Source** |**Format** | **Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| LOM 2.2 Status | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB  |  ![](Images/tick.png) |
| LOM 2.3.1 Role | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 39 KB |  ![](Images/tick.png) |
| LOM 4.4.1.1 Type | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 2 KB |  ![](Images/tick.png) |
| LOM 4.4.1.2 Name | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB |  ![](Images/tick.png) |
| LOM 9.1 Purpose | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 1 KB |  ![](Images/tick.png) |
| LOM 5.1 Interactivity Type | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB |  ![](Images/tick.png) |
| LOM 5.2 Learning Resource Type | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 10 KB |  ![](Images/tick.png) |
| LOM 5.3 Interactivity Level | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 4 KB |  ![](Images/tick.png) |
| LOM 5.4 Semantic Density Interactivity Level | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB |  ![](Images/tick.png) |
| LOM 5.5 Intended End User Role | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB  |  ![](Images/tick.png) |
| LOM 5.6 Context | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB |  ![](Images/tick.png) |
| LOM 5.8 Difficulty | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 4 KB |  ![](Images/tick.png) |
| LOM 10.1 Hochschulschriftentyp | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 14 KB |  ![](Images/tick.png) |
| Maßeinheiten | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 5 KB |  ![](Images/tick.png) |
| HISTKULT Stempel | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 15 KB |  ![](Images/tick.png) |
| HISTKULT Abbildungen | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 4 KB |  ![](Images/tick.png) |
| HISTKULT Bezugsnummern | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 16 KB |  ![](Images/tick.png) |
| Universität Wien Objektidentifikatoren | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 7 KB |  ![](Images/tick.png) |
| Medium - Book Reiter | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB |  ![](Images/tick.png) |
| info-eu-repo-AccessRights | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB |  ![](Images/tick.png) |
| info-eu-repo-Version-type | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 4 KB |  ![](Images/tick.png) |
| Metadata quality check | http://phaidra.org/phvoc | Phaidra's DB | MySQL | 3 KB |  ![](Images/tick.png) |



### Phaidra's custom classifications - local

| **Name** | **uriSpace and sparqlGraph** | ** Source** |**Format** | **Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| European Projects |  http://phaidra.org/phclass | Phaidra's DB | MySQL | 12 KB  |  ![](Images/tick.png) |
| Univerität Wien |http://phaidra.org/phclass | Phaidra's DB | MySQL | 84 KB |  ![](Images/tick.png) |