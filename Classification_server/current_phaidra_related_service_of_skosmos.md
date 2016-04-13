# Current Phaidra-related service of Skosmos

##Thesauri

| **Name** | **uriSpace and sparqlGraph** | **sparqlEndpoint / Source to download** |**Format** | **Text-index / TDB Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| **EuroVoc (4.2)**| http://eurovoc.europa.eu/ |  http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes  |  online ![](Images/tick.png) |
| | | http://eurovoc.europa.eu/http://open-data.europa.eu/en/data/dataset/eurovoc/ resource/8e0272e9-d12a-4e78-9fe7-8e8ceb5535d8 |.rdf |   ![](Images/question_mark.png) 900 MB | local ![](Images/tick.png) |
| **Getty Art & Architecture Thesaurus ** |http://vocab.getty.edu/aat/ | http://vocab.getty.edu/sparql | SKOS | *No* | online ![](Images/delete.png) |
| | | http://vocab.getty.edu/dataset/aat/full.zip | .nt | 5,9 GB | local ![](Images/tick.png)|  
| **Getty Thesaurus of Geographic Names** | http://vocab.getty.edu/tgn/ | http://vocab.getty.edu/sparql | SKOS | *No* | online ![](Images/delete.png)| 
| | | http://vocab.getty.edu/dataset/tgn/full.zip | .nt |69 GB |  ![](Images/tick.png) |
| **Getty Union List of Artist Names** | http://vocab.getty.edu/ulan/ | http://vocab.getty.edu/sparql | SKOS | *No* | online ![](Images/delete.png)| 
| | | http://vocab.getty.edu/dataset/ulan/full.zip | .nt |16 GB | local ![](Images/tick.png) |
|  **UNESCO Thesaurus** | http://skos.um.es/unescothes/ | http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes | online ![](Images/tick.png) |
|  |   | http://skos.um.es/unescothes/ |  .rdf| 25 MB  | local ![](Images/tick.png) |
| **STW Thesaurus for Economics** | http://zbw.eu/stw/ | http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes  |![](Images/tick.png) |
| | | http://zbw.eu/stw/version/latest/about | .rdf / .nt / .ttl | ![](Images/question_mark.png) |  local ![](Images/delete.png)|
| **AGROVOC - Multilingual agricultural thesaurus** | http://aims.fao.org/aos/agrovoc/ | http://api.skosmos.dev.finto.fi/sparql | SKOS | Yes | online ![](Images/tick.png) |


##Taxonomies

| **Name** | **uriSpace and sparqlGraph** | **sparqlEndpoint / Source to download** |**Format** | **Text-index / TDB Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| **ÖFOS 2012 - Statistik Austria** | http://www.statistik.at/ | doesn't exist | -  | - | online ![](Images/delete.png)  | 
|  |  | http://www.statistik.at/KDBWeb/kdb_DownloadsAnzeigen.do?KDBtoken=ignore | CSV, PDF, XLS |  ![](Images/question_mark.png)  | offline ![](Images/tick.png)  |

##Vocabularies

| **Name** | **uriSpace and sparqlGraph** | **sparqlEndpoint / Source to download** |**Format** | **Text-index / Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| **COAR - Resource Type Vocabulary** |  http://purl.org/coar/resource_type/ | http://vocabularies.coar-repositories.org/sparql/repositories/coar | *SKOS-XL* | ![](Images/question_mark.png) | online ![](Images/delete.png) |
| | | http://vocabularies.coar-repositories.org/pubby/page/ resource_type.xml | SKOS-XL |  ![](Images/question_mark.png) | offline ![](Images/tick.png) |

## General classifications

| **Name** | **uriSpace and sparqlGraph** | **sparqlEndpoint / Source to download** |**Format** | **Text-index / Size** | **Works** |
| -- | -- | -- | -- | -- | -- |
| The ACM Computing Classification System [1998 Version]| 1:7 | 2:7 | 3:7 | 4:7 | 5:7 | 
|  |  |  |  |  | ? |
|Physics and Astronomy Classification Scheme| 1:8 | 2:8 | 3:8 | 4:8 | 5:8 |
|  |  |  |  |  | ? |
| European Projects | 1:9 | 2:9 | 3:9 | 4:9 | 5:9 |
|  |  |  |  |  | ? |
| Basisklassifikation | 1:10 | 2:10 | 3:10 | 4:10 | 5:10 |
|  |  |  |  |  | ? |
| Dewey Decimal Classification | 1:11 | 2:11 | 3:11 | 4:11 | 5:11 |
|  |  |  |  |  | ? |
| BIC Standard Subject Qualifiers | 1:12 | 2:12 | 3:12 | 4:12 | 5:12 |
|  |  |  |  |  | ? |
| BIC Standard Subject Categories | 1:13 | 2:13 | 3:13 | 4:13 | 5:13 | 
