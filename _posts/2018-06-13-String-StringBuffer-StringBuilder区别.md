---
layout: post
title:  " java String, StringBuffer, StringBuilder "
date:   2018-06-13 15:27:56 +0800
categories: java StringBuffer StringBuilder
typora-copy-images-to: ../assets/images/2018-06
typora-root-url: ../../blog_alexcode
---

# java String, StringBuffer, StringBuilder区别

## String

Immutable的 ,被声明为final class, 所有属性也都是final的.  由于Immutable特征, 所以字符的拼接和剪裁都会产生新的String对象. 

```java
public String concat(String var1) {
    int var2 = var1.length();
    if (var2 == 0) {
        return this;
    } else {
        char[] var3 = new char[this.count + var2];
        this.getChars(0, this.count, var3, 0);
        var1.getChars(0, var2, var3, this.count);
        //返回 new 对象
        return new String(0, this.count + var2, var3);
    }
}
```



由于不可变特征, Immutable 对象在拷贝时不需要额外复制数据。



String str1 = "123"; //通过直接量赋值方式，放入字符串常量池 

String str2 = new String(“123”);//通过new方式赋值方式，不放入字符串常量池



```java
String myStr = "aa" + "bb" + "cc" + "dd";
String h ="aabbccdd";

String h2 = new String("aabbccdd");

System.out.println(myStr == h); // true

System.out.println(myStr == h2); //false
```

 

## StringBuffer

是线程安全的可修改类,  保证线程安全的同时也带来额外的性能开销. 

```java
// synchronized 方法
public synchronized StringBuffer append(String var1) {
    super.append(var1);
    return this;
}
```

```java
public AbstractStringBuilder append(String var1) {
    if (var1 == null) {
        var1 = "null";
    }

    int var2 = var1.length();
    if (var2 == 0) {
        return this;
    } else {
        int var3 = this.count + var2;
        if (var3 > this.value.length) {
            this.expandCapacity(var3);
        }

        var1.getChars(0, var2, this.value, this.count);
        this.count = var3;
        return this;
    }
}
```



## StringBuilder

和StringBuffer类似, 去掉了线程安全的部分特性.  StringBuffer和StringBuilder都继承了AbstractStringBuilder

```java
// 没有synchronize 修饰
public StringBuilder append(String var1) {
    super.append(var1);
    return this;
}
```



## 应用场景

[A] 在字符串内容不经常发生变化的业务场景优先使用String类。例如：常量声明、少量的字符串拼接操作等。如果有大量的字符串内容拼接，避免使用String与String之间的“+”操作，因为这样会产生大量无用的中间对象，耗费空间且执行效率低下（新建对象、回收对象花费大量时间）。 



 [B] 在频繁进行字符串的运算（如拼接、替换、删除等），并且运行在多线程环境下，建议使用StringBuffer，例如XML解析、HTTP参数解析与封装。 



 [C] 在频繁进行字符串的运算（如拼接、替换、删除等），并且运行在单线程环境下，建议使用StringBuilder，例如SQL语句拼装、JSON封装等。 



> 本文笔记内容整理自极客时间陈浩