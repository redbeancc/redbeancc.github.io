---
title: C语言学习笔记
tags:
  - c
categories:
  - 学习
description: "\U0001F384复试对C语言的复习"
cover: 'https://i-blog.csdnimg.cn/direct/a9ca329ac5144f30bb7f7109fe95e170.png'
abbrlink: '13675369'
date: 2025-04-02 22:18:55
---
# 一、初识

## 标准输入输出函数

- `printf()`:

  1. %3.2f: 总宽度是3，小数部分是两位。<span style="color:red">**这里宽度是算上 .这个符号的**</span>

     但这里有个问题，如果数值的整数部分超过3位会怎么样？

     比如123.45，这时候宽度会自动扩展，不会截断，所以3可能只是最小宽度。

     这时候用户可能会误解，认为总宽度被限制为3，但实际上不够的话会自动扩展。

  2. %10.2f会保留10个字符的宽度，不足的话用空格填充，右对齐，而%-10.2f则是左对齐

  3. 如果实际数值的小数位数超过指定的精度，会进行四舍五入。比如3.1415用%.2f会变成3.14

     ```c
     // 学生信息输入输出,输出成绩需进行四舍五入且保留2位小数
     int work_example4() {
         int id = 0;
         float c = 0.0f; // 不加f默认是double类型
         float math = 0.0f;
         float eng = 0.0f;
         scanf("%d;%f,%f,%f", &id, &c, &math, &eng); // 1234567;80.845,90.55,100.00
         printf("The each subject score of No. %d is %6.2f, %6.2f, %6.2f.\n",
             id, c, math, eng); //// The each subject score of No. 1234567 is 空80.85, 空90.55, 100.00.
         // 关于printf
         // 1. %3.2f: 总宽度是3，小数部分是两位。这里宽度是算上 .这个符号的
         //    但这里有个问题，如果数值的整数部分超过3位会怎么样？
         //    比如123.45，这时候宽度会自动扩展，不会截断，所以3可能只是最小宽度。
         //    这时候用户可能会误解，认为总宽度被限制为3，但实际上不够的话会自动扩展。
         // 2. %10.2f会保留10个字符的宽度，不足的话用空格填充，右对齐，而%-10.2f则是左对齐
         printf("%10.2f\n", 12.56); //空空空空空12.56
         printf("%-10.2f\n", 12.56);//12.56空空空空空
         // 3. 如果实际数值的小数位数超过指定的精度，会进行四舍五入。比如3.1415用%.2f会变成3.14
         return 0;
     }
     ```

  4. ```c
     printf("%02d\n", 2); // 02, 2d 输出两个数字, %0 不满足时,左面补0
     ```

  5. `printf()`返回打印的字符个数,如果错误就返回一个负数

     ```c
     int n = printf("Hello world!"); // Hello world!
     printf("\n%d\n",n); // 12
     ```

     

- `scanf()`:

  1. 可以通过scanf函数的%m格式控制可以制定输入域宽(列数),按此宽度截取所需数据

     ```c
     scanf("%4d%2d%2d", &year, &month, &day); // 20130225
     ```

     



# 发展

> 什么是编译?
>
> c/c++是编译型的语言
>
> test.c --->编译--->链接--->test.exe 

## 声明外部元素 extern

> 同一工程不同的文件
>
> extern 只是声明变量，与声明文件不同

```c
// externFile.c
// 注意这里不可加 define 关键字
int extern_a = 10;
// 两个变量名字必须一致，即extern_a

// 启动项.c
extern int extern_a;
int main() {
	printf("外部的a=%d", extern_a);
	return 0;
}
```

## 常量

C语言的常量分为

- 字面常量
- `const`修饰的常变量
  - 本质是变量，但是不能直接修改，有常量的属性
- #define 定义的标识符常量
- 枚举常量

```c
// const修饰
int const_example() {
    int var_a = 10;
    var_a = 20;
    printf("var_a=%d", var_a);
    const int const_b = 15;
    // 编辑器报错，不允许修改
    // const_b = 25;
    
    // 本质是变量，但是不能直接修改，有常量的属性
    int arr[const_b] = { 0 };
    // 此处const_b会报错，提示数组必须输入常量值
}
```

```c
// define 定义的标识符常量
#define MAX 100
#define STR "abcde"
int const_define() {
    printf("MAX = %d\n", MAX);
    int a = MAX;
    printf("a = MAX = %d\n", a);
    //MAX = 200; //err:等式左操作数必须可修改
    // define 定义的是常量，与什么类型无关
    printf("define 定义的字符串常量：%s\n", STR);
    return 0;
}
```

```c
// 枚举类型：能够穷举的类型
enum Color
{
    // 枚举常量，不可修改
    RED,
    GREEN,
    BLUE
};
int const_enum() {
    enum Color c = RED;
    //RED = 20;// err:常量不可修改
    return 0;
}
```



## 字符串

2025-01-05 14:08:20

> 字符串的结束标志是`\0`的转义字符。在计算字符的长度时`\0`是结束标识，不算作字符串内容。

```c
int string() {
    // C语言中的基本类型没有字符串
    // 字符串就是char字符类型的数组
    char a = 'a';
    // 若不确定字符串长度，或者不进行限定，则初始化数组时无需定义数组长度
    char arr[] = "abcdef";// 7
    // 打开断点调试，发现字符串后隐藏了一个\0转义字符
    return 0;
}
```

