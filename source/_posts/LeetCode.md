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

