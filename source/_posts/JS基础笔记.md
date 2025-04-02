---
title: JS基础笔记
tags:
  - js
categories:
  - 学习
description: "\U0001F957 JS 基础学习笔记"
abbrlink: 957e08e7
date: 2022-06-02 16:36:00
cover:
---
# 第一课

## 引入js

1. js代码使用<script>标签括起来
2. 注释：// 单行注释，/**/多行注释
3. js语句后面分号 **可加可不加**
4. js区分大小写

```js
// demo.js
console.log('你好 世界');
alert('请看控制台')
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初识js</title>
</head>
<body>
    
</body>
<!-- 内嵌式引入 -->
<script>
    // 输入方式 1.alert
    alert('这是一条js语句')
    // 输出方式 2.document
    document.write('你好 世界，我是document')
    document.write('<h1>你好 世界，我是document</h1>')
    // 输出方式 3.console 控制台输出
</script>
<!-- 外联式引入 -->
<script src="./js/demo.js"></script>
</html>
```

## 页面加载的顺序需要注意

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>js运行顺序</title>
    <!-- js脚本放在这里会报错，因为js的运行顺序式顺序
    需要放在页面元素的后面
    关注html页面代码执行的顺序，自上而下边解释边执行 -->

    <!-- <script>
        // 功能实现：向id为div1的元素（盒子）中添加文本内容
        let t = document.getElementById('div1')
        t.innerHTML = '<h1>js实现添加内容</h1>'
    </script> -->
</head>
<body>
    <div id="div1"></div>
</body>
<script>
    // 功能实现：向id为div1的元素（盒子）中添加文本内容
    let t = document.getElementById('div1')
    t.innerHTML = '<h1>js实现添加内容</h1>'
</script>
</html>
```

## js 基本语法

- 变量名称规则：字母、数字、下划线、$,不能以数字开头
- 声明（定义）变量：var   let（局部变量） const（变量）
  - let 声明局部变量
  - const 声明常量,常量一般大写，常量定义就要赋值，不要分开
- js声明变量特点：声明变量时不需要指定数据类型，通过变量值确定改变量的数据类型，**typeof** 函数查看数据类型

```js
    // 变量名称规则：字母、数字、下划线、$,不能以数字开头
    // 声明（定义）变量：var    let（局部变量） const（变量）
    // js声明变量特点：声明变量时不需要指定数据类型，通过变量值确定改变量的数据类型
    var a;
    var b = 10;
    var c = 2, d = true;
    a = 'hello'
    console.log(typeof(a));
    console.log(typeof(b));

    // let 声明局部变量
    {
        var a1 = 10
        let b1 = 5
    }
    console.log(a1);
    // console.log(b1);    // 这里运行就会报错 b1 is not defined

    // const 声明常量,常量一般大写
    const PI = 3.14
    console.log(PI);

    // const PI2;  // 缺失初始化
    // 常量定义就要赋值，不要分开
```

#### 小案例

```html
<body>
    <input type="text" id="num1">
    <input type="text" id="num2">
    <input type="button" value="加运算" onclick="add()">
    <script>
        function add() {
            let a = document.getElementById('num1').value
            let b = document.getElementById('num2').value
            alert('两者相加结果为'+ (Number(a)+Number(b))) 
        }
    </script>
</body>
```

## 数组

> js 数组特点：
>
> 1. 数组下标从0开始，数组元素值的类型任意
> 2. 数组长度是可变的，不会出现下标越界
> 3. 未赋值的数组元素默认值是undefined
> 4. 可以更改数组长度：
>    - 若大于原长度（数组最后多出empty元素）
>    - 若等于数组原长度（数组不变）
>    - 若小于数组原长度（直接截取，删除多出的元素）

### 创建数组 

​	1、使用Array()

​	2、创建数组方法2：  使用[]

```js
        // 创建数组方法 1：使用Array
        var arr1 = new Array(3) // 数组长度为 3
        arr1[0] = 300
        arr1[1] = 'hello'
        arr1[2] = true
        console.log(arr1);
        arr1[3] = false
        console.log(arr1);      // 数组长度为 4
        arr1[8] = 'world'
        console.log(arr1);      // 数组长度为 9，多出部分为 undefined 类型
        arr1.length = 18
        console.log(arr1);      // 数组长度为 18
        arr1.length = 4
        console.log(arr1);      // 数组长度为 4 

        // 创建数组时，直接赋值
        var arr2 = new Array('hello',300,true)
        console.log(arr2);

        // 创建数组方法2：  使用[]
        var stu = ['Tom','Jerry']
        console.log(stu);
```

### 访问数组元素

访问单个数组元素：数组名称[下标]

遍历数组元素

```js
        var stu = ['Tom','Jerry']
        console.log(stu);
        // 1、访问单个数组元素：数组名称[下标]
        console.log(stu[0]);
        console.log('-----------------------');
        // 2、遍历数组元素
        // forin：for(let 下标 in 数组名)
        for (let i in stu) {
            console.log(stu[i]);
        }
        console.log('-----------------------');
        for (let value of stu) {
            console.log(value);
        }