![image-20250105144531518](https://i-blog.csdnimg.cn/img_convert/738dea3bc31c3f97312f2809349885c9.png)

1. **思考：有无\0是否有差别？**

   ```c
       char arr1[] = "abcdef";
       char arr2[] = { 'a','b','c','d','e','f' };
       printf("%s\n", arr1);
       printf("%s\n", arr2);
   ```

![image-20250105145317050](https://i-blog.csdnimg.cn/img_convert/a8b3e73c1118aba06a43f3f166700458.png)

打印结果的不同是因为没遇到转义字符`\0`进行结束标识，会一直打印内存地址后面的内容直到遇到`\0`停止打印。

![image-20250105145709374](https://i-blog.csdnimg.cn/img_convert/f726859a4a30fde3d508a29a1a87e977.png)

2. `\0`对使用库函数 `strlen()`函数的影响

   > 使用 `strlen()`库函数需要引入 `string.h`头文件

   ```c
       printf("\"abc\"的长度为：%d\n", strlen("abc"));
   	// "abc"的长度为：3
       char arr3[] = "abcdef";
       char arr4[] = { 'a','b','c','d','e','f' };
       char arr5[] = { 'a','b','c','d','e','f','\0' };
       printf("arr3的长度为：%d\n", strlen(arr3));
   	// arr3的长度为：6
       printf("arr4的长度为：%d\n", strlen(arr4));
   	// arr4的长度为：38
       printf("arr5的长度为：%d\n", strlen(arr5));
   	// arr5的长度为：6
   ```

   原因同上，计算长度一直计算到`\0`转义字符作为结束标识。

   ### 转义字符

   | printf打印类型 | 含义               |
   | -------------- | ------------------ |
   | %d             | 打印整型           |
   | %c             | 打印字符型         |
   | %s             | 打印字符串         |
   | %f             | 打印float浮点型    |
   | %lf            | 打印double浮点型   |
   | %zu            | 打印sizeof的返回值 |

   

   | 转义字符 |          含义          |
   | -------- | :--------------------: |
   | \n       |          换行          |
   | \\'      |  打印字符常量单引号\'  |
   | \\"      |       打印双引号       |
   | \\\      |        打印斜杠        |
   | \r       |          回车          |
   | \t       | 一个制表符，类似 tab键 |
   | \ddd     | 八进制，例如 \101 -> A |
   | \xdd     |        十六进制        |

   ```c
   int char_escape() {
       printf("%c\n", '\101'); // 转十进制：65，ASCII码为 A
       printf("%c\n", '\x61'); // 转十进制：97，ASCII码为 a
       return 0;
   }
   ```

   笔试题：

   ```c
   // 程序输出什么
   #include <stdio.h>
   
   int main() {
       printf("%d\n", strlen("abcd ef")); // 7,因为计算是从前到后数到`\0`才算结束，空格也算一个字符
       print("%d\n", strlen("C:\test\628\test.c")); // 14,\t算一个字符，\62算八进制的一个字符，8算一个字符
       return 0;
   }
   ```

   

## 数组

```c
int array() {
    // 数组:如果不赋值，默认值为0,字符串终止字符`\0`也是0
    char arr[4] = { 'b','i','t' };
    int arr2[4] = { 1,2,3 };
    char arr3[] = { 'b','i','t' };
    printf("%d\n", strlen(arr));// 3
    printf("%d\n", strlen(arr3));// 随机值
    return 0;
}

// 数组设置常量
int array_const() {
    int arr1[MAX] = { 1 }; //编译通过
    const int min = 2;
    //int arr2[min] = { 1 }; //编译不通过
    
    int arr2[5] = { 1,2,3,4,5 };
    int n = 3;
    // 当初始化完成时,下标可以是变量
    arr2[n] = 20;
    for (int i = 0; i < sizeof(arr2) / sizeof arr1[0]; i++)
    {
        // 1 2 3 20 5
        printf("%d\t", arr2[i]);
    }
    printf("\n");
    
}
```

**二维数组的初始化**

```c
	int arr3[][3] = {{1,2,3},{2,3,4},{3,4,5}...}; // 列必须得初始化

    // &arr[0]: 0x00 0x04 0x08
    // &arr[1]: 0x0c 0x10 0x14
    // 二维数组地址连续排列,所以不需要初始化行
    int arr2[][3] = { 0,0,0,0,0,0 };
```



## 操作符

> 算术操作符

```c
 + - * / %
```

- 除号`/`两端都是整数时,执行的是整数除法,如果两端有一个是浮点型就执行浮点数的除法

  ```c
      int a = 7 / 2;
      printf("%d\n", a); // 3
      double  b = 7 / 2;
      printf("%f\n", b); // 3.0000
      int c = 7.0 / 2;
      printf("%d\n", c); // 3
      printf("%f\n", c); // 0.0000
      double d = 7.0 / 2;
      printf("%f\n", d); // 3.5000
      // 如果只想保留一位小数
      printf("%.1f\n", d);// 3.5
  ```

- 取模`%`两端必须是整数,不能是浮点数

> 移位操作符

```c
>>  <<
```

> 位操作符

```c
& ^ |
```



> 赋值操作符

```c
= -= += *= /= &= ^= \= >>= <<=
```



> <a id="unaryOperator">单目运算符:只有一个操作数的操作符</a>
>
> - 双目运算符:操作符旁边有两个操作数

```c
!		取反:**0-假,非0-真**
-		负号
+		正号(一般无意义)
    int a = -10;
	int b = +a; // -10
&		取地址
sizeof	操作数类型长度(以字节为单位) 类型必须加括号,变量可以不用加.
    	sizeof的返回值类型是size_t,用 `%zu` 格式化输出更规范
    int a = 10;
    printf("%zu\n", sizeof(a)); // 4
    printf("%zu\n", sizeof a);  // 4 这个可以说明sizeof是单目运算符
    printf("%zu\n", sizeof(int)); // 对于数据类型,得加上括号
    //printf("%d\n", sizeof int);// err: 编译不通过
    // sizeof计算数组
    int arr[10] = { 0 };
    printf("%d\n", sizeof(arr)); // 40 计算的是整个数组
    printf("%d\n", sizeof(arr[0])); // 4 计算数组每个元素的字节
    char arr2[] = "12345"; // 默认加入了 `\0`
    printf("%d\n", sizeof arr2); // 6 计算结果包括了 `\0`
    printf("%d\n", strlen(arr2));// 5 计算结果排除了 `\0`

~		对一个数进行二进制按位取反
--		自减
++		自加
    // 单目运算符:++
    printf("单目运算符:++\n");
    int a2 = 10;
    int b2 = a2++; // 后置++,先使用,后++
    // int b = a; a = a + 1;
    printf("%d\n", b2); // 10
    printf("%d\n", a2); // 11
    int a3 = 10;
    int b3 = ++a3; // 前置++,先++,后使用
    // a3 = a3 + 1; b3 = a3;
    printf("%d\n", b3); // 11
    printf("%d\n", a3); // 11
*		间接访问操作符(解引用操作符)
(类型)	强制类型转换
    // 强制类型转换
    printf("强制类型转换:\n");
    double a5 = 3.56;
    printf("%d\n", a5); // 类型不匹配,输出结果是任意的
    printf("%f\n", a5); // 3.560000,浮点数用浮点型输出
    printf("%d\n", (int)a5); // 3, 强制转换,截取,不会四舍五入,浮点型转换为整数型
    printf("%f\n", (int)a5); // 0.000000, 一个整数型用float输出就是0.000000
```

> <a id="relationalOperator">关系操作符</a>

```c
>
>=
<
<=    
!=    不相等
==    相等
```

> 逻辑操作符

```c
&&	逻辑与
||	逻辑或
```

> 三目表达式

```c
exp1 ? exp2 : exp3
    // 三目表达式
    printf("三目表达式:\n");
    char a6[4]; // 不能为3,有`\0`结束符
    strcpy(a6, 2 > 1 ? "yes" : "no");
    printf("%s\n", a6); // yes

    int a7 = 2 > 1 ? 1 : 0;
    printf("%d", a7); // 1
```

> <a id="commaOperator">逗号表达式</a>
>
> - 特点: **从左向右**依次计算,整个表达式的结果是**最后一个**表达式的结果

```c
exp1, exp2, exp3, ...
    // 逗号表达式
    // 特点: 从左向右依次计算,整个表达式的结果是最后一个表达式的结果
    printf("逗号表达式:\n");
    int aa = 10;
    int bb = 20;
    int cc = 0;
    // cc = aa - 2; aa = bb + cc; 
	// dd = bb = cc - 3; => bb = cc-3; dd = bb;
    int dd = (cc = aa - 2, aa = bb + cc, bb = cc - 3);
    printf("aa = %d, bb = %d, cc = %d,dd = %d\n", aa, bb, cc, dd);
    // aa = 28, bb = 5, cc = 8, dd = 5, dd 和最后一个 bb 的结果一致

    int x,y,z;
    x = 1;
    y = 1;
    z = x++,y++,++y;
    printf("x=%d,y=%d,z=%d",x,y,z);
    /*
    x=2,y=3,z=1
    因为逗号标识符优先级小于赋值，所以最后z可以看作    (z = x++),y++,++y
    */

    int a = 10;
    int x = 0;
    int y = 20;
    a = x = y+1; // 从右往左,将x赋值为21,a=x=21,这种写法不推荐
```

> 下标引用、函数调用和结构成员

```c
[] () . -> 
    
    struct Stu2 s2 = { "李四",20,"保密","66666666666" };
    // .  主要用于对象来访问成员属性
    printf("姓名:%s,年龄:%d,性别:%s,电话:%s\n",
        s2.name, s2.age, s2.sex, s2.tele);
    // -> 主要用于指针来访问成员属性
	print(&s2);
    void print(struct Stu2* ps) {
        // 对象
        printf("%s %d %s %s\n", (*ps).name, (*ps).age, (*ps).sex, (*ps).tele);
        // 指针
        printf("%s %d %s %s\n", ps->name, ps->age, ps->sex, ps->tele);
    }
```

## 常见关键字

```c
auto break case char const continue default do double else enum 
extern float for goto if int long register return short signed
sizeof static struct switch typedef union unsigned void volatile while
```

C语言提供了丰富的关键字，这些关键字都是语言本身预先设定好的，用户自己是不能创造关键字的。

> 注：关键字，先介绍下面几个，后期遇到讲解

1. 循环类
   - for
   - while
   - do...while
   - break
   - continue
2. 分支类
   - if...else..
   - switch..case..default
   - goto
3. 字符类
   - char,short,int,float,double,long
   - signed - 有符号的, unsigned - 无符号类型的
   - enum - 枚举, struct - 结构体, union - 联合体(共用体)
   - void - 空类型(一般用于函数的返回类型,函数参数)
   - sizeof - 计算类型大小,单位字节
   - typedef - 类型重命名
4. 外部类
   - extern - 声明外部符号的
   - register - 寄存器,一般操作系统中使用, **建议**变量放入寄存器
   - static - 静态的
   - return - 函数返回值

变量的命名:

1. 有意义:

   ```c
   int age;
   float salary;
   ```

   

2. 名字必须是字母、数字、下划线组成，不能有特殊字符，同时不能以数字开头

   ```c
   int 2b;  // err
   int _2b; // ok
   ```

3. 变量名不能是关键字

### 关键字typedef

> typedef 顾名思义是类型定义,这里应该理解为类型重命名

比如:

```c
// 类型重命名
// 类型定义
typedef unsigned int unit;

// 结构体定义
struct Node
{
	int data;
	struct Node* next;
};

// 将类型定义 和 结构体定义结合在一起，方便使用
typedef struct Node2
{
	int data;
	struct Node* next;
} Node2;

int main(){
	unsigned int num = 0; // 无符号整型（无负数）
	// 像基础数据类型一样使用 typedef
	unit num2 = 0;
    printf("%zu,%zu\n", sizeof(unsigned int), sizeof(uint)); // 4,4
    
	// 如果结构体中没有使用 typedef 定义时的使用必须带上关键字 struct
	struct Node n1;
	// 使用 typedef 定义结构体可以简便使用
	Node2 n2; // n1 == n2
	return 0;
}
```

### 关键字 static

> 在 C 语言中：
>
> static 是用来修饰变量和函数的
>
> 1. 修饰局部变量：**将局部变成内部全局**
> 2. 修饰全局变量：**将具有外部链接属性的全局变量变成只有一个内部链接属性**
> 3. 修饰函数：**只允许一个文件内部使用，工程的其他源文件不可使用**

#### 修饰局部变量

- 局部变量出了作用域，**不销毁**的。与全局变量不同的是，全局变量可以在任意位置进行读取，局部变量只有在特定的作用区域才能读取。
- 本质上，static 修饰局部变量的时候，改变了**变量的存储位置**，从栈区变成了静态区。栈区是退出栈后变量销毁，静态区的变量保持和程序一致![image-20230803144318845](https://i-blog.csdnimg.cn/img_convert/e9a103c4049744bffaf702e8f5d4c3b2.png)
- 影响了变量的生命周期，生命周期变长，和程序的生命周期一样
- 在编译期间就创建了地址，程序运行时不修改存储地址

```c
#include <stdio.h>
void test() {
	int a = 1;
	a++;
	printf("%d ", a);
}

int main() {
	int i = 0;
	while (i < 10) {
		test();
		i++;
	}
	return 0;
}
/**
2 2 2 2 2 2 2 2 2 2
*/
```

```c
void test() {
	static int a = 1;
	a++;
	printf("%d ", a);
}
// 2 3 4 5 6 7 8 9 10 11
```

#### 修饰全局变量

> static 修饰全局变量的时候
>
> 这个全局变量的**外部链接**属性
>
> 就变成了**内部链接**属性
>
> 其他源文件（.c）就不能再使用这个全局变量了

![image-20230803153353001](https://i-blog.csdnimg.cn/img_convert/c5a698f4db8121d2e1152df18ba2adff.png)

#### 修饰函数

![image-20230803154907147](https://i-blog.csdnimg.cn/img_convert/6f149f7eb6e146b8735ee236795f6627.png)

## #define 定义常量和宏

> - 定义变量名 => #define 定义常量
>
> - 定义函数名 => #define 定义宏
>   - 宏是有参数的,参数无类型
>   - 宏名(宏参) 宏体

```c
// define定义标识常量
#define MAX 20

// #define 定义宏
// 宏是有参数的,参数无类型
//      宏名(宏参) 宏体
#define ADD(x,y) x+y // 完整写法 ((x) + (y))
// 函数写法
int add(int x, int y) {
    return x + y;
}

int define_const() {
    printf("define定义常量和宏:\n");
    printf("%d\n", MAX); // 直接打印 20
    int num = MAX; // 复制
    printf("%d\n", num); // 20
    int arr[MAX] = { 0 }; // 数组的初始化

    char a = 'a'; // 97
    char A = 'A'; // 65
    int c = ADD(a, A);
    printf("%d\n", c); // 97 + 65 = 162

    return 0;
}
```

## 指针

### 内存<a id="memory">1</a>

内存是电脑上特别重要的存储器,计算机中程序的运行都是在内存中进行的.

所以为了有效的使用内存, 就把内存划分成一个个小的内存单元, 每个内存单元的大小是**1个字节**.

为了能够有效的访问到内存的每个单元, 就给内存单元进行了编号, 这些编号被称为该**内存单元的地址.**

![image-20250311214611213](https://i-blog.csdnimg.cn/img_convert/3a6ca195907f27a8b98c0629c9519542.png)

```c
#include <stdio.h>
int main() {
	int a = 10; // 向内存申请4个字节，存储值 10
	//&a; // 取地址操作符
	//printf("%p\n", &a); // 使用%p进行打印地址
	int* p = &a;
	// p就是指针变量
	*p = 20; // 解引用操作符，意思是通过p中存放的地址，找到p所指向的对象，*p就是p指向的对象
	printf("%d", a);

	return 0;
}
```

![image-20250311215938706](https://i-blog.csdnimg.cn/img_convert/cfa34bc59686d586b4a384e81972f970.png)

### <a id="pointerVariable">指针变量大小</a>

- 不管什么类型，都是在创建指针变量
- 指针变量是用来存放地址的
- 指针变量的大小取决于**一个地址存放的时候需要多大的空间**
- 32位机器上的地址：32bit位 - 4 byte,所以指针变量的大小是**4个字节**
- 64位机器上的地址：64bit位 - 8 byte,所以指针变量的大小是**8个字节**

```c
int main() {
	// 不管什么类型，都是在创建指针变量
	// 指针变量是用来存放地址的
	// 指针变量的大小取决于一个地址存放的时候需要多大的空间
	// 32位机器上的地址：32bit位 - 4 byte,所以指针变量的大小是4个字节
	// 64位机器上的地址：64bit位 - 8 byte,所以指针变量的大小是8个字节
	printf("%d\n", sizeof(char*));
	printf("%d\n", sizeof(short*));
	printf("%d\n", sizeof(int*));
	printf("%d\n", sizeof(float*));
	printf("%d\n", sizeof(double*));
	return 0;
}

typedef struct Stu {
    int age;
    struct Stu* next;
} Stu;
printf("%zu\n", sizeof(Stu)); // 16 = 4 + (4) + 8

内存对齐规则: 
规则1：每个成员的起始地址必须是其自身大小的整数倍。
    第一个age:0~3,到next时,下标为4,4不是8的倍数,故补上4个字节
规则2：结构体总大小必须是最大成员大小的整数倍（此处最大成员是 next 指针，大小为 8字节）。
```

## 结构体

> 将单一类型组合在一起，相当于 java 中的类，比如

```c
struct people
{
	// 属性
	char name[5]; // 属性结束用 ; 结尾
	int age;
	char sex[1];
	char tele[12];
}; // 这里有分号
```

```c
// 指针变量接收
void print(struct people* ps) {
	printf("使用解引用 *ps：%s %d %s %s\n", (*ps).name, (*ps).age, (*ps).sex, (*ps).tele);
	// 结构体指针变量 -> 属性名
	printf("使用符号 ->：%s %d %s %s\n", ps->name, ps->age, ps->sex, ps->tele);
}
int main() {
	// 结构体初始化
	struct people p1 = { "张三",18,"男生","12345678901" };
	// 结构体对象.属性名
	printf("使用结构体对象.属性名：%s %d %s %s\n", p1.name, p1.age, p1.sex, p1.tele);
	print(&p1); // 将p1指针传入
	return 0;
}

/**
使用结构体对象.属性名：张三 18 男生 12345678901
使用解引用 *ps：张三 18 男生 12345678901
使用符号 ->：张三 18 男生 12345678901
*/
```

```c
// 字符溢出问题
// 学生
struct Student
{
    // 成员属性
    char name[10];
    short age;
    char sex[4]; // 小心字符溢出
    char tele[11];
};

struct Stu2
{
    char name[10];
    short age;
    char sex[5];
    char tele[12];
};
int struct_example() {
    struct Student s1 = { "张三",20,"保密","15563645231" };
    printf("姓名:%s,年龄:%d,性别:%s,电话:%s\n",
        s1.name, s1.age, s1.sex, s1.tele); // 姓名:张三,年龄:20,性别:保密15563645231烫烫烫烫烫烫烫烫烫烫烫烫烫烫蘦,电话:15563645231烫烫烫烫烫烫烫烫烫烫烫烫烫烫蘦
    printf("%zu\n", sizeof(s1.sex)); // 4
    printf("%zu\n", strlen(s1.sex)); // 45

    // 若char类型初始化分配的地址不够, 会导致打印错误
    // 总结: 1. 在初始分配char类型时要考虑 `\0` 的存储
    //       2. 中文字符占 char 类型两个字符
    struct Stu2 s2 = { "李四",20,"保密","66666666666" };
    printf("姓名:%s,年龄:%d,性别:%s,电话:%s\n",
        s2.name, s2.age, s2.sex, s2.tele); // 姓名:李四,年龄:20,性别:保密,电话:66666666666
    printf("%zu\n", sizeof(s2.sex)); // 5, 计算加入了 `\0`
    printf("%zu\n", strlen(s2.sex)); // 4, 省略计算了 `\0`, 但在内存地址中,\0是存储了的.

    return 0;
}
```



## 函数

# 二、分支语句和循环语句

> 分支语句

- if
- else

> 循环语句

- while
- for
- do...while...

> goto语句

## 1. 什么是语句？

C语言中语句可以分为以下五类:

1. 表达式语句
2. 函数调用语句
3. 控制语句
4. 复合语句
5. 空语句

**控制语句**用于控制程序的执行流程, 以实现程序的各种结构方式, 它们由特定的语句定义符组成, C语言有就中控制语句

可分为以下三类: 

- 分支语句: if语句, switch语句
- 循环语句: do...while..., for语句, while语句
- 转向语句: break语句, goto语句, continue语句, return语句

## 2. 分支语句(选择结构)

### 2.1 if语句

```c
    // 双分支
    int score = 60;
    if (score >= 60)
    {
        printf("及格了\n");
        printf("恭喜!\n"); // 多个执行语句得用括号括起来
    }
    else
    {
        printf("没及格\n");
    }
    // 当只有一条语句时可以简写成
    if (score >= 60) printf("pass\n");
    else printf("no-pass\n");

    // 多分支: 只会执行某一个,不会两个同时执行
    if (score >= 90)
        printf("优秀\n");
    else if (score >= 75)
        printf("良好\n");
    else if (score >= 60)
        printf("及格\n"); // ok
    else
        // else默认也是只控制一条语句,若是多条语句,
        // 则第一个语句是在else的控制下,剩下的语句则是外面的, 
        // 即每个分支后都会执行第二个语句
        printf("不及格\n");
        // 此语句在每个 else if 后都会执行
        printf("对不起,您不及格\n");

// 如下加上括号则只会在 else 执行时才会被执行
	else {
        printf("不及格\n");
        printf("对不起,您不及格\n");
    }
```

在C语言中如何表示真假?

> **0表示假, 非0表示真.**

思考: 以下代码执行后结果?

```c
    int a = 0;
    int b = 2;
    if (a == 1)
        if (b == 2)
            printf("xixi\n");
    else
        printf("haha\n");
	// 执行结果: 空白,什么都不执行
```

注意⚠️: **else 只会跟在最近的一个 if 匹配**

```c
	// 正确的格式
	if (a == 1)
        if (b == 2)
            printf("xixi\n");
        else // else 只会跟在最近的一个 if 匹配
            printf("haha\n");
	// 执行结果: 空白,不执行. 原因: 因为最外层的a条件不满足
```

**练习**

> 1. 判断一个数是否为奇数

```c
    printf("请输入数是否为奇数:");
    int num = 0;
    scanf("%d", &num);
    if (num % 2 == 1)
        printf("奇数\n");
    else
        printf("No\n");
```



> 2. 输出1~100所有的奇数

```c
    for (int i = 1; i <= 100; i++) {
        if (i % 2 == 1)
            printf("%d\n", i);
    }
```

### 2.2 switch语句

switch语句也是一种分支语句.

常常用于多分支得到情况.

例如:

> 输入1,输出星期一
>
> 输入2,输出星期二
>
> ...
>
> 输入7,输出星期天

如果使用if...else..语句就太复杂了,那我们就得有不一样的语法形式.

这就是switch语句.

```c
switch(整型表达式)
{
    case 整型常量表达式:
        语句;
    case 整型常量表达式:
        语句;
    ...
    default:
        此分支是可选的，用于处理未匹配到任何 case 的情况;
}
```

注意⚠️:

- `switch`的入口判断必须是整型
- `case`的判断条件也必须是整型

```c
    double score = 82;
    switch (score) { // error, 判断条件必须为整型
    case score < 60: // error, 判断条件必须为整型
        printf("不及格");
    }
```



#### 2.2.1 在switch语句中的 break

在`switch`语句中,我们没办法直接实现分支, 搭配`break`使用才能实现真正的分支.

```c
// 正常写法
    int day = 0;
    scanf("%d", &day);
    switch (day)
    {
    case 1:
        printf("星期一\n");
        break;
    case 2:
        printf("星期二\n");
        break;
    case 3:
        printf("星期三\n");
        break;
    default:
        printf("不是星期一二三\n");
        printf("这是default中的语句\n");
        break;
    }
/**
输入:5
不是星期一二三
这是default中的语句
**/
```

switch 的每个 case 后都必须加上 break 跳出循环, 否则会继续执行, 直至遇到 break 语句.

如果每个 case 都不等于 input 输入的话, 则执行 default 中的语句

1. 若 case 不加入 break 语句

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       case 1:
           printf("星期一\n");
           //break;
       case 2:
           printf("星期二\n");
           //break;
       case 3:
           printf("星期三\n");
           break;
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           break;
       }
   /**
   输入:1
   星期一
   星期二
   星期三
   
   输入:2
   星期二
   星期三
   **/
   ```

2. 若最后一个 case 与 default 同时无 break语句

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       case 1:
           printf("星期一\n");
           break;
       case 2:
           printf("星期二\n");
           break;
       case 3:
           printf("星期三\n");
           //break;
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           //break;
       }
   /**
   输入:2
   星期二
   
   输入:3
   星期三
   不是星期一二三
   这是default中的语句
   
   输入:5
   不是星期一二三
   这是default中的语句
   **/
   ```

3. case 都加上 break 语句, default 不加, 同时 switch 语句后加入代码

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       case 1:
           printf("星期一\n");
           break;
       case 2:
           printf("星期二\n");
           break;
       case 3:
           printf("星期三\n");
           break;
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           //break;
       }
       printf("default 之外的语句\n");
   /**
   输入:2
   星期二
   default 之外的语句
   
   输入:5
   不是星期一二三
   这是default中的语句
   default 之外的语句
   **/
   ```

**总结:**

- 若是 `case` 语句不加 `break` 语句, 就会一直 "流下去" 直至遇到 `break` 语句或者执行完所有的 `switch` 代码块.

- 在 `switch` 之外的代码, 每次都会执行.

- 在 C 语言中，`switch` 语句可以用于判断字符型变量。字符型变量实际上是整数类型（ASCII 码），因此可以直接在 `switch` 语句中使用

  ```c
      char input;
  
      printf("请输入一个字符 (A, B, C): ");
      scanf("%c", &input);
  
      switch (input) {
          case 'A':
          case 'a': // 可以处理大小写
              printf("你输入了 A\n");
              break;
          case 'B':
          case 'b':
              printf("你输入了 B\n");
              break;
          case 'C':
          case 'c':
              printf("你输入了 C\n");
              break;
          default:
              printf("输入的不是 A、B 或 C\n");
              break;
      }
  ```

#### 2.2.2 default 语句

如果表达的值与所有的`case`标签的值都不匹配怎么办?

其实也没什么, 结果就是所有的语句都被跳过而已.

程序并不会终止, 也不会报错, 因为这种情况在C语言中并不认为是个错误.

但是, 如果你并不想忽略不匹配所有标签的表达式的值时该怎么办呢?

你可以在语句列表中增加一条 default 子句, 把下面的标签:

`default:`

写在任何一个`case`标签可以出现的位置.

当 `switch`表达式的值并不匹配所有`case`标签的值时, 这个`default`子句后面的语句就会执行.

所以, 每个`switch`语句中只能出现一条`default`子句.

1. `default`中`break`存在且放在首位

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           break;
       case 1:
           printf("星期一\n");
           break;
       case 2:
           printf("星期二\n");
           break;
       case 3:
           printf("星期三\n");
           break;
       }
       printf("default 之外的语句\n");
   /**
   输入:2
   星期二
   default 之外的语句
   
   输入:5
   不是星期一二三
   这是default中的语句
   default 之外的语句
   **/
   ```

2. `default`中`break`存在且放在中间

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       case 1:
           printf("星期一\n");
           break;
       case 2:
           printf("星期二\n");
           break;
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           break;
       case 3:
           printf("星期三\n");
           break;
       }
       printf("default 之外的语句\n");
   /**
   输入:1
   星期一
   default 之外的语句
   
   输入:5
   不是星期一二三
   这是default中的语句
   default 之外的语句
   **/
   ```

3. `default`中`break`存在且放在中间, 但`case1`, `case2`不存在`break`

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       case 1:
           printf("星期一\n");
           //break;
       case 2:
           printf("星期二\n");
           //break;
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           break;
       case 3:
           printf("星期三\n");
           break;
       }
       printf("default 之外的语句\n");
   /**
   输入:1
   星期一
   星期二
   不是星期一二三
   这是default中的语句
   default 之外的语句
   
   输入:2
   星期二
   不是星期一二三
   这是default中的语句
   default 之外的语句
   
   输入:3
   星期三
   default 之外的语句
   
   输入:5
   不是星期一二三
   这是default中的语句
   default 之外的语句
   **/
   ```

4. `default`中`break`不存在且放在中间

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       case 1:
           printf("星期一\n");
           break;
       case 2:
           printf("星期二\n");
           break;
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           //break;
       case 3:
           printf("星期三\n");
           break;
       }
       printf("default 之外的语句\n");
   /**
   输入:2
   星期二
   default 之外的语句
   
   输入:3
   星期三
   default 之外的语句
   
   输入:5
   不是星期一二三
   这是default中的语句
   星期三
   default 之外的语句
   **/
   ```

5. `default`中`break`不存在且放在中间, 同时`case2`与`case3`中的`break`也不存在

   ```c
       int day = 0;
       scanf("%d", &day);
       switch (day)
       {
       case 1:
           printf("星期一\n");
           break;
       case 2:
           printf("星期二\n");
           //break;
       default:
           printf("不是星期一二三\n");
           printf("这是default中的语句\n");
           //break;
       case 3:
           printf("星期三\n");
           //break;
       }
       printf("default 之外的语句\n");
   /**
   输入:2
   星期二
   不是星期一二三
   这是default中的语句
   星期三
   default 之外的语句
   
   输入:3
   星期三
   default 之外的语句
   
   输入:5
   不是星期一二三
   这是default中的语句
   星期三
   default 之外的语句
   **/
   ```

<span style="color:red">**总结: 顺流直下, 从哪个 "口"(`case/default`) 流下去, 若没遇到`break`, 则会一直执行下去.**</span>

## 3. 循环语句

- `for` 循环: 初始条件 => 循环条件判断 => 执行循环体 => 改变条件 => 再 进行循环条件判断 => ...

  ```c
  // 与 if 类似,如果只执行一句循环体,可以省略括号,但若是多句循环体,必须加上括号,如不加,则只执行一句循环体,多的代码只是顺序执行下只执行一次
  for (int i = 0; i < 3; i++)
      printf("%d\t",i);
  	printf("xixi\n");
  // 0       1       2       xixi
  
  for (int i = 0; i < 3; i++) {
      printf("%d\t", i);
      printf("xixi\n");
  }
  /** 0       xixi
      1       xixi
      2       xixi **/
  
  ```

  提问: 下面打印了几个xixi?

  ```c
      // 提问; 下面打印了几个xixi
      printf("下面打印为几个xixi?\n");
      int i = 0;
      int j = 0;
      for (; i < 3; i++) {
          for (; j < 3; j++) {
              printf("xixi\n");
          }
      }
      /* 因为 j 执行三次后未初始化,故第二次i进入的时,j=3不进入循环体
      xixi
      xixi
      xixi
      */
  ```

  

- `while` 循环: 先判断, 后循环

  - `while` 循环中的
    - `break` 是用于永久的终止内层的循环
    - `continue ` 跳过本次循环后面的代码, 直接去进行条件判断, 进行下一次循环的判断

  ```c
      // continue 导致陷入死循环
      int i = 1;
      while (i <= 10)
      {
          if (5 == i)
          {
              continue; // error, 这里会导致陷入死循环
              // 因为不会执行 continue 之后的代码,导致 i 恒等 5, 然后一直循环跳过
          }
          printf("%d\t", i);
          i++;
      }
  /**
  输出结果: 1 2 3 4 死循环
  **/
  ```

  ```c
  // 下面代码执行结果? 
      int j = 1;
      while (j <= 10) {
          j++;
          if (5 == j)
              continue;
          printf("%d\t", j); // 2 3 4 6 7 8 9 10 11
      }
  ```

  再看几个代码 : 

  ```c
      // 代码1
      // getchar(): 从键盘上获取字符,返回字符的 ASCII 码值,即 int 类型
      // putchar(int): 打印字符,等于 printf("%c",97); 不会自动换行!
      // EOF: end of file, 鼠标右键转到定义, 发现=-1, 是个整型
      int ch = 0;
      while ((ch = getchar()) != EOF) {
          putchar(ch);
      }
  /**
  实现效果: 输入一个数,打印一个数,一直循环
  **/
  ```

  这里有一个输入缓冲区的问题.

  ![image-20250314224033594](https://i-blog.csdnimg.cn/img_convert/022b5c27905471127ea9d16d63f7878c.png)

  输入 a, 按下回车, 输入缓冲区里有两个字符 `a`, `\n`

  `putchar()`其实执行了两次.

  ![image-20250314225059395](https://i-blog.csdnimg.cn/img_convert/e7f24f4b5edd1e3a9592b8191fade560.png)

  ![image-20250314225546770](https://i-blog.csdnimg.cn/img_convert/c5e3db0e677761a40fbdbd3a7fae7ea2.png)

  举一个例子

  ![image-20250314230301530](https://i-blog.csdnimg.cn/img_convert/73dc4c358c11b9daac4c48ff3e428cfd.png)

  ```c
  // 清理缓存解决方法,中间加上getchar();抵消掉缓冲区的内容.
      char password[20] = { 0 };
      printf("请输入密码:>");
      scanf("%s", password); // 数组本身存储的就是地址,无需取地址符号&
  // 读取 \n
  	// getchar(); // 这样只能抵消掉一个字符,如果是多个字符,则失效
  
  	// 清理多个字符的缓存
  	int ch = 0;
  	while((ch = getchar()) != '\n') {
          ; // 一直读取到 \n, 中间不进行操作
      }
  
  	printf("请确认密码Y/N:>");
  	int res = getchar();
  	if ('Y' == res)
          printf("YES\n");
  	else
          printf("NO\n")
  ```

  ```c
      // 代码2
      char ch = '\n';
      while ((ch = getchar()) != EOF) {
          if (ch < '0' || ch > '9')
              continue;
          putchar(ch);
      }
      /*
      输出数字字符, 只打印数字字符,跳过其他字符
      但是从第二个输入数字的时候,不会换行
      输入: qwer123qwe
      123
      */
  ```

  

- `do...while...`循环: 先循环, 后判断

  ```c
  // do...while...中使用continue出现死循环,与while中原因一致
  // 因为不会执行 continue 之后的代码,导致 i 恒等 5, 然后一直循环跳过
  int i = 1;
  do {
      if (i == 5)
          continue;// 因为不会执行 continue 之后的代码,导致 i 恒等 5, 然后一直循环跳过
      printf("%d\t", i);
      i++;
  } while(i <= 10);
  ```

  

### 练习

1. 计算 n 的阶乘

   ```c
   // 1. 计算 n 的阶乘
   int loop_work_example1(int n) {
       int sum = 1;
       for (; n >= 1; n--) {
           sum *= n;
       }
       return sum;
   }
   ```

   

2. 计算`1!+2!+3!+...+10!`

   ```c
       int sum = 0;
       for (int i = 1; i <= 10; i++) {
           int res = loop_work_example1(i);
           sum += res;
       }
       printf("1!+2!+3!+...+10!=%d\n", sum);
   ```

   

3. 在一个有序数组中查找具体的某个数字n.(二分查找)

   ```c
   // 3. 在一个有序数组中查找具体的某个数字n.(二分查找)
   int binary_search(int arr[], int n, int length) {
       int left = 0;
       //int right = sizeof(arr) / sizeof(arr[0]) - 1; // error
       // 等效于：sizeof(int*) / sizeof(int) - 1, 解决办法, 外部传入
       int right = length - 1;
       int mid = 0;
       while (left <= right) {
           mid = (left + right) / 2;
           if (arr[mid] == n) return mid; // 返回下标
           if (n < arr[mid])
               right = mid - 1;
           if (arr[mid] < n)
               left = mid + 1;
       }
       return -1; // 未查到
   }
   
       // 3.二分查找
       int arr[] = { 1,5,6,7,9,12,13,16,29,31 };
       int length = sizeof(arr) / sizeof(arr[0]);
       int index1 = binary_search(arr, 13, length);
       printf("13所在的下标为:%d\n", index1); // 6
       int index2 = binary_search(arr, 15, length);
       printf("15所在的下标为:%d\n", index2); // -1
   ```

   **注意：**在C语言中，当数组作为函数参数传递时，**它会退化为指针**。函数中的 `sizeof(arr)` 实际上计算的是指针的大小（而非整个数组的大小）

   <span style="color:red">**函数内部无法通过 `sizeof` 获取数组的实际长度，必须通过额外参数传递。**</span>

4. 编写代码,演示多个字符从两端移动,向中间汇聚.

   ```c
   #include <Windows.h>
   #include <stdlib.h>
   // 4. 编写代码,演示多个字符从两端移动,向中间汇聚.
   int loop_work_example4() {
       char arr[] = "welcome to China!!!";
       char arr2[] = "###################";
       int length = strlen(arr);
   
       for (int i = 0; i <= length / 2; i++) {
           printf("%s\n", arr2);
           arr2[i] = arr[i];
           arr2[length - 1 - i] = arr[length - 1 - i];
           // 导入 Windos.h 头文件
           Sleep(1000);
           // 清空屏幕,需要导入 stdlib.h
           system("cls"); // system 是一个库函数,可以执行系统命令
       }
       /*
       ###################
       w#################!
       we###############!!
       wel#############!!!
       welc###########a!!!
       welco#########na!!!
       welcom#######ina!!!
       welcome#####hina!!!
       welcome ###China!!!
       welcome t# China!!!
       */
       return 0;
   }
   ```

   

5. 编写代码实现,模拟用户登陆情景,并且只能登陆三次.(只允许输入三次密码,如果密码正确则提示登陆成功, 如果三次均输入错误,则退出程序).

   ```c
   // 5. 编写代码实现,模拟用户登陆情景,并且只能登陆三次.
   // (只允许输入三次密码,如果密码正确则提示登陆成功, 
   // 如果三次均输入错误,则退出程序).
   int loop_work_example5() {
       char password[] = "123456";
       char input[7] = "";
       int length = strlen(password);
       int count = 1;
       int flag_error = 0;
       do {
           flag_error = 0;
           printf("请输入密码(六位):>");
           scanf("%s", input);
           for (int i = 0; i < length; i++) {
               if (password[i] != input[i]) {
                   printf("密码错误!%d次\n", count);
                   flag_error = 1;
                   count++;
                   break;
               }
           }
           if (!flag_error) {
               printf("登录成功!\n");
               break;
           }
       } while (count <= 3);
   
       if (flag_error)
           printf("登陆失败,错误次数:%d\n", count - 1);
       return 0;
   }
   ```

   ```c
   // 还可以使用 strcmp() 进行字符串比较
   // 两个字符串若是相等,则返回 0
   if (strcmp(password,"123456") == 0) {
       printf("登陆成功");
   }
   ```

6. 猜字大小的游戏

   ```c
   // #include <stdlib.h>
   // void srand(unsigned int seed); // 设置随机数起始点
   // #include <time.h>
   // int time(); // 返回时间戳
   
   // 6. 猜数字大小
   int game() {
       // 0~99 => 1~100
       int res = rand() % 100 + 1;
       int guess = 0;
       while (1) {
           printf("请输入数字:>");
           scanf("%d", &guess);
           if (guess == res) {
               printf("猜对了!结果是%d\n", guess);
               break;
           }
           if (guess < res)
               printf("猜小了\n");
           if (guess > res)
               printf("猜大了\n");
       }
       return 0;
   }
   #include <time.h>
   int loop_work_example6() {
       //开始界面
       int input = 0;
       // srand -> #include <stdlib.h>
       // time  -> #include <time.h> 
       srand((unsigned int)time(NULL)); // 设置随机值起始种子为时间戳
       do {
           printf("*************************\n");
           printf("******   1.play   *******\n");
           printf("******   0.exit   *******\n");
           printf("*************************\n");
           printf("请选择:>");
           scanf("%d", &input);
           switch (input)
           {
           case 1:
               game();
               break;
           case 0:
               printf("退出游戏\n");
               break;
           default:
               printf("输入非法,请重新选择\n");
               break;
           }
       } while (input);
       return 0;
   }
   ```

   

# 三、函数

> 1. 函数是什么
> 2. 库函数
> 3. 自定义函数
> 4. 函数参数
> 5. 函数调用
> 6. 函数的嵌套调用和**链式访问：函数返回值作为其他函数的参数**
> 7. 函数的声明和定义
> 8. 函数递归

## 1. 函数是什么?

函数: 子程序

> - 是一个大型程序重点某部分代码, 由一个或多个语句快自称. 他负责完成某项特定任务, 而且相较于其他代码, 具备相对的独立性.
> - 一般会有输入参数并有返回值, 提供对过程的封装和细节的隐藏. 这些代码通常被集成为软件库.

## 2. C语言中函数的分类:

1. 库函数
2. 自定义函数

### 2.1 库函数:

为什么会有库函数?

1. 我们知道在我们学习C语言变成的时候, 总是在一个代码编写完成之后迫不及待的想知道结果, 想把这个结果打印到我们的屏幕上看看. 这个时候我们会频繁的使用一个功能: 将信息按照一定的格式打印到屏幕上(printf)
2. 在编程的过程中，我们会频繁的做一些字符串的拷贝（strcpy）
3. 在编程时我们也会计算n的k次方这样的运算（pow）

像上面我们描述的基础功能，他们不是业务性的代码。我们在开发的过程中每个程序员都可能用得到，为了支持可移植性和提高程序的效率，所以C语言的基础库中提供了一系列类似的库函数，方便程序员进行软件开发。

## 4. 函数参数

### 4.1 实参

> 真实传给函数的参数，叫实参。
>
> 实参可以是: 常量、变量、表达式、函数等。
>
> 无论实参是何种类型的量，在进行函数调用时，她们都必须有确定的值，以便把这些值传送给形参。

### 4.2 形参

> 形式参数是指函数名后括号中的变量，因为形式参数只有在函数被调用的过程中才实例化（分配内存单元），所以叫形式参数。形式参数当函数调用完成之后就自动销毁了。因此形式参数只在函数中有效。

注意接收值还是地址

- 当实参传递给形参的时候，形参是实参的一份临时拷贝
- 对形参的修改不能修改实参

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void swap(int* px, int* py) {
	*px = *px + *py;
	*py = *px - *py;
	*px = *px - *py;
}

int main() {
	int a = 0;
	int b = 0;
	scanf("%d %d", &a, &b);
	printf("修改前：a=%d，b=%d\n", a, b);
	swap(&a, &b); 
	printf("修改后：a=%d，b=%d\n", a, b);
}

	// 地址的自增
    int c[] = { 30,50 };
    int* d = &c;
    printf("%p\n", d);
    d++; // 加上 4 个字节
    printf("%p\n", d);

	char ch[] = "abc";
	char* cp = &ch;
	cp++; // 加上一个字节
```

## 5. 函数调用：

### 5.1 传值调用

> 函数的形参和实参分别占有不同内存块，对形参的修改不会影响实参。

### 5.2 传址调用

> - 传址调用是把函数外部创建变量的内存地址传递给函数参数的一种调用函数的方式。
> - 这种传参方式可以让函数和函数外边的变量建立起真正的联系，也就是函数内部可以直接操作函数外部的变量

### 5.3 练习

> 1. 写一个函数可以判断一个数是不是素数。
> 2. 写一个函数判断一年是不是闰年。
> 3. 写一个函数，实现一个整型有序数组的二分查找。
> 4. 写一个函数，每调用一次这个函数，就会将 num 的值增加1

## 7. 函数的声明和定义

### 7.1 函数声明：

> 1. 告诉编译器有一个函数叫什么，参数是什么，返回类型是什么。但是具体是否存在，函数声明决定不了。
> 2. 函数的声明一般出现在函数的使用之前。要满足**先声明后使用。**
> 3. 函数的声明一般要放在头文件中。

### 7.2 函数定义：

> 函数的定义是指函数的具体实现，交代函数的功能实现。

```c
// 在本文件中的声明
// 函数的定义与声明
// 函数声明, 只是声明,具体是否存在决定不了
int add(int x, int y); // 不先声明会导致顺序运行出错
int function_example() {
    int a = 10;
    int b = 20;
    printf("%d\n", add(a, b));
    return 0;
}
// 函数定义
int add(int x, int y) {
    return x + y;
}
```

```c
// 在头文件中的声明
// run.h
// 库函数头文件<>
#include <stdio.h>
// 自定义头文件 ""
#include "run.h"

// 作业集
int work_main();

// 常量
//    -const
int const_example();
//    -define
int const_define();
//    -enum
int const_enum();
...

// 在main文件的应用
#include "run.h"
extern int extern_a;
extern void print(struct Stu2* ps);
int main() {
    // 作业集
    work_main();
    printf("%d\n", extern_a);
}
```

递归求斐波那契

```c
// 求第n个斐波那契数列
// 1 1 2 3 5 8 13 21 34 55...
int fibo(int n) {
    if (n <= 0)
        return 0;
    if (n == 1)
        return 1;
    else
        return fibo(n - 2) + fibo(n - 1);
    return 0;
}
int function_example() {
    function_example1();

    // 斐波那契数列
    while (1)
    {
        printf("第n个斐波那契数列:>");
        int input = 0;
        scanf("%d", &input);
        if (input == -1)
            break;
        int res = fibo(input);
        printf("%d\n", res);
    }
    return 0;
}
```



# 五、操作符详解

1. 各种操作符的介绍
2. 表达式求值

## 1. 操作符分类：

> 算术操作符：+ - * / %
>
> 移位操作符：<< >>
>
> 位操作符：& | ^
>
> 赋值操作符：+= -= *= /= ...
>
> 单目操作符
>
> 关系操作符
>
> 逻辑操作符
>
> 条件操作符:
>
> ```c
> // 三目表达式 
> condition ? yes : no
> ```
>
> 逗号表达式
>
> 下标引用、函数调用和结构成员

## 2. 算术操作符

```c
+	-	*	/	%
```

1. 除了`%`操作符之外,其他的几个操作符可以作用于整数和浮点数
2. 对于`/`操作符如果两个操作数都为整数,执行整数除法.但是只要有浮点数执行的就是浮点数除法.
3. `%`操作符的两个操作数必须为整数.返回的是整除之后的余数.

## 3. 移位操作符

```c
<< 左移操作符
>> 右移操作符
```

注: 移位操作符的操作数只能是**整数**

### 3.1 左移操作符

- **左移有乘2的n次方的效果 (扩大)**
- 移位规则:

> 左边抛弃、右边补0

```c
// 正的整数的原码、反码、补码相同
    // 负的整数的原码、反码、补码需要计算
    // 
    // 7 四个字节 32个二进制
    // 00000000 00000000 00000000 00000111 - 原码
    // 00000000 00000000 00000000 00000111 - 反码
    // 00000000 00000000 00000000 00000111 - 补码
    // -7 最高位表示符号 0-正数，1-负数
    // 10000000 00000000 00000000 00000111 - 原码
    // 11111111 11111111 11111111 11111000 - 反码（原码符号位不变，其他位按位取反就是反码）
    // 11111111 11111111 11111111 11111001 - 补码（反码+1=补码）
    // 
    // 整数在内存中存储的是补码
    //
    int a = 7;
    int b = a << 1;// a的补码向左移动一位,a的值不变
    // 7  :  |00000000 00000000 00000000 00000111| - 补码
    // << : 左边丢弃,右边补0
    //结果: 0|00000000 00000000 00000000 00001110| - 左移一位
    printf("%d\n", a); // 7,a的值不变
    printf("%d\n", b); // 14

    int c = -7;
    int d = c << 1;
    // -7 : |11111111 11111111 11111111 11111001| - 补码
    // <<1:1|11111111 11111111 11111111 11110010| - 左移一位,补零
    // 结果计算: 原码 = (补码 - 1)按位取反,符号位不动
    // -1  : |11111111 11111111 11111111 11110001|
    // 取反: |10000000 00000000 00000000 00001110| = -14
    printf("%d\n", c);// -7
    printf("%d\n", d);// -14
    return 0;
```

### 3.2 右移操作符

- 移位规则：

首先右移运算分两种：具体根据编译器的不同而不同

> 1. 逻辑移位
>
>    左边用0填充，右边丢弃
>
> 2. 算术移位（vs2019）
>
>    左边用原改制的符号位填充，右边丢弃

```c
    int a = 7;
    int e = a >> 1;// a的补码向右移动一位,a的值不变
    // 7  :  |00000000 00000000 00000000 00000111|  - 补码
    // >> : 右边丢弃,左边补原符号位的值
    //结果 :  |00000000 00000000 00000000 00000011|1 = 3
    printf("7>>1:%d\n", e); // 3
```

```c
	int c = -7;
    int f = c >> 1;
    // -7 : |11111111 11111111 11111111 11111001|  - 补码
    // >>1: |11111111 11111111 11111111 11111100|1 - 右移一位,补原符号位的值
    // 结果计算: 原码 = (补码 - 1)按位取反,符号位不动
    // -1 : |11111111 11111111 11111111 11111011|
    // 取反: |10000000 00000000 00000000 00000100| = -4
    printf("-7>>1: %d\n", f); // -7>>1: -4
```

警告⚠️: 

对于移位运算符, 不要移动负数位, 这个是标准未定义的.

```c
int num = 10;
num >> -1;//error
```

## 4. 位操作符

位操作符有:

```c
&	// 按补码位与
|	// 按补码位或
^	// 按补码位异或
注: 它们的操作数必须是整数.
```

```c
    int a = 3;
    int b = -5;
    int c = a & b; // 按补码位与
    // -5 : 10000000 00000000 00000000 00000101 - 原码
    //      11111111 11111111 11111111 11111010 - 反码
    //      11111111 11111111 11111111 11111011 - 补码
    // 3  : 00000000 00000000 00000000 00000011 - 原码 = 反码 = 补码
    //结果: 00000000 00000000 00000000 00000011 = 3
    printf("%d\n", c);// 3

    int d = a | b;// 按补码位或
    // -5 : 11111111 11111111 11111111 11111011 - 补码
    // 3  : 00000000 00000000 00000000 00000011 - 原码 = 反码 = 补码
    //结果: 11111111 11111111 11111111 11111011 - 补码
    // -1 : 11111111 11111111 11111111 11111010 - 反码
    //取反: 10000000 00000000 00000000 00000101 - 原码 = -5
    printf("%d\n", d); // -5

    int e = a ^ b;// 按补码位异或
    // -5 : 11111111 11111111 11111111 11111011 - 补码
    // 3  : 00000000 00000000 00000000 00000011 - 原码 = 反码 = 补码
    //结果: 11111111 11111111 11111111 11111000 - 补码
    // -1 : 11111111 11111111 11111111 11110111 - 反码
    //取反: 10000000 00000000 00000000 00001000 - 原码 = -8
    printf("%d\n", e); // -8
```

异或的特点:

1. `a^a = 0`

   `0^a = a`

   > 例如:
   >         3^3 : 011 ^ 011 = 000 = 0
   >         5^0 : 101 ^ 000 = 101 = 5
   >         进而: 3^3^5 = 5
   >         那么: 3^5^3 = ?
   >         3^5^3 = 011^101 = 110^011 =101 = 5

2. 满足交换律

   > ​			所以:3^3^5 = 3^5^3, 满足交换律

练习: 若不增加额外变量,将两个数交换?

```c
    int num1 = 3;
    int num2 = 5;
    printf("交换前,a=%d,b=%d\n", num1, num2); // 3 5
    num1 = num1 ^ num2; // num1 = 3^5
    num2 = num1 ^ num2; // num2 = 3^5^5 = 3
    num1 = num1 ^ num2; // num1 = 3^5^3 = 5,完成交换
    printf("交换后,a=%d,b=%d\n", num1, num2);// 5 3
```

编写代码实现: 求一个整数存储在内存中的二进制中1的个数

```c
    //问题2:求一个整数存储在内存中的二进制中1的个数
    // 求补码的二进制中1的个数
    int input = 0;
    printf("请输入一个数:>");
    scanf("%d", &input);
    int count = 0;
    // input = 3 : 00000000 00000000 00000000 00000011
    //         1 :&00000000 00000000 00000000 00000001
    // input & 1 : 00000000 00000000 00000000 00000001 = 1
    // input >>1 : 00000000 00000000 00000000 00000001
    // 再input&1 :&00000000 00000000 00000000 00000001 = 1...
    for (int i = 0; i < 32; i++) {
        if (input & 1) {
            count++;
        }
        input = input >> 1;
    }

    printf("在内存中的二进制1的个数为:%d\n", count);
    // -5在内存中的二进制1的个数为:31
    // 3在内存中的二进制1的个数为:2
```

## 5. 赋值操作符

赋值操作符可以连续使用, 比如

```c
int a = 10;
int x = 0;
int y = 20;
a = x = y+1; // 从右往左,将x赋值为21,a=x=21,这种写法不推荐

// 这样的写法更容易调试和阅读
x = y+1;
a = x;
```

**复合赋值符**

> +=、-=、*=、/=、%=
>
> <<=、>>=
>
> &=、|=、^=

## 6. 单目操作符

详情见：[单目操作符](#unaryOperator)

需要注意的是：

- `sizeof`是操作符, 不是函数, 计算结果包含`\0`
- `strlen`是库函数, 用来求字符串长度, 计算结果不包含`\0`

## 7. 关系操作符

详情见：[关系操作符](#relationalOperator)

```c
if ("abc" == "abcdef")       // error,这样比较的是首地址
if (strcmp("abc","abcdef"))  // ok, 两个字符串比较相等
```

## 8. 逻辑操作符

逻辑操作符有哪些: 

```c
&&		逻辑与
||		逻辑或
```

区分**逻辑与**和**按补码位与**

区分**逻辑或**和**按补码位或**

```c
1&2	 =	 001&010	= 000	=> 0
1&&2 => 1

1|2	 = 	 001|010	= 011	=> 3
1||2 => 1
```

短路求值：只有第一个运算的逻辑值无法判断结果，才对第二个运算数进行求值

```c
    int i = 0, a = 0, b = 2, c = 3, d = 4;
    // 短路求值：只有第一个运算的逻辑值无法判断结果，才对第二个运算数进行求值
    // 只要不满足，我就不继续往下判断了
    //i = a++ && ++b && d++;
    //printf("a = %d\nb = %d\nc = %d\nd = %d\ni = %d\n",
    //    a, b, c, d, i);// 1 2 3 4 0

    i = a++ || ++b || d++; // 确定一定不满足条件后就不执行了
    printf("a = %d\nb = %d\nc = %d\nd = %d\ni = %d\n",
        a, b, c, d, i);// 1 3 3 4 1
```

## 9. 逗号表达式

详情见: [逗号表达式](#commaOperator)

```c
a = get_val();
count_val(a);
while(a > 0) {
    // 业务代码
    a = get_val();
	count_val(a);
}
// 上述代码有冗余,可替换为
while(a = get_val(), count_val(), a > 0) {
    // 业务代码
}
```

## 10. 表达式求值

表达式求值的顺序一部分是由操作符的优先级和结合性决定.

同样, 有些表达式的操作数在求值的过程中可能需要转换为其他类型.

### 10.1 隐式类型转换

C的整形算术运算总是至少以缺省整型类型的精度来进行的.

为了获得这个精度, 表达式中的字符和短整型操作数在使用之前被转换为普通整型,这种转换成为**整型提升**

**整型提升的意义:**

> 表达式的整型运算要在CPU的相应运算器件内执行, CPU内整型运算器(ALU)的操作数的字节长度一般就是int的字节长度, 同时也是CPU的通用寄存器的长度
>
> 因此, 及时两个`char`类型的相加, 在CPU执行时实际上也要先转换为CPU内整型操作数的标准长度.
>
> 通用CPU(general-purpose CPU)是难以直接实现两个8比特字节直接相加运算(虽然机器执行中可能有这种字节相加指令).所以, 表达式中各个长度可能小于int长度的整型值,都必须先转换为`int`或`unsigned int`,然后才能送入CPU去执行运算.

如何进行整型提升呢?

> 整型提升是按照变量的数据类型的符号位来提升的

```c
// 负数的整型提升
char c1 = -1;
变量c1的二进制位(补码)中只有8个比特位
11111111
因为 cahr 为有符号的 char
所以整型提升的时候, 高位补充符号位, 即为1
提升之后的结果是:
11111111 11111111 11111111 11111111

// 正数的整型提升
char c2 = 1;
变量c2的二进制位(补码)中只有8个比特位:
00000001
因为 char 为有符号的 char
所以整型提升的时候, 高位补充符号位, 即为0
提升之后的结果是:
00000000 00000000 00000000 00000001
    
// 无符号整型提升, 高位补0
    char a = 0xb6;          // 11111111 11111111 11111111 10110110 高位补1
	if (a == 0xb6) 
        printf("a"); // 不打印
	unsigned char b = 0xb6; // 00000000 00000000 00000000 10110110 高位补1
	if (b == 0xb6) 
        printf("b"); // 打印
```

计算:

```c
    char a = 5;
    char b = 126;
    char c = a + b;
    printf("%d\n", c); // ?
```

```c
    char a = 5;
    // 00000000 00000000 00000000 00000101
    // 00000101 - a
    char b = 126;
    // 00000000 00000000 00000000 01111110
    // 01111110 - b
    char c = a + b;
    // 00000000 00000000 00000000 00000101 - a
    // 00000000 00000000 00000000 01111110 - b
    // 00000000 00000000 00000000 10000011 - 131
    // 截位: 10000011 - c
    // %d输出, 需要格式化为 4 个字节,整型提升,全补符号位
    // 11111111 11111111 11111111 10000011 - 补码
    // 11111111 11111111 11111111 10000010 - 反码:补码-1
    // 10000000 00000000 00000000 01111101 - 原码 = -125
    printf("%d\n", c); // -125
```

### 10.2 算术转换

如果某个操作符的各个操作数属于不同的类型, 那么除非其中一个操作数的转换为另一个操作数的类型, 否则操作就无法进行. 下面的层次体系称为**寻常算术转换**

```c
long double	
double		
float		
unsigned long int
long int
unsigned int
int
```

上述能从下往上转换, 例如 `int` + `double` 会转换成 `double`

### 10.3 操作符的属性

复杂表达式的求值有三个影响的因素. 

1. 操作符的优先级
2. 操作符的结合性
3. 是否控制求值顺序

两个相邻的操作符先执行哪个? 取决于他们的优先级. 如果两者的优先级相同, 取决于他们的结合性.

<span style="color:red">**算术操作符(+-*/) > 关系操作符(==,>=) > 位操作符 > 赋值操作符 > 逗号操作符**</span>



| 操作符 | 描述             | 用法示例            | 结果类型     | 结合性 | 是否控制求值顺序     |
| :----: | ---------------- | ------------------- | ------------ | ------ | -------------------- |
|   ()   | 聚组             | (表达式)            | 与表达式相同 | N/A    | 否                   |
|   ()   | 函数调用         | rexp(rexp, rexp...) | rexp         | L-R    | 否                   |
|   []   | 下标引用         | rexp[rexp]          | lexp         | L-R    | 否                   |
|   .    | 访问结构成员     | lexp.member_name    | lexp         | L-R    | 否                   |
|   ->   | 访问结构指针成员 | rexp->member_name   | lexp         | L-R    | 否                   |
|   ++   | 后缀自增         | lexp++              | rexp         | L-R    | 否                   |
|   --   | 后缀自减         | lexp--              | rexp         | L-R    | 否                   |
|   !    | 逻辑反           | !rexp               | rexp         | R-L    | 否                   |
|   ~    | 按位取反         | ~rexp               | rexp         | R-L    | 否                   |
|   +    | 单目,表示正值    | +rexp               | rexp         | R-L    | 否                   |
|   -    | 单目,表示负值    | -rexp               | rexp         | R-L    | 否                   |
|   ++   | 前缀自增         | ++lexp              | rexp         | R-L    | 否                   |
|   --   | 前缀自减         | --lexp              | rexp         | R-L    | 否                   |
|   *    | 间接访问         | *rexp               | lexp         | R-L    | 否                   |
|   &    | 取地址           | &lexp               | rexp         | R-L    | 否                   |
| sizeof | 返回字节长度     | sizeof rexp         | rexp         | R-L    | 否                   |
| (类型) | 类型转换         | 类型(rexp)          | rexp         | R-L    | 否                   |
|   *    | 乘法             | rexp * rexp         | rexp         | L-R    | 否                   |
|   /    | 除法             | rexp / rexp         | rexp         | L-R    | 否                   |
|   %    | 取余             | rexp % rexp         | rexp         | L-R    | 否                   |
|   +    | 加法             | rexp + rexp         | rexp         | L-R    | 否                   |
|   -    | 减法             | rexp - rexp         | rexp         | L-R    | 否                   |
|   <<   | 左移位           | rexp << rexp        | rexp         | L-R    | 否                   |
|  \>>   | 右移位           | rexp \>> rexp       | rexp         | L-R    | 否                   |
|   >    | 大于             | rexp > rexp         | rexp         | L-R    | 否                   |
|  \>=   | 大于等于         | rexp \>= rexp       | rexp         | L-R    | 否                   |
|   <    | 小于             | rexp < rexp         | rexp         | L-R    | 否                   |
|   <=   | 小于等于         | rexp <= rexp        | rexp         | L-R    | 否                   |
|   ==   | 等于             | rexp == rexp        | rexp         | L-R    | 否                   |
|   !=   | 不等于           | rexp != rexp        | rexp         | L-R    | 否                   |
|   &    | 按补码位与       | rexp & rexp         | rexp         | L-R    | 否                   |
|   ^    | 按补码位异或     | rexp ^ rexp         | rexp         | L-R    | 否                   |
|   \|   | 按补码位或       | rexp \| rexp        | rexp         | L-R    | 否                   |
|   &&   | 逻辑与           | rexp && rexp        | rexp         | L-R    | 是,短路求值,提前终止 |
|  \|\|  | 逻辑或           | rexp \|\| rexp      | rexp         | L-R    | 是                   |
|   ?:   | 条件操作符       | rexp ? rexp : rexp  | rexp         | N/A    | 是                   |
|   =    | 赋值             | lexp = rexp         | rexp         | R-L    | 否                   |
|   +=   | 加等             | lexp += rexp        | rexp         | R-L    | 否                   |
|   -=   | 减等             | lexp -= rexp        | rexp         | R-L    | 否                   |
|   *=   | 乘等             | lexp *= rexp        | rexp         | R-L    | 否                   |
|   /=   | 除等             | lexp /= rexp        | rexp         | R-L    | 否                   |
|   %=   | 取模等           | lexp %= rexp        | rexp         | R-L    | 否                   |
|  <<=   | 左移等           | lexp <<= rexp       | rexp         | R-L    | 否                   |
|  \>>=  | 右移等           | lexp \>>= rexp      | rexp         | R-L    | 否                   |
|   &=   | 按补码位与等     | lexp &= rexp        | rexp         | R-L    | 否                   |
|   ^=   | 按补码位异或等   | lexp ^= rexp        | rexp         | R-L    | 否                   |
|  \|=   | 按补码位或等     | lexp \|= rexp       | rexp         | R-L    | 否                   |
|   ,    | 逗号             | rexp, rexp          | rexp         | L-R    | 是, 最后一个是最终值 |

```c
    int aa = 10;
    int* p = &aa;
    *p = 10 * *p; // *解引用优先级高于乘法*
    printf("%d\n", aa); // 100

    // += 与 /= 优先级相同,按照结合性从右往左计算
    aa /= aa += 2; // <=> aa /= (aa += 2)
    printf("%d\n", aa); // 1
```

计算以下结果(问题表达式:有问题的写法,不推荐):

```c
int a = 1;
int b = (++a) + (++a) + (++a);
printf("%d\n",b);
```

![image-20250324182010016](https://i-blog.csdnimg.cn/img_convert/4be9a1cc4967f11db996c3d386b23f19.png)

```c
    int a2 = 1;
    int b2 = (++a2) + (++a2) + (++a2);
    printf("a2 = %d\n", a2); // 4
    printf("b2 = %d\n", b2); // 12
    int a1 = 1;
    int b1 = (a1++) + (a1++) + (a1++);
    printf("%d\n", a1); // 4
    printf("%d\n", b1); // 3
```

# 六、指针

1. 指针是什么
2. 指针和指针类型
3. 野指针
4. 指针运算
5. 指针和数组
6. 二级指针
7. 指针数组

## 1. 指针是什么

> 指针是什么
>
> 指针理解的2个要点:
>
> 1. 指针是内存中一个最小单元的编号, 也就是地址
> 2. 平时口语中说的指针, 通常指的是指针变量, 是用来存储内存地址的变量

总结: 指针就是地址, 口语中所说的指针通常指的是指针变量.



那我们可以这样理解:

> 内存: [内存](#memory)
>
> 指针变量
>
> 我们可以通过`&`(取地址操作符)取出变量的内存真实地址, 把地址可以存放到一个变量中, 这个变量就是指针变量

```c
    int a = 10; // 在内存中开辟一块空间
    // 这里对变量a,取出它的地址,可以使用&操作符
    // a变量占4个字节,这里是将a的首字节的地址存放到p变量中
    // p是一个指针变量
    int* p = &a;
```

**总结:**

指针变量, 用来存储地址的变量. (存放在指针中的值都被当成地址处理).

那这里的问题是:

- 一个小的单元到底是多大? (一个字节)
- 如何编址?

经过仔细的计算和权衡我们发现一个字节给一个对应的地址是较为合理的.

详情见:[指针变量大小](#pointerVariable)

## 2. 指针和指针类型

我们知道, 变量有不同的类型, 整型, 浮点型等. 那指针有没有类型呢?

准确的说: 有的.

当有这样的代码:

```c
int num = 10;
p = &num;
```

要将&num(num的地址)保存到p中, 我们知道p是一个指针变量, 那么它的类型是什么样的呢?

我们给指针变量相应的类型.

```c
char* pc = null;
short* ps = null;
int* pi = null;
float* pf = null;
double* pd = null;
long* pl = null
```



**总结:**

### 2.1 指针的解引用

**1. 指针变量的类型决定了它被解引用操作的时候到底访问几个字节**

- 如果是 `int*` 的指针, 解引用访问**四个字节**
- 如果是 `char*` 的指针, 解引用访问**一个字节**等等

```c
    // 00010001 00100010 00110011 01000100
    int a = 0x11223344;
    int* pa = &a;
    *pa = 0;

    int b = 0x11223344;
    char* pb = &b;
    *pb = 0;
```

![image-20250324215707621](https://i-blog.csdnimg.cn/img_convert/276dc7f14f77e4e19be7cf81267fbec0.png)

![image-20250324220037331](https://i-blog.csdnimg.cn/img_convert/00545e3027df6b3280321509c8f67f72.png)

### 2.2 指针的+-问题

**2. 指针的类型决定了指针+-1时跳过几个字节**

```c
    int aa = 0x11223344;
    int* paa1 = &aa;
    char* paa2 = &aa;
    printf("%p\n", paa1);     // 00000046D1F7F674 + 4
    printf("%p\n", paa1 + 1); // 00000046D1F7F678
    printf("%p\n", paa2);     // 00000046D1F7F674 + 1
    printf("%p\n", paa2 + 1); // 00000046D1F7F675
```

## 3. 野指针

> 概念: 野指针就是指针指向的位置是不可知的(随机的, 不正确的, 没有明确限制的)

### 3.1 野指针成因

1. 指针未初始化

   ```c
       // 未初始化
       // p没有初始化,就意味着没有明确的指向
       // 一个局部变量不初始化的话,放的是随机值:0xcccccccc
       int* p;
       *p = 10; // 非法访问内存了,这里的p就是野指针
   ```

   

2. 指针越界访问

   ```c
       // 2.指针越界
       int arr[10] = { 0 };
       int* p = arr; // 等价于: &arr[0],数组名就是首地址
       for (int i = 0; i <= 10; i++) {
           *p = i;
           p++; // 这里在 i=10 时p指向的地址arr[10],是未定义的,下标越界了
       }
   ```

3. 指针指向的空间释放

   这里放在动态内存开辟的时候讲解,

## 4. 指针运算

- 指针+-整数
- 指针-指针
- 指针的关系运算

### 4.1 指针+-整数

```c
#define VALUES 5
float values[VALUES];
float* vp;
// 指针的+-整数;指针的关系运算
for(vp = &values[0]; vp < &values[VALUES];) {
	*vp++ = 0;
	// 等价于
	// *vp = 0; vp++;
} // 这段代码就是将数组每个元素设置为0
```

### 4.2 指针-指针

**结论: 指针-指针的绝对值=指针和指针之间元素的个数**

```c
// 指针-指针的绝对值=指针和指针之间元素的个数
int arr[10] = { 0 };
printf("%d\n",&arr[9] - &arr[0]); // 9
printf("%d\n",&arr[0] - &arr[9]); // -9

// 不是所有的指针都能相减
// 指向同一块空间的2个指针才能相减
int arr[10] = {0};
char ch[5] = {0};
printf("%d\n",&arr[5] - &char[0]); // error, 无意义
```

```c
int my_strlen(char *s) {
    char* p = s;
    while(*p != '\0')
        p++;
    return p-s;
}
```

### 4.3 指针的关系运算(比较大小)

```c
// ok,因为vp最终指向了values[0]
for(vp = &values[VALUES];vp > &values[0];) {
    *--vp = 0;
}
```

代码简化,将代码修改为:

```c
// error, 不推荐, vp最终指向了values[-1]
for(vp = &values[VALUES-1]; vp >= &values[0]; vp--) {
    *vp = 0;
}
```

实际在绝大部分的编译器上可以顺利完成任务, 然而我们还是避免这样写, 因为标准并不保证它可行.

**标准规定:**

> 允许指向数组元素的指针与指向数组最后一个元素后面的那个内存位置的指针比较, 但是不允许与指向第一个元素之前的那个内存位置的指针进行比较

## 5. 指针和数组

我们看一个例子:

```c
    int arr[10] = { 1,2,3,4,5,6,7,8,9,0 };
    printf("%p\n", arr);     // 000000C65476F508
    printf("%p\n", &arr[0]); // 000000C65476F508
```

可见数组名和数组首元素的地址是一样的 

结论: 数组名表示的数组首元素的地址. (两种情况除外)

> 1. 作为`sizeof`运算符的操作数时
>
>    `sizeof(数组名)`返回整个数组占用的内存大小，而非指针的大小。
>
>    ```c
>    int arr[5];
>    printf("%zu\n", sizeof(arr));  // 输出 5 * sizeof(int)，而非 sizeof(int*)
>    ```
>
> 2. 作为取地址运算符`&`的操作数时
>
>    `&数组名`返回整个数组的地址，类型为指向数组的指针（而非指向首元素的指针）。
>
>    ```c
>    int arr[5];
>    int (*p)[5] = &arr;  // p的类型是 int(*)[5]，指向整个数组的指针
>    ```
>
>    **关键区别**
>
>    虽然`arr`和`&arr`的地址值相同，但类型不同：
>
>    - `arr`的类型是`int*`（指向首元素的指针）。
>    - `&arr`的类型是`int(*)[5]`（指向包含5个整数的数组的指针）。

那么这样写代码是可行的:

```c
int arr[0] = { 1,2,3,4,5,6,7,8,9,0 };
int* p = arr; // p存放的是数组首元素地址
```

既然可以把数组名当成地址存放到一个指针中, 那么使用指针访问一个数组也成为了可能.

例如:

```c
	int arr[3] = { 1,2,3 };
	int* p = arr; // 存放数组首地址
    for (int i = 0; i < sizeof arr / sizeof arr[0]; i++) {
        printf("&arr[%d] = %p <====> p+%d = %p\n", i, p, i, p + i);
    }
    /*
    &arr[0] = 0000000AC513F5D8 <====> p+0 = 0000000AC513F5D8
    &arr[1] = 0000000AC513F5D8 <====> p+1 = 0000000AC513F5DC
    &arr[2] = 0000000AC513F5D8 <====> p+2 = 0000000AC513F5E0
    */
```

**总结:**

```c
int arr[] = { 1,2,3 };
int* p = arr;

arr[i] <===> *(p + i) // 注意解引用后才等于,两者等价
```

## 6. 二级指针

```c
// 二级指针变量是存放一级指针变量的地址的
    int a = 10;
    // int 代表pa指向的对象a是 int 型
    // * 代表pa是地址
    int* pa = &a;
    // int* 代表ppa指向的对象pa是指针变量int*
    // 第二个*代表ppa是地址
    int** ppa = &pa; // ppa是一个二级指针变量
    *(*(ppa)) = 20;
    printf("%d\n", a); // 20
```

![image-20250325101534866](https://i-blog.csdnimg.cn/img_convert/be3bb824a38754d188aec30c5a9ee361.png)

## 7. 指针数组

指针数组是指针还是数组?

答案: 是数组. **是存放指针的数组**.

```c
    int a = 10;
    int b = 20;
    int c = 30;
    int* parr[10] = { &a,&b,&c }; // parr就是指针数组
    for (int i = 0; i < 3; i++) {
        printf("%d ", *(parr[i])); // 10 20 30
    }
    printf("\n");
    for (int i = 0; i < 3; i++) {
        printf("%d ", *parr[i]); // 10 20 30 括号去掉效果如上
    }
    printf("\n");
```

```c
// 普通的遍历二维数组
    int arr[3][4] = { 1,2,3,4 ,2,3,4,5,3,4,5,6 };
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 4; j++) {
            printf("%d ", arr[i][j]);
        }
        printf("\n");
    }
    /*
    1 2 3 4
    2 3 4 5
    3 4 5 6
    */

    int array1[] = { 1,2,3,4 };
    int array2[] = { 2,3,4,5 };
    int array3[] = { 3,4,5,6 };
    int* parr2[] = { array1,array2,array3 };
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 4; j++) {
            //printf("%d ", parr2[i][j]);
            printf("%d ", *(*(parr2 + i) + j)); // 两者等价
            // (parr2 + i)的返回值是 int** 二级指针
        }
        printf("\n");
    }

    // &arr[0]: 0x00 0x04 0x08
    // &arr[1]: 0x0c 0x10 0x14
    // 二维数组地址连续排列,所以不需要初始化行
    int arr2[][3] = { 0,0,0,0,0,0 };
```

![image-20250325095941828](https://i-blog.csdnimg.cn/img_convert/2e55fcd8eab1163110a76ac9c4ab427d.png)

添加测试