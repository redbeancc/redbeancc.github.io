---
title: C语言编程设计
tags:
  - c
categories:
  - 学习
description: "\U0001F957对 C 语言编程设计的简单学习"
abbrlink: 9258bace
date: 2022-04-08 10:48:55
cover:
---
## C语言编程设计

### 1. C语言程序的结构

####  1.1 程序的构成，main函数和其他函数。

​	C语言是由**<u>函数</u>**构成的

​	main函数是启动函数，程序的开始就是从main函数开始，**有且只有一个**

####  1.2 头文件，数据说明，程序注释。

3. 程序注释

   //双斜杠：单行注释

   /**/：多行注释

####  1.3 源程序的书写格式。

​	一行一句，分号结尾，**\反斜杠**代表**续行符**

### 2. 数据类型及其运算

####  2.1 C语言的基本数据类型及定义方法。

​	基本数据类型

- char：字符型，占用一个字节
- int：整型，通常反映所用机器中整数的最自然长度，四个字节
- float：单精度浮点型，四个字节
- double：双精度浮点型，八个字节
- _Bool：布尔类型，一个字节

​     定义方法

- 数据类型 变量名
  - int a，unsigned int a
  - char b，unsigned char b
  - float c
  - double d

​    占位符（输出控制符）

- %d：整数值，d表示十进制
- %x：x表示十六进制
- %u：无符号整型（unsigned）
- %c：字符型
- %s：字符串型
- %f：浮点型

**<u>注意</u>**：%.2f中的2是精确到小数点后二位，**编译器会自动四舍五入**，整数部分有几位就打印出来几位

```c
#include <stdio.h>
int main()
{
    double a = 5553.1415926;
    double b = 3.145;
    printf("它有：%.2f",a);
    printf("四舍五入：%.2f",b);
    return 0;
}
// 它有：5553.14
// 四舍五入：3.15
```

如果是%8.2f：当中8是总宽度(总宽度：小数部分+整数部分+1)，包含小数部分和整数部分，如果大于显示的数，则用空格表示

```c
#include <stdio.h>
int main()
{
    double a = 5553.1455926;
    printf("%8.2f",a);
    return 0;
}
// 空格5553.15
```

​	在整数型中有**带符号**和**不带符号（unsigned）**，默认都是带符号整型，**unsigned没有负数**

####  2.2 C语言运算符的种类、运算优先级和结合性。

​	运算符的种类：<u>**算术 > 关系 > 逻辑 > 赋值 > 逗号**</u>

1. 赋值运算符：=       **优先级第二低，仅高于逗号运算符**

2. 算术运算符：+、-、*、/、%、++、--、**、()、！<b>（取非,不是取反）</b>、sizeof、~

   - 第一优先级：（）
   - 第二优先级：++、--、**、！、sizeof(长度运算符)、~(取反运算符)    <b><u>注意：此类运算符运算规则从右向左</u></b>
   - 第三优先级：*、/、%(取模，必须为**整型**运算对象)
   - 第四优先级：+、-

3. 关系运算符：比较两数大小来判断真假

   - |  <   | （相同）优先级高 |
     | :--: | :--------------: |
     |  <=  | （相同）优先级高 |
     |  >   | （相同）优先级高 |
     |  >=  | （相同）优先级高 |
     |  ==  | 优先级低（相同） |
     |  !=  | 优先级低（相同） |

   - 比算术运算符低，比赋值运算符高

4. 逻辑运算符：判断真假

   - | 运算符 |  含义  | 优先级 |  举例  |                             说明                             |
     | :----: | :----: | :----: | :----: | :----------------------------------------------------------: |
     |   ！   | 逻辑非 |   高   |   !a   |          如果a为真，!a为假；<br />如果a为假，!a为真          |
     |   &&   | 逻辑与 |   中   |  a&&b  | 只有a和b同时为真，结果为真；<br />a或b有一个为假，结果为假的 |
     |  \|\|  | 逻辑或 |   低   | a\|\|b | 只要a或b有一个为真，结果为真；<br />只有a和b同时为假，结果为假 |

