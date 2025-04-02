---
title: HTML基础笔记
tags:
  - html
categories:
  - 学习
description: "\U0001F96B HTML 基础学习笔记"
abbrlink: 74c0bbe
date: 2022-05-25 16:27:09
cover:
---
# 第一课

## 1. vscode 插件集合整理

|    插件名称     |              作用              |
| :-------------: | :----------------------------: |
| Auto Rename Tag |        自动更改标签名称        |
|     chinese     |              汉化              |
| open in browser | 右键打开默认浏览器执行html文件 |

## 2. HTML基本结构

```html
<!-- 文档说明 -->
<!DOCTYPE html>
主框架，所有代码写在html标签内
<html>
    并列关系head和body
	<head></head>
    <body></body>
</html>

<head>
    
</head>
```

```html
<!-- 文档说明 -->
<!DOCTYPE html>
<!-- 语言：en 为英语，zh-CN是中文 -->
<html lang="en">

<head>
    <!-- meta参数设置 -->
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>demo1</title>
</head>

<body>
    <div style="text-align: center;">
        <h1>演员</h1>
        <!-- p段落标签，默认会在段落的上方或下方空出一行 -->
        <p>简单点</p>
    </div>
</body>

</html>
```

# 第二课

div主要是分局、分块页面用的，是块级元素，无特殊意义。

div,span没有语义，用来布局，相当于一个盒子，用来装内容，对文本进行控制。

div：分割、分区，默认独占一行，大盒子

span：跨度、跨距，一行上可以有多个span

```html
<body>
    <div>一行</div>
    <span>一段</span>
    <span>一段</span>
</body>
```

### 文本格式化标签

| strong | 加粗标签，推荐，比b标签好一点 |
| :----- | ----------------------------- |
| b      | 加粗                          |
| em     | 斜体标签，比 i 好一点         |
| i      | 斜体                          |
| del    | 删除标签                      |
| u      | 下划线标签                    |
| sub    | 下标                          |
| sup    | 上标                          |

### a 标签（超链接标签）

a标签是超链接，在网页中跳转，href属性设置连接的地址

target="_self"打开窗口的方式，标识当前窗口打开页面

target="_blank" 用新窗口打开页面

href=“#”标识空连接，href="#1"表示寻找当前页面的锚点。

href属性可以放外部链接和内部链接，外部链接路径需要输入http:// 协议头 ，内部链接路径可以   **相对路径** 和 **绝对路径**

```html
	<h4 name="top">外部链接</h4>
    <a href="http://www.w3school.com.cn">链接到w3c网站</a><br><br>
    <a href="http://www.baidu.com" target="_blank">连接到百度，新建窗口打开</a>

    <h4>内部链接：网站内部页面之间的连接</h4>
    <a href="02-02-文本格式化标签.html" target="_blank">默认链接到文件同一级目录下</a><br>
    <br>
    <a href="下一级目录/index.html">链接到文件所在目录的下一级目录中的本地文档</a>

    <h4>空连接</h4>
    <div><a href="#">空链接</a></div>

    <h4>下载链接</h4>
    <div><a href="./材料/南信文本.txt" target="_blank">a标签 下载链接</a></div>
    
    <!-- 当前页面内进行跳转，使用命名锚内部链接 -->
    <h4>页面内进行跳转</h4>
    <a href="#bottom">跳转到底部</a>
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
    <div><a href="#top">回到顶部</a></div>
    <div id="bottom">这是底部</div>	
```

### 图像标签：img，单标签

src 属性设置为要插入图像文件的URL，alt 属性在浏览器无法载入图像时，设置显示的文本

src 路径的使用方法和a标签中href差不多，可以使用 **相对路径** 和 **绝对路径**

> <img>是单标签，图像尺寸
>
> src属性： 可以设置图片的路径
>
> alt：替换文本，图像显示不出来的时候用文字替换，设置提示
>
> title：不管图像是否成功显示，鼠标悬停在图像上就会有文字提示
>
> border: 设置边框
>
> hspace：上下留白
>
> vspace：左右留白

