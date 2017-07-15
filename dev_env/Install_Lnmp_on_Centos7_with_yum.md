## Upgrade system packages

`yum update`

mysql -uroot -p

show databases;

YouNeverKnow@123

MariaDB [mysql]> create user 'remoteuser'@'%' identified by 'YouNeverKnow@123';
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> select user, host, password from user;
+------------+---------------------+-------------------------------------------+
| user       | host                | password                                  |
+------------+---------------------+-------------------------------------------+
| root       | localhost           | *295E686D6647AB4EBA951FBE882CF0DE22EE3CE8 |
| root       | vm\_175\_84\_centos | *295E686D6647AB4EBA951FBE882CF0DE22EE3CE8 |
| root       | 127.0.0.1           | *295E686D6647AB4EBA951FBE882CF0DE22EE3CE8 |
| root       | ::1                 | *295E686D6647AB4EBA951FBE882CF0DE22EE3CE8 |
| remoteuser | %                   | *295E686D6647AB4EBA951FBE882CF0DE22EE3CE8 |
+------------+---------------------+-------------------------------------------+
5 rows in set (0.00 sec)

MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO 'remoteuser'@'%' IDENTIFIED BY 'YouNeverKnow@123' WITH GRANT OPTION;
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> FLUSH PRIVILEGES;



wget http://www.dbshop.net/download/websitedown/DBShop_1.0_Release_20170323.zip

今天把开发环境配置完

新服务器地址118.89.41.135
账号root
密码Noguess@123

mysql远程账号
remoteuser
YouNeverKnow@123

用phpstorm开发
remote host本地修改服务器代码

后台管理员admin密码aaaaa888


安装mariadb （mariadb和MySQL基本无区别，是MySQL的一个衍生分支）

Shell

sudo yum install -y mariadb-server
启动mariadb

Shell

sudo systemctl start mariadb
配置mariadb

Shell

sudo mysql_secure_installation
这里会提示是否设置root密码，直接回车然后输入Y设置root密码，两次输入密码（务必要记住）。
接下来的询问全部输入Y。

设置mariadb开机启动

Shell

sudo systemctl enable mariadb



[root@beyond ~]# mysql -u root
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.0.56-log Source distribution  退出；exit
 
mysql> 
设置root用户的mysql数据库密码；
[root@beyond ~]# mysqladmin -u root password "123.com" 
[root@beyond ~]# mysql -u root -p
Enter password: 
数据库结构，
//查询数据库的命令：
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema | 
| mysql              | 
| test               | 
+--------------------+
3 rows in set (0.00 sec)
 
查询数据库中的数据表；
mysql> USE mysql;
Database changed
mysql> SHOW TABLES;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              | 
| db                        | 
| func                      | 
| help_category             | 
| help_keyword              | 
| help_relation             | 
| help_topic                | 
| host                      | 
| proc                      | 
| procs_priv                | 
| tables_priv               | 
| time_zone                 | 
| time_zone_leap_second     | 
| time_zone_name            | 
| time_zone_transition      | 
| time_zone_transition_type | 
| user                      | 
+---------------------------+
17 rows in set (0.00 sec)
数据库的创建和删除；
mysql> CREATE DATABASE beyond;
Query OK, 1 row affected (0.00 sec)
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema | 
| beyond             | 
| mysql              | 
| test               | 
+--------------------+
4 rows in set (0.00 sec)
删除数据库；
mysql> DROP DATABASE taotao;
Query OK, 0 rows affected (0.00 sec)
创建和删除数据表；
mysql> CREATE TABLE songs (songs_name CHAR(30) NOT NULL, songs_passwd CHAR(20) NOT NULL DEFAULT '123.com',PRIMARY KEY (songs_name));
Query OK, 0 rows affected (0.02 sec)
插入新的数据记录；
mysql> INSERT INTO auth.users(user_name,user_passwd) VALUES('huangjiajv',ENCRYPT('123456'));
Query OK, 1 row affected (0.01 sec)
mysql> INSERT INTO auth.users(user_name,user_passwd) VALUES('huangguanzhong',ENCRYPT('com.123'));
Query OK, 1 row affected (0.00 sec)
修改数据表信息；
mysql> UPDATE auth.users SET user_passwd=ENCRYPT('123.com') WHERE user_name='taotao';
Query OK, 0 rows affected (0.00 sec)
Rows matched: 0  Changed: 0  Warnings: 0
数据库的备份和恢复；
[root@www ~]# mysqldump -u root -p auth > mysql-auth.sql
Enter password: 
[root@www ~]# ll mysql-auth.sql 
-rw-r--r-- 1 root root 1863 Dec 30 01:01 mysql-auth.sql     //备份数据库； 
删除数据库auth;
mysql> USE mysql;
Database changed
mysql> DROP DATABASE auth;
Query OK, 1 row affected (0.01 sec)
查看一下是否删除；
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema | 
| beyond             | 
| mysql              | 
| test               | 
+--------------------+
4 rows in set (0.00 sec)
[root@www ~]# mysql -u root -p auth < mysql-auth.sql
Enter password: 
[root@www ~]# mysqldump -u root -p beyond > /usr/src/mysql-beyond.sql
Enter password: 
[root@www ~]# ll /usr/src/mysql-beyond.sql 
-rw-r--r-- 1 root root 1775 Dec 30 02:00 /usr/src/mysql-beyond.sql
[root@www ~]# mysql -u root -p beyond < /usr/src/mysql-beyond.sql
Enter password: 
[root@www ~]# 
备份某个数据库中的某个表；
[root@www ~]# mysqldump -u root -p mysql host user > mysql.host-user.sql
Enter password: 
[root@www ~]# 
 
 
刚装了mysql
sudo apt-get install mysql
安装成功了，安装最后要求输入了密码，也输入了，OK
mysql -uroot -p
输入设置的密码
竟然报错了！
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YSE)
问朋友，他说初始密码是空的，可我命名设置了密码的阿。
密码留空
还是错误！
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
于是重改密码！
#sudo /etc/init.d/mysql stop
# mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
# mysql -u root mysql
mysql> UPDATE user SET Password=PASSWORD('newpassword') where USER='root';
mysql> FLUSH PRIVILEGES;
mysql> quit
# /etc/init.d/mysql restart
# mysql -u root -p
Enter password: 
mysql>
搞定！