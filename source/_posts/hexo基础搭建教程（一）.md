---
title: hexo基础搭建教程（一）
tags:
  - hexo
  - butterfly
categories:
  - 魔改教程
description: 🍔hexo基础搭建教程（一）
abbrlink: 3324b066
date: 2024-02-29 14:15:33
swiper_index: 4
cover: https://img-blog.csdnimg.cn/img_convert/6d3bd229788ab88cd1286813a33f92a3.jpeg
---

> **前言**📇
>
> 1. 本文参考[Hexo博客搭建基础教程(一)](https://www.fomal.cc/posts/e593433d.html)、[【2023最新版】Hexo+github搭建个人博客并绑定个人域名](https://blog.csdn.net/wushibo123/article/details/124619123)
> 2. 本系列基本上都是各位大佬造好的轮子，具体参考 [Fomalhaut大佬](https://www.fomal.cc/)。其目的在于防止各位大佬的链接失效，且个人复习总结使用，如有侵权请联系删除。
> 3. 本系列起始空白的虚拟机，一步一步搭建魔改页面，使用本地端口。若想部署在其它平台，可自寻查找。
> 4. 鉴于每个人的根目录名称都不一样，本帖**博客根目录**一律以`[BlogRoot]`指代。
> 5. 本帖涉及魔改源码的内容，会使用**diff代码块**标识，复制时请**不要忘记删除**前面的`+、-`符号。
> 6. 因为`.pug`和`.styl`以及`.yml`等对缩进要求较为严格，请尽量**不要使用记事本等无法提供语法高亮的文本编辑器**进行修改。
> 7. 本系列基于`Butterfly主题`进行魔改方案编写，hexo 版本 `6.3.0`，Butterfly 版本 `4.12.0`。
> 8. 魔改会过程常常引入**自定义的css与js文件**，具体方法见方法见[Hexo博客添加自定义css和js文件](https://b.leonus.cn/2022/custom.html)

**博客搭建与魔改系列教程导航🚥🚥🚥**

1. 🎀[hexo基础搭建教程（一）](https://redbeancc.github.io/post/3324b066.html)⬅当前位置🛸
2. 🎆[hexo基础搭建教程（二）](https://redbeancc.github.io/post/837c228f.html)
3. 🎇[魔改教程总结（一）](https://redbeancc.github.io/post/781096aa.html)
4. 🧨[魔改教程总结（二）](https://redbeancc.github.io/post/48067a72.html)
5. ✨[魔改教程总结（三）](https://redbeancc.github.io/post/518d920.html)

# 1. 环境与工具准备

- windows 10 操作系统
- node.js
- git
- hexo
- butterfly（魔改主题）

# 2. 安装配置 node

下载地址：https://nodejs.org/en

可以不用下载最新的 node，防止版本冲突，我的 node 版本为 [18.19.1](https://nodejs.org/download/release/v18.19.1/node-v18.19.1-x64.msi)

![image-20240229153439679](https://img-blog.csdnimg.cn/img_convert/8cc654b755b6be042f028dd0f5c64522.png)

一路 next 默认就好。安装完成后在 cmd 执行命令，查看是否安装成功。

```shell
C:\Users\XinPuSen>node --version
v18.19.1
# 更换镜像源
C:\Users\XinPuSen>npm config get registry
https://registry.npmjs.org/

C:\Users\XinPuSen>npm config set registry http://registry.npmjs.org/

C:\Users\XinPuSen>npm config get registry
http://registry.npmjs.org/
```

{% note warning flat %}

这里去掉 s，是为了安装后面的插件不报错，换成阿里的应该也行，没试过。

{% endnote %}

安装完成后，cmd不要关闭，继续安装需要的模块 express （本地服务器）

```shell
# -g 是在全局安装
C:\Users\XinPuSen>npm install express -g
```

# 3. 安装 git

> - hexo 初始化需要加载一些 github 上的依赖库，所以需要下载 git
>
> - git 官网下载很慢，这里用的 cnpm ：https://registry.npmmirror.com/binary.html?path=git-for-windows/，找个不需要太新的版本下载即可，防止版本冲突。
>
>   ![image-20240229163333379](https://img-blog.csdnimg.cn/img_convert/a1ed15b95dea7e28e665721e78024f9a.png)
>
> - 下载安装，一路默认 next 即可



# 4. 安装 hexo

创建新的目录 Blog，`[BlogRoot]`就是此目录。

```shell
# 安装 hexo
C:\Users\XinPuSen\Desktop\Blog>npm install -g hexo-cli
```

```shell
# 查看安装 hexo 是否成功
C:\Users\XinPuSen\Desktop\Blog>hexo -v
hexo-cli: 4.3.1
os: win32 10.0.19043
node: 18.19.1
acorn: 8.10.0
ada: 2.7.2
ares: 1.20.1
base64: 0.5.0
brotli: 1.0.9
cjs_module_lexer: 1.2.2
cldr: 43.1
icu: 73.2
llhttp: 6.1.0
modules: 108
napi: 9
nghttp2: 1.57.0
nghttp3: 0.7.0
ngtcp2: 0.8.1
openssl: 3.0.13+quic
simdutf: 3.2.18
tz: 2023c
undici: 5.28.3
unicode: 15.0
uv: 1.44.2
uvwasi: 0.0.19
v8: 10.2.154.26-node.28
zlib: 1.2.13.1-motley
```

![image-20240229163827294](https://img-blog.csdnimg.cn/img_convert/618002bada7a98074cc51dd1d4f97eea.png)

1. 初始化 hexo

   ```shell
   hexo init
   ```

   ![image-20240229164048852](https://img-blog.csdnimg.cn/img_convert/018a8104d3f0f4edcc69dbd18eefdbd6.png)

   这时候回到目录，发现文件下载下来了

   ![image-20240229164143949](https://img-blog.csdnimg.cn/img_convert/15f557369c63c6e3013b0404a2a6b0e3.png)

   - node_modules:：这是下载的依赖包，都在里面
   - scaffolds：模板文件，生成的新页面，文章页的模板就在里面
   - source：这是最主要的目录，我们写的笔记就存放在这里
   - themes：主题文件，这里有默认的 landscape，稍后换成 butterfly
   - .gitignore：顾名思义，上传 github 中需要忽略的文件名就写在里面
   - _config.landscape：主题配置文件，不用管
   - _config：hexo 的配置文件，网站名字，第三方插件配置信息写在里面
   - package：依赖版本等信息，在使用 `npm i `命令时需要下载的依赖及其版本信息就在这个文件中

2. hexo，启动！

   ```shell
   # git bash
   hexo cl && hexo g && hexo s
   # vscode
   hexo cl;hexo g;hexo s
   # 当然也可以分开来一个个运行
   hexo cl # 等价于 hexo clear，清除数据，比如修改主题后立即生效，防止缓存
   hexo g  # 等价于 hexo generate，生成静态网页
   hexo s  # 等价于 hexo server，启动本地服务器，-p 5000，可指定端口，默认4000
   ```

   ![image-20240229165231344](https://img-blog.csdnimg.cn/img_convert/2a713448662d8f0ac13f67e0f2de8d8e.png)

3. 浏览器输入 `http://localhost:4000`

   ![image-20240229165736569](https://img-blog.csdnimg.cn/img_convert/a6607f104c44cfed3ff9d53bb4b1e61a.png)

{% note warning flat %}

出现以上页面即为成功，本章已顺利完成，敬请期待下面教程。

{% endnote %}