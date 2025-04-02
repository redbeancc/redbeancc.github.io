---
title: idea运行java web项目
tags:
  - idea
  - java
categories:
  - 教程
description: "\U0001F347 java web 配置并运行在 tomcat 服务器"
abbrlink: 352b5fa6
date: 2022-06-12 00:18:29
cover:
---
# idea运行java web项目
 - 导入项目，选中项目鼠标右击，选择用 IDEA 打开
 - IDEA 打开后，选中项目，按F4进入项目配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b12b3f613cd432e818751f01e99147a.png)
## 1.在这里可以配置 jdk
 - 配置modules
为了让IDEA识别项目，告诉IDEA项目的类型、web.xml的位置，以及根目录
 - 项目类型：告诉IDEA当前项目类型为 Web

![在这里插入图片描述](https://img-blog.csdnimg.cn/71163ad1f63e41baaeaed99d256c2169.png)
 - 配置web.xml：选择项目中的web.xml文件的位置（一般在 \WebContent\WEB-INF 路径下）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2ae99e778bfd48d9995d081cfcc7db9e.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0bf33ca4458b432baa001b19971fa91e.png)
 - 设置web资源目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/16b2910712da4011a81211be6b7f6499.png)
这里一般设置的是静态的资源，一般指向WebContent目录下就行

![在这里插入图片描述](https://img-blog.csdnimg.cn/c2de2fece72e410fb720f9716ec7a0ac.png)
 - 最下面的是source Roots，是java源码位置，如果默认没选上，自己给选上就好

![在这里插入图片描述](https://img-blog.csdnimg.cn/8a91eec5b2ea48c8bb81d5b788c313fc.png)
## 2. 配置tomcat
首先在电脑中下载、安装并配置好tomcat
向项目中引入tomcat中的jar包
![在这里插入图片描述](https://img-blog.csdnimg.cn/b8d6a0d5184d45aa8af71e9017b8a6d3.png)

选择本地 tomcat 的 lib 目录，一路🆗

![在这里插入图片描述](https://img-blog.csdnimg.cn/5116968a004d4a988baf6175766d7196.png)
## 配置Artificts
一路🆗下去

![在这里插入图片描述](https://img-blog.csdnimg.cn/0bf4805318644129bff7f53a18cb6b6d.png)
## 最后Apply并ok后，配置tomcat环境
我这里是已经配好了，所以在这里我就是编辑，第一次配的话这里是添加

![在这里插入图片描述](https://img-blog.csdnimg.cn/c59200a71650425abe5234961f8824b0.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/417b3a77efd049d3bee42c0a62accb6c.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f88539b6e254e7cb6543048bf7cc32d.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/190694fdb2374fd99d20182034ad5a2c.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/59bc131e8ffa41b088ffeab68c801a32.png)

最后结果为

![在这里插入图片描述](https://img-blog.csdnimg.cn/ae0c0edaf0894cbfae470e845ffb3f22.png)
# 最后点击Apply和ok保存退出，运行即可
## 多说一句，如果遇到tomcat日志在IDEA控制台输出中文乱码的情况
![在这里插入图片描述](https://img-blog.csdnimg.cn/f1b62ffb3cd646809ad8ca139a3f018f.png)

**在最后加上一句**
```
-Dfile.encoding=UTF-8
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/70f6cdb7c24d4d1fa31b78fdd6e67b59.png)

保存退出IDEA重启后，查看控制台打印，可正常显示中文