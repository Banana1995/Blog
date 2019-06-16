---
title: ARTS打卡第九周
date: 2019-06-16 16:28:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第八周

本次ARTS主要包括：合并两个有序数组算法题；《effective java》 中关于类的访问权限设置；vim中标记跳转的用法；分享关于java中锁的底层实现机制。

<!--more-->

## Algorithm

> 88. 合并两个有序数组

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        if(m==0&&n==0){
            return;
        }
        for(int i=m+n-1;i>=0;i--){
            int a=0;
            int b=0;
            if(m>0){
               a= nums1[m-1]; 
            }else{
               nums1[i]=nums2[n-1];
               n--;
                continue;
            }
               if(n>0){
                 b= nums2[n-1];  
               } else{
                  nums1[i]=nums1[m-1];
                   m--;
                    continue;
               }
           if(a>b){
               nums1[i] = a;
               m--;
           }else{
               nums1[i] = b;
               n--;
           }
        }
    }
    
}
```

这题我是看了题解的思想后，再自己写的代码出来的。刚开始一直想着用类似合并有序链表的方式，用递归来做。但是一直没有弄出来递推表达式，后来看了题解，都没有用递归的。于是采用了从后往前遍历的方式来做。先将最大的数放在第m+n-1的位置，往前遍历，然后当其中一个数组遍历完成了之后，再把另个数组剩余的值全部搬到结果中来即可。

## Review

continue reading 《effective java》

- Item 15 : Minimize the accessibility of the classes and members

  > 1. The single most important factor that distinguishes a well-designed component from a poorly designed ont is the degree to which the component hides its internal data and other implementation details, clearnly separating its API from its implementation.
  > 2. The rule of thumb is simple : ***make each class or member as inaccessible as possible.***
  > 3. To summarize, ***you should reduce accessibility of program elements as much as possible(within reason).*** After carefully designing a minimal public API, you should prevent any stray classes, interfaces, or members from becoming part of the API. **With the exception of public static final fields, which serve as constants, public class should have no public fields .** Ensure that objects referenced by public static final fields are immutable.

- Item 16: In public classes, use accessor methods, not public fields

  > 1. If a classis accessible outside its package, provide accessor methods to preserve the flexibility to change the class’s internal representation
  > 2. If a class is package-private or is a private nested class, there is nothing inherently wrong with exposing its data fields
  > 3. In summary, public classes should never expose mutable fields.

## Tips

- 使用`m{a-zA-Z}`表示在当前光标所在位置设标记，小写字母标记只在缓冲区局部可见，大写字母则全局可见。
- `mm`和``m`是一对便于使用的命令，分别用于设置标记位m和跳转到编辑位m上。同时vim还会自动编辑一些位置：

![](ARTS-9\微信截图_20190616213221.png)

## Share

跟朋友做了个关于Java锁的实现机制的分享，ppt内容就不截图了，链接贴在这：

[锁的实现机制](https://github.com/Banana1995/PersonRepo/tree/master/Document/ShareDoc)