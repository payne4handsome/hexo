---
title: 关于java Integer的一个小问题
comments: true
date: 2017-08-19 16:43:10
categories: java snippet
tags: Java Integer
---

下面这段代码片段的输出是什么？
```
	Integer i1 = 127,i2 = 127,i3=128,i4=128;
    System.out.println(i1==i2);
    System.out.println(i1.equals(i2));
    System.out.println(i3==i4);
    System.out.println(i3.equals(i4));
```
答案如下
```
true
false
true
true
```
为什么是这样呢？首先equals方法我们是可以理解的，应为比较的值嘛，所以i1和i2都是等的，但是java中的“==”是比较的地址，为什么i1和i2是等的，i3和i4又是不等的呢？我们通过反编译程序把上面代码的字节码反编译一下，看看得到是什么  

```
   Integer i1 = Integer.valueOf(127);Integer i2 = Integer.valueOf(127);Integer i3 = Integer.valueOf(128);Integer i4 = Integer.valueOf(128);
    System.out.println(i1 == i2);
    System.out.println(i1.equals(i2));
    System.out.println(i3 == i4);
    System.out.println(i3.equals(i4));
```   

我们在看看valueOf的源码

```java
 public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```  

我们可以看到如果i的值是>=-128并且是<=127的（low，high分别是-128和127），那么java久就从一个缓冲里面读取数据，这就可以解释我们上面存在的问题呢。其实这么缓存是指也是可以通过jvm参数进行修改的