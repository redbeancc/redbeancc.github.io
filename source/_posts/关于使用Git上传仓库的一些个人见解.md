---
title: 关于使用Git上传仓库的一些个人见解
tags:
  - git
categories:
  - 教程
description: 🍿关于使用 git 进行简单的拉取、推送操作
cover: 'https://img-blog.csdnimg.cn/direct/8215fe56827c44dfb4039c73431f19db.png'
abbrlink: 11c02f3
date: 2021-09-12 00:33:24
---
# 关于使用Git上传仓库的一些个人见解

## 1. 当在远端仓库已经初始化建好时

1. git init											初始化仓库，在文件下面会生成一个.git 文件夹
2. git status
3. git add .   		      			    		将文件夹里的内容全部添加到git中
4. git commit -m "注释"        			添加注释
5. 在代码托管平台(github,gitee)新建远程仓库
6. git remote add origin https://gitee.com/****/*****.git
7. git push -u origin master			将本地仓库提交给远程仓库（第一次推送加（-u origin master），后面推送可以不加）

## 2. 当克隆下来的代码时

1. 拉代码到本地：在本地磁盘新建个文件夹，比如：D:\java每日一题，然后进入这个文件夹右键 git bash

2. 执行命令：

   ```shell
   git clone https://gitee.com/****/*****.git
   ```

3. 此文件夹下会出来另一个文件夹，在进入下一个文件夹后，写下自己的代码，写完后添加

   ```shell
   cd ./*****
   git add .
   ```

4. 通过 git status可以查看自己仓库的状态

5. 再提交代码到本地仓库

   ```shell
   git commit -m '提交代码  *****.md'
   ```

   > 注意：-m必须要写，后面的信息表示你本次提交的代码信息，比如新增文件，就写 'add xx文件'

6. 经过上面的步骤，本地仓库的代码就完成提交了，但是，我们还没有提交到远程Gitee仓库，需要再执行下面的命令，将代码推到远程仓库。

   1. 先拉代码：

      ```shell
      git pull
      ```

   2. 推代码

      ```shell
      git push
      ```

可以打开网页的gitee仓库，验证下代码是否已经提交成功。

> 注意：每次push之前一定要先pull拉代码，不然就会报错！