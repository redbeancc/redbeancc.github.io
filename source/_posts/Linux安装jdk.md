---
title: Linux安装jdk
tags:
  - linux
  - java
categories:
  - 教程
description: "\U0001F355使用 centOS7.6 安装 jdk"
abbrlink: c357b778
date: 2021-11-11 11:40:53
cover:
---
# Linux安装jdk

> 刚白嫖了一个服务器，美滋滋，然后在服务器上配置centos7的系统
>
> 在这里配置一下jdk

#### 1. 用xshell中sftp把windows文件上传到我的centos服务器

文件位置我放在了/opt/software-install

#### 2. 然后使用 rpm -ivh jdk名称.rpm --prefix=/opt/software

```shell
[root@cx software-install]# ls
jdk-8u221-linux-x64.rpm
[root@cx software-install]# rpm -ivh jdk-8u221-linux-x64.rpm --prefix=/opt/software
```

等待安装成功

#### 3. 改一下目录名字，改成Java

```shell
[root@cx software]# ls
jdk1.8.0_221-amd64
[root@cx software]# mv jdk1.8.0_221-amd64/ java
[root@cx software]# ls
java
```

#### 3. 软件安装完成了，那现在就开始配置一下环境变量

​	vi进入/etc/profile

```shell
vi /etc/profile
```

​	按shitf+g进入到最后一行，在最后一行按o加入以下信息

```shell
export JAVA_HOME=/opt/software/java
export PATH=$PATH:$JAVA_HOME/bin
```

​	保存退出，esc+:x+enter

#### 4. 重启配置文件信息

```shell
source /etc/profile
```

#### 5. 使用java -version查看是否安装成功

```shell
[root@cx software]# java -version
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)
```

### 出现版本信息，表示大功告成！你很棒哟