# Install with source code.

```
# pwd
/root/source
```

## Prepare pcre and zlib

Download list for pcre https://ftp.pcre.org/pub/pcre/
Download page for zlib http://zlib.net/

Download and unzip the tar files.

```
# wget https://ftp.pcre.org/pub/pcre/pcre-8.40.tar.gz
# wget http://zlib.net/zlib-1.2.11.tar.gz
# tar -zxvf pcre-8.40.tar.gz
# tar -zxvf zlib-1.2.11.tar.gz
```

## Download and install Nginx

Download list link http://nginx.org/download

```
# wget http://nginx.org/download/nginx-1.11.12.tar.gz
# tar -zxvf nginx-1.11.12.tar.gz
# cd nginx-1.11.12/
# ./configure --prefix=/root/software/nginx --with-pcre=/root/source/pcre-8.40 --with-zlib=/root/source/zlib-1.2.11
# make && make install
```

Nginx will be installed in /root/software/nginx. And source files can be removed now.

## Config root for test

Change the webroot
```diff
# cd /root/software/nginx/conf/
# vim nginx.conf

location / {
-    root  root;
+    root   /var/www;

    index  index.html index.htm;
}
```

Create webroot and index.html as you like.
```shell
cd /var
mkdir www
cd ./www
// create your onw index.html
```

## Mngt Nginx

```
# cd /root/software/nginx/sbin/
# ps aux|grep nginx
# ./nginx
# ./nginx -s stop
# ./nginx -s quit
# ./nginx -s reload

```

```
sudo systemctl start nginx
sudo systemctl stop nginx
```