```

### 数组操作方法

> push：在数组末尾添加元素（多个），返回值为数组长度
>
> pop：在数组末尾删除元素（一个），返回值为删除的元素值
>
> unshift：在数组前添加元素（多个），返回值为数组长度
>
> shift：在数组前删除元素（一个），返回值为删除的元素值

```js
        var a = [1,2,3,4,5,6]
        console.log(a);                 // [1, 2, 3, 4, 5, 6]

        a.push(7,8,9)               
        console.log(a);                 // [1, 2, 3, 4, 5, 6, 7, 8, 9]
        console.log(a.pop());           // 9
        console.log(a);                 // [1, 2, 3, 4, 5, 6, 7, 8]
        a.unshift(0,1)
        console.log(a);                 // [0, 1, 1, 2, 3, 4, 5, 6, 7, 8]
        a.shift()
        console.log(a);                 // [1, 1, 2, 3, 4, 5, 6, 7, 8]

        // 连接数组元素
        console.log(a.join());          // 1,1,2,3,4,5,6,7,8
        console.log(a.join('-'));       // 1-1-2-3-4-5-6-7-8

        var b = ['a','b','c','d']
        b.splice(1,2)   // splice(start,length):从start位置开始，删除length个数组元素
        console.log(b);                 // ['a', 'd']

        var c = ['a','b','c','d']
        c.splice(1,0,'aaa') // splice(start,length,data):从start位置开始，删除length个数组元素，用data进行替换
        console.log(c);                 // ['a', 'aaa', 'b', 'c', 'd']
```

## 函数

function	函数名（参数）{}；**记得调用**

```js
        // 1、创建无参函数
        function fun1() {
            console.log('这是一个无参的函数');
        }
        fun1()      // 函数声明好后调用

        // 2、创建有参函数
        function fun2(a,b) {
            console.log(a+b);
        }
        fun2(1,2)   // 3

        // 箭头函数
        fun3 = (a,b) => {
            console.log(a*b);
        }
        fun3(2,3)

        // 3、函数的参数不确定时，使用 arguments内置对象
        function fun4() {
            console.log('您输入了' + arguments.length + '个参数');
        }
        fun4(1,2,3,4)
```

#### 函数表达式

1. 函数表达式：是将函数定义赋值给一个变量
2. 设置函数表达式之后，函数名称就没有用了，调用函数需要用【变量(参数)】进行调用

```js
        // 函数表达式
        var f = function fun1() {
            console.log('这是一个无参函数');
        }
        // 调用函数
        f()
```

通过上面的例子，函数表达式中的函数名称无用，可以直接将函数名称删除，代码如下

```js
        // 函数表达式
        var f1 = function () {	// 匿名函数
            console.log('这是一个无参函数');
        }
        // 调用匿名函数
        f1()
```

# 第二课

## 变量的作用域

全局变量、局部变量、块级变量（let）

js 特有应用：函数内，未使用var定义的变量，是全局变量

```js
        f1 = () => {
            // js 特有应用：函数内，未使用var定义的变量，是全局变量
            a = 10
            console.log('函数内' + a);
        }
        f1()                               // 10
        console.log('函数外' + a);         
```

```js
        f2 = () => {
            var a1 = b1 = c1 = 100
            /*
            等价于 
                var a1 = 100
                b1 = 100
                c1 = 100
            */
            console.log(a1);
            console.log(b1);
            console.log(c1);
        }
        f2()
        // console.log(a1);         报错，因为是a1是局部
        console.log(b1);           // 正确，b1是全局变量
        console.log(c1);           // 正确，c1是全局变量
```

## 内置对象

### string对象

```js
        var str = 'hello'
        // 1 下标能取到，3 取不到，取到 3 之前的前一位
        console.log(str.substring(1,3));        // el
        // 这里 3 是长度   
        console.log(str.substr(1,3));           // ell
```

```html
	<div>
        输入字符串：<input type="text" id="old"><br><br>
        转换后的字符串：<input type="text" id="new" readonly><br><br>
        <input type="button" value="转为大写字母" onclick="toupper()">
        <input type="button" value="转为小写字母" onclick="tolower()">
    </div>
    <script>
        var old = document.getElementById('old')
        var new1 = document.getElementById('new')
        toupper = () => {
            console.log(old.value.toUpperCase());
            new1.value = old.value.toUpperCase()
        }
        tolower = () => {
            console.log(old.value.toUpperCase());
            new1.value = old.value.toLowerCase()
        }
    </script>
