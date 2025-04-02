---
title: Linux下载、配置MySQL8.0
tags:
  - linux
  - mysql
categories:
  - 教程
description: "\U0001F3AA手把手在 Linux 系统安装 MySQL"
abbrlink: cdaee8e0
date: 2021-11-14 14:50:12
cover:
---
# Linux下载、配置MySQL8.0

> 基础的环境都搭好了，现在搭一下数据库，用的是mysql

### 1. 查看Linux自带的数据库mariadb并删除

​	这个数据库是mysql一个分支，不需要它给删除

```
[root@cx ~]# rpm -qa|grep mariadb
mariadb-libs-5.5.60-1.el7_5.x86_64
[root@cx ~]# rpm -e --nodeps mariadb-libs-5.5.60-1.el7_5.x86_64
[root@cx ~]# rpm -qa|grep mariadb
[root@cx ~]# 
```

​	可以看到已经删了，下面就开始下载MySQL

​	进入下载页：https://downloads.mysql.com/archives/community/

​	查看Linux版本

```
[root@cx ~]# uname -a
Linux cx 3.10.0-957.21.3.el7.x86_64 #1 SMP Tue Jun 18 16:35:19 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```

​	然后就知道要下载的版本了，下载这个：mysql-8.0.21-1.el7.x86_64.rpm-bundle.tar



# **切记切记切记，下载的是centos7，看准了版本**

***

### 2. 在服务器上使用wget下载MySQL

```
[root@cx software-install]# wget https://downloads.mysql.com/archives/get/p/23/file/mysql-8.0.21-1.el7.x86_64.rpm-bundle.tar
```

​	等下载完成，查看一下目录下文件

```
[root@cx software-install]# ls
jdk-8u221-linux-x64.rpm  mysql-8.0.21-1.el7.x86_64.rpm-bundle.tar  node-v14.3.0-linux-x64.tar.gz  Python-3.6.3.tgz
```

​	可以看到已经下载成功了

### 3. 安装mysql

```
[root@cx software-install]# tar -xvf mysql-8.0.21-1.el7.x86_64.rpm-bundle.tar -C /opt/temp/
```

```
[root@cx temp]# ls
mysql-community-client-8.0.21-1.el7.x86_64.rpm           mysql-community-libs-8.0.21-1.el7.x86_64.rpm
mysql-community-common-8.0.21-1.el7.x86_64.rpm           mysql-community-libs-compat-8.0.21-1.el7.x86_64.rpm
mysql-community-devel-8.0.21-1.el7.x86_64.rpm            mysql-community-server-8.0.21-1.el7.x86_64.rpm
mysql-community-embedded-compat-8.0.21-1.el7.x86_64.rpm  mysql-community-test-8.0.21-1.el7.x86_64.rpm
```

​	依次安装MySQL数据库的mysql common、mysql libs、mysql client软件包，和server

```
[root@cx temp]# rpm -ivh mysql-community-common-8.0.21-1.el7.x86_64.rpm 
```

```
[root@cx temp]# rpm -ivh mysql-community-libs-8.0.21-1.el7.x86_64.rpm 
```

​	--nodeps：安装不检查依赖问题

​	--force：强制安装

```
[root@cx temp]# rpm -ivh mysql-community-client-8.0.21-1.el7.x86_64.rpm 
```

```
[root@cx temp]# rpm -ivh mysql-community-server-8.0.21-1.el7.x86_64.rpm 
warning: mysql-community-server-8.0.21-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
error: Failed dependencies:
	libaio.so.1()(64bit) is needed by mysql-community-server-8.0.21-1.el7.x86_64
	libaio.so.1(LIBAIO_0.1)(64bit) is needed by mysql-community-server-8.0.21-1.el7.x86_64
	libaio.so.1(LIBAIO_0.4)(64bit) is needed by mysql-community-server-8.0.21-1.el7.x86_64
```

​	执行出现以上错误，离线安装的mysql缺少libaio.so.1文件

​	文件地址：https://pkgs.org/download/libaio.so.1

