---
title: ARTS打卡第十二周
date: 2019-07-21 20:28:39
categories: "Java基础"
tags: "ARTS"
---

# ARTS第十二周

本次ARTS主要包括：853题（car fleet）算法题；《effective java》中关于接口应该只被用来定义类型和通过继承来实现标志类，以及关于内部类的使用和类的命名；关于DDIA的部分阅读后真理的脑图。

<!--more-->

## Algorithm

> 524. car fleet

```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
          int cars = position.length;
        if (cars <= 1) {
            return cars;
        }
        Map<Integer, Double> posiSpeedMap = new TreeMap<>();
        for (int i = 0; i < cars; i++) {
            int distence = target - position[i];
            double time =(double) distence / (double) speed[i] ;
            posiSpeedMap.put(position[i], time);
        }
        //from farest to earliest car
        Arrays.sort(position);
        int res = cars;
        for (int i = cars - 1; i >0 ; i--) {
            if (posiSpeedMap.get(position[i]) >= posiSpeedMap.get(position[i - 1])) {
                position[i - 1] = position[i];
                res--;
            }
        }
        return res;
    }
}
```

将这个问题换个角度思考，按照起始位置排列的汽车，远的那辆到达终点的时间比近的那辆到达终点的时间要短的话，那么必然会在途中产生一个fleet。有个需要注意的地方是，在追上前面那辆车之后，车速会被限制跟前车相同，此时则需考虑后车若快于被限制之后的情况。

## Review

continue reading 《effective java》

- Item 22: Use interfaces only to define types

  > - When a class implements an interface, the interface serves as a *type* that can be used to refer to instances of the class. 
  > - The constant interface pattern is a poor use of interfaces.It is of no consequence to the users of a class that the class implements a constant interface. In fact, it may even confuse them. Worse, it represents a commitment: if in a future release the class is modified so that it no longer needs to use the constants, it still must implement the interface to ensure binary compatibility. If a nonfinal class implements a constant interface, all of its subclasses will have their namespaces polluted by the constants in the interface.
  > - If you want to export constants, there are several reasonable choices. If the constants are strongly tied to an existing class or interface, you should add them to the class or interface.

- Item 23: Prefer class hierarchies to tagged classes

  > - Tagged classes are verbose, error-prone, and ineficient.
  > - In summary, tagged classes are seldom appropriate. If you’re tempted to write a class with an explicit tag field, think about whether the tag could be eliminated and the class replaced by a hierarchy. When you encounter an existing class with a tag field, consider refactoring it into a hierarchy.

- Item 24: Favor static member classes over nonstatic

  > - If you declare a member class that does not require access to an enclosing instance, always put the static modifier in its declaration.If you omit this modifier, each instance will have a hidden extraneous reference to its enclosing instance.
  > - To recap , there are four different kinds of nested classes, and each has its place. If a nested class needs to be visible outside of a single method or is too long to fit comfortably inside a method, use a member class.If each instance of a member class needs a referenece to its enclosing instance, make it nonstatic; otherwise, make it static. Assuming the class belongs inside a method, if you need to create instances from only one location and there is a preexisting type that characterizes the class, make if an anonymous class;otherwise, make it a local class.

## Tips

无

## Share

![](ARTS-12\微信截图_20190721231103.png)