5. 条件运算符：条件?真:假

   - 也叫三目运算符，自右向左运算
   - 低于关系运算符和算术运算符，但高于赋值运算符

6. 逗号运算符：,(自左向右顺序计算)；**<u>表达式的值是最后一个表达式的值</u>**，<u>***优先级最低***</u>

####  2.3 不同类型数据间的转换与运算。

​	当整型和浮点型计算，自动都转换为浮点型(字节比较大的)计算，例如：1+2.0 ==> 1.0+2.0

```c
#include <stdio.h>
int main(){

    int a = 1;
    float b = 2.0;
    char c = 'a';
    int d = 97;

    printf("整型结果：%d\n",a + b);
    printf("强制整型：%d\n",a + (int)b);
    printf("浮点型结果：%f\n",a + b);
    printf("char类型强制转换为整型：%d\n",(int)c);
    printf("整型强制转化为char：%c\n",(char)d);
    return 0;
}

/*
整型结果：0
强制整型：3
浮点型结果：3.000000      
char类型强制转换为整型：97
整型强制转化为char：a 
*/
```

####  2.4 C语言表达式类型（赋值表达式，算术表达式，关系表达式，逻辑表达式，条件表达式，逗号表达式）和求值规则。

- 求值规则：<u>**算术 > 关系 > 逻辑 > 赋值 > 逗号**</u>

  ```c
  #include <stdio.h>
  int main() {
  
      // 赋值表达式
      int a = 0,b = 8;
  
      // 算术表达式
      int c = (!a + b++) % 6;     // 这次运算过后 ==> b = 9
      printf("算数表达式：%d\n",c);
      /*
      算数表达式：3
      */
  
      // 条件表达式，条件表达式只有0或1，表示真和假
      _Bool d = a < b;
      printf("条件表达式：%d\n",d);
      printf("条件表达式(6>=7)：%d\n",6>=7);
      printf("条件表达式(33<=66)：%d\n",33<=66);
      printf("条件表达式(108==108)：%d\n",108==108);
      /*
      条件表达式：1
      条件表达式(6>=7)：0
      条件表达式(33<=66)：1
      条件表达式(108==108)：1
      */
  
      // 逻辑表达式，判断真假，是否满足条件
      // 逻辑表达式两边都得是逻辑值（即0或1），但是不为0的都是真
      printf("逻辑表达式(3 > 1 && 1 < 2)：%d\n",3 > 1 && 1 < 2);
      printf("逻辑表达式(3 + 1 || 2 == 0)：%d\n",3 + 1 || 2 == 0);
      printf("逻辑表达式(!0 + 1 < 1 || !(3 + 4))：%d\n",!0 + 1 < 1 || !(3 + 4));
      printf("逻辑表达式('a' - 'b' && 'c')：%d\n",'a' - 'b' && 'c');
      /*
      逻辑表达式(3 > 1 && 1 < 2)：1
      逻辑表达式(3 + 1 || 2 == 0)：1
      逻辑表达式(!0 + 1 < 1 || !(3 + 4))：0
      逻辑表达式('a' - 'b' && 'c')：1
      */
  
      // 短路求值：只有第一个运算的逻辑值无法判断结果，才对第二个运算数进行求职
      // 只要不满足，我就不继续往下判断了
      int first = 3,second = 3;
      (first = 0) && (second = 5);
      printf("短路求值((first = 0) && (second = 5))：first = %d, second = %d\n",first,second);
      (first = 1) || (second = 5);
      printf("短路求值((first = 1) || (second = 5))：first = %d, second = %d\n",first,second);
      /*
      短路求值((first = 0) && (second = 5))：first = 0, second = 3
      短路求值((first = 1) || (second = 5))：first = 1, second = 3
      */
  
      // 条件运算符，及三目运算符（condition？1：0）
      printf("5 > 6的结果是%d\n",5>6 ? 1 : 0);
      printf("5 < 6的结果是%d\n",5<6 ? 1 : 0);
      /*
      5 > 6的结果是0
      5 < 6的结果是1
      */
  
      // 逗号表达式，顺序计算，最后一个表达式是最终表达式的值，优先级最低，比赋值还低
      printf("(3+4),(5+6)的值是：%d\n",((3 + 4),(5 + 6)));
      /*
      (3+4),(5+6)的值是：11
      */
  
      int x,y,z;
      x = 1;
      y = 1;
      z = x++,y++,++y;
      printf("x=%d,y=%d,z=%d",x,y,z);
      /*
      x=2,y=3,z=1
      因为逗号标识符优先级小于赋值，所以最后z可以看作    (z = x++),y++,++y
      */
      return 0;
  }
  ```

