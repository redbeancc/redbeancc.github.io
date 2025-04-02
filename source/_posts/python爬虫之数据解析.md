---
title: python爬虫之数据解析
tags:
  - 爬虫
  - python
categories:
  - 学习
description: "\U0001F451对网页页面解析获取方法总结"
cover: 'https://img-blog.csdnimg.cn/direct/b04af0fae1e340ddaffdc9dea312dbb2.png'
swiper_index: 1
abbrlink: c7266a94
date: 2021-11-12 15:05:15
---

# 第五章 数据解析

> 1. 针对文本的解析，有正则表达式
> 2. 针对HTML/XML解析有XPath、Beautiful Soup、正则表达式
> 3. 针对JSON的解析，有jsonpath

## 一、正则表达式

### 1. 导入re模块,用re.search()方法和re.findall()方法

> re.search(想找的内容,一个整体)这是找第一个，后面的找不到

```python
import re
exam = 'pachong'
str = 'pachonghei pachong'
ret = re.search(exam,str)#从str里找exam，但是只能找到第一个，后一个找不到
print(ret)
#<re.Match object; span=(0, 7), match='pachong'>
```

> re.findall(正则语句,查找的文件)返回所有的符合条件的

```python
import re
s = 'waawfajwkfokawh34578fjahwfokfawf'
print(re.findall(r'(ok)',s))#从s中查找所有的ok字符串,中间用的是正则表达式
#['ok', 'ok']	返回的列表
print(len(re.findall(r'(ok)',s)))#查看有多少个ok
#2
```

> 从字符串中提取中文,当中文不连接时，返回的是两个元素的列表

```python
import re
#从字符串中提取中文
text = 'hello,世界world'
#1. 通过正则表达式
result = re.findall(r'[\u4e00-\u9fa5]+',text)
print(result)
#['世界']

import ew
#从字符串中提取中文
text = 'hello,世wada界world'
#1. 通过正则表达式
result = re.findall(r'[\u4e00-\u9fa5]+',text)
print(result)
#['世', '界']
```

## 二、XPath和lxml库

### 1. XPath语法

#### a)选取节点

|  表达式  | 说明                                                         |
| :------: | :----------------------------------------------------------- |
| nodename | 选取此节点的所有子节点                                       |
|    /     | 从根节点选取                                                 |
|    //    | 从匹配选择的当前节点选取文档中的节点，而不用考虑它们的位置（重要） |
|    .     | 选取当前节点（类似于Linux）                                  |
|    ..    | 选取当前节点的父节点                                         |
|    @     | 选取属性                                                     |

#### b)谓语

|               表达式               | 说明                                                         |
| :--------------------------------: | ------------------------------------------------------------ |
|         /bookstore/book[1]         | 选取属于bookstore子元素的第一个book元素                      |
|      /bookstore/book[last()]       | 选取属于bookstore子元素的最后一个book元素                    |
|     /bookstore/book[last()-1]      | 选取属于bookstore子元素的倒数第二个book元素                  |
|   /bookstore/book[position()<3]    | 选取最前面的两个属于bookstore元素的子元素的book元素          |
|           //title[@lang]           | 选取所有的title元素，且这些元素的拥有名称为lang的属性        |
|        //title[@lang='eng']        | 选取所有的title元素，且这些元素的拥有值为eng的lang属性       |
|    /bookstore/book[price>35.00]    | 选取bookstore元素的所有book元素，且其中的price元素的值大于35.00 |
| /bookstore/book[price>35.00]/title | 选取bookstore元素中book元素的所有title元素，且其中的price元素值必须大于35.00 |

### 2. lxml库概述（需要导入lxml.etree模块）

> 1. Element类：可以理解为XML的节点
> 2. ElementTree类：可以理解为一个完整的XML文档树
> 3. ElementPath类：可以理解为XPath，用于搜索和定位节点

#### a)Element类简介

> Element类是XML处理的核心类，可以直观的理解为XML节点，大部分XML节点的处理都是围绕篇Element类进行的
>
> 所以，我们要创建一个节点对象

```python
#导入模块etree
from lxml import etree
#1.创建节点 （element对象）
root = etree.Element('root')
```

​	上述示例中，参数root表示节点的名称