```html
<body>
    <h4>图像标签的使用</h4>
    <div><img src="./材料/img/大雄.PNG" alt="这是大雄，可惜没显示出来" title="我是大雄">
        <img src="./材料/img大雄.PNG" alt="这是大雄，可惜没显示出来">
    </div>

    <!-- 描述：图像边框设置 border属性 -->
    <div>
        <img src="./材料/img/face.PNG" alt="一张脸" border="1">
        <img src="./材料/img/face.PNG" alt="一张脸" border="1" height="100px">
        <!-- 百分比设置图像标签的尺寸 -->
        <img src="./材料/img/face.PNG" alt="一张脸" width="20%">
    </div>

    <!-- 设置图片的位置：水平和垂直方向 -->
    <p>
        这是一段文字，图片的align属性设置“left”。图像将“浮动”到文本的左侧
        <img src="./材料/img/face.PNG" alt="这是一张脸" align="left" width="100">
    </p>
    <br><br>
    <p>
        这是一段文字，图片的align属性设置“right”。图像将“浮动”到文本的右侧
        <img src="./材料/img/face.PNG" alt="这是一张脸" align="right" width="100">
    </p>
    <br><br>
    <p>
        这是一段文字，图片的align属性设置“top”。图像将“浮动”到文本的上侧
        <img src="./材料/img/face.PNG" alt="这是一张脸" align="top" width="100">
    </p>
    <br><br>
    <p>
        这是一段文字，图片的align属性设置“bottom”。图像将“浮动”到文本的下侧
        <img src="./材料/img/face.PNG" alt="这是一张脸" align="bottom" width="100">
    </p>
    <br><br>
    <p>
        这是一段文字，图片的align属性设置“middle”。图像将“浮动”到文本的居中侧
        <img src="./材料/img/face.PNG" alt="这是一张脸" align="middle" width="100" border="1">
    </p>

    <!-- 图像留白及边框线 -->
    <div>
        这是一段文字，垂直左右都留白
        <img src="./材料/img/face.PNG" alt="这是一张脸" align="middle" width="100" hspace="50" vspace="50" border="1">
    </div>

    <h4>图片超链接</h4>
    <a href="http://www.njcit.cn" target="_blank">
        <img src="./材料/img/logo.png" alt="这是南信院logo">
    </a>
</body>
```

### 图像地图 area标签

整张图片被分成的多块活动的 **区域**。用户自己定义这些热点，把他们分别连接到各自独立的URL地址。

> shape：区域类型，是圆形还是矩形，circle圆形；rect矩形
>
> coords：圆心（横坐标，据上距离纵坐标），半径；左上角（x，y），右下角（x，y）
>
> href：链接的地址

**<map>**定义一个客户端图像映射，应带有 id 和 name 属性，id 的值要和 name 的值 **保持一致**，并不可缺一。

img元素中的 **usemap** 属性引用 **map** 元素中的 **id** 或 **name** 属性，<u>**要用#**</u>

```html
<body>
    <h2>请点击图像上的星球，把它们放大</h2>
    <div>
        <img src="./img/eg_planets.jpg" alt="这是全图" usemap="#plants">
        <map id="plants" name="plants">
            <area shape="circle" coords="130,162,10" href="./mercur.html" alt="这是小的" target="_blank">
            <area shape="circle" coords="180,140,15" href="./venus.html" alt="这是大的" target="_blank">
            <area shape="rect" coords="0,0,112,259" href="./sun.html" alt="我就是太阳" title="我就是太阳" target="_blank">
        </map>
    </div>
</body>
```

# 第三课

### 音频标签 audio

可以播放音频文件或者音频流

audio内部可以嵌套 **source** 元素

source 可以链接 **不同** 的音频文件，浏览器识别第一个可播放的音频类型

> src：文件地址
>
> autoplay：自动播放
>
> controls：显示播放按钮等组件

```html
<body>
    <!-- 播放音频文件使用audio标签：controls 属性向用户显示音频控件 -->
    <!-- 也可使用多个不同的音频格式。HTML5 <audio> 元素会尝试以 mp3 或 ogg 来播放音频 -->
    <audio src="./材料/第三课/media/雨的印记.mp3" controls="controls">
        <!-- 浏览器或许不识别 mp3 格式文件，所以用 source 标签 -->
        <source src="./材料/第三课/media/雨的印记.ogg" type="audio/ogg">
    </audio>
</body>
```

### 视频标签 video

播放视频标签，属性与 audio 相差不大

```html
    <!-- 视频标签 video -->
    <video controls="controls">
        <source src="./材料/第三课/media/南信版成都.MP4" type="video/MP4">
    </video>
```

### embed标签 

视频、音频都可以

单标签

```html
    <div>
        <!-- 播放音频、视频文件 -->
        <embed src="./材料/第三课/media/卡农.mp3" type="">
        <embed src="./材料/第三课/media/凉凉.ogg" type="">
    </div>
```

### a 超链接标签

a 超链接标签也可以用于播放音频、视频

打开另一个页面，用 **辅助应用程序** 来播放文件

```
    <!-- 大多数浏览器会使用“辅助应用程序”来播放文件 -->
    <div>
        <a href="./材料/第三课/media/雨的印记.mp3">雨的印记</a>
    </div>
```

### marquee 标签 滚动文字

用于插入一段滚动文字

```
    <!-- 滚动文字 -->
    <marquee>你好世界</marquee>
```

## 表格标签 table

table 标签里有 tr 和 td 标签，tr 是行，td是列

