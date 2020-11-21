---
title: ARTS补卡第三周
date: 2019-04-16 21:20:29
categories: "Java基础"
tags: "ARTS"
---

上周工作任务很重，周日没来得及打卡，今天补上。真心希望忙完这一阵可以轻松点，不然真的撑不下去了。

<!--more-->

## Algorithm

> 237.删除链表中的结点

本来是一道很简单的题，没有理解清楚变量`x`的含义，结果导致使用了递归还是没有做出来，很可惜；

贴上最后的解法：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public void deleteNode(ListNode node) {
        ListNode nextNode =node.next;
        node.val = nextNode.val;
        node.next = nextNode.next;
    }
}
```

## Review

continue reading 《effective java》.

- Item 5 : Prefer Dependency injection to hardwiring resources.

  > 1. static utility classes and singletons are inappropriate for classes whose behavior is parameterized by an underlying resource.
  > 2. a simple pattern that satisfies that requirement is to pass the resource into the constructor when creating instance. This is one form of Dependency injection .
  > 3. Do not use a singleton or static utility class to implements a class that depends on one or more   underlying resource whose  behavior affects that of class. And do not have the class create these resource.Instead, pass the resources,or factories to create them, into the constructor.

- Item 6 : Avoid creating unnecessary object.

  > 1. you can often avoid creating unnecessary object by using `static factory method` in preference to constructor on immutable classes that provide both.like use `String a= "banana"` ,not `String a = new String("banana")`. Besides,`Boolean(String)` is deprecated in Java 9,cause it must create the instance each time it's invoked.The factory method `Boolean.valueOf(String)` is preferable to the constructor. 
  > 2. you can also reuse the **mutable **object if you know it won't be modified. Some object creations are so expensive , cache it for reuse will provides significant performance gains if they are invoked frequently. if the object is immutable , it is obvious it can be reused safely.
  > 3. Another way to create unnecessary object is **autoboxing** .we should prefer primitives to boxed primitives , and watch out unintentional autoboxing.
  > 4. Conversely,avoid object creation by maintaining you own object pool is a bad idea,unless the object is extremely heavyweight.

## Tips

这周在工作中遇到一个问题，当我试图把一个很大的文件通过流转换为`String`存储时，这个转换有时候会出错，报出`OutOfMemoryError:null`的错误。随即我开始调整内存大小，但是无论我怎么调整都没有用。

当我观察后发现，错误信息的后面是`null`而不是通常的堆栈溢出等。于是我开始跟踪源码，发现当我用流读进来后，用`StringBuilder`去拼接成字符串。然而`String`的字符数组的最大长度为：$$2^{31}=2^{10}\times2^{10}\times2^{10}\times2^{1}=1024\times1024\times1024\times2$$，即2GB。而我的文件刚好大于2GB。所以之前我在转其他文件的时候没有报错是由于文件小于2GB。此处的`OOM`错误是`StringBuilder`抛出的，而不是由于堆内存 不够导致的内存溢出。

## Share

无