​	关于Element类的相关操作，主要可分为三部分，分别是**节点操作、节点属性的操作、节点内文本的操作**

 1. 节点操作；若要获取节点的名称，可以通过tag属性获取

    ```python
    print(root.tag)
    #root
    print(etree.tostring(root))
    #b'<root/>'(我感觉这是节点的显示吧。。)
    #该函数将元素序列化为XML树的编码字符串表示形式
    ```

2. 节点属性的操作：在创建节点的同时，可以为节点增加属性。节点中的属性是以**键值对**的形式进行存储的，类似于字典的存储方式。通过构造方法创建节点时，可以在该方法中以参数的形式设置属性，其中**参数的名称表示属性的名称，参数的值表示为属性的值**。

   ```python
   #2.给节点增加属性
   #在创建的同时添加属性
   root = etree.Element('root',name='zhang')
   print(etree.tostring(root))
   #b'<root name="zhang"/>'
   ```

   还可以用set()方法，把**属性键值对**增加进已有的节点

   ```python
   #2.2增加属性 set
   root.set('age','18')
   print(etree.tostring(root))
   #b'<root name="zhang" age="18"/>'
   ```

3. 节点内文本的操作：一般情况下，可以通过text、tail属性或者xpath()方法来访问文本内容

   ```python
   #3.添加文本
   root.text='hello,world!'
   print(etree.tostring(root))
   #b'<root name="zhang" age="18">hello,world!</root>'
   ```

#### b)从字符串或文件中解析XML

> 为了能够将XML文件解析为树结构，etree模块中提供了如下3个函数
>
>    	1. fromstring()函数：从字符串中解析XML文档或片段，返回根节点
>    	2. XML()函数：从字符串常量中解析XML文档或片段，返回根节点
>    	3. HTML()函数：从字符串常量中解析HTML文档或片段，返回根节点
>
> 其中，XML函数的行为类似于fromstring函数；HTML()函数自动补全缺少的<html>和<body>标签

```python
import lxml from etree
#二、解析xml
xml_data='<root><a class="page">data</a></root>'
#方法1:用的是fromstring(),返回根节点
element= etree.fromstring(xml_data)
print(etree.tostring(element))
#b'<root><a class="page">data</a></root>'
```

```python
#方法2：用xml函数
element = etree.XML(xml_data)
print(etree.tostring(element))
#b'<root><a class="page">data</a></root>'
```

```python
#方法3：html函数，他会自动修正html
element = etree.HTML(xml_data)
print(etree.tostring(element))
#b'<html><body><root><a class="page">data</a></root></body></html>'
```

从文件中读取

```python
element = etree.parse('hello.html')#读取文件
```

#### c)ElementPath类简介

​	ElementTree类中附带了一个类似于XPath路径语言的ElementPath类。现提供以下三个常用的函数：

1. find()方法：返回匹配的第一个子元素

  2. findall()方法：以列表的形式返回所有匹配的子元素
  3. iterfind()方法：返回一个所有匹配元素的迭代器

```python
#三。查找与搜索元素
root = etree.XML("<root><a x='123'>aText</a><a>bText</a></root>")
#可以通过xpath（）语法搜元素
print(root.xpath('//a')) #返回列表
print(root.xpath('//a')[0].text)
#find（）方法，返回匹配的第一个子元素
print(root.find('a'))
#findall().以列表形式返回所有匹配的子节点
print(root.findall('./a'))

#[<Element a at 0x31df388>, <Element a at 0x31df3a8>]
#aText
#<Element a at 0x31df3c8>
#[<Element a at 0x31df388>, <Element a at 0x31df3a8>]
```

### 3.lxml库的基本使用

> 这里有一个测试用例文件，hello.html
>
> ```html
> <div>
>  <ul>
>      <li class="item-0"><a href="link1.html">first item</a></li>
>      <li class="item-1"><a href="link2.html">second item</a></li>
>      <li class="item-inactive"><a href="link3.html"><span class="bold">third item</span></a></li>
>      <li class="item-1"><a href="link4.html">fourth item</a></li>
>      <li class="item-0"><a href="link5.html">fifth item</a></li>
>  </ul>
> </div>
> ```

​	首先导入lxml.etree模块

```python
from lxml import etree	
```

​	读取文件，发现返回的是elementTree，不是element节点

```python
element = etree.parse('hello.html')#读取文件
print(element) #返回的是elementtree，不是element节点
#<lxml.etree._ElementTree object at 0x00A95908>
```

​	用xpath()方法，将hello.html文件中与该路径表达式匹配到的列表返回

