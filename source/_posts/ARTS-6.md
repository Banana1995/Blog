---
title: ARTS打卡第六周
date: 2019-05-09 23:05:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第五周

本次ARTS主要包括：Excel列名称算法题；《Java并发编程艺术》内存模型的思维导图；《effective java》中关于重写hashcode方法的章节。

<!--more-->

## Algorithm

> 168.Excel表列名称

```java
class Solution {
    public String convertToTitle(int n) {
        String contanier = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
        int temp =0 ;
        if(n<=26){
            return String.valueOf(contanier.charAt(n-1));
        }
        StringBuilder sb =  new StringBuilder();
        List<String> te = new ArrayList<>();
        while(n > 0){
            temp = (n-1)%26;
            n=(n-1)/26;
            te.add(String.valueOf(contanier.charAt(temp)));
        }
        for (int j= (te.size()-1);j>=0; j--){
            sb.append(te.get(j));
        }
        return sb.toString();
    }
}
```

我的思路是对26取余，从低到高位，一位一位的取。其中的`n-1`将整除26的数取不到Z字母的问题解决了。

见到题解中有同学使用`'A'+数字`来取值，引发了我对char类型数据的思考：char占有2个字节，用16位表示Unicode编码。可以使用'A'加上一个整数来表示其编码的字符，如'A'+1则表示'B'这个字符。

## Review

 continue reading 《effective java》

- Item 11 : Always override hashCode when you override equals

  > 1. You must override hashCode method in every class that overrides equals.Here are the contract,adapted from the Object specification:
  >    - when the hashCode method is invoked on an object repeatedly during an execution of an application, it must consistently return the same value.
  >    - if two objects are equal according to the equals method , then calling hashCode on the two  objects should return the same integer result.
  >    - if two objects are unequal according to the equals method , it is **not required** that calling hashCode method return distinct value.However, the programmer should be aware that producing distinct results for unequal objects may improve the performance of hash tables.(cause the same hashCode value can make the hashtable degenerate to linked list).
  > 2. A good hash function tends to produce unequal hash codes for unequal instances.
  > 3. When you compute the hash codes , you **must exclude** any fields that are not used in equals comparisons, or you risk violating the second provision of the hashCode contract.
  > 4. If you have a bona fide need for hash functions less likely to produce collisions , see Guava's `com.google.common.hash.Hashing`
  > 5. if a class if immutable and the cost of computing the hash code is significant, you might consider caching the hash code in the object rather than recalculating it each time it is requested.
  > 6. Do not be tempted to exclude significant fields from the hash code computation to improve perfoemance.

## Tips

无

## Share

本周新增了《并发编程的艺术》第三章内存模型相关内容的思维导图

![](ARTS-6\Java并发编程的艺术.png)



