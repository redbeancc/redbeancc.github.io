---
title: docker 学习笔记
tags:
  - docker
categories:
  - 学习
description: "\U0001F355 docker 学习笔记"
abbrlink: b48ae4c5
date: 2024-01-13 16:48:51
cover:
---
# Docker

## 1. 初识 Docker

快速构建、运行、管理应用的工具

### 1.1 安装

**删除已有的 docker 版本**

```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```



**安装所需的软件包**

首先安装 yum 工具

```
yun install -y yum-utils
```

安装成功后，配置 yum 源（阿里）

```
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

清华源

```
https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



**安装 Docker**

```
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```



**校验安装**

```
docker -v
```

```
docker images
```

> images 会出现错误，代表 docker 还没有启动



**启动和校验**

```
# 启动 docker
systemctl start docker

# 关闭
systemctl stop docker

# 重启
systemctl restart docker

# 开机自启
systemctl enable docker

# 不报错说明安装成功
docker ps
```



**配置镜像加速器**

阿里云为例：镜像容器服务 -> 镜像工具 -> 镜像加速器



您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://****.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 2. Docker 使用

### 2.1 部署 MYSQL

```
docker run -d --name mysql -p 3307:3306 -e TZ=Asia/Shanghai -e MYSQL_ROOT_PASSWORD=123456 mysql

# 3307:3306  映射端口:容器端口
# netstat -ntlp 查看所有端口
```

<span style="font-size:32px">**问题**</span>

```
# docker 暂停
docker stop mysql

# docker 删除
docker rm mysql
```

```
# 进入容器

# docker exec -it 容器号或名 /bin/bash
docker exec -it b30062adc08c /bin/bash
# 或
docker exec -it mysql /bin/bash

# 进入 mysql
mysql -uroot -p

# 查看信息
select host,user,plugin,authentication_string from mysql.user;

#更新

mysql> use mysql;
mysql> alter user 'root'@'%' identified with mysql_native_password by '123456';
mysql> flush privileges;
mysql> select host,user,plugin,authentication_string from mysql.user;
```

### 2.2 镜像和容器

当我们利用Docker安装应用时，Docker会自动搜索并下载应用**镜像**(image)。镜像不仅包含应用本身，还包含应用运行所需要的环境、配置、系统函数库。Docker会 在运行镜像时创建一个隔 离环境,称为**容器**(container) 。

![image-20240111172934762](https://img-blog.csdnimg.cn/img_convert/3c0ad1b790c12df5a392dcc5350ae0ad.png)

### 2.3 常用命令

#### run 命令

```shell
docker run -d \
--name mysql \
-p 3307:3306 \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123456 \
mysql
```

- run：创建并运行
- --name：容器名称
- -p：端口映射，docker端口:容器端口
- -e：envouriment，环境变量，具体看文档

镜像名称结构

- Repository:TAG，例 mysql:5.7

![image-20240111180725331](https://img-blog.csdnimg.cn/img_convert/d1613d3d3c89231c04576efc569df0d7.png)

<span style="font-size:32px">**注意**</span>

- rmi：删除镜像

- rm：删除容器

- exec：进入容器内部操作

  ```
  docker exec -it <容器名> bash
  ```

### 2.4 数据卷

![image-20240111182626491](https://img-blog.csdnimg.cn/img_convert/ef6a406618dbaabae533d1d46c152c9c.png)

|             命令             | 说明               |
| :--------------------------: | ------------------ |
|     docker volume create     | 创建数据卷         |
|       docker volume ls       | 查看所有数据卷     |
|       docker volume rm       | 删除指定数据卷     |
| docker volume inspect <卷名> | 查看某个数据卷详情 |
|     docker volume prune      | 清除数据卷         |

```
# 可以使用 help 命令查看参数详情
docker volume --help
```

#### 注意

- 必须在 **docker run -v 数据卷名(这里可用绝对路径):容器内目录** 时才能挂载映射，已经创建好的容器不可挂载

- 当创建容器时，如果挂载了数据卷且数据卷不存在，会自动创建

  ```
  doucker run -d --name nginx -p 80:80 -v html:/usr/share/nginx/html(需求目录) nginx
  ```

### 2.5 自定义镜像

#### 2.5.1 镜像结构

![image-20240111191232626](https://img-blog.csdnimg.cn/img_convert/921d27bb84b9d7af00184159e7c6a1ae.png)

#### 2.5.2 Dockerfile

```
# 基础镜像
FROM openjdk:11.0-jre-buster
# 设定时区
ENV TZ=Asia/Shanghai
RUN timedatectl set-timezone $TZ
# 拷贝jar包
COPY docker-demo. jar /app.jar
# 入口
ENTRYPOINT ["java","-jar","/app.jar"]
```

```
# 构建 docker 
docker build -t myImage:1.0 .
```

- **-t**：镜像起名，格式（镜像名：版本号，默认latest）
- **。**：是指Dockerfile所在目录，如果就在当前目录就是 . 。

### 2.6 网络

加入自定义网络的容器才可以通过容器名互相访问

|           命令            |         说明         |
| :-----------------------: | :------------------: |
|   docker network create   |       创建网络       |
|     docker network ls     |     查看所有网络     |
|     docker network rm     |     删除指定网络     |
|   docker network prune    |    清除未使用网络    |
|  docker network connect   | 使指定容器加入某网络 |
| docker network disconnect |      脱离某网络      |
|  docker network inspect   |     查看网络信息     |

```
# 创建
docker network create yuxuan

# 查看
docker network ls

# mysql容器加入yuxuan网络
docker network connect yuxuan mysql
docker run -d --name mysql -p 3306:3306 --network yuxuan mysql
```

## 3. 项目部署

