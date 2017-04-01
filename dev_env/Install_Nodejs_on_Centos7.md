# Install from source code.

Download list link http://nodejs.org/dist/

Dowload the tar file (source code)
> wget https://nodejs.org/dist/v6.10.1/node-v6.10.1.tar.gz

```
--2017-04-01 10:19:11--  https://nodejs.org/dist/v6.10.1/node-v6.10.1.tar.gz
Resolving nodejs.org (nodejs.org)... 104.20.22.46, 104.20.23.46, 2400:cb00:2048:1::6814:172e, ...
Connecting to nodejs.org (nodejs.org)|104.20.22.46|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 26721650 (25M) [application/gzip]
Saving to: 'node-v6.10.1.tar.gz'

100%[====================================================================================================>] 26,721,650   101KB/s   in 4m 44s 

2017-04-01 10:23:56 (91.8 KB/s) - 'node-v6.10.1.tar.gz' saved [26721650/26721650]
```

Create a new directory and move the tar file into it.
> mkdir software

> mv node-v6.10.1.tar.gz ./software

> cd software

Extract the tar file
> tar xvf node-v6.10.1.tar.gz

Remove the tar file and and get into the directory. Also, remove the tar file.
> rm -rf node-v6.10.1.tar.gz

> cd nodejs-v*

Insall gcc for building nodejs.
> sudo yum install gcc gcc-c++

Configure and make. `make` will takes a plenty of time.
> ./configure

> make

> sudo make install

Done.
```
[root@VM_240_212_centos /]# node -v
v6.10.1
```