---
title: ARTS打卡第十周
date: 2019-06-19 22:28:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第十周

本次ARTS主要包括：78题（颜色分类）和179题（最大数）算法题；《effective java》中关于类的最小可修改和接口优于继承的内容；《vim编程技巧》中关于在插入模式即时更正错误操作。

<!--more-->

## Algorithm

> 78. 颜色分类

```java
class Solution {
    public void sortColors(int[] nums) {
        recurisionFastOrder(nums,0,nums.length-1);
    }
    
    private void recurisionFastOrder(int[] nums,int start,int end){
        if(start>=end){
            return;
        }
        int pivot =  nums[end];
        int i=start;
        // int j =start;
        for(int j = start;j<end;j++){
            //j指向的数字小于中枢点时 则需要与i交换，同时i往后移，保证从i到j都是大于中枢点的值
            if(nums[j]<pivot){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
            }
        }
        int temp = nums[i];
        nums[i] = pivot;
        nums[end] = temp;
        recurisionFastOrder(nums,start,i-1);
        recurisionFastOrder(nums,i+1,end);
    }
}
```

我得解法与题解给的不同，用的是快排的算法，快排的递推公式为：`f(p,r)=f(p,q-1)+f(q+1,r)`.其中的q为中枢点。将整个排序的数据分为两部分，[p,q-1]为小于q的部分，[q+1,r]为大于q的部分。剩下的就是确定中枢点的位置了。快排算法是使用两个指针i和j，一个从头开始遍历，当遇到比尾部的值小的时候，则将两个指针的值交换，并将两个指针都往后移一步。这样保持从i到j之间的区间一直都是大于尾部的值的。一次遍历完成后，将i指针的值与尾部的值交换，i点则为中枢点。

> 179. 最大数

```java
class Solution {
    public String largestNumber(int[] nums) {
         if(nums.length==0){
             return "0";
         }
        if(nums.length==1){
            return String.valueOf(nums[0]);
        }
       int[] res = sort(nums,0,nums.length-1);
        if(res[0]==0){
            return "0";
        }
        StringBuilder re = new StringBuilder();
        for(int i =0 ;i<res.length;i++){
            re.append(res[i]);
        }
        return re.toString();
    }
    //合并排序的递推公式为：A[p,r] = merge(A[p,q],A[q+1,r]);
    public int[] sort(int[] nums,int p , int r){
        if(p>=r){
            int[] temp = {nums[r]};
            return temp ;
        }
        int q = (p+r)/2;
        int[] a = sort(nums,p,q);
        int[] b = sort(nums,q+1,r);
        int[] res = merge(a,a.length,b,b.length);
        return res;        
    }
    //合并a，b两个有序数组
    public int[] merge(int[] a ,int m, int[] b,int n){
        int[] res = new int[m+n];
        int k=0;
        int i = 0;
        int j = 0;
        while(i<m && j<n){
            res[k]=compare(a[i],b[j])?a[i++]:b[j++];
            k++;
        }
        if(i<m){
          System.arraycopy(a,i,res,k,m-i);  
        }
        if(j<n){
            System.arraycopy(b,j,res,k,n-j);  
        }
        return res;
    }
    
    //自定义比较大小的规则 x>y return true
    private boolean compare(int x, int y){
              StringBuilder sb = new StringBuilder();
                sb.append(x).append(y);
        Long a = Long.valueOf(sb.toString());
        
                     StringBuilder sb2 = new StringBuilder();
                sb2.append(y).append(x);
         Long b = Long.valueOf(sb2.toString());
        return a>=b;
    }
    
}
```

这题的核心思想在于自定义排序规则，一开始没想想到，看了题解才知道的。我是故意想着用合并排序的方法来做，练习下写合并排序的代码。合并排序的主要点在于两个：递推公式和合并有序数组；递推公式为：`f(p,r)=merge(f(p,q),f(q+1,r))`;合并有序数组就比较简单了，类似leetcode第88题的解法。

## Review

continue reading 《effective java》

- Item 17：Minimize mutability

  > 1. To make a class immutable, follow these rules:
  >    - Don't provide methods that modify the object's state .
  >    - Ensure that the class can't be extended.
  >    - Make all fields private and final.
  >    - Ensure exclusive access to any mutable components.
  > 2. Immutable objects are inherently thread-safe; they required no synchronization. Besides, they can be shared freely, you never have to make *defensive copies* of them.
  > 3. The major disadvantage of immutable classes is that they require a separate object for each distinct value , even if they are diffrent at one bit.
  > 4. To guarantee immutability, instaed of making the class final , you can make all of its constructors private or package-private and  add pbulic static factorices in place of the public constructor.
  > 5. To summarize, classes should be immutable unless there's a compelling reason to make them mutable.***Combining this item with item 15, your natural inclination should be to declare every field private final unless there are compelling reson to do otherwise.***

- Item 18 : Favor composition over inheritance

  > 1. Inheritance used inappropriately, it leads to fragile software.**Unlike method invocation, inheritance violates encapsulation.** A subclass depends on the implementation detail of its superclass for its proper function. The superclass's implementation may change from release to release.
  > 2. Instead of extending an existing class, give your new class a private field that references an instance of the existing class, give your new class a private field that references an instance of the existing class. This design is called **composition**. This is known as forwarding, and the methods in the new class are known as forwarding methods. This is also known as the ***Decorator*** pattern.
  > 3. Inheritance is appropriate only in circumstances where the subclass really is a subtype of the superclass.In the other words, a class B should extend a class A only if an "is-a" relationship exists between the two classes.

## Tips

1. 在插入模式中，使用如下操作更正错误：

![](ARTS-10\微信截图_20190623160456.png)

2. 在插入模式下，使用`ctrl+0`，可以将寄存器0中的内容粘贴到文本中。
3. 使用`xp`命令可调换光标之后的两个字符；使用`ddp`命令可调换当前行和它的下一行；

## Share

无