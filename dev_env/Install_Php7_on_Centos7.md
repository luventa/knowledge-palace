# Install with source code.

```
# pwd
/root/source
```

## Pre-install libxml2

Download list link ftp://xmlsoft.org/libxml2/

```
# cd /root/source && wget ftp://xmlsoft.org/libxml2/libxml2-2.9.4.tar.gz
# tar -zxvf libxml2-2.9.4.tar.gz && cd libxml2-2.9.4
# ./configure --prefix=/root/package/libxml2 --with-python=no
# make && make install
```

## Download and install Php

Download page http://cn2.php.net/downloads.php

```
# cd /root/source && wget http://cn2.php.net/distributions/php-7.0.12.tar.gz
# tar -zxvf php-7.0.12.tar.gz && cd php-7.0.12
# ./configure --prefix=/root/software/php --with-libxml-dir=/root/package/libxml2 --with-config-file-path=/root/software/php/etc --enable-mbstring --enable-fpm --with-mysqli
# make && make install
```

## Setup env path

I added a new file `php.sh` in directory `/etc/profile.d/`

```sh
PATH=$PATH:/root/software/php/bin
export PATH
```

And execute `source /etc/profile` to activate this new setting.

Done.

```
# php -v
PHP 7.0.12 (cli) (built: Apr  3 2017 00:08:56) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
```