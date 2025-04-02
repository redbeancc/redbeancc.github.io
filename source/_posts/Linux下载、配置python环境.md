---
title: Linux下载、配置python环境
tags:
  - linux
  - python
categories:
  - 教程
description: "\U0001F384手动安装 python 环境"
abbrlink: dc433e44
date: 2021-11-11 22:46:25
cover:
---
# Linux下载、配置python环境

> 刚刚把java环境配了，现在配python环境
>
> 因为centos7自带python2的环境
>
> 所以我们需要更新一下系统自带的版本

python官网：[https://www.python.org](https://www.python.org/)

### 1. 进入/opt/software-install，使用wget命令下载python

```shell
[root@cx software-install]# wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz
[root@cx software-install]# ls
jdk-8u221-linux-x64.rpm  Python-3.6.3.tgz
```

### 2. 解压到/opt/software下，更名为python

```shell
[root@cx software-install]# tar -zxvf Python-3.6.3.tgz -C /opt/software
[root@cx software]# ls
java  python
```

### 3. 进入到python目录下，安装配置

```shell
[root@cx python]# ./configure
```

​	等待执行完毕

### 4. 编译并安装

```shell
[root@cx python]# make
[root@cx python]# make install
```

### 5. 查看python3安装的位置

```shell
[root@cx python]# ll /usr/local/bin/python*
lrwxrwxrwx 1 root root        9 Nov 11 22:13 /usr/local/bin/python3 -> python3.6
-rwxr-xr-x 2 root root 12650320 Nov 11 22:12 /usr/local/bin/python3.6
lrwxrwxrwx 1 root root       17 Nov 11 22:13 /usr/local/bin/python3.6-config -> python3.6m-config
-rwxr-xr-x 2 root root 12650320 Nov 11 22:12 /usr/local/bin/python3.6m
-rwxr-xr-x 1 root root     3097 Nov 11 22:13 /usr/local/bin/python3.6m-config
lrwxrwxrwx 1 root root       16 Nov 11 22:13 /usr/local/bin/python3-config -> python3.6-config
```



```shell
[root@cx python]# which python
/usr/bin/python
[root@cx python]# ll /usr/bin/python*
lrwxrwxrwx 1 root root   22 Nov 11 22:20 /usr/bin/python -> /usr/local/bin/python3
lrwxrwxrwx 1 root root    9 Jul 11  2019 /usr/bin/python2 -> python2.7
-rwxr-xr-x 1 root root 7216 Jun 21  2019 /usr/bin/python2.7
-rwxr-xr-x 1 root root 1835 Jun 21  2019 /usr/bin/python2.7-config
lrwxrwxrwx 1 root root   16 Jul 11  2019 /usr/bin/python2-config -> python2.7-config
lrwxrwxrwx 1 root root    7 Jul 11  2019 /usr/bin/python.bak -> python2
lrwxrwxrwx 1 root root   14 Jul 11  2019 /usr/bin/python-config -> python2-config
```

​	这里我已经配置好了，所以可以看到我的软链接已经指向我新的python3，旧的话应该指的是python2

### 6. 备份原有配置，设置python默认版本号为3.x

```shell
mv /usr/bin/python /usr/bin/python.bak
ln -s /usr/local/bin/python3 /usr/bin/python
```

### 7. 这里使用python -V可以看到版本为python3了

### 8. 查看python2的位置

```shell
[root@cx python]# ll /usr/bin/python*
lrwxrwxrwx 1 root root   22 Nov 11 22:20 /usr/bin/python -> /usr/local/bin/python3
lrwxrwxrwx 1 root root    9 Jul 11  2019 /usr/bin/python2 -> python2.7
-rwxr-xr-x 1 root root 7216 Jun 21  2019 /usr/bin/python2.7
-rwxr-xr-x 1 root root 1835 Jun 21  2019 /usr/bin/python2.7-config
lrwxrwxrwx 1 root root   16 Jul 11  2019 /usr/bin/python2-config -> python2.7-config
lrwxrwxrwx 1 root root    7 Jul 11  2019 /usr/bin/python.bak -> python2
lrwxrwxrwx 1 root root   14 Jul 11  2019 /usr/bin/python-config -> python2-config
```

### 9. 为了使yum命令正常使用，需要将其配置的python依然指向2.x版本

/usr/bin/yum
/usr/libexec/urlgrabber-ext-down
将上面两个文件的头部文件修改为老版本即可
!/usr/bin/python 修改成 !/usr/bin/python2.7


=======================================================
# 新方法
查看 python 的所在目录
```shell
[root@VM-4-2-centos /]# whereis python
python: /usr/bin/python3.6m 
/usr/bin/python3.6 
/usr/bin/python 
/usr/bin/python2.7-config 
/usr/bin/python2.7 
/usr/lib/python3.6 
/usr/lib/python2.7 
/usr/lib64/python3.6 
/usr/lib64/python2.7 
/etc/python 
/usr/local/lib/python3.6 
/usr/include/python3.6m 
/usr/include/python2.7 
/usr/share/man/man1/python.1.gz

```
这里可以看到 python 在 /usr/bin 目录下

切换到这个目录
```shell
[root@VM-4-2-centos bin]# ll python*
lrwxrwxrwx 1 root root     7 Jan  8  2021 python -> python2
lrwxrwxrwx 1 root root     9 Jan  8  2021 python2 -> python2.7
-rwxr-xr-x 1 root root  7144 Nov 17  2020 python2.7
-rwxr-xr-x 1 root root  1835 Nov 17  2020 python2.7-config
lrwxrwxrwx 1 root root    16 Jan  8  2021 python2-config -> python2.7-config
lrwxrwxrwx 1 root root     9 Jan  8  2021 python3 -> python3.6
-rwxr-xr-x 2 root root 11328 Nov 17  2020 python3.6
-rwxr-xr-x 2 root root 11328 Nov 17  2020 python3.6m
lrwxrwxrwx 1 root root    14 Jan  8  2021 python-config -> python2-config
```
可以看到 centos7.6 这里已经自带了 python2 和 python3

可以使用 python --version 查看各自版本
```shell
[root@VM-4-2-centos bin]# python2 --version
Python 2.7.5
[root@VM-4-2-centos bin]# python3 --version
Python 3.6.8
```
接下来查看管理器 pip，依然在这个 /usr/bin 目录下
```shell
[root@VM-4-2-centos bin]# ll pip*
-rwxr-xr-x 1 root root 282 Sep  3  2020 pip
-rwxr-xr-x 1 root root 284 Sep  3  2020 pip2
-rwxr-xr-x 1 root root 288 Sep  3  2020 pip2.7
-rwxr-xr-x 1 root root 407 Oct 14  2020 pip3
lrwxrwxrwx 1 root root   9 Jan  8  2021 pip-3 -> ./pip-3.6
lrwxrwxrwx 1 root root   8 Jan  8  2021 pip-3.6 -> ./pip3.6
-rwxr-xr-x 1 root root 407 Oct 14  2020 pip3.6
```
这里代表的意思是：
1. 使用 pip2 命令的是 python2
2. 使用 pip3 命令的是 python3

可以使用各自的命令 pip list 查看各自的包依赖
```shell
[root@VM-4-2-centos bin]# pip3 list
Package            Version  
------------------ ---------
certifi            2022.9.24
charset-normalizer 2.0.12   
idna               3.4      
pip                9.0.3    
requests           2.27.1   
setuptools         39.2.0   
urllib3            1.26.12  
```
注意：如果这里出现警告的信息，原因是 pip 的版本需要更新。

也可以修改 pip 的配置文件。

```shell
[root@VM-4-2-centos bin]# cd /
[root@VM-4-2-centos /]# find -name pip.conf
./root/.pip/pip.conf
[root@VM-4-2-centos /]# cd /root/.pip
[root@VM-4-2-centos .pip]# ls
pip.conf
[root@VM-4-2-centos .pip]# vi pip.conf
```
加上 list section 即可
```shell
[global]
index-url = http://mirrors.tencentyun.com/pypi/simple
trusted-host = mirrors.tencentyun.com
[list]
format = columns
```