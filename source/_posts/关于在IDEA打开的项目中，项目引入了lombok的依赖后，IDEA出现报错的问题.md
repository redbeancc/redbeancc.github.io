---
title: 关于在IDEA打开的项目中，项目引入了lombok的依赖后，IDEA出现报错的问题
tags:
  - idea
  - lombok
categories:
  - 问题
description: 🧂在IDEA打开的项目中，项目引入了lombok的依赖后，IDEA出现报错
cover: 'https://img-blog.csdnimg.cn/direct/2958dbea421347aab16a2cb95645537e.png'
abbrlink: 1c82ad06
date: 2021-09-18 11:41:05
---
# 关于在IDEA打开的项目中，项目引入了lombok的依赖后，IDEA出现报错的问题

## 一、首先确定项目是否成功引入了lombok依赖

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

## 二、查看实体类中的方法是否添加了注解@Data

## 三、查看所使用lombok语法的方法类是否添加注解@Data

## 四、在IDEA的Plugins中下载插件lombok

