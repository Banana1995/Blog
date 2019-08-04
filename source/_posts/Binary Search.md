---
title: Binary Search
date: 2019-08-03 16:28:39
categories: "Java基础"
tags: "Algorithms"
---

# Binary Search 

This is a summary of binary search algorithm.

<!--more-->

## Item 1: need find a number is exactly equal to the target

This circumstances we can use this schema:

```java
boolean binarySearch(int[] num, int s, int e, int target) {
        while (s <= e) {
            int mid = (s + e) >> 1;
            if (num[mid] == target) {
                return true;
            }
            if (num[mid] > target) {
                e = mid - 1;
            }
            if (num[mid] < target) {
                s = mid + 1;
            }
        }
        return false;
    }
```

the recusive schema:

```java
 boolean recusiveBinarySearch(int[] num, int s, int e, int target) {
        if (s > e) {
            return false;
        }
        int mid = (s + e) >> 1;
        if (num[mid] == target) {
            return true;
        } else if (num[mid] > target) {
            return recusiveBinarySearch(num, s, mid - 1, target);
        } else {
            return recusiveBinarySearch(num, mid + 1, e, target);
        }
    }
```

the break condition is `s>e`,the expression is `f(s-e)=f(s-mid)||f(s-e)=f(mid-e)`.

## Item 2: find first no less than target 

To find no less than target value , we should use following schema:

```java
 int binarySearch(int[] nums, int s, int e, int target) {
        while (s < e) {
            int mid = (s + e) >> 1;
            if (nums[mid] < target) {
                s = mid + 1;
            } else {
                //end point is no less than target(>=target)
                e = mid;
            }
        }
        return e;
    }
```

this shcema need the end point equal to array's length , when `e==length` , it indicate the target value is greater than the array's biggest element. when `e==0`, it means that the target value is less than the array's smallest one.

Besides, this Item have a variant, which is  **find last less than target value**. we can solve this proble by return `e-1`.Note that 

## Item 3: find first greater than target

when you want to find first greater than target value , just change the condition is ok.use `nums[mid]<=target` ,you can solve this problem.

```java
int binarySearch(int[] nums, int s, int e, int target) {
        while (s < e) {
            int mid = (s + e) >> 1;
            if (nums[mid] <= target) {
                s = mid + 1;
            } else {
                //end point is greater than target(>target)
                e = mid;
            }
        }
        return e;
    }
```

now, the end point is greater than the target value.if  `e == length` , then array doesn't exist a element greater than target.

## Item 4: Use subfunction as the conditions



## Citations
[LeetCode Binary Search Summary 二分搜索法小结](https://www.cnblogs.com/grandyang/p/6854825.html)