```python
#1.获取所有的li标签
li_s = element.xpath('//li')
print(li_s)#打印的是列表
#[<Element li at 0x2e9f328>, <Element li at 0x2e9f388>, <Element li at 0x2e9f3a8>, <Element li at 0x2e9f3c8>, <Element li at 0x2e9f3e8>]这里返回的是列表，不是里面的值
print(li_s[0])#拿第一个
#<Element li at 0x2e9f328>
```

```python
#2. 获取所有li元素的class属性
lis_class = element.xpath('//li/@class')
print(lis_class)
#['item-0', 'item-1', 'item-inactive', 'item-1', 'item-0']
```

```python
#3. 获取li标签下的所有a标签
print(element.xpath('//li/a'))#返回的是列表
#[<Element a at 0x2e9f5a8>, <Element a at 0x2e9f4c8>, <Element a at 0x2e9f5c8>, <Element a at 0x2e9f5e8>, <Element a at 0x2e9f608>]
```

```python
#4. 获取倒数第二个li标签下的a标签的文本
#4.1 方式1
#//li[last()-1]/a返回的是列表，要用数组去拿一下
last_two_a = element.xpath('//li[last()-1]/a')[0].text
print(last_two_a)
#fourth item
#4.2 方式2 text()方法拿到的也是列表，要用数组读取
print(element.xpath('//li[last()-1]/a/text()'))
#['fourth item']
print(element.xpath('//li[last()-1]/a/text()')[0])
#fourth item
```

## 三、Beautiful Soup

> Beautiful Soup 和lxml库功能相似，但是Beautiful Soup 使用起来更加简洁方便
>
> 需安装**beautifulsoup4和bs4**

### 1. 导入bs4.beautifuSoup

```python
from bs4 import BeautifulSoup
```

### 2. 测试用例（‘’‘三个点表示原样式写入）

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dormouse"><b>The Dormouse's story</b></p>
<p class="sotry">Once upon a time there were three little sisters;and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
```

### 3. 构造beautifulSoup对象

BeautifulSoup(html,'lxml'),html是页面的代码(表示要解析的文档字符串或文件对象)，lxml是解析的的解析器，自动补全标签

```python
#构造beautifulsoup对象
bs = BeautifulSoup(html,'lxml')
print(bs)
#<html><head><title>The Dormouse's story</title></head>
#<body>
#<p class="title" name="dormouse"><b>The Dormouse's story</b></p>
#<p class="sotry">Once upon a time there were three little sisters;and their names were
#<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>
#<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
#<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
#and they lived at the bottom of a well.</p>
#<p class="story">...</p>
#</body></html>
```

可以用本地的HTML文件来构造beautifulsoup对象

```python
bs = BeautifulSoup(open('index.html'),'lxml')
```

格式化输出，用的是prettify()方法，输出的形式就类似于页面代码的格式，方便查看

```python
print(bs.prettify())#美观显示
```

### 4. 三个获取(获取节点、获取文本字符串、获取注释)

 1. 获取节点，bs.属性标签（**如果有多个标签，只取第一个**）

    ```python
    print(bs.p)     #如果有多个标签，只取第一个
    #<p class="title" name="dormouse"><b>The Dormouse's story</b></p>
    
    print(bs.a.name)#获取标签名称
    #a
    
    print(bs.a.attrs)#获取标签的所有属性
    #{'href': 'http://example.com/elsie', 'class': ['sister'], 'id': 'link1'}
    ```

    2. 获取文本字符串，用的.string

    ```python
    title = bs.title.string
    print(title)
    #The Dormouse's story
    ```

    ```python
    print(title.find_parent()) #获取父节点
    #<title>The Dormouse's story</title>
    ```

    ```python
    #如果没有下一节点，就会找父亲的下一节点，如果还是没有，会找爷爷的下一节点
    print(title.find_next())   #获取下一个节点（下一节点的意思是“兄弟”）
    
    #<body>
    #<p class="title" name="dormouse"><b>The Dormouse's story</b></p>
    #<p class="sotry">Once upon a time there were three little sisters;and their names were
    #<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>
    #<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    #<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    #and they lived at the bottom of a well.</p>
    #<p class="story">...</p>
    #</body>
    
    #因为title的下一节点没有，就去找了head标签的下一个节点，head标签下也没有下一节点，就去html节点下找，然后找到了body
    ```

    ```python
    print(title.find_previous())#获取上一节点(父节点)
    ##<title>The Dormouse's story</title>
    ```

    3. 获取注释的内容（也就是说**.string方法不规避注释**）

    ```python
    #3. 注释
    print(bs.a.string)
    # Elsie 
    #这里获取的是第一个a标签，而第一个a标签中是注释的内容，正好获取了注释的内容
    ```

### 5. 通过操作方法进行解读搜索

```python
from bs4 import BeautifulSoup
import re
bs = BeautifulSoup(open('index.html'),'lxml')
```

​	实际上，网页中有用的信息都存在于网页中的文本或者各种不同的标签的属性值，为了能够得到这些有用的网页信息，可以通过一些查找方法获取文本或者标签属性。因此，bs4库内置了一些方法，常用的有这两个方法：

​	(1) find()方法：用于查找符合查询条件的第一个标签节点。

​	(2) find_all()方法：查找所有符合查询条件的标签节点，并返回一个列表	

```python
    def find_all(self, name=None, attrs={}, recursive=True, text=None,
                 limit=None, **kwargs)