```

### math对象

```js
        console.log(Math.random());                     // 生成 [0,1) 之间的实数
        console.log(Math.max(1,2,3,4,5,6));
        console.log(Math.floor(Math.random()*10 + 1)); // 随机获取 1~10 之间的整数，floor：向下取整
```

### Date对象

获取当前时间，并格式化

```js
        // 获取当前日期
        var d1 = new Date()
        console.log(d1);                                // Mon May 30 2022 14:53:29 GMT+0800 (中国标准时间)

        var d2 = new Date(2022,6,1,00,00,00)            // 这里 月份 是 从 0 开始
        console.log(d2);                                // 这里显示的将是 7 月 1 日

        var d3 = new Date('2022-06-01 00:00:00')
        console.log(d3);                                // Wed Jun 01 2022 00:00:00 GMT+0800 (中国标准时间)

        // Date对象的常用方法
        var year = d1.getFullYear()
        var month = d1.getMonth()
        var day = d1.getDate()
        var hour = d1.getHours()
        var min = d1.getMinutes()
        var second = d1.getSeconds()
        // 现在时间是：2022-5-30 15:7:24
        console.log('现在时间是：' + year + '-' + (month+1) + '-' + day + ' ' + hour + ':' + min + ':' +  second);
        // 分钟和秒进行补零
        min = min < 10 ? '0'+min : min
        second = second < 10 ? '0'+second : second
        // 现在时间是：2022-5-30 15:07:24
        console.log('现在时间是：' + year + '-' + (month+1) + '-' + day + ' ' + hour + ':' + min + ':' +  second);
```

#### 定时器

**setInterval**(() => {

​    }, interval);

interval：定时的时间，单位 **毫秒**

```js
        getTime = () => {
            var d1 = new Date()

            var hour = d1.getHours()
            var min = d1.getMinutes()
            var second = d1.getSeconds()

            min = min < 10 ? '0' + min : min
            second = second < 10 ? '0' + second : second
            console.log(hour + ':' + min + ':' + second);
        }
        
        setInterval(() => {
            getTime()
        }, 1000);
```

```html
<body>
    <div id="div1"></div>
    <script>
        var a = document.getElementById('div1')
        getTime = () => {
            var d1 = new Date()

            var hour = d1.getHours()
            var min = d1.getMinutes()
            var second = d1.getSeconds()

            min = min < 10 ? '0' + min : min
            second = second < 10 ? '0' + second : second
            // document.write('<h1>' + hour + ':' + min + ':' + second + '</h1>')
            a.innerHTML = '<h1>' + hour + ':' + min + ':' + second + '</h1>'
        }
        
        setInterval(() => {
            getTime()
        }, 1000);
    </script>
</body>
```

# 第三课

## BOM

浏览器对象模型

window对象是 **顶层对象**，其它对象都是window对象的  **子对象**

### window 对象

```js
        alert('你好 世界')
        window.alert('window对象的alert')   // window对象可以省略，与上一条语句功能一致

        console.log(window.a);  // 输出：undefined，表示a存在，但是未赋值
        window.console.log(a)   // 报错，a is not defined
```

### window 对象打开对话框

```js
        // 1、警示框
        window.alert('这是一个警示框')

        // 2、选择确认对话框：confirm(提示字符串)，返回值为Boolean
        var choice = window.confirm('这是一个确认对话框吗？')
        console.log(choice);

        // 3、输入对话框：prompt(提示字符串，默认值)，返回值为 用户输入的字符串
        var name = window.prompt('姓名','cx')
        console.log(name);
```

### window 对象操作窗口

```html
<body>
    <input type="button" value="打开窗口" onclick="openwindow()">
    <input type="button" value="打开空白窗口" onclick="openwindow2()">
    <input type="button" value="关闭空白窗口" onclick="closewindow()">
    <input type="button" value="移动空白窗口" onclick="movewindow()">
    <script>
        function openwindow() {
            // open(url,窗口名称string,窗口特征值string)
            window.open('http://www.baidu.com','mywin','width=400,height=400,left=100,top=50')
        }
        var mywin
        function openwindow2() {
            // open(url,窗口名称string,窗口特征值string)
            // var mywin        在关闭的时候是局部变量，所以定义的时候在全局定义
            mywin = window.open('','mywin','width=400,height=400,left=100,top=50')
            mywin.document.write('<p>输出</p>')
            getPosition()
            mywin.document.title = '这是新窗口'
        }
        closewindow = () => {
            mywin.close()
        }
        movewindow = () => {
            // 每次移动一段距离，这里是距离
            // mywin.moveBy(100,100)
            // 移动到指定的位置
            mywin.moveTo(400,100)

            getPosition()
        }
        // 获取窗口当前坐标
        getPosition = () => {
            let x = mywin.screenX
            let y = mywin.screenY
            mywin.document.write('<p>窗口坐标为' + x + ' 纵坐标为 ' + y + '</p>')
        }
    </script>
</body>
```



## DOM

文档对象模型