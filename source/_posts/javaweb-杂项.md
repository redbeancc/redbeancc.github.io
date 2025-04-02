---
title: javaweb 杂项
tags:
  - javaweb
categories:
  - 问题
description: "\U0001F381 tomcat的jar位置和idea工程的jar包位置不同，有的jar包需要导入在tomcat中，有的是导入idea项目的库中就行"
abbrlink: '54428e33'
date: 2023-10-08 15:49:48
cover:
---
## tomcat 导入jar包
tomcat的jar位置和idea工程的jar包位置不同，有的jar包需要导入在tomcat中，有的是导入idea项目的库中就行。
##### 我所遇到的类加载不到的问题也就是类库没有放在上述可用三个路径之一，在当前IDE的.classpath中的添加类库只能辅助生成.class文件，如果要在服务器中直接使用这些类库中的类的话，就会引发类加载不到错误，而最方便的方法当然就是把类库直接放到WEB-INF路径下咯，当然放在Tomcat的/lib目录下面也是可以的,但要注意，相互依赖的两个包必须放在同一目录下，比如commons-beanutils.jar依赖commons-logging.jar那么它们要么放在服务器的/lib目录下要么放在WEB-INF/lib目录下。
