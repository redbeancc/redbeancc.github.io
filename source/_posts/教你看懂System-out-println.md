---
title: '教你看懂System.out::println'
tags:
  - java
  - lambda
categories:
  - 学习
description: "\U0001F384 看懂方法引用 lambda"
abbrlink: 223b52e1
date: 2022-11-09 10:29:46
cover:
---
# 教你看懂System.out::println
# 方法引用 ::
在不经意间， 我们会看到这样的代码

        // 创建出一个数组
        List<String> strList = Arrays.asList("YangHang", "AnXiaoHei", "LiuPengFei");
    
        strList.forEach(System.out::println);



第一印象， 哇， 好高大上的写法， 那么这究竟是怎样的一种语法呢。
我们一步一步来探究：
首先， 我们看一下是<span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">java.lang.Iterable<T></span>下的一个默认方法<span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">forEach</span>调用的， 好家伙， 一看到这个function包下面的被<span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">@FunctionalInterface</span>注解声明的<span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">Consumer<T></span>接口， 瞬间就了然了， 这不又是函数式编程搞的鬼么?
现在的问题应该很明朗了， <span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">System.out::println</span>这段代码其实就是<span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">Consumer<T></span>接口的一个实现方式啊。 具体是怎么实现的， 我们再码一段代码。

    @Test
    public void testDemo2() {
        List<String> strList = Arrays.asList("YangHang", "AnXiaoHei", "LiuPengFei");
    
        strList.forEach(x -> {
            System.out.println(x);
        });
    }



然后， 我们惊喜的发现和上面的代码运行结果是一制的， 我们基本上可以断定， 上面那种写法是下面这种的一种缩写形式。 就是把你遍历出来的每一个对象都用来去调用System.out（也就是PrintStream类的一个实例）的println方法。
最后， 大家是不是有一个想法， 想自己写一个<span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">Consumer<T></span>接口的实现类， 让<span style="color: #c7254e;background-color: #f9f2f4;border-radius: 2px;">foreach</span>调用一下。

```java
@Data
public class Student {
    private String name;
    private Integer age;

    public Student() {

    }
    
    public Student(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + ", " + age;
    }

    public void print(Student student) {
        System.out.print(student.getName() + ", " + student.getAge() + " \t");
    }
}
```

然后测试一下

```java
public class Case2 {
    public static void main(String[] args) throws IOException {
        Student[] stus = {
                new Student("张三",15),
                new Student("李四",16),
                new Student("王五",17),
                new Student("赵六",18),
        };
        List<Student> list = Arrays.stream(stus).collect(Collectors.toList());
//        List<Student> list = new ArrayList<>();
//        Collections.addAll(list, stus);   // result |= c.add(element); 位或运算，数据量大时高效。

        System.out.println("lambda表达式输出：");
        list.forEach(s -> System.out.print(s + " \t"));
        System.out.println();
        System.out.println("方法引用输出：");
//        list.forEach(System.out::print);
        list.forEach(new Student()::print);
    }
}
```

运行一下

```shell
lambda表达式输出：
张三, 15 	李四, 16 	王五, 17 	赵六, 18 	
方法引用输出：
张三, 15 	李四, 16 	王五, 17 	赵六, 18 
```

但是我发现， 如果是静态方法的时候必须得用类名双冒号静态方法， 注意就好。

