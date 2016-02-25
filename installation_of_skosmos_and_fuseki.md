# Installation and configuration of Skosmos and Fuseki

## On skosmos.phaidra.org

(to be added) 

## On a Windows 7 machine 

Installation on Windows 7 (Professional 64 bit, Service Pack 1, Intel Core i7-56000 CPU, 2.6 GHz, 16 GB RAM) using Java 1.8 (jre1.8.0_40), with installed XAMPP (xampp-win32-1-8-3-4-VC11) environment.

### 1. Install XAMPP

(Apache web server of XAMPP is required in order to run PHP.)

1. Download XAMPP (e.g. XAMPP Windows 5.6.3, xampp-win32-1-8-3-4-VC11-installer.exe from http://en.softonic.com/s/download-xampp-64-bit-windows-7-free-downloads) (Don't be surprised, that it looks like a win32 version, direct 64-bit version is not available, but it will not be a problem.)
2. Run xampp-win32-1-8-3-4-VC11-installer.exe with the default options (install XAMPP into the folder C:\xampp)
(with the installation of XAMPP we have installed Apache and PHP, as its is required for Skosmos)

### 2. Get Skosmos

1. Download Skosmos current master branch from https://github.com/NatLibFi/Skosmos.
2. Unzip Skosmos-master.zip to a skosmos subfolder under c:\xampp\htdocs folder (which should the default DocumentRoot for XAMPP.)
Result: c:\xampp\htdocs\skosmos contains index.php and other files and folders.
(Cloning the code from GitHub would be better, but will be tested in the next turn only.)

### 3. Install Git

(Git is required for running the Dependency Manager in the next step.)

1. Download git form https://git-scm.com/download/win
2. Run Git-2.5.3-64-bit.exe (options: Use Git and optional Unix tools from the Windows Command Prompt; Checkout Windows-style, commit Unix-style line endings; Use Windows' default console window)
3. Restart your computer to make the changes in the PATH environment variable in effect

### 4. Install Dependency Manager for PHP

(Skosmos requires a number of PHP libraries, which will be installed using Composer.)

1. Download the Windows installer of Composer (Composer-Setup.exe) from https://getcomposer.org/doc/00-intro.md#installation-windows.
2. Run Composer-Setup.exe
3. Open command prompt and cd into the skosmos directory (c:\xampp\htdocs\skosmos)
4. Run from this folder the command (this will install the dependencies): composer install --no-dev
(Successful result: lot of components will be installed, lock file will be written, and finally autoload file will be generated.)

If you download a new version of Skosmos (in case of by starting Skosmos you receive the error message: "Error: Dependencies managed by Composer missing. Please run "php composer.phar install"), that requires to set up PHP dependencies again.

### 5. Install Jena Fuseki (later with jena-text extension)

(Jena Fuseki is a SPARQL server and RDF triple store which is the recommended backend for Skosmos. 
The jena-text extension can be used for faster text search.)

1. Download the latest Fuseki distribution jena-fuseki-*.zip from http://jena.apache.org/download/#apache-jena-fuseki
2. Unpack the downloaded file (apache-jena-fuseki-2.3.0.zip) to c:\apache-jena-fuseki-2.3.0

(More details on installing Fuseki with the jena-text extension can be seen at https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText)


### 6. Configure Skosmos

Create config.inc file in the Skosmos directory (c:\xampp\htdocs\skosmos). There are example files named config.inc.dist that you can rename/copy as starting point.

Edit the config.inc file:
1. change the TEMPLATE_CACHE setting like this: define("TEMPLATE_CACHE", "c:/xampp/tmp/skosmos-template-cache");
2. check the default SPARQL endpoint setting and change that to match your SPARQL endpoint: 
define("DEFAULT_ENDPOINT", "http://localhost:3030/ds/sparql");
3. add this line to the bottom of file:
define("BASE_HREF", "http://localhost/skosmos/");

(See other settings of Skosmos in https://github.com/NatLibFi/Skosmos/wiki/Configuration.)


### 7. Configure PHP (optional)

The default PHP configuration provided by your distribution is probably fine for Skosmos, but you may want to check php.ini anyway. Here are some things to check:

* Make sure you have the date.timezone setting configured, otherwise you may get errors displaying date values.
* If you use vocabularies with potentially a large number of triples, you may need to adjust the memory_limit setting. The default is usually 128M but the recommended setting is 256M.