### 3. 基本语句

####  3.1 表达式语句，空语句，复合语句。

- 在表达式的后边加一个分号“；”就构成了表达式语句，例如  a = a + 1;

- 在C语言中或C++中，如果一个语句只有一个分号“;”，则称该语句为空语句。简单来说，就是没有执行代码，只有一个语句结束的标志“;”分号。

  空语句是什么都不执行的语句。在程序中，空语句主要用来做空循环体，如：while(getchar()!='\n');这个语句的功能是，只要从键盘输入的字符不是回车，则要求用户重新输入。即要求用户回车后才会继续后面的程序。在该部分代码中，接收用户按键，判断按键的内容都集中在while判断中，因此，循环体中不再需要执行任何功能。就在循环体中，输入一个空语句作为循环体。

- 复合语句是由若干条语句组合而成的一种语句，在C51中，用一个大括号“{  }”将若干条语句括在一起就形成了一个复合语句，复合语句最后不需要以分号“；”结束，但它内部的各条语句仍需以分号“；”结束。复合语句的一般形式为：

  {

  局部变量定义；

  语句l；

  语句2；

  }

####  3.2 输入输出函数的调用，正确输入数据并正确设计输出格式。

- **scanf中变量名前记得+<u>*&*</u>**，例如scanf("%d",**&**age);

  ```c
  #include <stdio.h>
  int main(){
      
      char name[5];
      int age;
  
      printf("请输入你的名字：");
      scanf("%s",&name);
      printf("请输入你的年龄：");
      scanf("%d",&age);
  
      if (age >= 18)
      {
          printf("恭喜您！%s,您已成年，今年%d岁了。",name,age);
      }
      else
      {
          printf("抱歉，%s，您未成年，还差%d岁成年。",name,18-age);
      }
      return 0;
  }
  
  /*
  请输入你的名字：cx
  请输入你的年龄：16
  抱歉，cx，您未成年，还差2岁成年。
  */
  
  /*
  请输入你的名字：cx
  请输入你的年龄：18
  恭喜您！cx,您已成年，今年18岁了。
  */
  ```

### 4. 选择结构程序设计

####  4.1 用if语句实现选择结构。

```c
#include <stdio.h>
int main()
{
    int score = 0;
    printf("请输入分数:");
    scanf("%d",&score);
    if (score >= 90)
    {
        printf("您的评价为：A");
    }
    else if (score >=80 && score < 90)
    {
        printf("您的评价为：B");
    }
    else if (score >= 60 && score < 80)
    {
        printf("您的评价为：C");
    }
    else
    {
        printf("您的评价为：D");
    }
    return 0;
}

/*
请输入分数:100
您的评价为：A
请输入分数:80
您的评价为：B
请输入分数:60
您的评价为：C
请输入分数:59
您的评价为：D
*/
```

####  4.2 用switch语句实现多分支选择结构。

```c
#include <stdio.h>
int main(){
    char score = 'z';
    printf("请输入成绩：");
    scanf("%c",&score);
    switch (score) {
        case 'A' : printf("您的分数在90分以上"); break;
        case 'B' : printf("您的分数在80分到90分之间"); break;
        case 'C' : printf("您的分数在60分到80分之间"); break;
        case 'D' : printf("您的分数在不足60分"); break;
        default : printf("请输入正确的成绩"); break;
    }
    return 0;
}

/*
请输入成绩：A
您的分数在90分以上
请输入成绩：a
请输入正确的成绩
*/
```

####  4.3 选择结构的嵌套。

