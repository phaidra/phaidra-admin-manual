# Knowledge Organization Systems
Classifications can be considered as a collection of organized knowledge, therefore the technical background of classification is based on knowledge organization systems. In knowledge organization systems we usually store the knowledge in form of triplets, as subject-predicate-object, or object-attribute-value.

Classifications can be represented in Simple Knowledge Organization
Systems (SKOS) [1] as a Resource Description Framework (RDF) vocabulary. Simple Knowledge Organization System (SKOS) is a W3C recommendation designed for representation of thesauri, classification schemes, taxonomies, subject-heading systems, or any other type of structured controlled vocabulary.

Using RDF allows knowledge organization systems to be used in distributed, decentralized metadata applications. Decentralized metadata is becoming a typical scenario, where service providers want to add value to metadata harvested from multiple sources. [2]

Each SKOS concept is defined as an RDF resource, and each concept can have RDF properties attached, which include one or more preferred terms, alternative terms or synonyms, and language specific definitions and notes. Established semantic relationships are expressed in SKOS and intended to emphasize concepts rather than terms/labels. [3]

It is clear, that SKOS - as a modern well established standard - can (potentially) support formal alignments and hierarchical grouping of concepts using different SKOS relations (e.g. skos:exactMatch, skos:closeMatch, skos:narrower, skos:broader, skos:related), translation of concept labels, and URI-based mapping to similar concepts in other KOS.
These technologies (RDF, SKOS, etc.) provides the possibility that data can be queried, and inferences can be drawn from the datasets stored in Knowledge Organisation Systems. It is also important to make available the relationships among data even in different datasets, to create a collection of interrelated datasets on the Web that is referred as Linked Data.



