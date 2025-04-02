---
title: 关于使用scrapy绕过400状态码报错，停止爬取的方法
tags:
  - python
  - scrapy
  - 爬虫
categories:
  - 问题
description: 🧂scrapy 遇到 400 状态码后忽略继续爬取的方法
abbrlink: 1fdc6d41
date: 2021-10-26 20:49:48
cover:
---
# 关于使用scrapy绕过400状态码报错，停止爬取的方法

> 我在爬起点小说网的时候，因为是扫网页，也就是一个一个去试，导致不可避免的会访问到400的请求
>
> 而scrapy在遇到400的状态码的时候会自动停止爬取，在爬虫文件中的parse方法里获取不到返回的信息
>
> 直接给你报错，然后终止爬虫工作，就很烦。。。
>
> 在网上找了好久，终于找到一个方法，能在获取到400后不停止，直接返回到爬虫文件的parse中的response

### 在settings.py中设置

```
# 忽略400报错，直接传入parse
HTTPERROR_ALLOWED_CODES = [400]
```

### 然后在爬虫文件中的parse方法的response获取到

```
def parse(self, response):
	print(response)
	print(response.status)
```

### 在控制台中可以看到信息了

```
<400 https://book.qidian.com/info/1030564368/>
400
```

### 下面就可以根据不同的需求进行相关的代码编写了

