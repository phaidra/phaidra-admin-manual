# Installation of Skosmos and Fuseki

Skosmos and Fuseki require Apache and PHP running on the server. We have installed them for test purposes on a Windows 7 environment (Professional 64 bit, Service Pack 1, Intel Core i7-56000 CPU, 2.6 GHz, 16 GB RAM) using Java 1.8 (jre1.8.0_40), with installed XAMPP (xampp-win32-1-8-3-4-VC11), as well as on a CENTOS 6.5 virtual machine (Intel Xeon CPU E5-2670 0 @ 2.60GHz).

The final version will work on Ubuntu 16.04. (Intel Xeon CPU E5-2670 0 @ 2.60GHz).

A detailed installation guide of Skosmos can be found on: https://github.com/NatLibFi/Skosmos/wiki/Installation 
and installation of Jena Fuseki with the jena-text extension on: https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText , but but there are some deviations on the Windows version, as well as there are some important issues that are worth highlighting. 

## Installation on Windows

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

### 5. Install Jena Fuseki

(Jena Fuseki is a SPARQL server and RDF triple store which is the recommended backend for Skosmos. 
The jena-text extension can be used for faster text search.)

1. Download the latest Fuseki distribution jena-fuseki-*.zip from http://jena.apache.org/download/#apache-jena-fuseki
2. Unpack the downloaded file (apache-jena-fuseki-2.3.0.zip) to c:\apache-jena-fuseki-2.3.0

## Installation on Linux

### 1. Check and set Apache

Before installing Skosmos, it is advisable to make sure that Apache works and PHP is enabled. After installing PHP, you may need to restart Apache.

It may be required to set up Apache to access Skosmos under http://localhost/skomos by adding a symbolic link to the skosmos folder (e.g. opt/skosmos) into the DocumentRoot (e.g. /var/www/) and/or also you will need to give Apache permissions to perform network connections to allow Skosmos access to SPARQL endpoints.

### 2. Get Skosmos

You can either clone the code of Skosmos from Github  or download the zipped version. It is worth using the current stable version (maintenance branch), and it is better to clone from Github because you can then easily upgrade to newer versions using ```git pull```.

### 3. Install Dependency Manager for PHP

Skosmos requires several PHP libraries which will be installed using a dependency manager called Composer . For this, you have to first download the Composer, and then install the dependencies with the ```install --no-dev``` option. After downloading a new version of Skosmos you may need to update the dependencies with the ```update --no-dev``` option.  

### 4. Chec and set PHP configuration

The default PHP configuration is probably sufficient for Skosmos, but you may want to check php.ini just in case. Make sure that the date.timezone setting is configured correctly, otherwise Skosmos pages displaying date values may not work at all. If you use vocabularies with potentially large number of triples, you may need to adjust the memory_limit setting. The default is usually 128M but the recommended setting is 256M.

### 5. Install Jena Fuseki

JenaFuseki is a SPARQL server and triple store, which is the recommended backend for Skosmos. The jena text extension can be used for faster text search. Simply download the latest Fuseki distribution and unpack the downloaded file to the intended folder of Fuseki.

