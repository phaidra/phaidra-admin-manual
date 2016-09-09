#Modify phaidrastatsDB.properties (Hibernate Database Properties)
Once the WAR has been deployed, you will need to modify an XML file to match your database settings.

##File location:
/WEB-INF/Classes/phaidrastatsDB.properties


**Modify ONLY these settings:**
* **connectionURL:**
  change the uri host:port/database

     sample: 

phaidrastatsDB.connectionUrl=jdbc\:mysql\://**mydb.mysqlserver.domain.com\:3306/mydb**?useUnicode\=yes&characterEncoding\=UTF-8&zeroDateTimeBehavior\=convertToNull

* ** username: **your database user
* ** password**

**Once done, simply reload the app in your application server.**


#Spring
WaveMaker Applications use Spring. This and other settings are available are available in the same directory that allow further configuration.

# phaidrastatsDB.properties (Hibernate Database Properties)


phaidrastatsDB.alias=phaistatmysql1

phaidrastatsDB.schemaFilter=.*

phaidrastatsDB.dialect=com.wavemaker.runtime.data.dialect.MySQLDialect

phaidrastatsDB.tableFilter=users,phaidrasites

phaidrastatsDB.reverseNamingStrategy=com.wavemaker.tools.data.reveng.DefaultRevengNamingStrategy

phaidrastatsDB.driverClassName=com.mysql.jdbc.Driver

phaidrastatsDB.username=myuser

phaidrastatsDB.connectionUrl=jdbc\:mysql\://mydb.mysqlserver.domain.comt\:3306/mydb?useUnicode\=yes&characterEncoding\=UTF-8&zeroDateTimeBehavior\=convertToNull

phaidrastatsDB.password= mypassword