```

1. name参数，查找所有名字为name的标签，但**字符串会被自动忽略**。

   ```python
   #1.1 标签名
   print(bs.find_all('a'))
   '''
   [<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, 
   <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, 
   <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
   '''
   ```

   ```python
   #1.2 正则 必须是编译后的正则 
   # 查找以b开头的标签
   print(bs.find_all(re.compile('^b')))
   
   '''
   [<body>
   <div data-foo="value">foo!</div>
   <p class="title" name="dormouse">
   <b>The Dormouse's story</b>
   </p>
   <p class="sotry">Once upon a time there were three little sisters;and their names were
           <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>,
           <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
           <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
           and they lived at the bottom of a well.
       </p>
   <p class="story">...</p>
   </body>, 
   <b>The Dormouse's story</b>]
   '''
   ```

   ```python
   # 1.3 列表
   # 查找a标签和b标签
   print(bs.find_all(['a','b']))
   
   '''
   [<b>The Dormouse's story</b>, 
   <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, 
   <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, 
   <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
   '''
   ```

2. kwargs：根据属性进行查找

   ```python
   #2.1 直接传入属性值
   print(bs.find_all(id='link2'))
   #[<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
   ```

   ```python
   #2.2 传入编译后的正则
   #查找href属性包含elsie的标签
   print(bs.find_all(href=re.compile('elsie')))
   #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
   ```

   ```python
   #2.3 查找class属性'sister'的标签 class是关键字，写成class_
   print(bs.find_all(class_='sister'))
   '''
   [<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, 
   <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, 
   <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
   '''
   ```

3. attrs：如果某个指定名字的参数不是搜索方法中内置的参数名，那么在进行搜索时，会把该参数当作指定名称中的属性来搜索

   传入的是字典

   ```python
   #2.4 查找data-foo为'value'的标签
   !!!print(bs.find_all(data-foo='value'))  #错，参数不能有中划线
   
   #3。 attrs:根据属性进行查找，参数是字典
   print(bs.find_all(attrs={
       'data-foo':'value'
   }))
   #[<div data-foo="value">foo!</div>]
   ```

4. text：搜索文档中的字符串内容，可以接受字符串、正则表达式和列表。**此方法不查找注释的内容**

   ```python
   #4。 text:根据文本进行查找，可以传入字符串，正则，列表
   #4.1 传入字符串 查找内容为'Lacie'的标签
   print(bs.find_all(text='Lacie'))
   print(bs.find_all(text='Lacie')[0].find_parent())
   #['Lacie']
   #<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
   ```

   ```python
   #4.2 传入正则
   print(bs.find_all(text=re.compile('story')))
   #["The Dormouse's story", "The Dormouse's story"]
   ```

   ```python
   #4.3 传入列表   text参数不查找注释
   print(bs.find_all(text=['Elsie','Lacie','Tillie']))
   #['Lacie', 'Tillie']
   ```

5. limit参数：限制查找的个数

   ```python
   #5 limit:用于限制最多查几个
   print(bs.find_all('a',limit=2))
   '''
   [<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, 
   <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
   '''
   ```

6. recursive参数：调用tag的 find_all() 方法时,Beautiful Soup会检索当前tag的所有子孙节点,如果只想搜索tag的直接子节点,可以使用参数 recursive=False

   ```python
   #6 recursive:是否要递归查找，默认是True,如果指定为False 只能查找直接子标签
   print(bs.find_all('title'))
   print(bs.find_all('title',recursive=False))
   #[<title>The Dormouse's story</title>]
   #[]
   ```

### 6. 通过CSS选择器进行搜索

​	为了使用CSS选择器达到筛选节点的目的，在bs4库的BeautifulSoup类中提供了一个**select()方法**，该方法会将搜索到的结果放入**列表**。

```python
from bs4 import BeautifulSoup
import re
html = '''
    <html>
        <head>
            <title>The Dormouse's story</title>
        </head>
    <body>
    <div data-foo="value">foo!</div>
    <p class="title" name="dormouse">
        <b>The Dormouse's story</b>
    </p>
    <p class="sotry">Once upon a time there were three little sisters;and their names were
        <a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
        <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
        <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
        and they lived at the bottom of a well.
    </p>
    <p class="story">...</p>
'''
bs = BeautifulSoup(html,'lxml')
```

 1. 通过标签查找

    ```python
    print(bs.select('title'))
    #[<title>The Dormouse's story</title>]
    ```

    2. 通过类名查找

    ```python
    print(bs.select('.sister'))
    '''
    [<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, 
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, 
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
    '''
    ```

    3. 通过id查找

    ```python
    print(bs.select('#link1'))
    #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
    ```

    4. 组合查找

    ```python
    print(bs.select('p #link2'))#查找p标签下的link2的标签
    #[<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
    print(bs.select('head > title'))#查找head下的title
    #[<title>The Dormouse's story</title>]
    print(bs.select('body > title'))#查找body下的title
    #[<title>The Dormouse's story</title>]
    print(bs.select('body .sister'))#查找body下class为title的标签
    '''
    [<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, 
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, 
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
    '''
    ```

    5. 通过属性查找，查找href='http://example.com/elsie'的a标签

    ```python
    print(bs.select('a[href="http://example.com/elsie"]'))
    #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
    ```

## 四、JSONPath和json模块

> 从Python 2.6 开始加入了json模块，使用import json导入就可以使用。json模块提供了Python 对象的序列化和反序列化功能。
>
> ​	(1) 序列化：将一个python对象编码转换为JSON字符串的过程，**dump()和dumps()**
>
> ​	(2)反序列化：将以给JSON字符串解码转换为python对象的过程，**load()和loads()**

### 1. json模块基本运用

|  函数   |               作用               |
| :-----: | :------------------------------: |
| loads() | 将**json字符串**转换为python对象 |
| load()  |  将**json文件**转换为python对象  |
| dumps() |   将python类型转换为json字符串   |
| dump()  |    将python类型转换为json文件    |

#### (1) loads() json字符串->python

```python
import json
json_obj = '{"name":"张三","age":18}'
dic = json.loads(json_obj)
print(dic)
print(type(dic))
#{'name': '张三', 'age': 16, 'gender': '男'}
#<class 'dict'>
```

#### (2) jumps() python->json字符串	！！注意：jumps()方法默认使用ascii码，禁用，使用utf-8编码

```python
json_obj = json.dumps(dic)
print(json_obj)
#{"name": "\u5f20\u4e09", "age": 16, "gender": "\u7537"}

