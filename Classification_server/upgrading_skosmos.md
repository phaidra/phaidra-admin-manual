g# Upgrading Skosmos

\(source: [https://github.com/NatLibFi/Skosmos/wiki/Upgrading](https://github.com/NatLibFi/Skosmos/wiki/Upgrading)\)

1. cd to the folder of Skosmos \(/var/www/skosmos/\)

2. make a backup of your current Skosmos installation, especially config.inc and vocabularies.ttl

3. You can also make a backup of fuseki files (except tdb and lucene), but it is not required, since Skomos upgrade normally doesn't effect fuseki

4. upgrade your code to the new version:

  4/A download a new version (e.g. v1.8) of Skosmos form [https://github.com/NatLibFi/Skosmos/](https://github.com/NatLibFi/Skosmos/):  



```
wget "https://github.com/NatLibFi/Skosmos/archive/v1.8.tar.gz"

```

   unzip it: 

```
tar -xf v1.8.tar.gz

```


   or

   4/B switch to the new version tag/branch using  
      1. `git fetch` to fetch new versions  
      2. `git checkout` the version you want \(e.g. `git checkout v1.8-maintenance`\)

5. migrate \(copy\) config.inc and vocabularies.ttl to your new installation if necessary (normally the new version contains config.inc.dist and vocabularies.ttl.dist, so the original config.inc and vocabularies.ttl files, as well as the translation files will remain unattached).

6. update Composer just in case: `php composer.phar self-update`

7. update dependencies: `php composer.phar update --no-dev`

8. restart Apache \(this will clear gettext and APC caches - reloading is not enough!\):
 `service httpd restart` on CENTOS and `service apache2 restart` on UBUNTU 
9. clear your browser cache, as it may contain JavaScript or CSS files from the old version (on Chrome: History / Clear browsing data / 'Cookies and other site and plugin data', 'Cached images and files')

### Version specific notes

* Skosmos 1.4 requires Jena Fuseki 1.3.0 or 2.3.0 if you use the JenaText index \(NOTE: Fuseki 1.3.1 and 2.3.1 have a bug which affects Skosmos so they are not recommended\).

* You will also need to change the Fuseki configuration and rebuild the text index.

* See the InstallFusekiJenaText page for details about the new text index configuration \(you will need to add text:storeValues, text:uidField and text:langField settings\).

* Note that these versions of Jena Fuseki require a Java 8 environment, so you may need to upgrade that first. \(Check: $ java -version\)



