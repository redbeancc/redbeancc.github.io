---
title: Linux安装、配置node环境
tags:
  - linux
  - node
categories:
  - 教程
description: "\U0001F381手把手安装 node 环境"
abbrlink: bb18755d
date: 2021-11-12 14:10:19
cover:
---
# Linux安装、配置node环境

> java和python环境已经配置好了，现在尝试一下配置node环境

### 1. node下载地址：https://nodejs.org/dist

​	进入到/opt/software-install目录下执行

```shell
[root@cx software-install]# wget https://nodejs.org/dist/v14.3.0/node-v14.3.0-linux-x64.tar.gz
[root@cx software-install]# ls
jdk-8u221-linux-x64.rpm  node-v14.3.0-linux-x64.tar.gz  Python-3.6.3.tgz
```

​	等待下载成功后，可以ls看到文件夹下已经有了node安装文件

### 2. 解压node的安装包

```shell
[root@cx software-install]# tar -zxvf node-v14.3.0-linux-x64.tar.gz /opt/software
```

​	等待解压，进入/opt/software目录下可以看到已经生成了文件

```shell
[root@cx software]# ls
java  node-v14.3.0-linux-x64  python
```

​	更改一下名字，这个名字太长了

```shell
[root@cx software]# mv node-v14.3.0-linux-x64/ node
[root@cx software]# ls
java  node  python
```

### 3. 通过修改环境变量来设置node

```shell
[root@cx software]# vi /etc/profile
```

```shell
[root@cx software]# cat /etc/profile|tail -n 5

export JAVA_HOME=/opt/software/java
export PATH=$PATH:$JAVA_HOME/bin

export PATH=$PATH:/opt/software/node/bin
```

 重新加载/etc/profile文件

```shell
[root@cx software]# source /etc/profile
```

### 4. 查看版本，验证是否安装成功

```shell
[root@cx software]# node -v
v14.3.0
[root@cx software]# npm -v
6.14.5
```

至此，安装成功

三句话：下载，解压，配置环境变量
