---
title: Linux 配置大数据环境
tags:
  - linux
  - 大数据
categories:
  - 教程
description: "\U0001F45B 手把手安装配置大数据需求环境"
abbrlink: 7d7a29ad
date: 2023-05-23 16:53:56
cover:
---
# linux配置大数据环境
## 修改主机名

```
hostnamectl set-hostname xxx
```

## 配置 yum

```
[root@localhost ~]# mkdir /mnt/cdrom
[root@localhost ~]# df /mnt/cdrom
文件系统          1K-块    已用     可用 已用% 挂载点
/dev/sda3      39517336 7718416 31798920   20% /
```

```
[root@localhost ~]# mount /dev/cdrom /mnt/cdrom
mount: /dev/sr0 写保护，将以只读方式挂载
```

查看挂载记录

```
[root@localhost ~]# df -hT /mnt/cdrom
文件系统       类型     容量  已用  可用 已用% 挂载点
/dev/sr0       iso9660  4.3G  4.3G     0  100% /mnt/cdrom
```

### **更改配置文件**

```
[root@localhost ~]# cd /etc/yum.repos.d/
[root@localhost yum.repos.d]# ll
总用量 28
-rw-r--r--. 1 root root 1664 8月  30 2017 CentOS-Base.repo
-rw-r--r--. 1 root root 1309 8月  30 2017 CentOS-CR.repo
-rw-r--r--. 1 root root  649 8月  30 2017 CentOS-Debuginfo.repo
-rw-r--r--. 1 root root  314 8月  30 2017 CentOS-fasttrack.repo
-rw-r--r--. 1 root root  630 8月  30 2017 CentOS-Media.repo
-rw-r--r--. 1 root root 1331 8月  30 2017 CentOS-Sources.repo
-rw-r--r--. 1 root root 3830 8月  30 2017 CentOS-Vault.repo
```

2.将CentOS-Base.repo和CentOS-Debuginfo.repo改名或者移动，绕过网络安装，以便使用本地安装

```
#本次使用改名 方便作为备份文件
[root@localhost yum.repos.d]# mv CentOS-Debuginfo.repo CentOS-Debuginfo.repo.bak
[root@localhost yum.repos.d]# mv CentOS-Base.repo CentOS-Base.repo.bak
```

3.编辑文件CentOS-Media.repo（使用vim编辑器）

```
[root@localhost yum.repos.d]# vim CentOS-Media.repo
# CentOS-Media.repo
#
#  This repo can be used with mounted DVD media, verify the mount point for
#  CentOS-7.  You can use this repo and yum to install items directly off the
#  DVD ISO that we release.
#
# To use this repo, put in your DVD and use it with the other repos too:
#  yum --enablerepo=c7-media [command]
#  
# or for ONLY the media repo, do this:
#
#  yum --disablerepo=\* --enablerepo=c7-media [command]

[c7-media]
name=CentOS-$releasever - Media
baseurl=file:///mnt/cdrom
gpgcheck=0		#用来检查GPG-KEY，0为不检查，1为检查
enabled=1		#是否用该yum源，0为禁用，1为使用
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

~                                                                               
~                                                                               
~                                                                               
"CentOS-Media.repo" 20L, 563C                                 17,1         全部
```

清除yum缓存，测试yum源配置

```
[root@localhost yum.repos.d]# yum clean all
[root@localhost yum.repos.d]# yum list
```

### 网络 yum 源

> 配置完本地后可以先安装 wget
>
> ```
> yum install wget
> ```
>
> 

**步骤一：备份**

1)进入/etc/yum.repos.d 查看目录下文件

```
[root@localhost yum.repos.d]# ll
总用量 28
-rw-r--r--. 1 root root 1664 8月  30 2017 CentOS-Base.repo.bak
-rw-r--r--. 1 root root 1309 8月  30 2017 CentOS-CR.repo
-rw-r--r--. 1 root root  649 8月  30 2017 CentOS-Debuginfo.repo.bak
-rw-r--r--. 1 root root  314 8月  30 2017 CentOS-fasttrack.repo
-rw-r--r--. 1 root root  563 3月  18 19:37 CentOS-Media.repo
-rw-r--r--. 1 root root 1331 8月  30 2017 CentOS-Sources.repo
-rw-r--r--. 1 root root 3830 8月  30 2017 CentOS-Vault.repo
```

