---
title: ARTS打卡第一周
date: 2019-03-31 23:28:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第一周

第一周参加ARTS打卡，将本周的总结都记录在个人的blog里。刚开始第二天，由于周末加班准备的很仓促，质量感觉一般，最后的share没有做，下周补上。

<!--more-->

## Algorithm

先是LeetCode上简单的算法题，两数之和。上次刷算法题应该是大四秋招前，那会儿什么也不懂，也没坚持下去，甚至当时都没觉得以后可以成为一个程序猿。直到阴差阳错，真的成为了程序猿后，发现基础太薄弱，需要补的东西太多了。索性一步一步来，花两年时间把改补的尽量都补上。下面是我的答案，做的很不好，只想到了暴力解法

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] result = new int[2];
        for(int i =0 ;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
            if( nums[i]+nums[j] == target){
                result[0]=i;
                result[1]=j;
                return result;
                }
            }   
        }
        return result;
    }
}
```

看了题解后发现可以用`map`来做，而让我没能转过弯的是可以把两数之和转换成`x` 和 `target-x` 来从数组中查找。我只想到了单纯的相加。

## Review

读了中英对照版的《effective java》Chapter 2 Item 1，英语不太好，对于其中的很多点理解的有些不到位，所以 需要对照着中文版才能看懂一些。模仿github上的总结，将自己的总结写在下面：

>1. static factory methods have names,but constructors not;
>
>2. not required to create new object when they're invoked;
>3. they can return any subtype of return type;
>4. return type can vary from call to call as a function of the input parameters;
>5. the return type can not exist when the static factory method is written;

there are two main limitation of static factory method :

>1. the class without public or protected contructors can not be subclassed;
>2. documentation is not frendly for programmers;

[gitbook中英对照版](<https://jiapengcai.gitbooks.io/effective-java/content/chapter1/di-1-tiao-ff1a-kao-lv-yong-jing-tai-fang-fa-er-bu-shi-gou-zao-qi.html>)

## Tip

最近在练习使用`vim`插件编辑代码，想到上次在客户现场，`json`文件是一行数据，很长很长。我用`vim`编辑不知道如何使用`replace`功能在同一行中选中部分进行替换。后来问了同学，他抛给我一个链接，是英文搜索出来的，而且讲解很详细。这次让我认识到学习英语对于查找问题很重要，很多信息确实在中文里搜索很难找到解答。在此记录下`vim`在`visual`模式下的替换技巧：可以使用`\%V`来指定在`visual`模式中替换选中部分的查找对象。

示例：

```t
music amuse fuse refuse
```

In normal mode, type `^wvee` to visually select "amuse fuse" (`^` goes to first nonblank character, `w` moves forward a word, `v` enters visual mode, `e` moves forward to end of next word). Then press Escape and enter the following command to change all "us" to "az" in the last-selected area within the current line:

```
:'<,'>s/\%Vus/az/g
```

the result is :

```
music amaze faze refaze
```

注意V是大写，不是小写。

[原文地址链接](https://vim.fandom.com/wiki/Search_and_replace_in_a_visual_selection)

## Share

无



