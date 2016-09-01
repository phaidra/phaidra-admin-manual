
# Packages

## build
```bash
apt-get install dpkg-dev libdpkg-dev debhelper dh-make dh-autoreconf
apt-get install libjpeg-dev libtiff5-dev
```

## Nginx
```bash
apt-get install nginx nginx-common nginx-core nginx-doc```
```bash
apt-get install fcgiwrap
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
 