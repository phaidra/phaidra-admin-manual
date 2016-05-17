# Connecting to Phaidra

Phaidra will require the Classification Server when the user ingests new items and wants to add metadata to this from a controlled vocabulary, as well as when the user searches for some documents classified with some metadata from a controlled vocabulary and wants to display or resolve them.

The connection between Phaidra and the Classification Server will be realised using the REST API of Skosmos and/or Fuseki. Skosmos provides a REST-style API and Linked Data access to the underlying vocabulary data. The REST API is a read-only interface to the data stored in the Classification Server. 

REST URLs must begin with /rest/v1. Most of the methods return the data as UTF-8 encoded JSON-LD, served using the application/json MIME type. The data consists of a single JSON object which includes JSON-LD context information (in the @context field) and one or more fields which contain the actual data. 
