---
title: ARTS打卡第五周
date: 2019-04-27 23:05:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第五周

本次ARTS主要包括：回文数、最长公共前缀算法、《effective java》关于复写equals方法的Item、VIM使用文本对象选择选区操作、《Java并发编程艺术》前两章的思维导图。

<!--more-->

## Algorithm

> 9. 回文数

```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0){
            return false;
        }
        int rightToLeft =0;
        int te =0;
        int y = x;
        while(y!=0){
            te = y%10;
            rightToLeft = rightToLeft*10+te;
            y=y/10;
        }
        if(x==rightToLeft){
            return true;
        }
            return false;
    }
}
```

刚好昨晚刷题刷到了整数反转的题目，这里顺手一上来就想到了使用整数反转的方式来做。果然效果很不错。对于整数溢出的问题其实可以不用担心，因为回文数字肯定不会溢出。

> 14. 最长公共前缀

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length<=0){
            return "";
        }
        StringBuilder res = new StringBuilder("");
        for(int j=0;;j++){
            boolean flag =true;
            char publicChar = ' ';
            if(strs[0]!=null && strs[0].length()>=j+1){
                publicChar = strs[0].charAt(j);
            }else{
                break;
            }
           for(int i =0;i<strs.length;i++){
                String te = strs[i];
                if(te!=null &&  te.length()>=j+1 && publicChar==te.charAt(j)){
                    continue;
                }else{
                    flag=false;
                    break;
                }
            }
            if(flag ){
                res.append(publicChar);
            }else{
                break;
            }
            
        }
        return res.toString();
        
    }
}
```

最开始写的时候没有考虑到String获取长度的是length()方法与数组的length调用方式是不同的，导致编译未通过。后来测试案例又未通过`""`的校验。测试了一番之后才知道`""`的长度为0，在调用length()方法后，应该判断的条件是字符串的长度大于数组下标+1的值。

我的这个算法的最坏的时间复杂度为O(m*n)，m为数组的长度，n为公共前缀的字符数。查看了题解后发现与题解中的算法二思路相同，但是代码实现没有它的简单。

## Review

continue reading 《effective java 》

- Item 10 Obey the general contract when overriding equals

  > - Each instance of the class is inherently unique.
  > - **There is no need for the class to privide a "logical equality" test.** For example,*java.util.regex.Pattern* could have overridden equals to check whether two Pattern instances represented exactly the same regular expression.But the client user doesn't need this function.
  > - A superclass has already overriden equals, and the superclass behavior is approriate for this class.For example, most Set implementations inherit their equals implementation from AbstractSet, List implementations from AbstractList, and Map implementations from AbstractMap.
  > - **It's  appropriate for you to overriden equal method when a class has a notion of *logical equality* that differ from mere object identity and a superclass has not already overridden equals.** This is generally the case for value classes. A value class is simply a class that represents a value, such as Integer or String. A clent user need invoke equals method to find out whether they are logically equivalent or they refer to same object.
  > - Here are some contract for overriden the *equals method*. 
  >   - Reflexive: For any non-null reference value x, x.equals(x) must return true.
  >   - Symmetric: For any non-null reference values x and y, x.equals(y) must return true if and only if y.equals(x) returns true.
  >   - Transitive: For any non-null reference values x, y, z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) must return true.
  >   - Consistent: For any non-null reference values x and y, multiple invocations of x.equals(y) must consistently return true or consistently return false, provided no information used in equals comparisons is modified.
  >   - For any non-null reference value x, x.equals(null) must return false.

## Tips

在使用vim编辑时，可以使用文本对象来进行选择。如：

![](ARTS-5\微信截图_20190505222619.png)

同时还可以使用`ci}`或`da"`、`ya"`这种操作来将选中的部分替换、删除、复制。其中的`i`和`a`可以理解为`inside`和`arround`;`a"`会选中由双引号括起来的字,`i}`会选中在一对大括号里面的内容。

除此之外，还可以使用`daw`、`ciw`、`yaw`这种操作来对一个单词进行删除、替换、复制等。

## Share

放假期间在读《Java并发编程艺术》这本书。在读完了《Java并发编程实战》再来读这本书，发现很多东西，理解起来更容易了。同时这本书也对底层的指令实现原理讲解的更透彻，推荐阅读。

![](ARTS-5\Java并发编程的艺术.png)





