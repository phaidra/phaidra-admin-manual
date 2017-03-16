# Installation of Skosmos and Fuseki

Skosmos and Fuseki require Apache and PHP running on the server. We have installed them for test purposes on a Windows 7 environment \(Professional 64 bit, Service Pack 1, Intel Core i7-56000 CPU, 2.6 GHz, 16 GB RAM\) using Java 1.8 \(jre1.8.0\_40\), with installed XAMPP \(xampp-win32-1-8-3-4-VC11\), as well as on a CENTOS 6.5 virtual machine \(Intel Xeon CPU E5-2670 0 @ 2.60GHz\).

The final version will work on Ubuntu 16.04. \(Intel Xeon CPU E5-2670 0 @ 2.60GHz\).

A detailed installation guide of Skosmos can be found on: [https://github.com/NatLibFi/Skosmos/wiki/Installation](https://github.com/NatLibFi/Skosmos/wiki/Installation)   
and installation of Jena Fuseki with the jena-text extension on: [https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText](https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText) , but but there are some deviations on the Windows version, as well as there are some important issues that are worth highlighting.

## 1. Installation on Windows

### 1.1 Install XAMPP

\(Apache web server of XAMPP is required in order to run PHP.\)

1. Download XAMPP \(e.g. XAMPP Windows 5.6.3, xampp-win32-1-8-3-4-VC11-installer.exe from [http://en.softonic.com/s/download-xampp-64-bit-windows-7-free-downloads](http://en.softonic.com/s/download-xampp-64-bit-windows-7-free-downloads)\) \(Don't be surprised, that it looks like a win32 version, direct 64-bit version is not available, but it will not be a problem.\)
2. Run xampp-win32-1-8-3-4-VC11-installer.exe with the default options \(install XAMPP into the folder C:\xampp\)
   \(with the installation of XAMPP we have installed Apache and PHP, as its is required for Skosmos\)

### 1.2 Get Skosmos

1. Download Skosmos current master branch from [https://github.com/NatLibFi/Skosmos](https://github.com/NatLibFi/Skosmos).
2. Unzip Skosmos-master.zip to a skosmos subfolder under c:\xampp\htdocs folder \(which should the default DocumentRoot for XAMPP.\)
   Result: c:\xampp\htdocs\skosmos contains index.php and other files and folders.
   \(Cloning the code from GitHub would be better, but will be tested in the next turn only.\)

### 1.3 Install Git

\(Git is required for running the Dependency Manager in the next step.\)

1. Download git form [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Run Git-2.5.3-64-bit.exe \(options: Use Git and optional Unix tools from the Windows Command Prompt; Checkout Windows-style, commit Unix-style line endings; Use Windows' default console window\)
3. Restart your computer to make the changes in the PATH environment variable in effect

### 1.4 Install Dependency Manager for PHP

\(Skosmos requires a number of PHP libraries, which will be installed using Composer.\)

1. Download the Windows installer of Composer \(Composer-Setup.exe\) from [https://getcomposer.org/doc/00-intro.md\#installation-windows](https://getcomposer.org/doc/00-intro.md#installation-windows).
2. Run Composer-Setup.exe
3. Open command prompt and cd into the skosmos directory \(c:\xampp\htdocs\skosmos\)
4. Run from this folder the command \(this will install the dependencies\): composer install --no-dev
   \(Successful result: lot of components will be installed, lock file will be written, and finally autoload file will be generated.\)

If you download a new version of Skosmos \(in case of by starting Skosmos you receive the error message: "Error: Dependencies managed by Composer missing. Please run "php composer.phar install"\), that requires to set up PHP dependencies again.

### 1.5 Install Jena Fuseki

1. Download the latest Fuseki distribution jena-fuseki-\*.zip from [http://jena.apache.org/download/\#apache-jena-fuseki](http://jena.apache.org/download/#apache-jena-fuseki)
2. Unpack the downloaded file \(e.g. apache-jena-fuseki-2.3.0.zip\) to the desired folder \(e.g. c:\apache-jena-fuseki-2.3.0\)

Fuseki v1.3.0 onwards and Fuseki v2.3.0 onwards require Java 8, so from scratch you have to install this version.

Theoretically you could set the `JAVA_HOME` sytem variable to the location of Java 8, but unfortunately fuseki-server script ignores JAVA\_HOME variable while it executes the "java" command.

## 2. Installation on Linux

### 2.1 Check and set Apache

Before installing Skosmos, it is advisable to make sure that Apache works and PHP is enabled. After installing PHP, you may need to restart Apache.

### 2.2 Get Skosmos

You can either clone the code of Skosmos from Github  or download the zipped version. It is worth using the current stable version \(maintenance branch\), and it is better to clone from Github because you can then easily upgrade to newer versions using `git pull`.

Go to the parent directory:

`cd /var/www`

`git clone -b v1.6-maintenance `[`https://github.com/NatLibFi/Skosmos.git`](https://github.com/NatLibFi/Skosmos.git)` skosmos`

### 2.3 Install Dependency Manager for PHP

Skosmos requires several PHP libraries which will be installed using a dependency manager called Composer. For this, you have to first download the Composer to the directory of Skosmos:

`cd skosmos`

`wget https://getcomposer.org/composer.phar`

, and then install the dependencies with the `install --no-dev` option:

`php composer.phar install --no-dev`

After downloading a new version of Skosmos you may need to update the dependencies with the `update --no-dev` option

`php composer.phar update --no-dev`

### 2.4 Setup Apache

It may be required to set up Apache to access Skosmos under [http://localhost/skomos](http://localhost/skomos) by adding a symbolic link to the skosmos folder \(e.g. var/www/skosmos\) into the DocumentRoot \(e.g. /var/www/html\):

`cd /var/www/html`

`ln -s /var/www/skosmos skosmos`

Now you should be able to access the front page of Skosmos using the URL [http://myhost/skosmos](http://myhost/skosmos). \(E.g.: [http://vocab.phaidra.org/skomos](http://vocab.phaidra.org/skomos)\) If not, check the following:

Check that `mod_rewrite` is enabled in the Apache configuration:  
Type `<?php phpinfo(); ?>` in a php file and save it and run that file in the server, and check if mod\_rewrite is among the loaded modules:

\(E.g.: `http://vocab.phaidra.org/phpinfo.php`\)

\(If not, then you can enable mode\_rewrite in Apache2 on Ubuntu as follow:

`sudo a2enmod rewrite`

This will activate the module or alert you that the module is already in effect. To put these changes into effect, restart Apache:

`sudo service apache2 restart`

\)

Check that `AllowOverride All` is set for the `DocumentRoot` \(or the directory where you installed Skosmos\)

\(E.g. in Apache2 on Ubuntu check /etc/apache2/apache2.conf:

```
<Directory /var/www/>
        ...
        AllowOverride All
        ...
</Directory>
```

\)

If you are using SELinux \(e.g. RHEL/CentOS\) you will also need to give Apache permissions to perform network connections to allow Skosmos access to SPARQL endpoints:

`setsebool -P httpd_can_network_connect on`

### 2.5 Check and set PHP configuration

The default PHP configuration is probably sufficient for Skosmos, but you may want to check php.ini just in case. Make sure that the date.timezone setting is configured correctly, otherwise Skosmos pages displaying date values may not work at all. If you use vocabularies with potentially large number of triples, you may need to adjust the memory\_limit setting. The default is usually 128M but the recommended setting is 256M.

### 2.6 Download and install Jena Fuseki

Simply download the latest Fuseki distribution and unpack the downloaded file to the intended folder of Fuseki.

Look for the newest jena-fuseki-\*-distribution.tar.gz: [http://www.apache.org/dist/jena/binaries/](http://www.apache.org/dist/jena/binaries/)

In linux use the

```
wget http://www.apache.org/dist/jena/binaries/jena-fuseki-*-distribution.tar.gz

tar xzf jena-fuseki-*-distribution.tar.gz

rm jena-fuseki-*-distribution.tar.gz
```

Fuseki v1.3.0 onwards and Fuseki v2.3.0 onwards require Java 8, so from scratch you have to install this version.

If you have not only Java 8 installed on your machine, before you start Fuseki, you have to check which is the currently used version:

`java -version`

If it is not Java 8, then you can switch to Java 8 using:

`update-alternatives --config java`

Theoretically you could set the `JAVA_HOME` sytem variable to the location of Java 8, but unfortunately fuseki-server script ignores JAVA\_HOME variable while it executes the "java" command.

If all went well, you should be able to test Fuseki by running `./fuseki-server --mem /ds`

See for details: [https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText](https://github.com/NatLibFi/Skosmos/wiki/InstallFusekiJenaText)

