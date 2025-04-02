---
title: 修改 red hat 6 系统时间
tags:
  - linux
  - redhat6
categories:
  - 教程
description: 🌭修改 Redhat6 的系统时间为东八区的
abbrlink: 82fcd2c0
date: 2021-10-21 17:54:12
cover:
---
# 修改red hat 6系统时间

## date，查看系统时间

```
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

执行完这个命令后

再次执行date查看系统时间，发现时间更改为上海时间

用**reboot**重新启动查看date也是上海时间