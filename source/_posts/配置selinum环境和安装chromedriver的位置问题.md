---
title: 配置selinum环境和安装chromedriver的位置问题
tags:
  - selinum
  - chromedriver
  - 自动化
  - 爬虫
categories:
  - 问题
description: "\U0001F38A浏览器自动化测试"
abbrlink: c9385a35
date: 2021-11-12 15:02:52
cover:
---
## 一、安装selenium

```powershell
pip install selenium
```

## 二、安装chromedriver_win32和phantomjs-2.1.1-windows

1. 在c盘下新建一个文件夹，取名为driver;如C:\driver
2. 将两个压缩包解压
3. 将解压包中的两个exe文件放在C:\driver下
4. 配置系统变量Path添加C:\driver

ps:如果不能用的话如下

在这里下载chromedriver文件：http://npm.taobao.org/mirrors/chromedriver/

​		在python.exe同目录下，把chromeDriver.exe文件放在这里

** ​(注意这里放的位置能放三个地方，一个是随便放一个地方，然后把系统的环境变量引到此位置
​		第二个 是放在python.exe存放的位置，第三个则放在项目的目录下) **