- 嵌套，顾名思义，就是if语句里再套if

  ```c
  #include <stdio.h>
  int main(){
      int a,b;
      printf("请输入任意两个整数(中间用空格或回车隔开)：");
      scanf("%d %d",&a,&b);
      if (a != b ){
          if (a > b){
              printf("%d大于%d",a,b);
          }
          else if (a < b){
              printf("%d小于%d",a,b);
          }
      }
      else{
          printf("%d等于%d",a,b);
      }
      return 0;
  }
  
  /*
  请输入任意两个整数(中间用空格或回车隔开)：16 16
  16等于16
  
  请输入任意两个整数(中间用空格或回车隔开)：12
  16
  12小于16
  
  请输入任意两个整数(中间用空格或回车隔开)：16 12
  16大于12
  */
  ```

### 5. 循环结构程序设计

####  5.1 for循环结构。

```c
#include <stdio.h>
int main(){
    for(int i = 1; i < 10; i++){
        for(int j = 1; j <= i; j++){
            printf("%d×%d=%d\t",j,i,i*j);
        }
        printf("\n");
    }
    return 0;
}
/*
1×1=1
1×2=2   2×2=4
1×3=3   2×3=6   3×3=9
1×4=4   2×4=8   3×4=12  4×4=16
1×5=5   2×5=10  3×5=15  4×5=20  5×5=25
1×6=6   2×6=12  3×6=18  4×6=24  5×6=30  6×6=36
1×7=7   2×7=14  3×7=21  4×7=28  5×7=35  6×7=42  7×7=49
1×8=8   2×8=16  3×8=24  4×8=32  5×8=40  6×8=48  7×8=56  8×8=64
1×9=9   2×9=18  3×9=27  4×9=36  5×9=45  6×9=54  7×9=63  8×9=72  9×9=81
*/
```

####  5.2 while和do-while循环结构。

- while循环：先判断，后循环

  ```c
  #include <stdio.h>
  int main(){
      int count = 1;
      int sum = 0;
      while (count <= 100){
          sum = sum + count;
          count = count + 1;
      }
      printf("从1加到100的结果是：%d\n",sum);
  
      // 例二
      count = 0;
      printf("请输入字符串：");
      while (getchar() != '\n'){
          count = count + 1;
      }
      printf("您一共输入了%d个字符！",count);
      return 0;
  }
  /*
  从1加到100的结果是：5050
  请输入字符串：你好
  您一共输入了4个字符！
  */
  
  /*
  从1加到100的结果是：5050
  请输入字符串：nihao
  您一共输入了5个字符！
  */
  ```

- do-while循环：先循环，后判断；记得while语句后有**分号结尾**

  ```c
  #include <stdio.h>
  int main(){
      // do-while循环,有分号结尾
      int password;
      do {
          printf("请输入密码：");
          scanf("%d",&password);
      }while (password != 521);
      return 0;
  }
  /*
  请输入密码：55
  请输入密码：521
  */
  ```

####  5.3 continue语句和break语句。

- break语句跳出循环，注意只跳出本层循环，外层循环需要再在外层break
  - 终止循环
- continue：跳过本次循环，不对循环进行终止

### 6. 数组的定义和引用

####  6.1 一维数组的定义、初始化和数组元素的引用。

1. 一维数组的定义：可变，意思是在定义元素个数时是动态的，定义完成后不可修改
   - 类型 数组名[元素个数]
   - int a[6];
   - char n[33];
2. 初始化：如果不赋值，默认值为0
   - int a[10] = {1,2,3,4,5,6,7,8,9,0};		// 1 2 3 4 5 6 7 8 9 0
   - int b[10] = {1,2,3,4,5}                         // 1 2 3 4 5 0 0 0 0 0 
   - 可以不表明数组长度，int a[] = {1,2,3,4,5}，长度自动计算为5 
3. 引用：
   - 数组名[下标]	的格式进行引用
   - int a[]={1,2}  -->  a[1] ==> 2

####  6.2 二维数组的定义、初始化和数组元素的引用。

