# Upgrading Skosmos

(source: https://github.com/NatLibFi/Skosmos/wiki/Upgrading)

1. make a backup of your current installation, especially config.inc and vocabularies.ttl  

2. (make a backup of fuseki files except tdb and lucene)    

3. upgrade your code to the new version: 

  1. download a new version form https://github.com/NatLibFi/Skosmos/releases and replace the files or

  2. switch to the new version tag/branch using e.g. git checkout tags/v1.4 or git checkout v1.4-maintenance

4. migrate (copy) config.inc and vocabularies.ttl to your new installation if necessary

5. update Composer just in case: php composer.phar self-update   

6. update dependencies: php composer.phar update --no-dev   

7. restart Apache (this will clear gettext and APC caches - reloading is not enough!)

### Version specific notes

* Skosmos 1.4 requires Jena Fuseki 1.3.0 or 2.3.0 if you use the JenaText index (NOTE: Fuseki 1.3.1 and 2.3.1 have a bug which affects Skosmos so they are not recommended). 

* You will also need to change the Fuseki configuration and rebuild the text index.
 
* See the InstallFusekiJenaText page for details about the new text index configuration (you will need to add text:storeValues, text:uidField and text:langField settings). 

* Note that these versions of Jena Fuseki require a Java 8 environment, so you may need to upgrade that first. (Check: $ java -version)
