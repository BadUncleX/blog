---
layout: post
title:  " java Exception和Error的区别 "
date:   2018-06-13 15:24:46 +0800
categories: java exception
typora-copy-images-to: ../assets/images/2018-06
typora-root-url: ../../blog_alexcode
---


# java Exception和Error的区别



## Error

Error 是指在正常情况下，不大可能出现的情况，绝大部分的 Error 都会导致程序（比如 JVM 自身）处于非正常的、不可恢复状态。既然是非正常情况，所以不便于也不需要捕获，常见的比如 OutOfMemoryError 之类，都是 Error 的子类。



Error的子类有:

- OutOfMemoryError
- NoClassDefFoundError
- NoSuchMethodError



## Exception

分为:

checked Exception: 编译期间需要检查, 必需捕获

unchecked Exception, 即运行时异常, 可以不用捕获. 

- NullPointerException
- ArrayIndexOutOfBoundsException



## 异常设计

![](/assets/images/2018-06/2018-06-13-030941.jpg)



## NoClassDefFoundError 和 ClassNotFoundException 的区别

### NoClassDefFoundError:

编译的时候正常, 运行时找不到类, 可能是classPath的问题. 也可能是类初始化的时候出问题. 



```java
public class ClassWithInitErrors {
    // 致命错误
    static int data = 1 / 0;
}
```

The error occurs when a compiler could successfully compile the class, but Java runtime could not locate the class file. It usually happens when there is an exception while executing a static block or initializing static fields of the class, so class initialization fails.



```java
public class NoClassDefFoundErrorExample {
    public ClassWithInitErrors getClassWithInitErrors() {
        ClassWithInitErrors test;
        try {
            //调用一个依赖的class, 但这个class初始化出错
            test = new ClassWithInitErrors();
        } catch (Throwable t) {
            System.out.println(t);
        }
        test = new ClassWithInitErrors();
        return test;
    }
}
```

```java
@Test(expected = NoClassDefFoundError.class)
public void givenInitErrorInClass_whenloadClass_thenNoClassDefFoundError() {
  
    NoClassDefFoundErrorExample sample
     = new NoClassDefFoundErrorExample();
    sample.getClassWithInitErrors();
}
```





### ClassNotFoundException 

*ClassNotFoundException* is a checked exception which occurs when an application tries to load a class through its fully-qualified name and can not find its definition on the classpath.

This occurs mainly when trying to load classes using **Class.forName()**, **ClassLoader.loadClass()** or **ClassLoader.findSystemClass()**. Therefore, we need to be extra careful of *java.lang.ClassNotFoundException* while working with reflection.

For example, let’s try to load the JDBC driver class without adding necessary dependencies which will get us *ClassNotFoundException:*

```java
@Test(expected = ClassNotFoundException.class)
public void givenNoDrivers_whenLoadDriverClass_thenClassNotFoundException() 
  throws ClassNotFoundException {
      Class.forName("oracle.jdbc.driver.OracleDriver");
}
```



### Solution

1. We need to make sure whether class or jar containing that class is available in the classpath. If not, we need to add it. (类或者jar包丢失)
2. If it’s available on application’s classpath then most probably classpath is getting overridden. To fix that we need to find the exact classpath used by our application. (类路径被覆盖)
3. Also, if an application is using multiple class loaders, classes loaded by one classloader may not be available by other class loaders. To troubleshoot it well, it’s essential to know how classloaders work in Java (classloader冲突)





##  try-with-resources Statement关闭资源

```java
static String readFirstLineFromFile(String path) throws IOException {
    try (BufferedReader br =
                   new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```

实现java.lang.AutoCloseable接口的都可以. 



或者try finally方式

```java
static String readFirstLineFromFileWithFinallyBlock(String path)
                                                     throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        if (br != null) br.close();
    }
}
```



## 一些原则

### 不要捕获通用异常

```java
try {
  // 业务代码
  // …
  Thread.sleep(1000L);
} catch (Exception e) {//不过了Exception异常, 偷懒行为
  // Ignore it
}
```



### 不要生吞异常

```java
try {
   // 业务代码
   // …
} catch (IOException e) {
    //只打印到控制台不易最终
    e.printStackTrace();
}
```



### Throw early, catch late

```java
public void readPreferences(String filename) {
    //throw early, 就是之前说的判空处理
    Objects.requireNonNull(filename);
    //...perform other operations... 
    InputStream in = new FileInputStream(filename);
     //...read the preferences file...
}
```



### 只try有异常的部分

try-catch 代码段会产生额外的性能开销，或者换个角度说，它往往会影响 JVM 对代码进行优化，所以建议仅捕获有必要的代码段，尽量不要一个大的 try 包住整段的代码；与此同时，利用异常控制代码流程，也不是一个好主意，远比我们通常意义上的条件语句（if/else、switch）要低效。



