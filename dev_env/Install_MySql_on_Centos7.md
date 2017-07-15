# Install with source code

This section records how to install MySql with souce code. In other words, make install.

## Pre-install CMake

Download list link https://cmake.org/files/

```
# cd /root/source && wget https://cmake.org/files/v3.7/cmake-3.7.0.tar.gz
# tar -zxvf cmake-3.7.0.tar.gz && cd cmake-3.7.0
# ./configure --prefix=/root/package/cmake
# make && make install
```

## Pre-install ncurses

Download page http://ftp.gnu.org/gnu/ncurses/

```
# cd /root/source && wget http://ftp.gnu.org/gnu/ncurses/ncurses-6.0.tar.gz
# tar -zxvf ncurses-6.0.tar.gz && cd ncurses-6.0
# ./configure --prefix=/root/package/ncurese
# make && make install
```

## Prepare boost

Download page https://sourceforge.net/projects/boost/files/boost/

```
# cd /root/source && wget http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
# tar -zxvf boost_1_59_0.tar.gz && cd bison-3.0.4
# ./configure --prefix=/root/package/bison
# make && make install
```

## Install MySql

Home page of MySql http://www.mysql.com/

```
# cd /root/source && wget http://cdn.mysql.com/Downloads/MySQL-5.7/mysql-boost-5.7.17.tar.gz
# tar -zxvf mysql-boost-5.7.17.tar.gz && cd  mysql-5.7.17
# cmake . -DCMAKE_INSTALL_PREFIX=/root/software/mysql -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/root/package/boost -DCURSES_LIBRARY=/root/package/ncurese/lib/libncurses.a -DCURSES_INCLUDE_PATH=/root/package/ncurses/include
# make && make install
```

And execute `source /etc/profile` to activate this new setting. Done.

---

# Install with yum

Execute this command and yum will install mysql and dependencies automatically if the network is working.

`# yum install mysql mysql-server mysql-devel -y`

Completed.

---

# Management for MySql

This part records some commands for daily management for MySql

## check mysqld sevice and set it auto launch

`# chkconfig mysqld on` 

## start/stop mysqld service

`# service mysqld start`

`# service mysqld stop`

## check if mysql service is running

`# ps -ef |grep mysql|grep -v grep`

## change password for root

launch MySql cli

`# mysql`

execute sql

```mysql
use mysql;
update user set Password = PASSWORD('YOURPASSWORD') where User ='root';
flush privileges;
```