#！！dumps()方法默认使用ascii码,可以禁用，那么以utf-8编码
print(json.dumps(dic,ensure_ascii=False))
#{"name": "张三", "age": 16, "gender": "男"}
```

> #### 这里有个小技巧，可以设置缩进，让json格式看起来更爽洁
>
> #### 格式化输出，用indent参数设定缩进的空格数，可以设置为2，或者4
>
> ```python
> print(json.dumps(dic,ensure_ascii=False,indent=4))
> '''
> {
>  "name": "张三",
>  "age": 16,
>  "gender": "男"
> }
> '''
> ```

#### (3) dump() python->json文件对象

​	打开文件，如没有此文件就创建，如果有就写，with可以自动关闭

```python
with open('person.json','w',encoding='utf-8') as f:
	json.dump(dic,f,ensure_ascii=False,indent=2)
```

#### (4) load() json文件->python

```python
with open('person.json','r',encoding='utf-8') as f:
	dic = json.load(f)
	print(dic)
'''{'name': '张三', 'age': 16}'''
```

### 2. JSONPath简介

​	JSONPath是一种信息抽取类库，是从JSON文档中抽取指定信息的工具。

​	要提前安装jsonpath库

```powershell
pip install jsonpath
```

​	使用要导入josnpath模块

```python
import jsonpath
```

### 3. JSONPath语法对比

> JSON结构清晰，可读性高，复杂度低，非常容易匹配。JSONPath的语法和XPath类似。

| XPath | JSONPath | 描述                                                         |
| :---: | :------: | ------------------------------------------------------------ |
|   /   |    $     | 根节点                                                       |
|   .   |    @     | 现行节点                                                     |
|   /   |  .or[]   | 取子节点                                                     |
|  ..   |   n/a    | 取父节点，JSONPath未支持                                     |
|  //   |    ..    | 不管位置，选择所有符合条件的节点                             |
|   *   |    *     | 匹配所有元素节点                                             |
|   @   |   n/a    | 根据属性访问，JSON不支持，因为JSON是键值对的结构，不需要属性访问 |
|  []   |    []    | 迭代器表示(可以在里面做简单的迭代操作，如数组下标、根据内容选值等) |
|  \|   |   [,]    | 支持迭代器多选                                               |
|  []   |   ?()    | 过滤操作                                                     |

1. 首先获得一个json文件，导入json和jsonpath

   ```python
   import json
   import jsonpath
   json_str = '''
   {
     "store": {
       "book": [
         { "category":"reference",
           "author":"Nigel Rees",
           "title":"Sayings of the Century",
           "price":8.95
         },
         { "category":"fiction",
           "author":"J. R. R. Tolkien",
           "title":"The Lord of the Rings",
           "isbn":"0-395-19395-8",
           "price":22.99
         }
       ],
       "bicycle":{
         "color":"red",
         "price":19.95
        }
     }
   }
   '''
   ```

2. 将json格式转换为python对象

   ```python
   json_param = json.loads(json_str)
   print(type(json_param))
   #<class 'dict'>
   ```

3. 进行jsonpath对文件进行数据解析

   1. 查看json_param下的bicycle的color属性

      ```python
      check_url = '$.store.bicycle.color'
      print(jsonpath.jsonpath(json_param,check_url))
      #['red']
      
      #或者直接用..进行定位(最好在最前面加个$表示一下在根节点内进行查找)
      check_url = '$..color'
      print(jsonpath.jsonpath(json_param,check_url))
      #['red']
      ```

   2. 输出所有的book

      ```python
      check_url = '$.store.book[*]'
      print(jsonpath.jsonpath(json_param,check_url))
      '''
      [{'category': 'reference', 'author': 'Nigel Rees', 'title': 'Sayings of the Century', 'price': 8.95}, {'category': 'fiction', 'author': 'J. R. R. Tolkien', 'title': 'The Lord of the Rings', 'isbn': '0-395-19395-8', 'price': 22.99}]
      '''
      ```

   3. 输出第一本book，**注意：这里第一个索引是从0开始**

      ```python
      check_url = '$.store.book[0]'
      print(jsonpath.jsonpath(json_param,check_url))
      #[{'category': 'reference', 'author': 'Nigel Rees', 'title': 'Sayings of the Century', 'price': 8.95}]
      ```

   4. 输出所有的书名

      ```python
      check_url = '$.store.book[*].title'
      print(jsonpath.jsonpath(json_param,check_url))
      #['Sayings of the Century', 'The Lord of the Rings']
      ```

   5. 过滤 输出book中的price为22.99的所有对象

      **?() ?就是那个要找的对象，()是要满足的要求,@要加，表示在book里找。点表示在下一个节点了，也主要要加**

      ```python
      check_url = '$.store.book[?(@.price==22.99)]'
      print(jsonpath.jsonpath(json_param,check_url))
      #[{'category': 'fiction', 'author': 'J. R. R. Tolkien', 'title': 'The Lord of the Rings', 'isbn': '0-395-19395-8', 'price': 22.99}]
      ```

   6. 输出所有价格小于10的book

      ```python
      check_url = '$.store.book[?(@.price<10)]'
      print(jsonpath.jsonpath(json_param,check_url))
      #[{'category': 'reference', 'author': 'Nigel Rees', 'title': 'Sayings of the Century', 'price': 8.95}]
      ```

   7. 输出所有含有isbn的book

      ```python
      check_url = '$.store.book[?(@.isbn)]'
      print(jsonpath.jsonpath(json_param,check_url))
      #[{'category': 'fiction', 'author': 'J. R. R. Tolkien', 'title': 'The Lord of the Rings', 'isbn': '0-395-19395-8', 'price': 22.99}]
      ```

      