> border：设置边框
>
> width：设置表格宽度
>
> height： 设置表格高度
>
> align：设置表格相对于页面的对其方式
>
> cellspacing：设置单元格的间距，一般设置0，完全重叠
>
> cellpadding：设置单元边距与内容之间的空白，设置为 1 
>
> ​	caption：添加标题
>
> ​	th：是表头标签
>
> ​	tr：行标签
>
> ​	td：列标签
>
> **语义标签：**<thead> 和 <tbody>，只是增加可读性，在页面上不显示
>
> td 标签中 valign="top"，设置 td 标签的对齐格式

```html
	<div>
        <table border="1" align="center" cellpadding="10" cellspacing="0">
            <!-- 添加标题 -->
            <caption>学生信息表</caption>
            <thead>
                <tr>
                    <!-- 设置表头 th 标签，加粗加黑、居中 -->
                    <th>姓名</th>
                    <th>性别</th>
                    <th>年龄</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>张三</td>
                    <td>男</td>
                    <td>18</td>
                </tr>
                <tr>
                    <td>李四</td>
                    <td>女</td>
                    <td>18</td>
                </tr>
            </tbody>
        </table>
    </div>
```

**跨行合并：rowspan=“要进行行合并的列数”**

**跨列合并：colspan=“要进行列合并的行数”**

```html
<body>
    <!-- 表格跨行跨列
    colspan 属性设置跨列
    rowspan 属性设置跨行 -->
    <div>
        <table border="1" width="400" height="200" align="center" cellspacing="0">
            <caption>横跨两列的单元格</caption>
            <tr align="center">
                <td>1</td>
                <td colspan="2">2</td>
                <!-- <td>3</td> -->
            </tr>
            <tr align="center">
                <td>4</td>
                <td>5</td>
                <td>6</td>
            </tr>
        </table>
    </div>
    <hr style="size: 5px;">
    <div>
        <table border="1" width="400" height="200" align="center" cellspacing="0">
            <caption>横跨两行的单元格</caption>
            <tr align="center">
                <td>1</td>
                <td rowspan="2">2</td>
                <td>3</td>
            </tr>
            <tr align="center">
                <td>4</td>
                <!-- <td>5</td> -->
                <td>6</td>
            </tr>
        </table>
    </div>
</body>
```

