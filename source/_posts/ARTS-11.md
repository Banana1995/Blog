---
title: ARTS打卡第十一周
date: 2019-06-30 20:28:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第十一周

本次ARTS主要包括：564题（通过删除匹配到字典里最长的单词）和767题（重构字符串）算法题；《effective java》中关于类的最小可修改和接口优于继承的内容。

<!--more-->

## Algorithm

> 524. Longest word in Dictionary through Deleting

```java
class Solution {
     public String findLongestWord(String s, List<String> d) {
        if (s == null || s.length()<=0||d.size() <= 0) {
            return "";
        }
        Collections.sort(d, new compareSpecial());
        for (String word : d) {
            if (isSubStr(s,word)) {
                return word;
            }
        }
        return "";
    }

    private boolean isSubStr(String s, String word) {
        if(word==""){
            return true;
        }
        int j = 0;
        for (int i = 0; i < word.length(); i++) {
            if(j>=s.length()){
                return false;
            }
            while (!(s.charAt(j) == word.charAt(i))) {
                j++;
                if (j >= s.length()) {
                    return false;
                }
            }
            j++;
        }
        return true;
    }

    class compareSpecial implements Comparator<String> {
        @Override
        public int compare(String o1, String o2) {
            if (o1.length() > o2.length()) {
                return -1;
            } else if (o1.length() == o2.length()) {
                return o1.compareTo(o2);
            }
            return 1;
        }
    }
}
```

整体的思想是，通过自定义的Comparator实现由大到小先按照字符的长度，再按照字典顺序排序。将整个数组里的字符串都排序好，再遍历给定的字符串s和数组里的单个单词进行字符匹配。

> 767. Reorganize String 

```java
class Solution {
    public String reorganizeString(String S) {
        int N = S.length();
    int[] counts = new int[26];
        for (char c: S.toCharArray()) counts[c-'a'] += 100;
        for (int i = 0; i < 26; ++i) counts[i] += i;
    //Encoded counts[i] = 100*(actual count) + (i)
        Arrays.sort(counts);

    char[] ans = new char[N];
    int t = 1;
        for (int code: counts) {
        int ct = code / 100;
        char ch = (char) ('a' + (code % 100));
        if (ct > (N+1) / 2) return "";
        for (int i = 0; i < ct; ++i) {
            if (t >= N) t = 0;
            ans[t] = ch;
            t += 2;
        }
    }
        return String.valueOf(ans);
    }
}
```

这一题没做出来，采用的是讨论区一位前辈的思想。大体上运用了计数排序的思想，不过这个解法有两个点很厉害：一是采用乘100，再加上数组下标的方式，来保存排序前每个数组下标的值，方便排序后转换为字符。另一个是采用循环数组的方法将每个字符放到字符串对应的位置上。

## Review

continue reading 《effective java》

- Item 19: Design and document for inheritance or else prohibit it

  > - First, the class must document its self-use of override methods;
  > - a class may have to provide hooks into its internal working in the form of judiciously chosen protected methods;
  > - The onlt way to test a class designed for inheritance is to write subclasses;
  > - Constructors must not invoke overridable methods, directly or indirectly;
  > - Prohibit subclassing in classes that are not designed and documented to be safety subclassed. One way to prohibit subclassing is declare the class final. The Second is make all constructors private or package-private and to add public static factories in place of the constructors.
  
- Item 20: Prefer interfaces to abstract classes

  > - Existing classes can easily be retrofitted to implement a new interface.
  > - To summarize, an interface is generally the best way to define a type that permits multiple implementations. If you export a nontrivial interface, you should strongly consider providing a skeletal implementation to go with it. To the extent possible, you should provide the skeletal implementation via default methods on the interface so that all implementors of the interface can make use of it. That said, restrictions on interfaces typically mandate that a skeletal implementation take the form of an abstract class.

- Item 21: Design interface for posterity

  > - Java 8 provide *default* to allow the addition of  method to  existing interfaces.But it is ***fraught*** !
  > - The declaration for a default method includes a default implementation that is used by all classes that implement the interface but not implement the default method.
  > - In the presence of default methods, existing implementations of an interface  may compile without error or warning but fail at runtime.
  > - Using default methods to add new methods should be avoided unless the need is critical, in which case you should think long and hard about whether an existing interface implementation might be broken by your default method implementation.
  > - It may be possible to correct some interface flaws after an interface is released , But you cannot count on it.

## Tips

无

## Share

无