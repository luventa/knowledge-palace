# Install with source code

This section records how to install MySql with souce code. In other words, make install.

C and C++ compiler must be installed. ncurses also can be installed by yum

`yum install gcc gcc-c++ ncurses-devel`

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

## Create user and user group for mysql

```
# groupadd mysql  
# useradd -r -g mysql mysql  
```

## Create directory for mysql

```
# mkdir -p /data/mysqldb
# mkdir -p /usr/local/mysql
# mkdir -p /var/log/mysql
```

## Install MySql

Home page of MySql http://www.mysql.com/

The link for downloading package can be found on net.

```
# cd /root/source && wget http://cdn.mysql.com/Downloads/MySQL-5.7/mysql-boost-5.7.17.tar.gz
# tar -zxvf mysql-boost-5.7.17.tar.gz && cd  mysql-5.7.17
# cmake \   
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \   
-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \   
-DDEFAULT_CHARSET=utf8 \   
-DDEFAULT_COLLATION=utf8_general_ci \   
-DWITH_INNOBASE_STORAGE_ENGINE=1 \   
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \   
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \   
-DMYSQL_DATADIR=/data/mysqldb \   
-DMYSQL_TCP_PORT=3306 \   
-DENABLE_DOWNLOADS=1
# make && make install
```
## Change owner of mysql directories

```
# cd /usr/local/mysql
# chown -R mysql:mysql .
# cd /data/mysqldb  
# chown -R mysql:mysql . 
# cd /var/log/mysql
# chown -R mysql:mysql . 
```

## Initialize database

```
# cd /usr/local/mysql/scripts && ./mysql_install_db --user=mysql --datadir=/data/mysqldb  
```

## Config database service

```
# cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf  
# vi /etc/my.cnf

datadir = /data/mysqldb
log_error = /var/log/mysql/error.log

# cp support-files/mysql.server /etc/init.d/mysqld
# vi /etc/profile

PATH=/usr/local/mysql/bin:/usr/local/mysql/lib:$PATH 
export PATH
```

And execute `source /etc/profile` to activate this new setting.

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

`mysqladmin -u root password 'YOURPASSWORD'`

## create user

```
mysql> grant select,insert,update,delete on SCHEMANAME.* to USERNAME@% identified by "PASSWORD";
mysql> flush privileges
```

## remove user and privileges

```
mysql> DELETE FROM user WHERE user='USERNAME' AND host='HOSTNAME';
mysql> flush privileges;
mysql> drop database USERSDB;
mysql> drop user USERNAME@'%';
```

## use MySql with password

`# mysql -u USERNAME -p`

### permission

select,insert,update,delete,create,drop,index,alter,grant,references,reload,shutdown,process,file
