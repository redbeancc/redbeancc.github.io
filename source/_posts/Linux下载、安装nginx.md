---
title: Linux下载、安装nginx
tags:
  - linux
  - Nginx
categories:
  - 教程
description: "\U0001F963手把手在 Linux 系统安装 nginx"
abbrlink: 4a0cedfb
date: 2021-11-14 19:30:42
cover:
---
# Linux下载、安装nginx

> 好的，我们只剩下最后一步，安装配置nginx

### 1. 下载并解压nginx

```shell
[root@cx software-install]# wget http://nginx.org/download/nginx-1.18.0.tar.gz
[root@cx software-install]# tar -xvf nginx-1.18.0.tar.gz -C /opt/software
```

```shell
[root@cx software]# ls
java  nginx-1.18.0  node  python
```

### 2. 进入安装目录，执行编译安装

```shell
[root@cx nginx-1.18.0]# ls
auto  CHANGES  CHANGES.ru  conf  configure  contrib  html  LICENSE  man  README  src
[root@cx nginx-1.18.0]# pwd
/opt/software/nginx-1.18.0
```

```shell
[root@cx nginx-1.18.0]# ./configure 
[root@cx nginx-1.18.0]# make
[root@cx nginx-1.18.0]# make install
```

如果在执行编译安装时发生错误，则需要安装nginx相关依赖包，然后重新执行编译安装

```shell
[root@cx nginx-1.18.0]# yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
```

### 3. 配置/usr/local/nginx/conf/nginx.conf

```shell
[root@cx novel]# vi /usr/local/nginx/conf/nginx.conf
```

```
server {
        listen       80;
        server_name  ###.###.###.###; #服务端地址
 
        location / {
            root   html;
            index  index.html index.htm;
        }
}
```
```shell
# 检查配置文件
sudo /usr/local/nginx/sbin/nginx  -t
#启动
sudo /usr/local/nginx/sbin/nginx
# 重启加载配置
sudo /usr/local/nginx/sbin/nginx -s reload
```

### 4. 访问###.###.###.###:80

出现

# Welcome to nginx!

代表安装成功！