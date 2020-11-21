---
title: ARTS打卡第四周
date: 2019-04-27 23:05:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第四周

快五一了，最近这段时间都是在看并发相关的知识。刚看完《Java并发编程实战》，基础部分还是需要恶补呀。加油，一步步来。

<!--more-->

## Algorithm

> 7. 整数反转

标准答案的解法是通过使用取余法不断地取出整数的每一位，再用乘法加到新数的后面去，再判断一下整数是否溢出了即可。

```java
class Solution {
    public int reverse(int x) {
        int rev = 0 ;
        while(x!=0){
            int te = x%10;
          
            if(rev>Integer.MAX_VALUE/10 || (rev ==  Integer.MAX_VALUE/10 && te>7)){
                return 0;
            }
            if(rev<Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE/10 && te <-8)){
                return 0;
            }
            
            rev = rev*10+te;
              x=x/10;
        }
        return rev;
        
    }
}
```

## Review

continue reading 《effective java》

- Item 7 Eliminate obsolete object references

  > 1. **Nulling out object references should be the exception rather than the norm **
  > 2. when a class manager its own memory , the programmer should be alert for memory leaks.
  > 3. caches is common source of memory leaks

- Item 8  Avoid Finalizers and cleaners

  > 1. Finalizers are unpredictable , often dangerous, and generally unneccessary.Cleaners are less dangerous than finalizers,but still unpredicatable,slow,and generally unnecessary.
  > 2. There is a severe performance penatly for using finalizer and cleaners.
  > 3. They have two legitimate uses. One is to act as a safety net in case the owner of a resource neglects to call its close method .Another is native peers

- Item 9 Perfer try-with-resources to try-finally

## Tips

在工作中遇到了需要用sql处理字符串的问题，使用到了substring_index这个函数，具体用法如下：

>  **substring_index("待截取字符"，"分隔符"，截取第n个字符)**

当最后的数字n为负数时，代表倒着数第n个字符。

将从`1010a2|192b23|33c23|12383d|12312f`这段字符中取出竖线分割的倒数第一个字符的用法为：`substring_index("1010a2|192b23|33c23|12383d|12312f","|",-1)`,取出第三个用法为`substring_index("1010a2|192b23|33c23|12383d|12312f","1",3)`

## Share

最近几周都在死磕并发编程这本书，终于读完，收获颇丰，贴上自己整理的思维导图

![](ARTS-4\Java 并发编程实战.png)



