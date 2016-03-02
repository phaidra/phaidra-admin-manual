# Converting SKOS-XL to plain SKOS core fomat


1. Download library 'owlart-2.3-dist.zip' from https://bitbucket.org/art-uniroma2/owlart/downloads

2. copy inputfile e.g. 'skosxl.xml' into workbench/rdf_files
3. create in 'workbench/output' directory output file e.g. 'resultSkos.rdf'
4. make config file in workbench/config dir e.g. workbench/config/skosxl2skos.config and set 'baseuri', 'namespace', and 'defaultScheme' according to your project settings.
Here is an example of the config file:
<code>
#ModelFactory Implementation: required
modelFactoryImplClassName=it.uniroma2.art.owlart.sesame2impl.factory.ARTModelFactorySesame2Impl

#modelConfigClass Implementation: not required
modelConfigClass=it.uniroma2.art.owlart.sesame2impl.models.conf.Sesame2NonPersistentInMemoryModelConfiguration

#modelConfigFile : not required in Sesame2, though we have to disable inference, which is set to true by default
modelConfigFile=workbench/config/model_sesame-noinference.config

modelDataDir=workbench/storage

baseuri=http\://purl.org/coar/resource_type/
namespace=https\://www.gitbook.com/book/phaidra/phaidra-admin-manual/edit#
defaultScheme=http\://my.defaultScheme/
</code>
5. run library with dependencies in project root directory(dir above workbench directory):
5.1. windows:

java -classpath ".;dependency\commons-codec-1.2.jar;dependency\commons-httpclient-3.1.jar;dependency\commons-io-2.4.jar;dependency\commons-logging-1.0.4.jar;dependency\guava-15.0.jar;dependency\jackson-core-2.4.0.jar;dependency\log4j-1.2.16.jar;dependency\opencsv-2.0.jar;dependency\owlart-api-2.3.jar;dependency\owlart-sesame2impl-1.3.jar;dependency\sesame-onejar-2.7.10.jar;dependency\slf4j-api-1.6.1.jar;dependency\slf4j-log4j12-1.6.1.jar" it.uniroma2.art.owlart.utilities.transform.SKOSXL2SKOSConverter workbench\config\skosxl2skos.config workbench\rdf_files\skosxl.xml workbench\output\resultSkos.rdf true

5.2. linux:

java -classpath ".:dependency/commons-codec-1.2.jar:dependency/commons-httpclient-3.1.jar:dependency/commons-io-2.4.jar:dependency/commons-logging-1.0.4.jar:dependency/guava-15.0.jar:dependency/jackson-core-2.4.0.jar:dependency/log4j-1.2.16.jar:dependency/opencsv-2.0.jar:dependency/owlart-api-2.3.jar:dependency/owlart-sesame2impl-1.3.jar:dependency/sesame-onejar-2.7.10.jar:dependency/slf4j-api-1.6.1.jar:dependency/slf4j-log4j12-1.6.1.jar" it.uniroma2.art.owlart.utilities.transform.SKOSXL2SKOSConverter workbench/config/skosxl2skos.config workbench/rdf_files/skosxl.xml workbench/output/resultSkos.rdf true

6. Result skos file is in workbench/output/resultSkos.rdf
7. tested in java version "1.8.0_72"
