# Using Skosmos with Fuseki

## In Windows

### 1. Start Apache web server

1. Launch XAMPP Control Panel
2. Start the Apache web server
3. Start a web browser
4. Check if the Apache web server is running: open http://localhost/
(You should first try »Status« on the left navigation to make sure everything works fine, i.e. PHP must be activated.)

###2. Start Jena Fuseki

1. Open a command prompt and cd into apache-jena-fuseki directory (cd c:\apache-jena-fuseki-2.3.0)
2. Run Fuseki Server from the command prompt: fuseki-server --update --mem /ds
(--update --mem / ds options mean that allowing updates data set will be in the memory.)
(After starting the server the last INFO will tell us the port (e.g. 3030), where the server is available.)
3. To check if the Fuseki Server is running open the control panel from the browser: http://localhost:3030/ 
and see if in the top right corner the Server status: is a green disk.

### 3. Adding vocabularies to Fuseki server (in case of local vocabularies)

In order to read (preferably checked vocabularies (e.g. checked_resource_types.xml or checked_tgn_7011179.rdf) into the Fuseki server use the control panel of the Fuseki server from a browser: http://localhost:3030/

1. click on the "add data" button in the middle
2. on the next page (Upload files) click on the "+ select files" button in the middle
3. select the vocabulary (e.g. checked_resource_types.xml or checked_tgn_7011179.rdf) file from the c:\xampp\htdocs\skosmos\vocabularies dictionary
4. click on the "upload now" button

### 4. Start Skosmos
Enter in the address bar of your web browser http://localhost/skosmos
If you receive the error message: "Error: Dependencies managed by Composer missing. Please run "php composer.phar install", then set up PHP dependencies again. (It may happen after downloading a new version of Skosmos.)

## In skosmos.phaidra.org
