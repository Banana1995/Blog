---
title: ARTS打卡第八周
date: 2019-06-04 22:41:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第八周

本次ARTS主要包括：合并两个有序链表算法题；《effective java》 关于建议实现comparable接口；分享mysql中关于插入重复记录的几种处理方式。

<!--more-->

## Algorithm

> 783. Merge Two sorted list

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        
        if(l1==null){
            return l2;
        }
        if(l2==null){
            return l1;
        }
        ListNode res = null;
        if(l1.val>=l2.val){
            res = l2;
            res.next = mergeTwoLists(l1,l2.next);
        }else{
            res = l1;
            res.next =  mergeTwoLists(l1.next,l2);
        }
        return res;
    }
}
```

这题一开始使用暴力解法没有解出来，采用了题解的一个递归方法。也学习到了递归的一些思想，可以用一个变量来保存需要返回的结果，然后再缩小范围继续递归。

## Review

 continue reading 《effective java》

- Item 14 : consider implement comparable

  > 1. By implement comparable, you allow you class to interoperate with all of many generic algorithms and collection implementations that depends on this interface.
  >
  > 2. If  a field does not implement comparable or you need a nonstandard ordering , use a Comparator instead.
  >
  > 3. Use of the relational operators < and > in compareTo methods is
  >    verbose and error-prone and no longer recommended
  >
  > 4. In Java8, the Comparator interface was outfitted with a set of comparator construction methods, which enable fluent construction of comparators.These comparators can then be used to implement a compareTo method, as required by the Comparable interface.
  >
  >    ```java
  >    // Comparable with comparator construction methods
  >    private static final Comparator<PhoneNumber> COMPARATOR =
  >    comparingInt((PhoneNumber pn) -> pn.areaCode)
  >    .thenComparingInt(pn -> pn.prefix)
  >    .thenComparingInt(pn -> pn.lineNum);
  >    public int compareTo(PhoneNumber pn) {
  >    return COMPARATOR.compare(this, pn);
  >    }
  >    ```
  >
  > 5. **In summary, whenever you implement a value class that has a sensible ordering, you should have the class implement the Comparable interface so that its instances can be easily sorted , searched, and user in comparison-based collections. When comparing field values in the implementations of the compareTo methods, avoid the use of < and > operators. Instead, user the static compare methods inthe boxed primitive classes or the comparator construction methods in the Comparator interface.**

## Tip

处理mysql插入记录重复的几种方式

- 使用`insert ignore`

> Use the **INSERT IGNORE** command rather than the **INSERT** command. If a record doesn't duplicate an existing record, then MySQL inserts it as usual. If the record is a duplicate, then the **IGNORE** keyword tells MySQL to discard it silently without generating an error.
>
> 当记录不存在是，mysql会正常插入；当记录已经存在时，`ignore`关键字会告诉mysql静默的将数据丢掉而不报出错误。下面的sql语句将不会报错：
>
> ```sql
> mysql> INSERT IGNORE INTO person_tbl (last_name, first_name)
>    -> VALUES( 'Jay', 'Thomas');
> Query OK, 1 row affected (0.00 sec)
> 
> mysql> INSERT IGNORE INTO person_tbl (last_name, first_name)
>    -> VALUES( 'Jay', 'Thomas');
> Query OK, 0 rows affected (0.00 sec)
> ```

- 使用`replace` 

> Use the **REPLACE** command rather than the INSERT command. If the record is new, it is inserted just as with INSERT. If it is a duplicate, the new record replaces the old one.
>
> 使用`replace`命令来插入时，当记录不存在，则会正常插入。若已经存在，则会用新的记录代替原先的记录（先删再插）。示例如下：
>
> ```sql
> mysql> REPLACE INTO person_tbl (last_name, first_name)
>    -> VALUES( 'Ajay', 'Kumar');
> Query OK, 1 row affected (0.00 sec)
> 
> mysql> REPLACE INTO person_tbl (last_name, first_name)
>    -> VALUES( 'Ajay', 'Kumar');
> Query OK, 2 rows affected (0.00 sec)
> ```

- ***当处理重复插入记录时，应该在`insert ignore`和`replace`中选择一个，`insert ignore`会保留第一次插入的记录，而`replace`则保留的是最后的一条记录。*** 
- 使用`insert into...on duplicate key update id=id`

> 这里使用`id=id`来在更新时做无用操作来完成在key重复时什么都不做。

## Share

