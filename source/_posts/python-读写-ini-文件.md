---
title: python 读写 ini 文件
tags:
  - python
categories:
  - 问题
description: "\U0001F384 通过模块来对 ini 文件进行读写，ini 文件可以当作配置文件使用"
abbrlink: fed6a8a
date: 2022-09-27 15:13:11
cover:
---
# python 读写 ini 文件

.ini 文件是**配置文件**，一般存放配置信息

一个 .ini 文件由多个 section 组成，每个 section 下可以有多个键值对

结构如下：

```
[user] 					# 这里是 section
username = admin 		# k = v 的形式保存
password = passwd 		# 一个 section 下可以允许多个键值对

[info] 					# 可以存在多个 section
nickname = test
```

### 导包

python 下读取 ini 文件需要 configparser 模块

```shell
pip install configparser
```

### 写入

```python
import configparser

config = configparser.ConfigParser()  # 实例化对象

# 数据写入
config.add_section('header')  # 创建名称为 header 的 section

# 写入在 header 下，键值对数据
config.set('header', 'Referer', 'xxxx')
# 文件默认在代码文件的目录下
config.write(open('config.ini','a'))
```

config.ini

```ini
[header]
referer = xxxx
```

### 读取（展示方法获取的数据为 str 类型，注意数据转换）

```python
# 读取数据
# 首先读取文件
config.read('config.ini')
# 提取文件信息
value = config.get('header', 'Referer')
print(value)		# xxxx
# 注意这里获取的数据类型都是 str，注意数据转换
print(type(value))	# <class 'str'>

# 注意：这种获取方式也可以，同样获取的数据类型为 str 类型
value = config['header']['Referer']
```

