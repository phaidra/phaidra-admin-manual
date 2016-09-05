
# Packages

## build
```bash
apt-get install dpkg-dev libdpkg-dev debhelper dh-make dh-autoreconf
apt-get install libjpeg-dev libtiff5-dev
```

## IIP
```bash
git clone https://github.com/ruven/iipsrv.git && \
        cd iipsrv && \       
        ./autogen.sh && \       
        ./configure && \
        make && \
        mkdir -p /usr/local/iipsrv/ && \
        cp src/iipsrv.fcgi  /usr/local/iipsrv/ && \
        chown -R http.http /usr/local/iipsrv 

```

## vips
```bash
apt-get install libvips-tools libvips-dev libvips-doc libvips42 python-vipscc
```
 
 ## Apache
```bash
apt-get install apache2
```

Config (/etc/apache2/sites-available/000-default.conf)

```
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet

<VirtualHost *:8001>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        ServerName pige.phaidra.org

        ServerAdmin admin.phaidra@univie.ac.at
        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf

AddHandler fcgid-script .fcgi
FcgidInitialEnv VERBOSITY "10"
FcgidInitialEnv LOGFILE "/tmp/iipsrv.log"
FcgidInitialEnv MAX_IMAGE_CACHE_SIZE "10"
FcgidInitialEnv JPEG_QUALITY "90"
FcgidInitialEnv MAX_CVT "5000"
FcgidInitialEnv MAX_CVT "5000"
FcgidInitialEnv FILESYSTEM_PREFIX "/var/www/iipsrv/data/"
#FcgidInitialEnv MEMCACHED_SERVERS "localhost"
# Define the idle timeout as unlimited and the number of
# processes we want
FcgidIdleTimeout 0
FcgidMaxProcessesPerClass 1024
# FcgidMaxProcesses 2048

# pixeldragon (compiled stuff):
ScriptAlias /iipsrv/ "/usr/local/iipsrv/"
# pixelgecko (Ubuntu packages) (the iip code does not work!):
# ScriptAlias /iipsrv/ "/usr/lib/iipimage-server/"

# Set the options on that directory
<Directory /usr/local/iipsrv/>
   AllowOverride None
   Options None
   Require all granted
</Directory>

<Directory /usr/lib/iipimage-server/>
   AllowOverride None
   Options None
   Require all granted
</Directory>


</VirtualHost>

```


## Nginx
```bash
apt-get install fcgiwrap
```
```bash
apt-get install nginx nginx-common nginx-core nginx-doc
```
Config (/etc/nginx/conf.d/iipsrv.conf):
```
server {
    listen       80;
    # server_name  pige.phaidra.org;
    server_name  imageserver.phaidra.org;
    # server_name  localhost;
    charset utf-8;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    location /iipsrv/iipsrv.fcgi {
      proxy_pass   http://127.0.0.1:8001;
      proxy_set_header Host $host;
      proxy_redirect off;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      access_log      /var/log/nginx/iipsrv.access.log;
      error_log       /var/log/nginx/iipsrv.error.log;

      # Cross-origin resource sharing (CORS)
      if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        #
        # Om nom nom cookies
        #
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        #
        # Custom headers and headers various browsers *should* be OK with but aren't
        #
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,X-Prototype-Version,X-Requested-With';
        #
        # Tell client that this pre-flight info is valid for 20 days
        #
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain charset=UTF-8';
        add_header 'Content-Length' 0;
        return 204;
      }
      if ($request_method = 'POST') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,X-Prototype-Version,X-Requested-With';
      }
      if ($request_method = 'GET') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,X-Prototype-Version,X-Requested-With';
      }
   }

}

server {
    listen       443;
    server_name  imageserver.phaidra.org;
    # server_name  localhost;
    charset utf-8;

    ssl    on;
    ssl_certificate       /etc/nginx/ssl/bundle.crt;
    ssl_certificate_key   /etc/nginx/ssl/imageserver.phaidra.org.key;

    location /iipsrv/iipsrv.fcgi {
      proxy_pass   http://127.0.0.1:8001;
      proxy_set_header Host $host;
      proxy_redirect off;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      access_log      /var/log/nginx/iipsrv.ssl-access.log;
      error_log       /var/log/nginx/iipsrv.ssl-error.log;

      # Cross-origin resource sharing (CORS)
      if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        #
        # Om nom nom cookies
        #
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        #
        # Custom headers and headers various browsers *should* be OK with but aren't
        #
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,X-Prototype-Version,X-Requested-With';
        #
        # Tell client that this pre-flight info is valid for 20 days
        #
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain charset=UTF-8';
        add_header 'Content-Length' 0;
        return 204;
      }
      if ($request_method = 'POST') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,X-Prototype-Version,X-Requested-With';
      }
      if ($request_method = 'GET') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,X-Prototype-Version,X-Requested-With';
      }
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

```

## Imageserver agent

Prerequisities
```bash
libmongodb-perl
libjson-perl
libjson-pps-perl
libjson-xs-perl
```

Repo
```
https://github.com/phaidra/pixelgecko/blob/master/wrk_vips.pl
```

Run it in screen like
```bash
./wrk_vips.pl —config conf.json —watch
```

The two directories included as 'lib' at the beginning are:
* /home/pige/work/phaidra/forge/perl 
``` 
https://svn.phaidra.univie.ac.at/phaidra/forge/perl 
```
 * ls
 ```bash
 FedoraCommons  PAF  Phaidra  README.textile  UniVie
 ```
 
* /home/pige/work/sf/aix-pm/modules/util 
``` 
home: https://sourceforge.net/projects/aix-pm/
view: http://aix-pm.cvs.sourceforge.net/viewvc/aix-pm/modules/util/
download: http://aix-pm.cvs.sourceforge.net/viewvc/aix-pm/modules/util/?view=tar
```
 * ls
 ```bash
 csv.pl	CVS  example1.csv  t_csv.pl  Util
 ```