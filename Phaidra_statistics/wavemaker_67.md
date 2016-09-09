#  Optional: WaveMaker Studio 6.7

The application has been built using the open source version of WaveMaker Studio 6.7 

**Wavemaker 6.7 is not required to deploy the application in your premises
**


**Wavemaker 6.7 is   required to modify the application**

##SSL
If your application server does not support SSL, you can turn off SSL in the security settings of the application (using WaveMaker 6.7).

#JDBC over SSH
it is possible to implement JDBC calls that require SSH Tunneling. The Java code is included but commented out. You will need to edit the code in WaveMaker 6.7, rebuild and deploy the application.

##Deployment Configuration Settings  with WaveMaker 6.7
You will need to configure the deployment settings to match your environment:

*  Application Ports (i.e. 8080 for http, 8443 for SSL)
*  Database name
* Host (where the database runs)
* Database Ports
* User (database user allowed to see the Phaidra Statistics database and its tables)
* Password 



##Deploying with WaveMaker 6.7
* import zip of the application
* File,New Deployment, Deploy to WAR file, 
* Enter your settings (see above)
* Deploy and generate WAR file. Copy this WAR file to the webapps directory of your application server