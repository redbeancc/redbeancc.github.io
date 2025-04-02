---
title: IDEA 的初始化配置
tags:
  - idea
categories:
  - 教程
description: ⚽ 个人顺手的 IDEA 配置
abbrlink: 631eafec
date: 2024-01-18 10:15:59
cover:
---
# 初始化 IDEA

## 插件

> 1. Lombok
> 2. Material Theme UI
> 3. Maven Helper
> 4. MybatisX
> 5. Rainbow Brackets
> 6. Scala
> 7. **Spring Assistant**

## 设置

1. 滑轮改变字体大小

   Setting -> Editor -> Genral 

   Change font size with Ctrl+Mouse Wheel 勾上

2. 自动导包

   Setting -> Editor -> Genral -> Auto import

   Add unambiguous imports on the fly 勾上

   Optimize imports on the fly（for current project）勾上

3. 忽略大小写

   Setting -> Editor -> Genral -> Code Completion

   Match case 取消勾选

4. 文件注释
   File > settings > Editor -> File and Code Templates

   标签页中有 includes -> File Header

   ```
   /**
       @author 你的名字
       @create ${YEAR}-${MONTH}-${DAY}-${TIME}
   */
   ```

5. 鼠标悬停弹出解释说明

   setting > Editor > General > Code Completion

   勾选 show the parameter info popup in 1000 ms

6. 默认编码 utf-8

   File > settings > Editor > File Encodings

   全改成 UTF-8

7. 设置maven

   ```
   <?xml version="1.0" encoding="UTF-8"?> <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd"> <localRepository>C:\Users\25361\.m2\repository</localRepository> <mirrors> <mirror> <id>alimaven</id> <mirrorOf>central</mirrorOf> <name>aliyun maven</name> <url>http://maven.aliyun.com/nexus/content/repositories/central/</url> </mirror> </mirrors> <profiles> <profile> <id>jdk-1.8</id> <activation> <activeByDefault>true</activeByDefault> <jdk>1.8</jdk> </activation> <properties> <maven.compiler.source>1.8</maven.compiler.source> <maven.compiler.target>1.8</maven.compiler.target> <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion> </properties> </profile> </profiles> </settings>
   ```

8. 忽略 .idea 文件夹