2)将所有文件备份到新建目录repo_bak下

```
[root@localhost yum.repos.d]# mkdir repo_bak
[root@localhost yum.repos.d]# mv *.repo repo_bak/
[root@localhost yum.repos.d]# mv *.repo.bak repo_bak/
[root@localhost yum.repos.d]# ll
总用量 0
drwxr-xr-x. 2 root root 195 3月  18 23:13 repo_bak
```

**步骤二：下载阿里的 yum 源：下载阿里的CentOS-Base.repo 到/etc/yum.repos.d/**

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

2）运行yum clean all 清除缓存，运行 yum makecache 生成新的缓存

```
[root@localhost yum.repos.d]# yum clean all #清空缓存
[root@localhost yum.repos.d]# yum makecache #生成新的缓存 
```

## 安装 jdk

### 1. 检查是否有 jdk 文件

```
[root@cc ~]# yum list installed | grep java
[root@cc ~]# 
```

没有文件显示表明没有 jdk 文件

### 2. 安装 jdk，利用 yum 源

- 查看 yum 里的软件包列表

```
[root@cc ~]# yum search java | grep jdk
```

- 选择要安装的 jdk 版本进行安装

```
[root@cc ~]# yum install -y java-1.8.0-openjdk*
...
  pcsc-lite-libs.x86_64 0:1.8.8-8.el7                 pixman.x86_64 0:0.34.0-1.el7                       
  psmisc.x86_64 0:22.20-17.el7                        python-javapackages.noarch 0:3.4.1-11.el7          
  python-lxml.x86_64 0:3.2.1-4.el7                    ttmkfdir.x86_64 0:3.0.9-42.el7                     
  tzdata-java.noarch 0:2023c-1.el7                    xorg-x11-font-utils.x86_64 1:7.5-21.el7            
  xorg-x11-fonts-Type1.noarch 0:7.5-9.el7             xorg-x11-utils.x86_64 0:7.5-23.el7                 

完毕！
```

- 查看 jdk

```
[root@cc yum.repos.d]# java -version
openjdk version "1.8.0_362"
OpenJDK Runtime Environment (build 1.8.0_362-b08)
OpenJDK 64-Bit Server VM (build 25.362-b08, mixed mode)
```

### 3. 配置环境变量

- jdk 默认安装路径 /usr/lib/jvm

```
[root@cc jvm]# pwd
/usr/lib/jvm
[root@cc jvm]# ls
java                                             jre
java-1.8.0                                       jre-1.8.0
java-1.8.0-openjdk                               jre-1.8.0-openjdk
java-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64  jre-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64
java-openjdk                                     jre-openjdk
[root@cc jvm]# 
```

> **看准上面的 java 目录名称，每个都不一样**

- 在 /etc/profile 文件添加目录

```
# set java environment  
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64
PATH=$PATH:$JAVA_HOME/bin  
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  
export JAVA_HOME  CLASSPATH  PATH 

```

- 刷新文件并查看是否生效

```
[root@cc jvm]# source /etc/profile	# 刷新文件
[root@cc jvm]# echo $JAVA_HOME
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64
```

## 安装 hadoop

### 1. 生成密钥

```
[root@cc jvm]# ssh localhost
[root@cc jvm]# cd ~
[root@cc ~]# ls -al
总用量 28
dr-xr-x---.  3 root root  147 4月  16 19:20 .
dr-xr-xr-x. 17 root root  224 4月  16 08:23 ..
-rw-------.  1 root root 1257 4月  16 08:24 anaconda-ks.cfg
-rw-------.  1 root root 1828 4月  16 18:59 .bash_history
-rw-r--r--.  1 root root   18 12月 29 2013 .bash_logout
-rw-r--r--.  1 root root  176 12月 29 2013 .bash_profile
-rw-r--r--.  1 root root  176 12月 29 2013 .bashrc
-rw-r--r--.  1 root root  100 12月 29 2013 .cshrc
drwx------.  2 root root   25 4月  16 19:20 .ssh
-rw-r--r--.  1 root root  129 12月 29 2013 .tcshrc
[root@cc ~]# cd .ssh


ssh-keygen -t rsa    # 会有提示，都按回车即可
cat id_rsa.pub >> authorized_keys # 加入授权
chmod 600 ./authorized_keys # 修改文件权限
```

### 2. 安装和配置 hadoop

- 官网下载对应版本的 hadoop

> 这里相关软件安装包都放在 /opt/install-software 中，解压的软件放在 /opt/software 中
>
> 镜像源文件可能会更新导致下载地址失效，故网页打开确认下载地址

```
[root@cc install-software]# wget https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.2.3/hadoop-3.2.3.tar.gz
```

- 解压安装包

```
[root@cc install-software]# tar -zxvf hadoop-3.2.3.tar.gz -C /opt/software/
```

- 改名

```
[root@cc install-software]# cd ../software/
[root@cc software]# ls
hadoop-3.2.3
[root@cc software]# mv hadoop-3.2.3 hadoop
[root@cc software]# ls
hadoop
[root@cc software]# 
```

- 修改 core-site.xml 文件

```
[root@cc hadoop]# pwd
/opt/software/hadoop/etc/hadoop
[root@cc hadoop]# ls
capacity-scheduler.xml            httpfs-log4j.properties     mapred-site.xml
configuration.xsl                 httpfs-signature.secret     shellprofile.d
container-executor.cfg            httpfs-site.xml             ssl-client.xml.example
core-site.xml                     kms-acls.xml                ssl-server.xml.example
hadoop-env.cmd                    kms-env.sh                  user_ec_policies.xml.template
hadoop-env.sh                     kms-log4j.properties        workers
hadoop-metrics2.properties        kms-site.xml                yarn-env.cmd
hadoop-policy.xml                 log4j.properties            yarn-env.sh
hadoop-user-functions.sh.example  mapred-env.cmd              yarnservice-log4j.properties
hdfs-site.xml                     mapred-env.sh               yarn-site.xml
httpfs-env.sh                     mapred-queues.xml.template
[root@cc hadoop]# vi core-site.xml 
```

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <!-- 内网IP地址-->
        <value>hdfs://192.168.138.10:9000</value>
    </property>
    <!-- 缓存存储路径 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/app/hadooptemp</value>
    </property>
</configuration>
```

- 修改 hdfs-site.xml

```
<configuration>
    <!-- 默认为3，由于是单机，所以配置1 -->
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <!-- 配置http访问地址 -->
    <property>
          <name>dfs.http.address</name>
          <value>0.0.0.0:9870</value>
    </property>
</configuration>
```

- 修改 hadoop-env.sh 文件

```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64
```

- 修改 yarn-env.sh

```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64
```

- 修改sbin/stop-dfs.sh文件，在顶部增加

```
[root@cc sbin]# pwd
/opt/software/hadoop/sbin
[root@cc sbin]# ls
distribute-exclude.sh  mr-jobhistory-daemon.sh  start-dfs.sh         stop-balancer.sh    workers.sh
FederationStateStore   refresh-namenodes.sh     start-secure-dns.sh  stop-dfs.cmd        yarn-daemon.sh
hadoop-daemon.sh       start-all.cmd            start-yarn.cmd       stop-dfs.sh         yarn-daemons.sh
hadoop-daemons.sh      start-all.sh             start-yarn.sh        stop-secure-dns.sh
httpfs.sh              start-balancer.sh        stop-all.cmd         stop-yarn.cmd
kms.sh                 start-dfs.cmd            stop-all.sh          stop-yarn.sh
[root@cc sbin]# vi stop-dfs.sh 
# 顶部
HDFS_DATANODE_USER=root  
HDFS_DATANODE_SECURE_USER=hdfs  
HDFS_NAMENODE_USER=root  
HDFS_SECONDARYNAMENODE_USER=root 
...
```

- 修改 start-dfs.sh 文件顶部添加

```
HDFS_DATANODE_USER=root  
HDFS_DATANODE_SECURE_USER=hdfs  
HDFS_NAMENODE_USER=root  
HDFS_SECONDARYNAMENODE_USER=root 
```

- 格式化，进入 hadoop/bin 的文件夹，执行

```
[root@cc bin]# pwd
/opt/software/hadoop/bin
[root@cc bin]# ls
container-executor  hadoop.cmd  hdfs.cmd  mapred.cmd    test-container-executor  yarn.cmd
hadoop              hdfs        mapred    oom-listener  yarn
[root@cc bin]# ./hdfs namenode -format
```

- 修改 /etc/profile 文件，添加如下，用 source 刷新

```
export HADOOP_HOME=/opt/software/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

## 安装 spark

### 1. wget 下载

```
[root@cc install-software]# wget https://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-3.3.2/spark-3.3.2-bin-hadoop3.tgz
```

### 2. 解压并改名

```
[root@cc install-software]# ls
hadoop-3.2.3.tar.gz  spark-3.3.2-bin-hadoop3.tgz
[root@cc install-software]# tar -zxvf spark-3.3.2-bin-hadoop3.tgz -C /opt/software/
...
[root@cc install-software]# cd /opt/software/
[root@cc software]# ls
hadoop  spark-3.3.2-bin-hadoop3
[root@cc software]# mv spark-3.3.2-bin-hadoop3/ spark
[root@cc software]# ls
hadoop  spark
```

### 3. 修改配置文件

- spark/conf 备份模板文件 spark-env.sh.template

```
[root@cc conf]# ls
fairscheduler.xml.template  log4j2.properties.template  metrics.properties.template  spark-defaults.conf.template  spark-env.sh.template  workers.template
[root@cc conf]# cp spark-env.sh.template spark-env.sh
```

- 修改 spark-env.sh 文件

```
...
# Options read in YARN client/cluster mode
# - YARN_CONF_DIR, to point Spark towards YARN configuration files when you use YARN

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b08-1.el7_9.x86_64
export SPARK_MASTER_HOST=192.168.138.10
export SPARK_MASTER_PORT=7077


# Options for the daemons used in the standalone deploy mode
# - SPARK_MASTER_HOST, to bind the master to a different IP address or hostname
# - SPARK_MASTER_PORT / SPARK_MASTER_WEBUI_PORT, to use non-default ports for the master
# - SPARK_MASTER_OPTS, to set config properties only for the master (e.g. "-Dx=y")
...
```

- 进入 spark 目录下

```
[root@cc spark]# bin/spark-shell
```

## 安装 mysql

### 1. 查看是否有 MySQL，并查看软件列表

```
[root@cc spark]# rpm -qa|grep mariadb
mariadb-libs-5.5.68-1.el7.x86_64
[root@cc spark]# rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64
### 删除 mariadb
```

### 2. 查看linux版本

```
[root@cc software]# uname -a
Linux cc 3.10.0-1160.71.1.el7.x86_64 #1 SMP Tue Jun 28 15:37:28 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

### 3. 服务器使用 wget 下载 mysql

```
[root@cc install-software]# wget https://downloads.mysql.com/archives/get/p/23/file/mysql-8.0.21-1.el7.x86_64.rpm-bundle.tar
```

> 版本不要选错，x86_64版本

```
[root@cc install-software]# ls
hadoop-3.2.3.tar.gz  mysql-8.0.21-1.el7.x86_64.rpm-bundle.tar  spark-3.3.2-bin-hadoop3.tgz
```

> 下载成功，可以先用 Windows下载后上传至服务器中

### 4. 解压 MySQL 压缩包

```
[root@cc install-software]# tar -xvf mysql-8.0.21-1.el7.x86_64.rpm-bundle.tar -C /opt/temp/
```

<h3>依次安装MySQL数据库的mysql common、mysql libs、mysql client软件包，和server</h3>

```
[root@cc temp]# rpm -ivh mysql-community-common-8.0.21-1.el7.x86_64.rpm 
```

```
[root@cc temp]# rpm -ivh mysql-community-libs-8.0.21-1.el7.x86_64.rpm 
```

```
[root@cc temp]# rpm -ivh mysql-community-client-8.0.21-1.el7.x86_64.rpm 
```

```
[root@cc temp]# rpm -ivh mysql-community-server-8.0.21-1.el7.x86_64.rpm 
```

> 出现问题
>
> ```
> [root@cc temp]# rpm -ivh mysql-community-server-8.0.21-1.el7.x86_64.rpm 
> 警告：mysql-community-server-8.0.21-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
> 错误：依赖检测失败：
> 	/usr/bin/perl 被 mysql-community-server-8.0.21-1.el7.x86_64 需要
> 	net-tools 被 mysql-community-server-8.0.21-1.el7.x86_64 需要
> 	perl(Getopt::Long) 被 mysql-community-server-8.0.21-1.el7.x86_64 需要
> 	perl(strict) 被 mysql-community-server-8.0.21-1.el7.x86_64 需要
> ```
>
> 以上可以看出需要两个依赖：perl 和 net-tools，满足它
>
> 解决办法：安装依赖
>
> ```
> [root@cc temp]# yum install -y perl-Module-Install.noarch
> [root@cc temp]# yum -y install net-tools
> ```
>
> 重新执行安装 server 命令

最后可以用 rpm 查看一下安装包情况

```
[root@cc temp]# rpm -qa | grep mysql
mysql-community-client-8.0.21-1.el7.x86_64
mysql-community-libs-8.0.21-1.el7.x86_64
mysql-community-server-8.0.21-1.el7.x86_64
mysql-community-common-8.0.21-1.el7.x86_64
```

### 5. 初始化 mysql 数据库

```
[root@cx temp]# mysqld --initialize;									# 初始化
[root@cx temp]# chown mysql:mysql /var/lib/mysql -R;					# 授权
[root@cx temp]# systemctl start mysqld									# 开启服务
[root@cx temp]# systemctl status mysqld									# 查看服务状态
[root@cx temp]# systemctl enable mysql									# 开机自启
```

### 6. 查看密码，重设置密码

查看密码

```
[root@cc temp]# cat /var/log/mysqld.log | grep password
2023-06-04T14:31:37.415021Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: XPgh5q1r&qYQ
```

登录 mysql

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

修改密码

```
mysql> alter user 'root'@'localhost' identified with mysql_native_password by '密码';
Query OK, 0 rows affected (0.00 sec)
```

退出重新登录

```
[root@cc temp]# mysql -u root -p
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

允许远程访问服务器的MySQL，root是登录账户，%代表任意地址，可以设置你固定的ip地址，密码是远程登录的密码

1. 先创建用户

```
mysql> create user 'root'@'%' identified with mysql_native_password by '密码';
```


2. 设置远程访问并用flush privileges;刷新一下

  ```
  mysql> grant all privileges on *.* to 'root'@'%' with grant option;
  mysql> flush privileges;
  ```

  

### 7. 本地数据库可视化软件连接服务器中的MySQL

​	先看一下服务器的防火墙关没关

```
systemctl status firewalld
```

​	主要看activate，如果是绿色的表示正在运行，灰色的是关闭的

```
systemctl stop firewalld
```

## yum 安装 redis 数据库

### 1. 下载 epel 仓库

```
yum install epel-release -y
```

### 2. 下载 redis 数据库

```
yum install redis -y
```

### 3. 启动 redis 服务

```
systemctl start redis
```

### 4. redis 常见命令

```
systemctl status redis 查看服务状态
systemctl stop redis 停止服务
systemctl restart redis 重启服务
ps -ef | grep redis 查看reids服务信息
systemctl enable redis redis开机启动
```

### 5. 设置redis 远程连接和密码

```
vim /etc/redis.conf	如果没有vim先yum install -y vim
1. 注释 #bind 127.0.0.1
2. 修改protected-mode no
3. 修改 daemonize yes					# 后台运行
4. 修改 requirepass Dd123=123
```

### 6. 关闭防火墙或者开放6379端口(请自行百度)

### 7. 重启redis

## 安装 zookeeper （kafka 前提，默认端口：2181）

> zookeeper 下载地址：https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.5.10/
>
> kafka 下载地址：https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.8.2/

### 1. 解压 zookeeper 压缩包

```
[root@cc install-software]# tar -zxvf apache-zookeeper-3.5.10-bin.tar.gz -C /opt/software/
[root@cc install-software]# cd ../software/
[root@cc software]# ls
apache-zookeeper-3.5.10-bin  hadoop  kafka  spark
[root@cc software]# mv apache-zookeeper-3.5.10-bin zookeeper
[root@cc software]# ls
hadoop  kafka  spark  zookeeper
[root@cc software]# 
```

### 2. 修改配置文件，同时复制备份

```
[root@cc conf]# pwd
/opt/software/zookeeper/conf
[root@cc conf]# ls
configuration.xsl  log4j.properties  zoo_sample.cfg
[root@cc conf]# cp zoo_sample.cfg zoo.cfg
[root@cc conf]# ls
configuration.xsl  log4j.properties  zoo.cfg  zoo_sample.cfg
[root@cc conf]# 
```

### 3. 进入 bin 目录下启动 zookeeper 服务

```
[root@cc bin]# pwd
/opt/software/zookeeper/bin
[root@cc bin]# ls
README.txt    zkCli.cmd  zkEnv.cmd  zkServer.cmd            zkServer.sh          zkTxnLogToolkit.sh
zkCleanup.sh  zkCli.sh   zkEnv.sh   zkServer-initialize.sh  zkTxnLogToolkit.cmd
[root@cc bin]# ./zkServer.sh start
```

### 4. 查看运行状态

```
[root@cc bin]# pwd
/opt/software/zookeeper/bin
[root@cc bin]# ./zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/software/zookeeper/bin/../conf/zoo.cfg
Client port found: 2181. Client address: localhost. Client SSL: false.
Mode: standalone	# 代表单机模式
```

> 停止命令：./zkServer.sh stop

### 5. 本地链接 ./zkCli.sh 测试一下

```
[root@cc bin]# ./zkCli.sh 
Connecting to localhost:2181
...
```

### 6. 查看端口号

```
[root@cc software]# lsof -i:2181
-bash: lsof: 未找到命令
[root@cc software]# yum install lsof -y
```

```
[root@cc software]# lsof -i:2181
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    2064 root   69u  IPv6  40667      0t0  TCP *:eforward (LISTEN)
```



## 安装 kafka（端口默认：9092）

### 1. 解压 kafka 压缩包

```
[root@cc install-software]# tar -zxvf kafka_2.12-2.8.2.tgz -C /opt/software/
[root@cc opt]# cd software/
[root@cc software]# ls
hadoop  kafka_2.12-2.8.2  spark
[root@cc software]# mv kafka_2.12-2.8.2 kafka
[root@cc software]# ls
hadoop  kafka  spark
[root@cc software]# 
```

> - config 目录下 server.properties
>
> ```
> # 标识 broker 编号，集群中有多个broker，则每个broker的编号需要设置不同
> broker.id=0
> 
> # 修改下面两个配置（listeners 配置的ip和advertised.listeners相同时启动kafka会报错）
> listeners（内网ip）
> advertised.listeners(公网ip)
> 
> # 修改zk地址，默认地址
> zookeeper.connection=localhost:2181
> ```
>
> - bin 目录启动
>
>   ```
>   #启动
>   kafka-server-start.sh config/server.properties &
>   
>   #停止
>   kafka-server-stop.sh
>   ```
>
> - 创建 topic（第二个1为序号，xdclass-topic为名称）
>
>   ```
>   ./kafka-topics.sh --create --zookeeper 192.168.138.10:2181 --replication-factor --partitions 1 --topic xdclass-topic
>   ```
>
> - 查看 topic
>
>   ```
>   ./kafka-topics.sh --list --zookeeper 192.168.138.10:2181
>   ```
>
>   

### 2. 修改 config 配置文件

```
[root@cc config]# pwd
/opt/software/kafka/config
[root@cc config]# ls
connect-console-sink.properties    connect-file-source.properties   consumer.properties  server.properties
connect-console-source.properties  connect-log4j.properties         kraft                tools-log4j.properties
connect-distributed.properties     connect-mirror-maker.properties  log4j.properties     trogdor.conf
connect-file-sink.properties       connect-standalone.properties    producer.properties  zookeeper.properties
[root@cc config]# vim server.properties 
```

```
############################# Socket Server Settings #############################

# The address the socket server listens on. It will get the value returned from 
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
listeners=PLAINTEXT://192.168.138.10:9092	# 设置本地局域网地址
```

### 3. 启动 kafka（config要指明目录）

```
[root@cc bin]# pwd
/opt/software/kafka/bin
[root@cc bin]# ls
connect-distributed.sh        kafka-dump-log.sh                    kafka-storage.sh
connect-mirror-maker.sh       kafka-features.sh                    kafka-streams-application-reset.sh
connect-standalone.sh         kafka-leader-election.sh             kafka-topics.sh
kafka-acls.sh                 kafka-log-dirs.sh                    kafka-verifiable-consumer.sh
kafka-broker-api-versions.sh  kafka-metadata-shell.sh              kafka-verifiable-producer.sh
kafka-cluster.sh              kafka-mirror-maker.sh                trogdor.sh
kafka-configs.sh              kafka-preferred-replica-election.sh  windows
kafka-console-consumer.sh     kafka-producer-perf-test.sh          zookeeper-security-migration.sh
kafka-console-producer.sh     kafka-reassign-partitions.sh         zookeeper-server-start.sh
kafka-consumer-groups.sh      kafka-replica-verification.sh        zookeeper-server-stop.sh
kafka-consumer-perf-test.sh   kafka-run-class.sh                   zookeeper-shell.sh
kafka-delegation-tokens.sh    kafka-server-start.sh
kafka-delete-records.sh       kafka-server-stop.sh
[root@cc bin]# [root@cc bin]# ./kafka-server-start.sh ../config/server.properties &
# config 指明地址
# ctrl + c 退出
```

### 4. 查看端口号

```
[root@cc bin]# lsof -i:9092
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    2613 root  132u  IPv6  32230      0t0  TCP cc:XmlIpcRegSvc (LISTEN)
java    2613 root  151u  IPv6  32235      0t0  TCP cc:40136->cc:XmlIpcRegSvc (ESTABLISHED)
java    2613 root  152u  IPv6  43283      0t0  TCP cc:XmlIpcRegSvc->cc:40136 (ESTABLISHED)
```

```
[root@cc bin]# netstat -tunlp|egrep "(2181|9092)"
tcp6       0      0 192.168.138.10:9092     :::*                    LISTEN      2613/java           
tcp6       0      0 :::2181                 :::*                    LISTEN      2064/java 
```

> zk：2181端口。pid：2064
>
> kafka：9092端口。pid：2613

### 5. 单机连通性测试

- 再启动两个xshell客户端，一个用于生产者返送信息，一个用于消费者接受信息，原先的不要关

- 两台都要 cd 到安装目录下：

  ```
  cd /opt/software/kafka
  ```

- 一个shell创建topic名为test

  ```
  [root@cc ~]# cd /opt/software/kafka/
  [root@cc kafka]# ./bin/kafka-topics.sh --create --zookeeper 192.168.138.10:2181 --replication-factor 1 --partitions 1 --topic test
  Created topic test.
  [root@cc kafka]# 
  
  ```

  - 打开一个 Producer

    ```
    [root@cc kafka]# ./bin/kafka-console-producer.sh --broker-list 192.168.138.10:9092 --topic test
    ```

- 另一个打开 Consumer

  ```
  [root@cc ~]# cd /opt/software/kafka/
  [root@cc kafka]# ./bin/kafka-console-consumer.sh --bootstrap-server 192.168.138.10:9092 --topic itcast_order --
  ```

  

