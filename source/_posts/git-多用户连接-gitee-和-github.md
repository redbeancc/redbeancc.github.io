---
title: git 多用户连接 gitee 和 github
tags:
  - git
categories:
  - 教程
description: "\U0001F3AA 一台电脑同时登录 GitHub 和 Gitee"
abbrlink: 4d5b51ef
date: 2024-02-21 16:58:23
cover:
---
# git 多用户连接 gitee 和 github

## 一、通过邮箱生成公私钥（使用 git bash）

```bash
## git bash
# 生成 gitee 的公私钥
ssh-keygen -t rsa -f ~/.ssh/id_rsa_gitee -C "youxiang@qq.com"
# 生成 github 的公私钥
ssh-keygen -t rsa -f ~/.ssh/id_rsa_github -C "youxiang@qq.com"
# 连按三下回车
```

![image-20240221215650404](https://img-blog.csdnimg.cn/img_convert/2efb40498199c237acd3d2887adf4895.png)

![image-20240221215906227](https://img-blog.csdnimg.cn/img_convert/88d6de9b156b7530668d75ac3942e1a0.png)

## 二、公钥设置进网站账户

![img](https://img-blog.csdnimg.cn/img_convert/fc9d2e000aab308b3a8c3c7120d947dd.png)

![img](https://img-blog.csdnimg.cn/img_convert/669668262abe3bccd8ecc838d81b8c12.png)

## 三、在 git bash 上激活公钥，并授权

```bash
## git bash
# 激活 gitee 公钥
ssh -T git@gitee.com -i ~/.ssh/id_rsa_gitee
# 授权
授权 控制台输入yes

# 激活 github 公钥
ssh -T git@github.com -i ~/.ssh/id_rsa_github
# 授权
授权 控制台输入yes
```

![image-20240221220336995](https://img-blog.csdnimg.cn/img_convert/9590bffb15afa5fdd80ed9bfea3b6368.png)

> 若出现失败
>
> 1. 首先查看 .ssh 目录下是否有 known_hosts 文件，若有，先删除
> 2. 查看ssh链接的 github 地址
>
> ```bash
> # 查看链接GitHub的地址
> ssh -vT git@github.com
> ```
>
> 如果出现 127.0.0.1 或 ::1，说明 github 的 ip 地址访问不对，使用以下命令获取github的ip地址存入hosts文件
>
> ```
> ## 下面命令都是在 cmd 中执行
> # 获取 github ip地址存入 hosts 文件
> nslookup github.com 8.8.8.8
> # 刷新 dns 缓存（windows）
> ipconfig /flushdns
> ```
> 出现原因是用了 steam++ 修改了hosts

## 四、将私钥文件添加到 git

```bash
## git bash
# 启动链接输入
ssh-agent bash
# 私钥文件添加到 git
ssh-add ~/.ssh/id_rsa_gitee
ssh-add ~/.ssh/id_rsa_github
```

## 五、配置 config 文件

```bash
## git bash
#创建 config 文件，一般在 .ssh/config 文件内
touch ~/.ssh/config
```

文件内容如下：

```
# config
Host gitee.com
HostName gitee.com
IdentityFile C:\\Users\\25361\\.ssh\\id_rsa_gitee
PreferredAuthentications publickey
User git


Host github.com
HostName github.com
IdentityFile C:\\Users\\25361\\.ssh\\id_rsa_github
PreferredAuthentications publickey
User git
```

测试如下，即为成功

![image-20240221220935850](https://img-blog.csdnimg.cn/img_convert/17ea764b4ff03b87938451a134a2df70.png)