# Oaiprovider

How to install the oaiprovider, Fedora's OAI-PMH interface.

1. Generate Identify.xml  
```bash
cd /var/www/static.phaidra/proai/
/usr/local/fedora/config/phaidra_tmpl.pl
```

1. Generate Identify.xml  
```bash
cp phaidra/release_2.9/config/oaiprovider-1.1.3/xlink-1999.xsd /usr/share/tomcat6/
```

1. Download the oaiprovider from github
```bash
cd /home/<you>
mkdir oaibuild && cd oaibuild
wget https://github.com/fcrepo/oaiprovider/archive/master.zip
unzip master
```

1. To build it you might need to install ant and ant-trax1
```bash
yum install ant
yum install ant-trax
```

1. Build it
```bash
cd oaiprovider-master
ant release
```

1. Extract it
```bash
cd dist
mkdir extract && cd extract
jar -xf ../oaiprovider.war
```

1. Get the configuration from repository and run the template processing
```bash
svn export https://svn.phaidra.univie.ac.at/phaidra/branches/release_2.9/config/oaiprovider-1.1.3/WEB-INF/ --force
/usr/local/fedora/config/phaidra_tmpl.pl 
```



Note: If the instance should use mpt store as triplestore, you have to add the patched one (as for 2013-10-04). You can get it here, build it into eg mptstore-0.9.6-SNAPSHOT-phaidra.jar, then
```bash
sudo rm oaiprovider-master/lib/mptstore-0.9.3.jar
sudo cp mptstore-0.9.6-SNAPSHOT-phaidra.jar oaiprovider-master/lib/
```
then edit the oaiprovider-master/build.properties file to change the path to the library
```bash
lib.mptstore = lib/mptstore-0.9.6-SNAPSHOT-phaidra.jar
```







