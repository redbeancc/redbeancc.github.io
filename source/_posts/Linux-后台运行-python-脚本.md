---
title: Linux 后台运行 python 脚本
tags:
  - linux
  - python
categories:
  - 教程
description: "\U0001F3AE 在服务器上，在退出终端后，仍需程序继续运行，需要设置后台运行"
abbrlink: bcf4774e
date: 2022-11-08 15:18:46
cover:
---
# Linux 后台运行 python 脚本

在服务器上，在退出终端后，仍需程序继续运行，需要设置后台运行

## 关键命令：nohup

### 基本用法

```shell
nohup python3 -u Sign.py > sign.log 2>&1 &
```

## 含义解释

### nohup： \*不挂起\* 的意思

|        参数        | 解释                                                         |
| :----------------: | ------------------------------------------------------------ |
| python3	Sign.py | 运行 python 程序                                             |
|         -u         | 代表程序不启用缓存，也就是把输出直接放到 log 文件中，没这个参数，log文件的生成会有延迟。 |
|    \> sign.log     | 将输出日志保存到这个 log 中                                  |
|        2>1         | 2与>结合代表错误重定向，而1则代表错误重定向到一个文件1，而不代表标准输出； |
|        2>&1        | 换成2>&1，&与1结合就代表标准输出了，就变成错误重定向到标准输出. |
|         &          | 最后一个& ，代表该命令在后台执行                             |

## *命令运行后会有提示，示例：

```
[1] 29812
```

代表进程 29812 中运行，在 kill 时　进程 ID　就是　29812

## *查看nohub命令下运行的所有后台进程：

## jobs

```shell
[root@VM-4-2-centos heath-punch]# jobs
[1]+  Running                 nohup python3 -u Sign.py > sign.log 2>&1 &
```

## *查看后台运行的所有python 进程：

```shell
[root@VM-4-2-centos heath-punch]# ps -ef | grep python3
进程拥有者	PID  	CPU使用率
root      29812 	27854  	2 16:57 pts/0    00:00:00 python3 -u Sign.py
root      29863 	27854  	0 16:57 pts/0    00:00:00 grep --color=auto python3
```

## *删除进程

kill -9 [进程id]

-9 的意思是强制删除

```shell
[root@VM-4-2-centos heath-punch]# kill -9 29812
[root@VM-4-2-centos heath-punch]# ps -ef | grep python3
root     30052 27854  0 16:58 pts/0    00:00:00 grep --color=auto python3
[1]+  Killed                  nohup python3 -u Sign.py > sign.log 2>&1
```

第二个的意思就是正在删除进程，稍等一会后再次查看进程

```shell
[root@VM-4-2-centos heath-punch]# jobs
[root@VM-4-2-centos heath-punch]# ps -ef | grep python3
root     30410 27854  0 17:00 pts/0    00:00:00 grep --color=auto python3
```

