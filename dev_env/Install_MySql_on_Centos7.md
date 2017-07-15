# Install with source code.

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

And execute `source /etc/profile` to activate this new setting.

Done.

```
# php -v
PHP 7.0.12 (cli) (built: Apr  3 2017 00:08:56) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
```

# Install with yum

yum install mysql mysql-server mysql-devel -y

2、查看是否生成了mysqld服务, 并设置随机启动
# chkconfig --list |grep mysql 

数字代码服务器启动级别，off  代表不随机启动mysqld服务，on代表随机启动服务
我们需要设置mysqld随机启动，执行下面命令进行设置
# chkconfig mysqld on 
这样的结果代表正常 
# chkconfig --list |grep mysql   

3、启动mysqld服务
执行如下命令进行启动，两种方法都可以:
# /etc/init.d/mysqld start     
# service mysqld start 

启动后，ps一下，看下进程是否起来 
# ps -ef |grep mysql|grep -v grep 
root      1582     1  0 23:26 pts/0    00:00:00 /bin/sh /usr/bin/mysqld_safe --datadir=/var/lib/mysql --socket=/var/lib/mysql/mysql.sock --pid-file=/var/run/mysqld/mysqld.pid --basedir=/usr --user=mysql
mysql     1684  1582  1 23:26 pts/0    00:00:00 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --user=mysql --log-error=/var/log/mysqld.log --pid-file=/var/run/mysqld/mysqld.pid --socket=/var/lib/mysql/mysql.sock
根据进程信息可以看到，mysql的数据库data目录是 /var/lib/mysql ，错误日志文件是  /var/log/mysqld.log

查看都有哪些库
# cd /var/lib/mysql 
# ls -l 

发现有两个库，都是mysql默认自带的，如何手动创建数据库，会在后续的教程中说明。

查看占用端口，默认占用3306端口
# netstat -nutlp | grep mysql
tcp        0      0 0.0.0.0:3306                0.0.0.0:*                   LISTEN      1684/mysqld  

/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
/etc/init.d/iptables save
service iptables restart
/etc/init.d/iptables status

CentOS 7.0默认使用的是firewall作为防火墙，这里改为iptables防火墙。
1、关闭firewall：
systemctl stop firewalld.service
systemctl disable firewalld.service
systemctl mask firewalld.service

2、安装iptables防火墙
yum install iptables-services -y

3.启动设置防火墙

# systemctl enable iptables
# systemctl start iptables

4.查看防火墙状态

systemctl status iptables

5编辑防火墙，增加端口
vi /etc/sysconfig/iptables #编辑防火墙配置文件
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
:wq! #保存退出

3.重启配置，重启系统
systemctl restart iptables.service #重启防火墙使配置生效
systemctl enable iptables.service #设置防火墙开机启动

4、停止mysqld服务
执行如下命令进行停止，两种方法都可以:
# /etc/init.d/mysqld stop   
# service mysqld stop

5、重启mysqld服务
执行如下命令进行重启，两种方法都可以:
# /etc/init.d/mysqld restart
# service mysqld srestart

6、命令行测试连接mysql ，后续可以在命令行中直接管理数据库
直接执行，yum安装的mysql，本地root密码默认为空
# mysql


7、设置初始密码\权限

设置新的密码并同时授权限

mysql> grant all on *.* to root@'%' identified by 'youpassword';

刷新使之生效

mysql> flush privileges;

退出

mysql> exit;
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 8、重新登陆查看新密码和权限是否生效
mysql> [root@nzp_redhat bin]# mysql -u root -p
Enter password:"输入设置的密码"

use mysql;
update user set Password = PASSWORD('pass@word') where User ='root';
flush privileges;