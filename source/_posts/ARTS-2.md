---
title: ARTS打卡第二周
date: 2019-04-03 23:28:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第二周

第二周赶上清明假期，察觉自己欠缺诸多，还是踏踏实实的一步一步来。所谓慢就是快，时常想尽力追赶前面的东西，没把基础打好，后面搞得自己很狼狈，实则是得不偿失。

<!--more-->

## Algorithm

本周做的题目是最长回文子串，第一遍做的时候没有理解清楚什么是回文。第二次提交的时候未能通过测试用例。看了题解才做出来的。

思路是用中心点方法，从中心点开始往两侧推移。

贴出大佬的解法：

```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int[] range = new int[2];
        char[] str = s.toCharArray();
        for (int i = 0; i < s.length(); i++) {
            i = findLongest(str, i, range);
        }
        return s.substring(range[0], range[1] + 1);
    }
    
    public static int findLongest(char[] str, int low, int[] range) {
        int high = low;
        while (high < str.length - 1 && str[high + 1] == str[low]) {
            high++;
        }
        int ans = high;
        while (low > 0 && high < str.length - 1 && str[low - 1] == str[high + 1]) {
            low--;
            high++;
        }
        if (high - low > range[1] - range[0]) {
            range[0] = low;
            range[1] = high;
        }
        return ans;
    }
}
```

对于动态规划、字符串查找等算法还是很迷糊，还是需要加强训练和看书啊。

## Review

本周继续阅读英文版《effective java》。

- Item2 consider a builder pattern when face many constructor parameters.

> 1. The Builder pattern is a good choice when designing classes whose construct or static factories would have more than a handful parameters.
>
> 2. builder pattern is provide a public method which return 'Builder' type Object.And,The instant of Builder type Object would provide a build() method,which return the instant of outclass Object.

- Item 3 Enforce the singleton property with a private constructor or an enum type

> 1. the normal approach to implement singletons are base on keeping the construstor private and export a public static member to provide access to the sole instance.but serialized instantce will create a new instance,unless you declare all instance fields `transient` and provide `readResolve` method.
> 2. a single-element enum type is often the best way to implement singleton.

- Item 4 Enforce nonistantiablity with a private contructor

> 1. some utility classes is not designed for instantied. but the default construct would make it can be instantiated.we could make the constructor private to avoid user instantiated it.
> 2. private construct can make the subclass could't instantiated too.cause the all constructors must invoke superclass contructor,explicitly or implicitly. and the private constructor is inaccessible from outside class.
> 3. The AccessError is not strictly required,but it provides insurance in case the constructor is invoked from inside class.

## Tips

学习《VIM使用技巧》，工作中使用vim一直不会快速移动，在此记录下，最近学到的快速移动技巧

1. 区分屏幕行和实际行。例如在工作中编辑baseInfo文件时，其实就是一个实际行，但是这一行的内容非常多，导致在做查找、跳转等动作时很不方便，但是vim可以在移动命令前加上`g`表示移动屏幕行。如`gk`表示向上移动一个屏幕行。

   | 命令 | 操作                           |
   | ---- | ------------------------------ |
   | `g0` | 移动到屏幕行的行首             |
   | `0`  | 移动到实际行的行首             |
   | `gk` | 向上移动一个屏幕行             |
   | `^`  | 移动到实际行的第一个非空白字符 |
   | `g^` | 移动到屏幕行的第一个非空白字符 |
   | `$`  |  移动到实际行的行尾 |
   | `g$` | 移动到屏幕行的行尾 |

2. 基于单词的移动，下面这张截图即可了解清楚各个命令的操作：

   ![](ARTS-2\moveBaseWord.png)

    

3. 区分单词和字符串：对于vim来说，单词是以各种逗号，括号，空格等符号分割的，而字符串则是单纯的以空格分割开来。基于这个认知，我们在做移动命令的时候，便可使用`W`/`E`/`B`等命令基于字符串移动。这样会比基于单词移动快很多，如果希望以细粒·度来移动则可以使用基于单词的方式。

4. 使用`f{char}`+`;`命令可以在同一行中，根据查找的字母快速移动到自己希望的位置上去。若不小心跳过头了，还可以使用`,`命令往回跳。

   > 可以把 `t{char}` 及 `T{char} `命令当成“直到查找到指定的字符为止”（search till 
   > the specified character）的命令，它们使光标停留在 {char} 前面的那个字符上，而
   > `f{char}` 和 `F{char} `命令则把光标移动到指定字符上。



## Share

和同学一起做了个分享会，把提纲内容贴在这里，这个内容还没做完，后续还会在这里更新。

![](ARTS-2\Java-concurrency-in-practice.png)



