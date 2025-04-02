---
title: vm虚拟机挂起后docker容器访问失败
tags:
  - docker
  - VMware
categories:
  - 问题
description: "\U0001F370 虚拟机挂载后重新启动，docker 访问受限问题"
abbrlink: d35f956c
date: 2024-01-13 16:50:35
cover:
---
# vm虚拟机挂起后docker容器访问失败

> 前提，防火前全部关闭
>
> ```
> # 关闭防火墙
> systemctl stop firewalld
> # 禁止开机自启
> systemctl disable firewalld
> ```
>
> 

## 解决方法

### 1. Docker 重启（治标不治本）

```
# 重启docker服务
systemctl restart docker
# docker -q 静默模式，只显示容器编号
docker start $(docker ps -qa)
```

### 2. 开启 ipv4 转发功能

```
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
systemctl restart network
# 查看
sysctl net.ipv4.ip_forward
# 出现 net.ipv4.ip_forward=1 即为成功
```

> **注意：这里如果报错可尝试将MAC地址添加到 ifcfg-ens33（网卡配置文件）中**
>
> ```
> # 查看 ip
> [root@cc ~]# ip addr
> 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
>     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
>     inet 127.0.0.1/8 scope host lo
>        valid_lft forever preferred_lft forever
>     inet6 ::1/128 scope host 
>        valid_lft forever preferred_lft forever
> 2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
>     link/ether 00:0c:29:04:ca:00 brd ff:ff:ff:ff:ff:ff
>     inet 192.168.138.10/24 brd 192.168.138.255 scope global noprefixroute ens33...
> 
> # 00:0c:29:04:ca:00 就是MAC地址
> [root@cc ~]# vi /etc/sysconfig/network-scripts/ifcfg-ens33 
> # 文件添加 HWADDR=00:0c:29:04:ca:00
> # 重新启动 
> systemctl restart network
> ```

### 3. 将docker的网络接口设置为不被NetworkManager管理

新建配置文件

```
vi /etc/NetworkManager/conf.d/10-unmanage-docker-interfaces.conf
```

在文件中添加 **注意：这里文件不要强迫症用回车分号处断开**

```
[keyfile]
unmanaged-devices=interface-name:docker*;interface-name:veth*;interface-name:br-*;interface-name:vmnet*;interface-name:vboxnet*
```

保存退出，重启服务

```
systemctl restart NetworkManager
```

#### 经此三步后，虚拟机挂起后恢复，docker容器也可以正常访问

感谢阅读