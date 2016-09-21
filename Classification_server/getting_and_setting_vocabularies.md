# Getting and setting vocabularies

The basic usage of our Classification Server is to store the classifications locally (if its access time is acceptable), and we also provide the links to the remote SPARQL endpoints of the classifications, if they are available.
If you want to use certain vocabularies locally, you have to get it in the right format, and upload it to the local SPARQL server, that is to Jena Fuseki.

## Downloading and converting vocabularies 

Vocabularies can be downloaded from the original dataset provider (e.g. from Getty, COAR, Statistics Austria, etc.), or in case of a small dataset, it can be created manually. The vocabulary needs to be expressed using SKOS Core representation in order to publish it via Skosmos, but SKOS-XL representation or even files in Excel can be also easily converted to SKOS Core. 

### Downloading vocabularies in Windows

(source: https://redmine.phaidra.org/redmine/projects/labtech/wiki/Skosmos#vocabularies)

1. Create a new directory called vocabularies in Skosmos folder (c:\xampp\htdocs\skosmos)
2. Open a browser and go to the vocabulary that you want to use (e.g. COAR - Resource Type Vocabulary or Getty TGN voabulary
3. Download (save) the vocabulary as an RDF file (in XML, Turtle, or N-Triples syntax) (e.g. resource_types.xml or tgn_7011179.rdf) into c:\xampp\htdocs\skosmos\vocabularies

### Converting SKOS-XL to plain SKOS Core fomat using owlart

For the SKOS-XL to SKOS Core conversion you can use for example the owlart (https://bitbucket.org/art-uniroma2/owlart/downloads) converter.

1. Download library 'owlart-2.3-dist.zip' from https://bitbucket.org/art-uniroma2/owlart/downloads

2. copy inputfile e.g. 'skosxl.xml' into workbench/rdf_files
3. create in 'workbench/output' directory output file e.g. 'resultSkos.rdf'
4. make config file in workbench/config dir e.g. workbench/config/skosxl2skos.config and set 'baseuri', 'namespace', and 'defaultScheme' according to your project settings.
Here is an example of the config file:

```
###ModelFactory Implementation: required
modelFactoryImplClassName=it.uniroma2.art.owlart.sesame2impl.factory.ARTModelFactorySesame2Impl

###modelConfigClass Implementation: not required
modelConfigClass=it.uniroma2.art.owlart.sesame2impl.models.conf.Sesame2NonPersistentInMemoryModelConfiguration

###modelConfigFile : not required in Sesame2, though we have to disable inference, which is set to true by default
modelConfigFile=workbench/config/model_sesame-noinference.config

modelDataDir=workbench/storage

baseuri=http\://purl.org/coar/resource_type/
namespace=http\://phaidra.org/
defaultScheme=http\://phaidra.org/
```
5. run library with dependencies in project root directory(dir above workbench directory):
5.1. windows:

java -classpath ".;dependency\commons-codec-1.2.jar;dependency\commons-httpclient-3.1.jar;dependency\commons-io-2.4.jar;dependency\commons-logging-1.0.4.jar;dependency\guava-15.0.jar;dependency\jackson-core-2.4.0.jar;dependency\log4j-1.2.16.jar;dependency\opencsv-2.0.jar;dependency\owlart-api-2.3.jar;dependency\owlart-sesame2impl-1.3.jar;dependency\sesame-onejar-2.7.10.jar;dependency\slf4j-api-1.6.1.jar;dependency\slf4j-log4j12-1.6.1.jar" it.uniroma2.art.owlart.utilities.transform.SKOSXL2SKOSConverter workbench\config\skosxl2skos.config workbench\rdf_files\skosxl.xml workbench\output\resultSkos.rdf false

5.2. linux:

java -classpath ".:dependency/commons-codec-1.2.jar:dependency/commons-httpclient-3.1.jar:dependency/commons-io-2.4.jar:dependency/commons-logging-1.0.4.jar:dependency/guava-15.0.jar:dependency/jackson-core-2.4.0.jar:dependency/log4j-1.2.16.jar:dependency/opencsv-2.0.jar:dependency/owlart-api-2.3.jar:dependency/owlart-sesame2impl-1.3.jar:dependency/sesame-onejar-2.7.10.jar:dependency/slf4j-api-1.6.1.jar:dependency/slf4j-log4j12-1.6.1.jar" it.uniroma2.art.owlart.utilities.transform.SKOSXL2SKOSConverter workbench/config/skosxl2skos.config workbench/rdf_files/skosxl.xml workbench/output/resultSkos.rdf false

6. Result skos file is in workbench/output/resultSkos.rdf
7. tested in java version "1.8.0_72"

### Converting SKOS-XL to plain SKOS Core fomat using SPARQL Update query

You can also convert SKOS-XL labels to SKOS Core labels by executing this SPARQL Update query:

```
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
 ```
 
 ### Converting MS Excel format to SKOS/TTL
 
 For converting the MS Excel format to SKOS file in Turtel format we have created and apllied the followin VBA Macro:
 
``` 
Sub Create_ttl()
'
' Create_ttl Macro
' Create ttl file
'
' Keyboard Shortcut: Ctrl+t
'


Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Select
Dim startCell As String
startCell = ActiveCell.Address

'1-digit-concept

Dim startCode As Integer
startCode = CInt(ActiveCell.Value)

Worksheets("ttl").Activate
ActiveSheet.Range("A1").Select

ActiveCell.FormulaR1C1 = "oefos:"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = startCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = "rdf:type skos:Concept;"
ActiveCell.Offset(1, -2).Select

ActiveCell.FormulaR1C1 = "skos:notation"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = startCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = ";"

ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:prefLabel """
ActiveCell.Offset(0, 1).Select
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(0, 1).Select
Selection.Copy
Worksheets("ttl").Activate
ActiveSheet.Paste
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = """@de;"
    
ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:prefLabel """
ActiveCell.Offset(0, 1).Select
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(0, 1).Select
Selection.Copy
Worksheets("ttl").Activate
ActiveSheet.Paste
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = """@en;"
ActiveCell.Offset(1, -2).Select

Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(2, -2).Select

Dim three_digitCell As String
three_digitCell = ActiveCell.Address
  
Do
     
    If ActiveCell.Value > 100 And ActiveCell.Value < 1000 Then
        
        Selection.Copy
        Worksheets("ttl").Activate
        ActiveCell.Select
        ActiveCell.FormulaR1C1 = "skos:narrower oefos:"
        ActiveCell.Offset(0, 1).Select
        ActiveSheet.Paste
        ActiveCell.Offset(0, 1).Select
        ActiveCell.FormulaR1C1 = ";"
        ActiveCell.Offset(1, -2).Select
     End If
    
    Worksheets("TeilA_2012_Deutsch_Englisch").Activate
    ActiveCell.Offset(1, 0).Select
    
Loop Until (ActiveCell.Value = startCode + 1)

Worksheets("ttl").Activate
ActiveCell.Select
ActiveCell.FormulaR1C1 = "skos:inScheme oefos:"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = 0
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = ";"
ActiveCell.Offset(1, -2).Select
        
ActiveCell.FormulaR1C1 = "skos:topConceptOf oefos:"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = 0
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = "."
ActiveCell.Offset(1, -2).Select

ActiveCell.FormulaR1C1 = ""
ActiveCell.Offset(1, 0).Select

'3-digit-concepts

Dim three_digitCode As Integer
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
Range(three_digitCell).Select
three_digitCode = CInt(ActiveCell.Value)

Dim four_digitCell As String
four_digitCell = Range(three_digitCell).Offset(1, 0).Address

Dim new_three_digits As Boolean
new_three_digits = True

Do '3-digit-concepts

If new_three_digits Then

Worksheets("TeilA_2012_Deutsch_Englisch").Activate
Range(three_digitCell).Select '?

Worksheets("ttl").Activate

ActiveCell.FormulaR1C1 = "oefos:"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = three_digitCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = "rdf:type skos:Concept;"
ActiveCell.Offset(1, -2).Select

ActiveCell.FormulaR1C1 = "skos:notation"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = three_digitCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = ";"

ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:prefLabel """
ActiveCell.Offset(0, 1).Select
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(0, 1).Select
Selection.Copy
Worksheets("ttl").Activate
ActiveSheet.Paste
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = """@de;"
    
ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:prefLabel """
ActiveCell.Offset(0, 1).Select
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(0, 1).Select
Selection.Copy
Worksheets("ttl").Activate
ActiveSheet.Paste
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = """@en;"

Worksheets("ttl").Activate
ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:broader oefos:"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = startCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = ";"
ActiveCell.Offset(1, -2).Select
        
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(1, -2).Select
         
Do
      
    If ActiveCell.Value > 1000 And ActiveCell.Value < 10000 Then
        
       Selection.Copy
       Worksheets("ttl").Activate
       ActiveCell.Select
       ActiveCell.FormulaR1C1 = "skos:narrower oefos:"
       ActiveCell.Offset(0, 1).Select
       ActiveSheet.Paste
       ActiveCell.Offset(0, 1).Select
       ActiveCell.FormulaR1C1 = ";"
       ActiveCell.Offset(1, -2).Select
       
    End If
    
    Worksheets("TeilA_2012_Deutsch_Englisch").Activate
    ActiveCell.Offset(1, 0).Select
      
Loop While (ActiveCell.Value > 1000 Or ActiveCell.Value = "")
        
Worksheets("ttl").Activate
ActiveCell.Offset(-1, 2).Select
ActiveCell.FormulaR1C1 = "."
ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = ""
ActiveCell.Offset(1, 0).Select


End If 'new_three_digits

'4-digit-concepts

Worksheets("TeilA_2012_Deutsch_Englisch").Activate

Range(four_digitCell).Select

Dim four_digitCode As Integer
four_digitCode = CInt(ActiveCell.Value)


Worksheets("ttl").Activate
ActiveCell.FormulaR1C1 = "oefos:"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = four_digitCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = "rdf:type skos:Concept;"
ActiveCell.Offset(1, -2).Select

ActiveCell.FormulaR1C1 = "skos:notation"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = four_digitCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = ";"

ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:prefLabel """
ActiveCell.Offset(0, 1).Select
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(0, 1).Select
Selection.Copy
Worksheets("ttl").Activate
ActiveSheet.Paste
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = """@de;"
    
ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:prefLabel """
ActiveCell.Offset(0, 1).Select
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(0, 1).Select
Selection.Copy
Worksheets("ttl").Activate
ActiveSheet.Paste
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = """@en;"

Worksheets("ttl").Activate
ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = "skos:broader oefos:"
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = three_digitCode
ActiveCell.Offset(0, 1).Select
ActiveCell.FormulaR1C1 = ";"
ActiveCell.Offset(1, -2).Select
        
Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(1, -2).Select

Dim j As Integer

j = 0

Do
    Selection.Copy

    Worksheets("ttl").Activate
    ActiveCell.Select
    ActiveCell.FormulaR1C1 = "skos:narrower oefos:"
    ActiveCell.Offset(0, 1).Select
    ActiveSheet.Paste
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = ";"
    ActiveCell.Offset(1, -2).Select

    Worksheets("TeilA_2012_Deutsch_Englisch").Activate
    ActiveCell.Offset(1, 0).Select

    j = j + 1

Loop Until IsEmpty(ActiveCell)

Worksheets("ttl").Activate
ActiveCell.Offset(-1, 2).Select
ActiveCell.FormulaR1C1 = "."
ActiveCell.Offset(1, -2).Select
ActiveCell.FormulaR1C1 = ""
ActiveCell.Offset(1, 0).Select

'6-digit-concepts

Worksheets("TeilA_2012_Deutsch_Englisch").Activate
ActiveCell.Offset(-j - 1, 0).Select

Dim code As String
code = ActiveCell.Value

ActiveCell.Offset(1, 0).Select

Dim i As Integer

For i = 1 To j

    Selection.Copy

    Worksheets("ttl").Activate

    ActiveCell.Select
    ActiveCell.FormulaR1C1 = "oefos:"
    ActiveCell.Offset(0, 1).Select
    ActiveSheet.Paste
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = "rdf:type"
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = "skos:Concept;"

    ActiveCell.Offset(1, -3).Select
    ActiveCell.FormulaR1C1 = "skos:notation"
    ActiveCell.Offset(0, 1).Select
    ActiveSheet.Paste
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = ";"

    ActiveCell.Offset(1, -2).Select
    ActiveCell.FormulaR1C1 = "skos:prefLabel """
    ActiveCell.Offset(0, 1).Select
    Worksheets("TeilA_2012_Deutsch_Englisch").Activate
    ActiveCell.Offset(0, 1).Select
    Selection.Copy
    Worksheets("ttl").Activate
    ActiveSheet.Paste
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = """@de;"

    ActiveCell.Offset(1, -2).Select
    ActiveCell.FormulaR1C1 = "skos:prefLabel """
    ActiveCell.Offset(0, 1).Select
    Worksheets("TeilA_2012_Deutsch_Englisch").Activate
    ActiveCell.Offset(0, 1).Select
    Selection.Copy
    Worksheets("ttl").Activate
    ActiveSheet.Paste
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = """@en;"

    ActiveCell.Offset(1, -2).Select
    ActiveCell.FormulaR1C1 = "skos:broader oefos:"
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = code
    ActiveCell.Offset(0, 1).Select
    ActiveCell.FormulaR1C1 = "."

    ActiveCell.Offset(1, -2).Select
    ActiveCell.FormulaR1C1 = ""

    ActiveCell.Offset(1, 0).Select

    Worksheets("TeilA_2012_Deutsch_Englisch").Activate
    ActiveCell.Offset(1, -2).Select

Next i

ActiveCell.Offset(1, 0).Select

new_three_digits = False
If ActiveCell.Value > 100 And ActiveCell.Value < 1000 Then
    three_digitCell = ActiveCell.Address
    three_digitCode = CInt(ActiveCell.Value)
    new_three_digits = True
    ActiveCell.Offset(1, 0).Select
End If

    four_digitCell = ActiveCell.Address
    ''four_digitCode = CInt(ActiveCell.Value)
   

Loop Until (ActiveCell.Value = startCode + 1) '3-digit-concepts

End Sub
```

## Creating your own classifications

If you want to create your classifications from scratch you have to represent your classification using the SKOS data model. SKOS, which stands for Simple Knowledge Organization System, is a W3C standard, based on other Semantic Web standards (RDF and OWL), that provides a way to represent controlled vocabularies, taxonomies and thesauri. Specifically, SKOS itself is an OWL ontology and it can be written out in any RDF syntax.

For some guidance, see the SKOS Primer [1] and introductory articles [2,3]. An excellent general introduction to RDF and Linked Data is given for example in the Linked Data book [4] available for free online. For specifics on what aspects of SKOS and other RDF vocabularies Skosmos supports, see the wiki page Data Model [5]. 

[1] https://www.w3.org/TR/skos-primer/ 
[2] http://www.dataversity.net/introduction-to-skos/
[3] http://www.ala.org/alcts/resources/z687/skos 
[4] http://linkeddatabook.com/ 
[5] https://github.com/NatLibFi/Skosmos/wiki/Data-Model

## Skosify the downloaded, converted, created classifications (optional)

If the SKOS file was downloaded from external resources, or it has been converted from other formats, or was created from scratch, it is recommended to pre-process the vocabularies using a SKOS proofing tool, like Skosify. This will ensure, e.g., that the broader/narrower relations work in both directions, and that related relationships are symmetric. Skosify will report and try to correct lots of potential problems in SKOS vocabularies. It can also be used to convert non-SKOS RDF data into SKOS. 

An online version of the Skosify tool is (usually) available here: https://code.google.com/p/skosify/, and use as follows
1. Select the vocabulary to be checked as input
2. Leave the default options as they are
3. Click on the Process button
After successful conversion process you will get a processed vocabulary that you can download and rename (e.g. as checked_resource_types.xml) into a local folder.

The offline Skosify tool requires Python (2.x or 3.x) and the rdflib library. It should run fine even on Windows after installing them.

If there are special characters (e.g. ä, Ä, ü, Ü, ö, Ö, etc.) in the SKOS file to be skosified, the Skosify tool may decode them to an unusable format.

## Uploading files to Fuseki

If you want to use Skosmos for accessing classifications in your local SPARQL triple store you have to upload the datasets to Fuseki. First, you have to consider if Fuseki will run either in memory or in a predefined folder, usually called TDB. 
You also have to consider that in a SPARQL triple store there is always a default (unnamed) graph, and there can also be multiple named graphs. 
You can upload datasets to Fuseki online, when Fuseki is running, or offline, when Fuseki is not running.

### Running Fuseki in the memory

If you run Fuseki in memory, then all uploads and updates (if you allow that) will be temporary.

### Runing Fuseki in the TDB

If you run Fuseki in the TDB, then the uploads and updates will remain there even if we exit from Fuseki and restart it.

### Named and default graph in SPARQL triple store

In a SPARQL triple store there is a default (unnamed) graph, and there can be multiple named graphs. In other words, there is only one default graph (with no name), but there can be any number of named graphs on a SPARQL endpoint/dataset. The URI namespaces can be used as graph names. E.g. http://vocab.getty.edu/tgn/ would store Getty's TGN data.

### Uploading files online

To upload online you can use the control panel of the web interface of Fuseki or you can use command line instructions. 

When uploading datasets online to Fuseki through its control panel, you can set the Graph to “default” or provide a graph name.

If you use a graph name when uploading a dataset to Fuseki, you have to make sure of giving the same graph name to this dataset in skosmos:sparqlGraph (e.g. http://vocab.getty.edu/tgn/) by setting Skomos vocabularies in vocabularies.ttl.

#### Using the web interface of Fuseki

1. start Fuseki server
2. open Fuseki's web interface on [http://skosmos.phaidra.org:3030/](http://skosmos.phaidra.org:3030/)
3. Select Server Management / Control Panel
4. Select Dataset: /ds
5. File upload / Choose Files

##### To the default graph

1. Graph: default
2. Upload

##### To a named graph

1. Graph: *write here the graph name according to the skosmos:sparqlGraph parameter in vocabularies.ttl*
2. Upload

#### From the command line in Fuseki's folder on Linux

If you want to upload datasets online to Fuseki from the command line, you have two options: the s-put and s-post utilities, which are part of Fuseki. 

The Fuseki file upload handling is not very good at processing large files. It will load them first into memory, only then to the on-disk TDB database (and also the Lucene/jena text index). It can to run out of memory on the first step ("OutOfMemoryError: java heap space" is a typical error message when this happens). If you give several GB memory to Fuseki (for example Setting JVM heap to 8 GB: export JVM_ARGS=-Xmx8000M) it should be able to upload large (several hundreds of MB) files, though it might take a while and you may want to restart Fuseki afterwards to free some memory.

##### s-put 

You can use s-put, if you want to add a single data file to a dataset using a graph name (an URI) or using the default graph. If there is something already at that graph name (URI) or in the default graph, it will be replaced. 

1. Start Fuseki server (with text index) from its directory:
  ```./fuseki-server --config jena-text-config.ttl```
2. ```./s-put *SPARQL-server* *Graph-name* *data-to-be-uploaded*```
    * e.g.: ```./s-put   http://skos.um.es/unescothes/ unescothes.ttl ```

##### s-post

If you want to add new triples to an existing graph without clearing it, you should use the s-post utility.

1. Start Fuseki server (with text index) from its directory:
  ```./fuseki-server --config jena-text-config.ttl```
2. ```./s-post *SPARQL-server* *Graph-name* *data-to-be-uploaded*```
    * e.g.: ```./s-post http://localhost:3030/ds/data http://vocab.getty.edu/aat/ ./vocabularies/skos.rdf```

### Uploading files offline

If you have several and large files to upload to Fuseki, and there are memory problems by uploading, it might be better to do it when Fuseki is offline. 

The dataset and the index can be built in two steps using command line tools: 

#### 1. Build the RDF dataset in TDB with tdbloader

```java -cp $FUSEKI_HOME/fuseki-server.jar tdb.tdbloader --tdb=assembler_file data_file```

E.g.:

```$java -cp ./fuseki-server.jar tdb.tdbloader --tdb=jena-text-config.ttl --graph=http://vocab.getty.edu/aat/ ./vocabularies/ontology.rdf```
  
  where
  * jena-text-config.ttl is a configuration file 
  *  --graph=http://vocab.getty.edu/aat/ is the named graph according to the skosmos:sparqlGraph parameter in vocabularies.ttl 
  
Using the tbdloader you have to shut down Fuseki (since only one process can use the TDB at the same time). For the tdbloader you have to provide a configuration file, the graph name with which you want access the vocabulary, and the file name you want to upload. You can use tdbloader for several files in sequence, then you can build the text index as a separate step with the jena text indexer tool.
If you allow updates to the dataset through Fuseki, the configured index will be automatically updated on every modification. This means that you do not have to run the above mentioned jena.textindexer after updates, only when you want to rebuild the index from scratch.

You can use the same .ttl file (/var/www/skosmos/jena-fuseki1-1.3.0/jena-text-config.ttl)
that you used for the Fuseki database and text index configuration, i.e. the one documented here:
https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText#configuration
Alternatively, use one of the TDB utilities tdbloader or tdbloader2 which are better for bulk loading:
$JENA_HOME/bin/tdbloader --loc=directory  data_file

A tutorial for using tdbloader with the text index can be found here: 
https://jena.apache.org/documentation/query/text-query.html#building-a-text-index 
Jena Assemblers are recipes (written in RDF) for constructing RDF stores:
https://jena.apache.org/documentation/assembler/index.html
https://jena.apache.org/documentation/assembler/assembler-howto.html

#### 2. Build the text-index from the existing RDF dataset 

You can build the text index with the jena.textindexer tool:

```java -cp $FUSEKI_HOME/fuseki-server.jar jena.textindexer --desc=assembler_file```

E.g.:

```$java -cp ./fuseki-server.jar jena.textindexer --desc=jena-text-config.ttl```


Because a Fuseki assembler description can have several datasets descriptions, and several text indexes, it may be necessary to extract a single dataset and index description into a separate assembler file for use in loading.

A short tutorial of this is included in the jena-text documentation:  https://jena.apache.org/documentation/query/text-query.html#building-a-text-index 

#### Updating the text index

If you allow updates to the dataset through Fuseki, the configured index will automatically be updated on every modification. This means that you do not have to run the above mentioned jena.textindexer after updates, only when you want to rebuild the index from scratch.

### Adding vocabularies to Fuseki server on Windows

In order to add vocabularies to the Fuseki server use the control panel of the Fuseki server from a browser: http://localhost:3030/

1. click on the "add data" button in the middle
2. on the next page (Upload files) click on the "+ select files" button in the middle
3. select the vocabulary (e.g. checked_resource_types.xml) file from the c:\xampp\htdocs\skosmos\vocabularies dictionary
4. click on the "upload now" button


## Checking data in Fuseki server

These SPARQL queries that you could execute in the Fuseki user interface could help to see if there are any problems: 

#### The amount of triples 

* in a named graph 

```
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { ?s ?p ?o } 
}
```

* in the default graph 

```SELECT (COUNT(*) AS ?count) { ?s ?p ?o } ```

This should be a large number, maybe 10 or 20 times the number of concepts. 

#### The number of SKOS concepts 

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#> 
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { 
     ?s a skos:Concept 
   } 
}
```

#### The number of SKOS prefLabels 

```
PREFIX skos: <http://www.w3.org/2004/02/skos/core#> 
SELECT (COUNT(*) AS ?count) { 
   GRAPH <http://vocab.getty.edu/aat/> { 
     ?s skos:prefLabel ?label 
   } 
}
```

## Setting vocabularies

(The vocabularies to show in Skosmos are configured in the file `vocabularies.ttl` which is an RDF file in Turtle syntax.)

First, you need to create a vocabularies.ttl file in the Skosmos directory. 
In this file after defining the prefixes (like rdf, skos, dc) you have to create new sessions for each vocabulary starting with the line :id a skosmos:Vocabulary, void:Dataset; where id is just an identifier that will be used in the URL after /skosmos/.
In vocabularies.ttl you have to set the following required parameters:

1.	the title of the vocabulary in different languages: dc:title "title_of_the_vocabulary"@ language; where language can be: en, de, it, etc.
2.	the category of the vocabulary: dc:subject : category; where category can be: cat_general, or other defined categories in vocabularies.ttl expressed as SKOS. The categorization is used to group the vocabularies shown in the front page. 
3.	the URI namespace for vocabulary objects (these may not overlap): void:uriSpace "URI_namespace"; When you represent your data as RDF, the best practice is to coin a new URI namespace for your data set. Then use that as the value of the uriSpace setting.
4.	the language(s) the vocabulary supports: skosmos:language " language ", " language ", ... ; where language can be: : en, de, it, etc.
5.	the URI of the SPARQL endpoint containing this vocabulary void:sparqlEndpoint <URI_SPARQL_endpoint> ;URI_SPARQL_endpoint can be http://localhost:3030/ if you want to use the vocabulary locally, or the URL of the SPARQL endpoint of the remote vocabulary

It is recommended to set the following optional parameters:
1.	the default language of the vocabulary, if the vocabulary supports multiple languages: skosmos:defaultLanguage " language "; where language can be: : en, de, it, etc.
2.	setting skosmos:showTopConcepts true should display the top level hierarchy - assuming that the dataset contains the skos:hasTopConcept and/or skos:topConceptOf relationships that are necessary for this to work. If you want to enable the Hierarchy tab showing top-level concepts on the vocabulary home page: skosmos:showTopConcepts should be set to "true";
3.	Group index is meant for thematic groupsClass of resources to display as concept groups, or as arrays (subdivisions) of sibling concepts (typical values are skos:Collection or isothes:ConceptGroup): skosmos:groupClass isothes:ConceptGroup; If you don't need this tab, simply remove the skosmos:groupClass setting.
4.	if you do not want Skosmos to query the mapping concept URIs for labels if they haven't been found at the configured SPARQL endpoint: skosmos:loadExternalResources "false";
5.	URI of the main skos:ConceptScheme (instance of the current vocabulary) should be specified if the vocabulary contains multiple skos:ConceptScheme instances skosmos:mainConceptScheme <main_Concept_Scheme_URI>.
6.	if the vocabulary is relatively small (e.g. 100 concepts) you can show the alphabetical index with all the concepts instead of showing only the concepts of one letter at a time: skosmos:fullAlphabeticalIndex "true";. It is not recommended to use fullAlphabeticalIndex for large vocabularies

(See more details of configuring the vocabularies.ttl here: https://github.com/NatLibFi/Skosmos/wiki/Vocabularies)