1. 二维数组的定义

   - 数据类型	数组名\[常量表达式][常量表达式]
   - int a\[6][6];		//6行6列
   - char b\[8][8];	//8行8列

2. 初始化：**只有第一维度的数值可以不写，其它维度的数值必须写上**

   - int a\[][6] = {

     ​	{1,2,3,4,5,6},

     ​	{7,8,9,10,11,12}

     }			//	这里第一维度自动表示为2，第二个维度必须写上

3. 数组元素的引用

   - 数组名\[下标][下标]
   - a\[0][0]      //  访问a数组中第一行第一列的数据

####  6.3 字符串与字符数组。

​	字符串中结尾有一个\0结尾，表示读取到\0结束

​	其中，sizeof和strlen的不同在于，sizeof的结果包含了\0，strlen不包含结尾字符\0

```c
#include <stdio.h>
#include <string.h>
int main()
{
    char a[] = "nihao";
    // char a[] = "nihao";   ==>  char a[] = {'n','i','h','a','o','\0'}
    printf("英文字符串占字节：%d\n",sizeof(a));
    printf("英文字符串长度(单位：字节)：%d\n",strlen(a));

    char b[] = "你好世界";
    printf("中文字符串占字节：%d\n",sizeof(b));
    printf("中文字符串长度(单位：字节)：%d\n",strlen(b));
    return 0;
}
/*
英文字符串占字节：6
英文字符串长度(单位：字节)：5
中文字符串占字节：9
中文字符串长度(单位：字节)：8
*/

// 从结果可以看出一个中文占两个字符，英文占一个字符，结尾字符\0也占一个字符
```

- 字符串处理函数
  - 获取字符串的长度：strlen
  - 拷贝字符串：strcpy和strncpy
  - 连接字符串：strcat和strncat
  - 比较字符串：strcmp和strncmp

####  6.4 使用一维数组解决批量数据查找或排序等问题。

- 快速排序：分而治之的思想，先找中间量，然后比中间量大的放在中间量后面，比中间量小的放在中间量前面；然后采用递归，依次排序，直到最后后下标到0和前下标到最后

  ```c
  #include <stdio.h>
  void quick_sort(int arr[], int left, int right){
      int i = left,j = right;
      int pivot = arr[(left + right) / 2];
      int temp = 0;
  
      while (i <= j) {
          while (arr[i] < pivot){
              i++;
          }
          while (arr[j] > pivot){
              j--;
          }
          if (i <= j){
              temp = arr[i];
              arr[i] = arr[j];
              arr[j] = temp;
              i++;
              j--;
          }
      }
      if (left < j){
          quick_sort(arr,left,j);
      }
      if (i < right){
          quick_sort(arr,i,right);
      }
  }
  int main(){
      int arr[] = {153,261,352,321,46,398,125,526,23,568,521,123};
      int length = sizeof(arr) / sizeof(arr[0]);
      quick_sort(arr,0,length-1);
      printf("排序后的顺序：");
      for (int i = 0; i < length; i++){
          printf("%d ",arr[i]);
      }
      return 0;
  }
  // 排序后的顺序：23 46 123 125 153 261 321 352 398 521 526 568 
  ```

- 冒泡排序：数值像泡泡一样从小到大，一点一点浮现出来；两个相近的数据进行比较，大的调到后面，然后在和后面的进行比较，如果前面那个数值大就和后面的数据进行互换。

  - 注意的是，如果十个数，排九次即可，所以j < length -1
  - 每进行一次冒泡，下次排序就可以把最后的排除，就是减去排的次数，所以i < length - j
  - 第一次排序注意下标越界，所以i < length-1

  ```c
  #include <stdio.h>
  void main(){
      int arr[] = {236,251,623,20,15,365,23,568,124,521,30,128,128};
      int length = sizeof(arr) / sizeof(arr[0]);
      int temp;
      for (int j = 0; j < length-1; j++){// 
          for (int i = 0; i < length-1-j; i++){
              if (arr[i] > arr[i + 1]){
                  temp = arr[i];
                  arr[i] = arr[i + 1];
                  arr[i + 1] = temp;
              }
          }
      }
      for (int i = 0; i < length; i++){
          printf("%d ",arr[i]);
      }
  }
  // 15 20 23 30 124 128 128 236 251 365 521 568 623
  ```

