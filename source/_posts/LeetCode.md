---
title: leetcode记录
date: 2020-03-04 22:35:39
categories: "Java基础"
tags: "ARTS"
---

# LeetCode记录总结

本文章主要记录LeetCode刷题学到的知识

<!--more-->

## 242.Valid Anagram

题目：

> Given two strings s and t , write a function to determine if t is an anagram of s.
>

我的解法：

```java
class Solution {
      public boolean isAnagram(String s, String t) {
     if ((s.isEmpty() && !t.isEmpty())||s.length()!=t.length()) {
            return false;
        }
        char[] tchars = t.toCharArray();
        char[] schars = s.toCharArray();
       Arrays.sort(tchars);
        Arrays.sort(schars);
         for (int i = 0; i < tchars.length; i++) {
            if (tchars[i] != schars[i]) {
                return false;
            }
        }
        return true;
    }
}
```

思路是想通过将字符串排序，然后逐一对比字符。看到官方有种解法是通过哈希映射，先准备一个26长度的int数组，将每个字符与`a`相减得出对应的ASCII码，将其作为数组的下标，s的字符对下标上的值++，t的字符对下标上的值--。最后，判断这个数组有没有不为0的，若不为0，则说明字符不一样。**以后遇到字符串比较的问题，可以往数组映射上进行考虑**

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] counter = new int[26];
    for (int i = 0; i < s.length(); i++) {
        counter[s.charAt(i) - 'a']++;
        counter[t.charAt(i) - 'a']--;
    }
    for (int count : counter) {
        if (count != 0) {
            return false;
        }
    }
    return true;
}
```

这题的follow up是，如果字符串是unicode编码的，那么怎么实现。那样的话我的这种解法依然有效，但是使用定长数组的解法则不行，需要使用hashmap来代替数组。

Unicode相关：[字符编码笔记：ASCII，Unicode 和 UTF-8](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)

## LCCI.01 Is Unique

题目：

> Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?
>

我的思路是直接采用整形数组，将每个字符与`A`相减得出的数字作为数组下标，将数组的值+1，当发现某个字符对应的数组值大于1时，则说明该字符重复了，返回false。

题目说的是不采用其他数据结构，而我用了数组来解，其实也是一种数据结构。看了题解，发现可以用整形的bit位代替数组，将每个字符与`A`的差作为数字1的左移位数，这样每次和mark的值相与，若字符没重复着则会为0，不为0则说明重复了。在不重复的时候，用或运算来将mark的bit位置位。贴出我修改后的解：

```java
 public boolean isUnique(String astr) {
        if (astr.isEmpty()) {
            return true;
        }
        char[] chars = astr.toCharArray();
        long flag =0;
        for (char aChar : chars) {
            long i = aChar - 'A';
            if ((flag & (1L << i)) != 0) {
                return false;
            }else {
                flag =flag|(1L << i);
            }
        }
        return true;
}
```

##LCCI.02 Check Permutation

题目：

> Given two strings,write a method to decide if one is a permutation of the other.
>

此题类似与#242 valid anagram 那道题。可以采用相同的解法。但是看到题解里有采用异或的方式来解的，异或本身的原理是：`0^a=a;1^a=a取反`。因此当用0跟字符串每个字符异或后，得到的值是字符串每个字符相异或的。这样对两个字符串进行异或后再对两个值进行比较，若相等则说明是同个字符串，否则为不同字符串。

解法如下：

```java
 public boolean CheckPermutation(String s1, String s2) {
        if (s1 == null || s2 == null) {
            return s1 == null && s2 == null;
        }
        char[] s1Chars = s1.toCharArray();
        char[] s2Chars = s2.toCharArray();
        int a = 0;
        int b = 0;
        for (char s1Char : s1Chars) {
            a = a ^ s1Char;
        }
        for (char s2Char : s2Chars) {
            b = b ^ s2Char;
        }
        return a == b;
    }
```

## LCCI.03 String toURL 

题目：

> Write a method to replace all spaces in a string with '%20'. You may assume that the string has sufficient space at the end to hold the additional characters,and that you are given the "true" length of the string. (Note: If implementing in Java,please use a character array so that you can perform this operation in place.)
>

刚开始理解错了题目的意思，以为要将字符串中的空格全部替换为`%20`，后来测试才发现，需要转换的字符串其实是给定的长度，所以只需要遍历字符串，将给定长度内的空格字符替换为`%20`即可。

解法如下：

```java
public String replaceSpaces(String S, int length) {
        if (length <= 0) {
            return null;
        }
        StringBuilder res = new StringBuilder();
        char[] charArray = S.toCharArray();
        for (int i = 0; i < length; i++) {
            char c = charArray[i];
            if (c == ' ') {
                res.append("%20");
            } else {
                res.append(c);
            }
        }
        return res.toString();
    }
```

## LCCI.04 Palindrome Permutation

题目：

> Given a string, write a function to check if it is a permutation of a palin­ drome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.
>

可以把题目中的回文字符串理解为一个字符串中字母出现次数为奇数的不超过1个。理解了这个之后，就可以使用哈希映射的方式来解决这个问题了，先将每个字母对应的数字放到数组中，然后将数组的值奇数，作为出现此处。最后判断次数是否为奇数即可。此处判断是否为奇数可以使用`(a&1)==1`来判断，若位与后结果为1，则说明是奇数。

