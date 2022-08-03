---
title: "Java中的final"
date: 2022-08-26T12:04:49+08:00
draft: false
categories:
- 
tags:
- Java
searchHidden: false
---

## 修饰类

不可被继承

## 修饰方法

不可被覆盖

特别地, 对于private修饰的父类方法, 由于子类接触不到, 可以写同名同参的方法. 此时相当于在子类中重新定义了新的方法.  
*private方法会被隐式地指定为final*

## 修饰变量

final修饰变量时, 变量内容只能被赋值一次, 赋值后不再改变.
- 对基本数据类型: 值在初始化后不再改变.
- 对引用类型: 值即地址, 所以只能引用该对象. 但仅限地址不改变, 所以对象的内容可以发生变化.

### final变量的编译器优化

```
public class Test {
    public static void main(String[] args) {
        String a = "helloworld";
        final String b = "hello";
        String c = "hello";
        String x = b + "world";
        String y = c + "world";
        System.out.println(a == x);
        System.out.println(a == y);
    }
}

>>> true
>>> false
```

### 修饰参数时的情况

> 参考文献  
> [final关键字用法总结](https://blog.csdn.net/riemann_/article/details/86618495)