- 二分查找法：利用查找数和中间数进行比较，比中间数大的，中间数左面的就不要了，继续比对右面的中间数，看看是比右面的中间数大还是小，再舍弃一半，最后确定是否查找数是否存在（**利用下标**）

  ```c
  #include <stdio.h>
  int binary_search(int arr[], int left, int right, int search){
      while (left <= right){// 这里等于号确保数字在最开始或者最末尾的时候
          int middle = (left + right) / 2;
          if (search == arr[middle]){
              return middle;
          }
          else if (search > arr[middle]){
              left = middle + 1;
          }
          else if (search < arr[middle]){
              right = middle - 1;
          }
      }
      return -1;
  }
  
  void main(){
      int arr[] = {8,12,16,23,36,38,45,46,52,59,65,66,68};
      printf("请输入一个数：");
      int num = 0;
      scanf("%d",&num);
      int length = sizeof(arr) / sizeof(arr[0]);
      int result = binary_search(arr,0,length-1,num);
      printf("查找结果为：第%d个下标",result);
  }
  /*
  请输入一个数：8
  查找结果为：第0个下标
  
  请输入一个数：66
  查找结果为：第11个下标
  
  请输入一个数：3
  查找结果为：第-1个下标
  */
  ```

### 7. 函数定义与调用

####  7.1 库函数的正确调用。

​	#include <函数库名>

####  7.2 用户自定义函数的定义、类型与返回值。

```
返回的数据类型 自定义函数名(参数值){
	函数体;
}
例如：
void print_x(){
	printf("#	#\n");
	printf(" # #\n");
	printf("  #\n");
	printf(" # #\n");
	printf("#	#\n");
}
注意：如果不写返回类型，默认值为int
```

####  7.3 函数的正确调用、参数的值传递。

​		正确调用：自定义函数要写在main函数之前，如果写在之后则应在main函数之前**声明**

```c
#include <stdio.h>
void print_x();	// 这就是声明，没有函数体
int main(){
    print_x(2);
    return 0;
}
void print_x(int count){
    for(int i = 0; i < count; i++){
	    printf(" # #\n");
	    printf("  #\n");
	    printf(" # #\n");
        printf("=====================\n");
    }
}
/*
 # #
  #
 # #
=====================
 # #
  #
 # #
=====================
*/
```

####  7.4 函数的嵌套调用、递归调用。

```c
#include <stdio.h>
int factorial(int max){
    if (max == 1){
        return 1;
    }
    return max * factorial(max - 1);
}

int main(){
    int count = 0;
    printf("请输入阶乘数：");
    scanf("%d",&count);
    printf("结果为：%d",factorial(count));
    return 0;
}
/*
请输入阶乘数：5
结果为：120
*/
```

####  7.5 局部变量与全局变量。

​	不同函数的变量无法相互访问

​	在函数里面定义的叫局部变量，在函数外面定义的叫全局变量，全局变量默认初始化为0

​	如果在局部变量中有和全局变量名字相同的，编译器不会报错，会暂时屏蔽全局变量

​	如果函数调用全局变量在定义全局变量之前，则可以使用关键字extern，告诉编译器，我后面定义了，别忙报错

```c
#include <stdio.h>
void a(){   // 调用在全局变量定义之前
    extern global; // 后面定义了，此时不要报错
    global++;
    printf("IN a,blobal = %d\n",global);
}
int global = 6;     // 全局变量
int undefind_global;// 未定义的全局变量
void b(){
    undefind_global++;
    printf("IN b,undefind_global = %d\n",undefind_global);
}
int main(){
    // 测试全局变量默认值
    printf("IN main,undefind_global = %d\n",undefind_global);
    b();
    // 调用在全局变量定义之前
    a();
    return 0;
}
/*
IN main,undefind_global = 0
IN b,undefind_global = 1
IN a,blobal = 7
*/
```