​	我们要下载的具体文件的下载页面：https://centos.pkgs.org/7/centos-x86_64/libaio-0.3.109-13.el7.x86_64.rpm.html

​	具体文件地址：http://mirror.centos.org/centos/7/os/x86_64/Packages/libaio-0.3.109-13.el7.x86_64.rpm

```
[root@cx software-install]# wget http://mirror.centos.org/centos/7/os/x86_64/Packages/libaio-0.3.109-13.el7.x86_64.rpm
[root@cx software-install]# ls
jdk-8u221-linux-x64.rpm           mysql-8.0.21-1.el8.x86_64.rpm-bundle.tar  Python-3.6.3.tgz
libaio-0.3.109-13.el7.x86_64.rpm  node-v14.3.0-linux-x64.tar.gz
```

​	进入/opt/software-install目录下，进行下载

```
[root@cx software-install]# rpm -ivh libaio-0.3.109-13.el7.x86_64.rpm
[root@cx software-install]# whereis libaio.so.1
libaio.so: /usr/lib64/libaio.so.1
```

​	安装libaio并查看是否安装成功

​	重新执行安装server的命令

```
[root@cx temp]# rpm -ivh mysql-community-server-8.0.21-1.el7.x86_64.rpm 
```

​	看到安装成功了

​	安装完后可以用rpm -qa查看一下

```
[root@cx temp]# rpm -qa|grep mysql
mysql-community-libs-8.0.21-1.el8.x86_64
mysql-community-server-8.0.21-1.el8.x86_64
mysql-community-common-8.0.21-1.el8.x86_64
mysql-community-client-8.0.21-1.el8.x86_64
```

### 4. 初始化mysql数据库

```
[root@cx temp]# mysqld --initialize;									# 初始化
[root@cx temp]# chown mysql:mysql /var/lib/mysql -R;					# 授权
[root@cx temp]# systemctl start mysqld									# 开启服务
[root@cx temp]# systemctl status mysqld									# 查看服务状态
[root@cx temp]# systemctl enable mysql									# 开机自启
```

### 5. 查看密码，重设置密码

​	查看密码

```
[root@cx temp]# cat /var/log/mysqld.log | grep password
2021-11-14T05:16:17.754515Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: :Cld9aHkri:Q
```

​	登录mysql

```
[root@cx temp]# mysql -uroot -p
Enter password: # 密码输入上面的那个，直接复制就行，密码不会显示
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.21

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> # 登录成功
```

​	修改密码

```
mysql> alter user 'root'@'localhost' identified with mysql_native_password by '密码';
Query OK, 0 rows affected (0.00 sec)
```

​	退出重新登录

```
mysql> exit;
Bye
[root@cx temp]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.21 MySQL Community Server - GPL

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

​	允许远程访问服务器的MySQL，root是登录账户，%代表任意地址，可以设置你固定的ip地址，密码是远程登录的密码

​	1. 先创建用户

```
mysql> create user 'root'@'%' identified with mysql_native_password by '密码';

```

 	2. 设置远程访问并用flush privileges;刷新一下

```
mysql> grant all privileges on *.* to 'root'@'%' with grant option;
mysql> flush privileges;
```

### 6. 本地数据库可视化软件连接服务器中的MySQL

​	先看一下服务器的防火墙关没关

```
systemctl status firewalld
```

​	主要看activate，如果是绿色的表示正在运行，灰色的是关闭的

```
systemctl stop firewalld
```
‘
 	关完后去阿里云的控制台，防火墙规则里添加一个规则，开放MySQL的3306端口
 	![阿里](https://img-blog.csdnimg.cn/2c2edb8cb9a54503ba755956d1743420.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQWxvbmVsaWVz,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
	本地可视化软件连接一下就🆗了
	![本地](https://img-blog.csdnimg.cn/919e5acb7ac342db8349c6fe4d1a4248.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQWxvbmVsaWVz,size_17,color_FFFFFF,t_70,g_se,x_16#pic_center)