![合并单元格](https://img-blog.csdnimg.cn/img_convert/ab7858a7864ac399fbe05d7a0ecc48b9.png)

#### 表格嵌套

在单纯的跨行合并与跨列行并不好合并的情况下，可以用 **嵌套** 进行表示

```html
    <!-- 表格嵌套 -->
    <div>
        <table border="1" align="center" cellspacing="0" cellpadding="10">
            <caption>表格嵌套</caption>
            <tr>
                <td>教材全称</td>
                <td>
                    <table border="1" align="center" cellspacing="0" cellpadding="10" width="100%">
                        <tr>
                            <td>名称</td>
                            <td>HTML网页设计</td>
                            <td>编者</td>
                            <td>王强</td>
                        </tr>
                        <tr>
                            <td>出版社</td>
                            <td>清华大学出版社</td>
                            <td>版 次</td>
                            <td>第 1 版</td>
                        </tr>
                    </table>
                </td>
            </tr>
        </table>
    </div>
```

# 第四课

> 列表：有序列表，<u>**无序列表**</u>，自定义列表
>
> 特点：整齐、整洁、有序

### 无序列表 ul 标签

ul 标签里 **只能** 嵌套 li 标签

li 标签是个容器，可以容纳所有元素

> type：修改列表项前面的符号，disc：实心小圆点（默认）；circle：空心小圆点；square：实心小方块
>
> li 标签：ul 标签中唯一存在的标签

```html
    <div>
        <h4>无序列表</h4>
        <ul type="circle">
            <li>菠萝</li>
            <li>香蕉</li>
            <li>西瓜</li>
        </ul>
    </div>
```

### 有序列表 ol 标签

ol 标签里 **只能** 嵌套 li 标签

li 标签是个容器，可以容纳所有元素

基本使用方法与无序列表相同

```html
    <div>
        <h4>有序列表</h4>
        <ol>
            <li>有序列表项</li>
            <li>有序列表项</li>
            <li>有序列表项</li>
        </ol>
    </div>
```

### 自定义列表 dl 标签

常用于对术语或名词进行解释和描述，定义列表的列表项前没有任何项目符号

dl 标签里面**只能**包含 dt 标签

dt 标签和 dd 标签个数没有限制，经常是一个 dt 标签对应多个 dd 标签

```html
    <div>
        <h4>自定义列表</h4>
        <dl>
            <dt>帮助中心
                <dd>账户管理</dd>
                <dd>购物指南</dd>
            </dt>
            <dt>关于小米
                <dd>了解</dd>
                <dd>加入</dd>
                <dd>联系我们</dd>
            </dt>
        </dl>
    </div>
```

### 浮动框架 iframe 标签

可以在网页上开辟一个小区域显示单独的网页

1. 在 iframe 标签中使用 name 属性定义一个名称，src 属性设置连接的目标网址
2. 在 a 标签的 target 属性上设置 iframe 的 name 属性值
3. **属性值与浮动框架的 *<u>name</u>* 值一致**

> src：URL 地址
>
> width：宽度

```html
    <div>
        <ul>
            <li><a href="https://www.qidian.com/rank/" target="display">点此进入起点</a></li>
            <li><a href="http://www.w3school.com.cn" target="display">点此进入w3c网站</a></li>
            <iframe src="http://www.w3school.com.cn" 
            name="display" 
            style="width: 600px;height: 300px;" 
            align="right" 
            frameborder="0"></iframe>
        </ul>
    </div>
```

# 第五课

表单域、表单控件以及提示信息

## form 标签 

是HTML页面中用来收集用户信息的所有元素集合，然后把这些信息发送到服务器

> action：规定当提交表单时，向何处发送表单数据

### input 标签

规定用户可输入数据的输入字段，根据不同的 **type** ，输入字段有多种形态。

> name：获取文本框的name
>
> value：文本框中默认填充的文字
>
> maxlength：限制输入的字符
>
> size：文本框的宽度
>
> checked=“checked”：是否默认选中

type 的属性

> text：文件输入框
>
> radio：是单选框，内容字可以用 **label** 标签中 **for**属性进行绑定。
>
> checkbox：复选框，内容字可以用 **label** 标签中 **for**属性进行绑定。
>
> select 标签是下拉列表框，option 标签是下拉列表框中选中的表现
>
> submit：是提交按钮
>
> reset：重置按钮
>
> button：按钮
>
> file：文件上传按钮
>
> hidden：隐藏按钮

```html
<body>
    <div>
        <form action="./03-03-小说排行榜.html">
            <!-- 整体是 11 行 2 列 -->
            <table align="center" cellspacing="0" cellpadding="5">
                <caption align="center">
                    <h1>用户注册</h1>
                </caption>
                <tr>
                    <td>
                        <span>用户名：</span>
                    </td>
                    <td>
                        <input type="text" name="useName" value="请输入用户名" tabindex="1">
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>密码：</span>
                    </td>
                    <td>
                        <input type="password" name="password" id="password">
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>性别：</span>
                    </td>
                    <td>
                        <!-- radio 单选框注意设置相同的 name 实现单选效果 -->
                        <input type="radio" name="sex" id="nan" checked="checked">
                        <label for="nan">
                            男
                        </label>

                        <input type="radio" name="sex" id="nv">
                        <label for="nv">
                            女
                        </label>
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>爱好：</span>
                    </td>
                    <td>
                        <!-- value 属性：获取用户的选中情况 -->
                        <input type="checkbox" name="favourite" id="foot" value="foot">
                        <label for="foot">
                            足球
                        </label>

                        <input type="checkbox" name="favourite" id="basket" value="basket">
                        <label for="basket">
                            篮球
                        </label>

                        <input type="checkbox" name="favourite" id="volleyball" value="volleyball">
                        <label for="volleyball">
                            排球
                        </label>
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>出生年月日：</span>
                    </td>
                    <td>
                        <input type="date" name="date" id="date">
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>电子邮箱</span>
                    </td>
                    <td>
                        <input type="email" name="email" id="email" placeholder="请输入邮件地址">
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>籍贯：</span>
                    </td>
                    <td>
                        <!-- 下拉选择器 -->
                        <select name="province" id="province">
                            <option>--请输入籍贯--</option>
                            <option value="jiangsu">江苏</option>
                            <option value="shandong">山东</option>
                        </select>
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>我最喜欢的颜色是：</span>
                    </td>
                    <td>
                        <!-- datalist 标签，除了从列表中选择，还可以在文本框中输入 -->
                        <!-- datalist 标签id 要与 input 的 list 属性进行绑定 -->
                        <input type="text" name="color" id="color" list="colorslist">
                        <datalist id="colorslist">
                            <option value="red"></option>
                            <option value="green"></option>
                        </datalist>
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>今日反馈：</span>
                    </td>
                    <td>
                        <textarea name="today" id="today" cols="30" rows="5" placeholder="请输入要反馈的内容"></textarea>
                    </td>
                </tr>
                <tr>
                    <td>
                        <span>上传头像</span>
                    </td>
                    <td>
                        <input type="file" name="file" id="file">
                    </td>
                </tr>
                <tr>
                    <td>
                        <input type="submit" value="提交" onclick="alert('提交成功')">
                    </td>
                    <td>
                        <input type="reset" value="重置">
                    </td>
                </tr>
            </table>
        </form>
    </div>
</body>
```