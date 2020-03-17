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

## LCCI.01.01 Is Unique

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

## LCCI.01.02 Check Permutation

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

## LCCI.01.03 String toURL 

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

## LCCI.01.04 Palindrome Permutation

题目：

> Given a string, write a function to check if it is a permutation of a palin­ drome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.
>

可以把题目中的回文字符串理解为一个字符串中字母出现次数为奇数的不超过1个。理解了这个之后，就可以使用哈希映射的方式来解决这个问题了，先将每个字母对应的数字放到数组中，然后将数组的值奇数，作为出现此处。最后判断次数是否为奇数即可。此处判断是否为奇数可以使用`(a&1)==1`来判断，若位与后结果为1，则说明是奇数。

关于位运算总结可以参考这篇博客：[位运算总结](http://blog.tomtung.com/2007/05/bitwise-operation/)

```java
    public boolean canPermutePalindrome(String s) {
        if (s==null|| s.isEmpty()) {
            return false;
        }
        char[] chars = s.toCharArray();
        HashMap<Character, Integer> letters = new HashMap<>();
        for (char aChar : chars) {
            letters.merge(aChar, 1, (a, b) -> a + b);
        }
        int flag=0;
        for (int letter : letters.values()) {
            if (((letter&1)==1)&& (++flag)>1) {
                return false;
            }
        }
        return true;
    }
```

## LCCI.01.05 One Away

题目：

> There are three types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.
>

此题并不是单词的判断字符串少了几个字符的问题，而是两个字符串除了不一样的那个字符外，其他字符的顺序也需要一直。没有想到更好的办法，于是我采用的是双指针暴力法。双指针类的题目需要分清不同情况下两个指针的处理，在这题中，两个字符串长度相差大于1肯定返回false，其他的分为长度相同和长度相差为1来分别处理。长度相同时，最多有一个字符可以不一样。长度相差为1时，短字符串的顺序必须与长字符串一样。

```java
   public boolean oneEditAway(String first, String second) {
        if (first == null || second == null) {
            return true;
        }
        String longer = first.length() >= second.length() ? first : second;
        String shorter = first.length() < second.length() ? first : second;
        if (longer.length() - shorter.length() > 1) {
            return false;
        }
        if (longer.isEmpty() || shorter.isEmpty()) {
            return longer.length() - shorter.length() <= 1;
        }
        int flag = 0;
        int i = 0;
        int j = 0;
        if (longer.length() == shorter.length()) {
                    
            for (int k = 0; k < longer.length(); k++) {
                if (longer.charAt(k) != shorter.charAt(k)) {
                    flag++;
                    if (flag > 1) {
                        return false;
                    }
                }
            }
        }else {
            for (; j < shorter.length(); ) {
                if (longer.charAt(i) != shorter.charAt(j)) {
                    i++;
                    flag++;
                    if (flag > 1) {
                        return false;
                    }
                    continue;
                }
                i++;
                j++;
            }
        }
        return true;
    }
```

## LCCI.01.06 Compress String

题目：

> Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2blc5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).

这题我采用普通的遍历法做出来了，需要注意的是不能使用哈希映射的方法。因为相同字母在不连续位置出现是不能统计到一起去的。看题解中，有同学**在字符的最后补了一位来规避遍历时处理最后一个字符的特殊情况**，这种思路值得学习。还有的同学采用了双指针的思路，一个指针遍历字符，另个指针统计相同的字符个数。

我的解法：

```java
    public String compressString(String S) {
        if (S.isEmpty()||S.length()<3) {
            return S;
        }
        char[] chars = S.toCharArray();
        StringBuilder sb = new StringBuilder();
        int repeat = 1;
        for (int i = 0; i < chars.length; i++) {
            if (i > 0) {
                if (chars[i] == chars[i - 1]) {
                    repeat++;
                    if (i == chars.length - 1) {
                        sb.append(chars[i]).append(repeat);
                    }
                } else {
                    sb.append(chars[i - 1]).append(repeat);
                    repeat = 1;
                    if (i == chars.length - 1) {
                        sb.append(chars[i]).append(repeat);
                    }
                }
            }
        }
        return S.length() <= sb.length() ? S : sb.toString();
    }
```

## LCCI.01.07 Rotate Matrix

题目：

> Given an image represented by an N x N matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?
>

一开始我的解题思路是通过新建一个二维数组，然后，将矩阵进行转换，按照`a=j,b=N-i-j`的逻辑转换，最后再将转换后的数组复制到原数组中。这样会导致空间复杂度为O(N)。看了书本上的解法是原地旋转，由外层向内层每一层都是将上边移到右边，右边移到下边，下边移到左边，左边移到上边。通过这种方式完成整个矩阵的旋转。这种方式的实现逻辑交易较为清晰，时间复杂度为O(N2)。修改我的解法后，如下：

```java
    public void rotate(int[][] matrix) {
        int N = matrix.length;
        if (N <= 1) return;
        for (int layer = 0; layer < N / 2; layer++) {
            for (int j = layer; j < N - layer - 1; j++) {
                int top = matrix[layer][j];
                matrix[layer][j] = matrix[N - j - 1][layer];//left->top
                matrix[N - j - 1][layer] = matrix[N - layer - 1][N - j - 1];//bottom->left
                matrix[N - layer - 1][N - j - 1] = matrix[j][N - layer - 1];//right->bottom
                matrix[j][N - layer - 1] = top;//top->right
            }
        }
    }
```

矩阵旋转时，可以将矩阵的数组下标写出来，然后找出每一步转换的`i，j`的规律。一般无非就是`x=j,y=length-i-1`这种情况。

## LCCI.01.08 Zero Matrix

题目：

> Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column are set to 0.
>

我的思路是将矩阵中应该被清0的行和列分别用数组记录下来。然后再拿行数组和列数组处理矩阵中对应的行和列。但是这样占用的空间复杂度为O(N)。参考书本上给出的优化方案是，将矩阵的第一行和第一列作为行数组和列数组。

```java
	public void setZeroes(int[][] matrix) {
        int rows = matrix.length;
        int cloumns = matrix[0].length;
        int[] rowsToZero = new int[rows];
        int[] cloums = new int[cloumns];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                if (matrix[i][j] == 0) {
                    rowsToZero[i]=1;
                    cloums[j]=1;
                }
            }
        }
        for (int i = 0; i < rowsToZero.length; i++) {
            if (rowsToZero[i] == 1) {
                //set i row to 0
                for (int j = 0; j < matrix[i].length; j++) {
                    matrix[i][j]=0;
                }
            }
        }
        for (int i = 0; i < cloums.length; i++) {
            if (cloums[i] == 1) {
                for (int j = 0; j < matrix.length; j++) {
                    matrix[j][i]=0;
                }
            }
        }
    }
```

## LCCI.01.09 String Rotation

题目：

> Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 (e.g.,"waterbottle" is a rotation of"erbottlewat"). Can you use only one call to the method that checks if one word is a substring of another?
>

这一题我给它想的复杂了，第一反应是通过双指针来做，结果没有做出来。考虑的情况漏了。看了书本才想起用包含子串的方法应该是最简单的。

```java
    public boolean isFlipedString(String s1, String s2) {
        if (s1.isEmpty() || s2.isEmpty()) {
            return s1.isEmpty() && s2.isEmpty();
        }
        if (s1.length() != s2.length()) {
            return false;
        }
           String s1s1 = s1+s1;
        return s1s1.contains(s2);
    }
```

## LCCI.02.01 Remove Duplicate Node

题目：

> Write code to remove duplicates from an unsorted linked list.

此题的follow up是想让不用额外的空间来实现，于是我直接采用双指针法。一个指针指向尾部，另个之前在前面做判重。这样会增加时间复杂度，为O(N2)；

```java
    public ListNode removeDuplicateNodes(ListNode head) {
        if (head==null || head.next == null) {
            return head;
        }
        ListNode tail = head;
        ListNode p1 = head;
        while (tail.next != null) {
            ListNode next = tail.next;
            boolean nextIsDup = false;
            while (p1 != tail.next) {
                if (p1.val == next.val) {
                    nextIsDup = true;
                    break;
                }
                p1 = p1.next;
            }
            p1= head;
            if (nextIsDup) {
                //delete current node
                tail.next= next.next;
            }else {
                tail = next;
            }
        }
        return head;
    }
```

## LCCI.02.02 Kth Node From End of List

题目：

> Implement an algorithm to find the kth to last element of a singly linked list. Return the value of the element.

这题的第一想法觉得很简单，用遍历就可以实现。看了follow up 说可以用递归和双指针。递归没有什么想法，于是便实现了一个双指针版本的：

```java
    public int kthToLast(ListNode head, int k) {
        ListNode b = head;
        ListNode a = head;
        for (int i = 1; i < k; i++) {
            b=b.next;
        }
        while (b.next != null) {
            b=b.next;
            a=a.next;
        }
        return a.val;
    }
```

贴上题解里的递归解法：

```java
// 递归
class Solution {
    // 开始全局变量 K 保持不变
    int K = 1;
    public int kthToLast(ListNode head, int k) {
        // 当节点在最末尾时触发返回
        if (head.next == null) return head.val;
        // 返回的值
        int val = kthToLast(head.next, k);
        // 一旦触发返回，从第一个产生返回的位置用 K 计数
        if (K++ >= k) {
            // 当到达或超过倒数第 k 时，即 K >= k 时保持返回值不变
            return val;
        } else {
            // 没到达则更新需要返回的值
            return head.val;
        }
    }
}
```

## LCCI.02.03 Delete Middle Node

题目：

> Implement an algorithm to delete a node in the middle (i.e., any node but the first and last node, not necessarily the exact middle) of a singly linked list, given only access to that node.
>

这题我并没有想到解法，其实链表的操作不应该被固定思维给限制住。这题可以通过将当前节点替换为下一节点，并将下一节点删除的方式来实现。代码也非常简单。主要考察的是这种思维。

```java
    public void deleteNode(ListNode node) {
        node.val=node.next.val;
        node.next=node.next.next;
    }
```

## LCCI.02.04 Partition List

题目：

> Write code to partition a linked list around a value x, such that all nodes less than x come before all nodes greater than or equal to x. If x is contained within the list, the values of x only need to be after the elements less than x (see below). The partition element x can appear anywhere in the "right partition"; it does not need to appear between the left and right partitions.
>

这一题我的思路是通过使用双指针来做，一个指针做遍历，另个指针指向小于x的数据的next。若遍历遇到小于x的值，则将其与小于x的next交换即可。

```java
    public ListNode partition(ListNode head, int x) {
        if (head == null) {
            return null;
        }
        ListNode p1 = head;
        ListNode p2 = head;
        while (p2 != null) {
            if (p2.val < x) {
                int temp = p1.val;
                p1.val = p2.val;
                p2.val = temp;
                p1 = p1.next;
            }
            p2 = p2.next;
        }
        return head;
    }
```

此处需要注意的是，当我尝试使用如下异或来做交换操作时：

```java
p1.val = p1.val^p2.val;
p2.val = p1.val^p2.val;
p1.val = p1.val^p2.val;
```

发现在刚开始头指针指向同个对象时，对`p1.val`和`p2.val`做异或操作得出的值为0，再将0复制给`p1.val`，这样使得`p2.val`同时变为了0，因为`p1.val`和`p2.val`指向的是相同的值。

## LCCI.02.05 Sum Lists

题目：

> You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.

这题一开始我的思路是将链表给合并成整数，再相加，然后再拆成链表。后来看了提示里说尝试用递归，才想到可以用递归来解决这个问题。刚开始的时候，递归写出来了，但是有几个用例总是执行不过，原因是我用的整型变量结果相加之后溢出了，改为了long之后发现还是会溢出。然后想到了不应该用中累加再拆成链表的方式。应该每一个节点算出对应的结果里的节点。递归时直接将节点拼接在一起。

```java
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        return sum(l1,l2,0);
    }

    ListNode sum(ListNode s1, ListNode s2, int buy) {
        int currentLevelSum = s1.val + s2.val + buy;
        ListNode currentNode = new ListNode(currentLevelSum % 10);
        if (s1.next == null && s2.next == null) {
            if (currentLevelSum / 10 != 0) {
                currentLevelSum = currentLevelSum/10;
                currentNode.next = new ListNode(currentLevelSum);
            }
            return currentNode;
        }
        s1 = s1.next != null ? s1.next : new ListNode(0);
        s2 = s2.next != null ? s2.next : new ListNode(0);

        currentNode.next = sum(s1, s2, currentLevelSum / 10);
        return currentNode;
    }
```

看题解中也有没有用递归的，不过也是类似的思想，不能将链表所有的数据都加起来再拆成链表， 那样会导致数据溢出。对于递归的题目我还是不太熟悉，做出来有点难度。

## LCCI.02.06  Palindrome Linked List

题目：

> Implement a function to check if a linked list is a palindrome.

这题我的思路是采用双指针或者反转整个链表再对比的方式。双指针我没有想到采用快慢指针这种方式，所以用了反转链表，但是没能实现follow up中的O(n)时间和O(1)空间的复杂度。贴上我的解法：

```java
   public boolean isPalindrome(ListNode head) {
        if (head == null) {
            return false;
        }
        ListNode p1 = head;
        ListNode a = null;
        while (p1 != null) {
            ListNode b = new ListNode(p1.val);
            p1 = p1.next;
            b.next = a;
            a = b;
        }
        while (head != null) {
            if (a.val != head.val) {
                return false;
            }
            head = head.next;
            a = a.next;
        }
        return true;
    }
```

## LCCI.02.07 Intersection of Two Linked Lists

题目:

> Given two (singly) linked lists, determine if the two lists intersect. Return the inter­ secting node. Note that the intersection is defined based on reference, not value. That is, if the kth node of the first linked list is the exact same node (by reference) as the jth node of the second linked list, then they are intersecting.
>

这题我刚开始没什么好的思路，只有个暴力的算法思想，找不到能在O(n)时间和O(1)空间内解决的办法。看到了提示才想起来先将长度对齐再用双指针同时往后遍历找。

```java
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        ListNode p1 = headA;
        ListNode p2 = headB;
        int len1 = 0;
        int len2 = 0;
        while (p1 != null) {
            len1++;
            p1 = p1.next;
        }
        while (p2 != null) {
            len2++;
            p2 = p2.next;
        }
        ListNode fast = len1 > len2 ? headA : headB;
        ListNode lag = len1 > len2 ? headB : headA;
        int dis = len1 > len2 ? len1 - len2 : len2 - len1;
        while (dis > 0) {
            fast=fast.next;
            dis--;
        }
        while (fast != lag) {
            fast=fast.next;
            lag=lag.next;
            if (fast == null) {
                break;
            }
        }
        if (fast == null) {
            return null;
        }
        return fast;
    }

```

## LCCI.02.08 Linked List Cycle

题目：

> Given a circular linked list, implement an algorithm that returns the node at the beginning of the loop.
>
> Circular linked list: A (corrupt) linked list in which a node's next pointer points to an earlier node, so as to make a loop in the linked list.
>

这题我一开始没什么好的思路，后来只好用额外的空间将这题解出来了。使用set存储已经遍历过的节点，然后不停的往后将next 节点添加进set中，当发现添加失败时，即为循环的开始点。

```java
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        Set<ListNode> set = new HashSet<>();
        while (head != null) {
            if (!set.add(head)) {
                return head;
            }
            head=head.next;
        }
        return null;

    }
```

我看题解和书本上有种解法用的是快慢指针，先用快慢指针判断是否会碰撞，若不会碰撞则不会有循环。再判断快慢指针碰撞的位置。当慢指针到达loop点时，快指针为距离loop点的loopsize-k的位置，k为head距离loop点的距离。因此，当碰撞后，碰撞点距离loop点的距离为k。此时只需将慢指针指向head，快指针从碰撞点开始，二者以不断的next，当再次相等时，即为loop点。

## LCCI.03.01 Three in One

题目：

> Describe how you could use a single array to implement three stacks.
>
> Yout should implement push(stackNum, value)、pop(stackNum)、isEmpty(stackNum)、peek(stackNum) methods. stackNum is the index of the stack. value is the value that pushed to the stack.
>
> The constructor requires a stackSize parameter, which represents the size of each stack.
>

该题我一开始理解错了题意，并没有找出很好的解决办法。第一反应是将数组拆分为三份。但是我没有理解到构造器传进来的会是每个栈的长度。看了提示里有说使用环形数组来动态构建栈。这个我没有实现出来，感觉难度有点高，劝退了。

```java
class TripleInOne {

        private int[] stackArray;
        private int eachStackSize;
        private int[] points = new int[3];

        public TripleInOne(int stackSize) {
            stackArray = new int[stackSize*3];
            eachStackSize = stackSize ;
            points[0] = eachStackSize;
            points[1] = eachStackSize * 2;
            points[2] = eachStackSize * 3;
        }

        public boolean isfull(int stackNum) {
            if (points[stackNum] == eachStackSize * stackNum) {
                return true;
            }
            return false;
        }

        public void push(int stackNum, int value) {
            if (isfull(stackNum)) {
                return;
            }
            points[stackNum] = points[stackNum] - 1;
            stackArray[points[stackNum]] = value;
        }

        public int pop(int stackNum) {
            if (isEmpty(stackNum)) {
                return -1;
            }
            points[stackNum] = points[stackNum] + 1;
            return stackArray[points[stackNum] - 1];
        }

        public int peek(int stackNum) {
            if (isEmpty(stackNum)) {
                return -1;
            }
            return stackArray[points[stackNum]];
        }

        public boolean isEmpty(int stackNum) {
            if (points[stackNum] == eachStackSize * (stackNum + 1)) {
                return true;
            }
            return false;
        }
    }
```

## LCCI.03.02 Min Stack 

题目：

> How would you design a stack which, in addition to push and pop, has a function min which returns the minimum element? Push, pop and min should all operate in 0(1) time.
>

这题我的思路是通过数组实现一个堆栈，要求在O(1)时间内返回pop和min，那么就只有用一个指针来记录栈顶和栈中最小值得位置。这么实现的后果是导致这个指针的维护比较复杂，从而在提交的时候报了好几次错。

```java
class MinStack {

        private int[] array = new int[3];
        private int topPoint = array.length;
        private int minPoint = array.length - 1;

        /**
         * initialize your data structure here.
         */
        public MinStack() {

        }

        public void push(int x) {
            if (topPoint == 0) {
                int[] temp = new int[array.length * 2];
                System.arraycopy(array, 0, temp, temp.length - array.length, array.length);
                topPoint = temp.length - array.length;
                minPoint = minPoint + temp.length - array.length;
                array = temp;
            }
            topPoint = topPoint - 1;
            array[topPoint] = x;
            if (x < array[minPoint]) {
                minPoint = topPoint;
            }
        }

        public void pop() {
            if (minPoint == topPoint && minPoint < array.length - 1) {
                minPoint = minPoint + 1;
                for (int i = minPoint ; i < array.length; i++) {
                    if (array[i] < array[minPoint]) {
                        minPoint=i;
                    }
                }
            }
            topPoint = topPoint + 1;
        }

        public int top() {
            return array[topPoint];
        }

        public int getMin() {
            return array[minPoint];
        }
}
```

我看书本上这题用的是java本身的Stack类来实现的。再用额外的栈来保存最小值。这样实现起来会简单很多。

## LCCI.03.03 Stack of Plates

题目：

> Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stack exceeds some threshold. Implement a data structure SetOfStacks that mimics this. SetOfStacks should be composed of several stacks and should create a new stack once the previous one exceeds capacity. SetOfStacks.push() and SetOfStacks.pop() should behave identically to a single stack (that is, pop() should return the same values as it would if there were just a single stack). Follow Up: Implement a function popAt(int index) which performs a pop operation on a specific sub-stack.
>
> You should delete the sub-stack when it becomes empty. pop, popAt should return -1 when there's no element to pop.
>

这题的思想比较简单，但是代码实现较为复杂。我写了很久没能够通过所有的测试用例。最后参考题解里的答案才写了出来。基本思路跟我的是类似的，但是我在代码实现的时候没能够处理好。

```java
class StackOfPlates {
        private int cap;
        private List<Stack> stack = new ArrayList<>();

        public StackOfPlates(int cap) {
            stack.add(new Stack<>());
            this.cap = cap;
        }

        public void push(int val) {
            if (cap <= 0) {
                return;
            }
            if (stack.isEmpty()||stack.get(stack.size()-1).size() == cap) {
                stack.add(new Stack<>());
            }
            stack.get(stack.size()-1).push(val);
        }

        public int pop() {
            return popAt(stack.size()-1);
        }

        public int popAt(int index) {
            if (index<0 || index >= stack.size()) {
                return -1;
            }
            Stack<Integer> indexStack = stack.get(index);
            if (indexStack.isEmpty()) {
                return -1;
            }
            int val = indexStack.pop();
            if (indexStack.isEmpty()) {
                stack.remove(indexStack);
            }
            return val;
        }
    }
```

## LCCI.03.04 Implement Queue using Stacks

题目：

> Implement a MyQueue class which implements a queue using two stacks.

这题的关键在于队列是先入先出的顺序，而堆栈是后入先出的顺序。因此使用两个堆栈，一个堆栈用于队列的pop和peek，另个队列用于队列的push。这样在需要出队时，只需要将用于push的堆栈出栈再入栈到另个堆栈中。最后使用另个堆栈完成pop和peek即可。

```java
 class MyQueue {

        private Stack<Integer> oldStack = new Stack<>();
        private Stack<Integer> newStack = new Stack<>();

        /**
         * Initialize your data structure here.
         */
        public MyQueue() {

        }

        /**
         * Push element x to the back of queue.
         */
        public void push(int x) {
            newStack.push(x);
        }

        /**
         * Removes the element from in front of queue and returns that element.
         */
        public int pop() {
            if (oldStack.isEmpty()) {
                while (!newStack.isEmpty()) {
                    oldStack.push(newStack.pop());
                }
            }
            return oldStack.pop();
        }

        /**
         * Get the front element.
         */
        public int peek() {
            if (oldStack.isEmpty()) {
                while (!newStack.isEmpty()) {
                    oldStack.push(newStack.pop());
                }
            }
            return oldStack.peek();
        }

        /**
         * Returns whether the queue is empty.
         */
        public boolean empty() {
            return oldStack.isEmpty() && newStack.isEmpty();
        }
    }
```

## LCCI.03.05 Sort of Stacks

题目：

> Write a program to sort a stack such that the smallest items are on the top. You can use an additional temporary stack, but you may not copy the elements into any other data structure (such as an array). The stack supports the following operations: push, pop, peek, and isEmpty. When the stack is empty, peek should return -1.
>

这题跟上面题目类似，采用额外的一个堆栈来存储一部分数据。因为排序的堆栈栈顶是最小的，因此可以用另外个堆栈存小于push的值得数据。然后再把小于的那部分值pop出来，加入到排序堆栈中。一开始我在每次push完后都会将小于当前push值得数据都放回到排序堆栈中，后来看了题解，可以采用懒加载的方式。只在需要pop和peek的时候才做这些工作。这样使得我的运行时间得到了很大的优化，从205ms降到了27ms。

```java
class SortedStack {
        private Stack<Integer> sortedStack = new Stack<>();
        private Stack<Integer> tempStack = new Stack<>();

        public SortedStack() {

        }

        public void push(int val) {
            if (sortedStack.isEmpty() && tempStack.isEmpty()) {
                sortedStack.push(val);
                return;
            }
            while (sortedStack.peek() < val) {
                tempStack.push(sortedStack.pop());
                if (sortedStack.isEmpty()) {
                    break;
                }
            }
            while (!sortedStack.isEmpty() && !tempStack.isEmpty() && sortedStack.peek() > val &&
                    tempStack.peek() > val) {
                sortedStack.push(tempStack.pop());
            }
            sortedStack.push(val);

        }

        public void pop() {
            while (!tempStack.isEmpty()) {
                sortedStack.push(tempStack.pop());
            }
            if (sortedStack.isEmpty()) {
                return;
            }
            sortedStack.pop();
        }

        public int peek() {
            while (!tempStack.isEmpty()) {
                sortedStack.push(tempStack.pop());
            }
            if (sortedStack.isEmpty()) {
                return -1;
            }
            return sortedStack.peek();
        }

        public boolean isEmpty() {
            return sortedStack.isEmpty();
        }
}
```

## LCCI.03.06 Animal Shelter 

题目：

> An animal shelter, which holds only dogs and cats, operates on a strictly"first in, first out" basis. People must adopt either the"oldest" (based on arrival time) of all animals at the shelter, or they can select whether they would prefer a dog or a cat (and will receive the oldest animal of that type). They cannot select which specific animal they would like. Create the data structures to maintain this system and implement operations such as enqueue, dequeueAny, dequeueDog, and dequeueCat. You may use the built-in Linked list data structure.
>
> enqueue method has a animal parameter, animal[0] represents the number of the animal, animal[1] represents the type of the animal, 0 for cat and 1 for dog.
>
> dequeue* method returns [animal number, animal type], if there's no animal that can be adopted, return [-1, -1].
>

这题比较简单，思路就是使用LinkedList来将保存数组。然后在取出的时候，遍历链表判断是猫是狗，再将取出的节点删除即可。

```java
class AnimalShelf {

        private LinkedList<int[]> shelter = new LinkedList<>();
        private int[] negative = new int[]{-1, -1};

        public AnimalShelf() {

        }

        public void enqueue(int[] animal) {
            shelter.add(animal);
        }

        public int[] dequeueAny() {
            if (shelter.isEmpty()) {
                return negative;
            }
            return shelter.removeFirst();
        }

        public int[] dequeueDog() {
            if (shelter.isEmpty()) {
                return negative;
            }
            for (int i = 0; i < shelter.size(); i++) {
                if (shelter.get(i)[1] == 1) {
                    return shelter.remove(i);
                }
            }
            return negative;
        }

        public int[] dequeueCat() {
            if (shelter.isEmpty()) {
                return negative;
            }
            for (int i = 0; i < shelter.size(); i++) {
                if (shelter.get(i)[1] == 0) {
                    return shelter.remove(i);
                }
            }
            return negative;
        }
}
```

有一个需要注意的点就是，LinkedList是可以用来保存数组的，而且不需要是包装类型`Integer`，直接是原始类型数组即可`int[]`。

## LCCI.04.01 Route Between Nodes

题目：

> Given a directed graph, design an algorithm to find out whether there is a route between two nodes.
>

这题是典型的图的搜索应用场景，可以使用BFS和DFS。使用BFS可以尽快的搜索到需要的目标，因此这题我采用的是BFS。BFS有着典型的实现方式模板，这题我就是套用了模板。

```java
public boolean findWhetherExistsPath(int n, int[][] graph, int start, int target) {
        LinkedList<Integer> breathList = new LinkedList<>();
        HashMap<Integer, Set<Integer>> temp = new HashMap<>();
        for (int[] ints : graph) {
            int key = ints[0];
            Set<Integer> valueList = temp.computeIfAbsent(key,r->new HashSet<>());
            valueList.add(ints[1]);
        }
        Set<Integer> nodeSet = new HashSet<>();
        nodeSet.add(start);
        breathList.add(start);
        while (!breathList.isEmpty()) {
            Integer first = breathList.removeFirst();
            if (first == target) {
                return true;
            }
            //从图中获取该节点的所有相关点
            Set<Integer> adjacent = temp.get(first);
            if (adjacent == null) {
                continue;
            }
            for (Integer i : adjacent) {
                if (!nodeSet.contains(i)) {
                    if (i == target) {
                        return true;
                    } else {
                        nodeSet.add(i);
                        breathList.add(i);
                    }
                }
            }
            nodeSet.add(first);
        }
        return false;
    }
```

## LCCI.04.02 Minimum Height Tree 

题目：

> Given a sorted (increasing order) array with unique integer elements, write an algo­rithm to create a binary search tree with minimal height.
>

这题我参考了书本上的解答，才发现可以这么用递归来解决。有点类似与二分法的解答：

```java
public TreeNode sortedArrayToBST(int[] nums) {
        return createTreeNode(nums, 0, nums.length - 1);
    }

    TreeNode createTreeNode(int[] nums, int start, int end) {
        if (end < start) {
            return null;
        }
        int midIndex = (start + end) / 2;
        TreeNode mid = new TreeNode(nums[midIndex]);
        mid.left = createTreeNode(nums, start, midIndex - 1);
        mid.right = createTreeNode(nums, midIndex + 1, end);
        return mid;
    }
```

## LCCI.04.03 List of Depth

题目：

> Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you'll have D linked lists). Return a array containing all the linked lists.
>

我的思路是按照广度优先的算法进行遍历，然后将每一层的数据放入到list中。实现起来比我想象的要难一些：

```java
public ListNode[] listOfDepth(TreeNode tree) {
        List<ListNode> res = new ArrayList<>();
        ListNode depthPoint = new ListNode(tree.val);
        TreeNode head = tree;
        Queue<TreeNode> nextDepList = new LinkedList<>();
        Queue<TreeNode> tempnextDepList = new LinkedList<>();
        nextDepList.add(head);
        while (!nextDepList.isEmpty()) {
            tempnextDepList = new LinkedList<>();
            TreeNode poll = nextDepList.poll();
            if (poll.left!=null) {
                tempnextDepList.add(poll.left);
            }
            if (poll.right!=null) {
                tempnextDepList.add(poll.right);
            }
            depthPoint = new ListNode(poll.val);
            ListNode a = depthPoint;
            for (TreeNode treeNode : nextDepList) {
                if (treeNode.left!=null) {
                    tempnextDepList.add(treeNode.left);
                }
                if (treeNode.right!=null) {
                    tempnextDepList.add(treeNode.right);
                }
                a.next = new ListNode(treeNode.val);
                a = a.next;
            }
            res.add(depthPoint);
            nextDepList = tempnextDepList;
        }
        ListNode[] resArray = new ListNode[res.size()];
        res.toArray(resArray);
        return resArray ;
    }
```

## LCCI.04.04 Check Balance

题目：

> Implement a function to check if a binary tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differ by more than one.
>

一开始想的是用DFS做，看了提示后觉得可以用递归来实现。但是一直没有完成将结果返回出来的处理。参考书本后写出了传递结果的处理：

```java
    public boolean isBalanced(TreeNode root) {
        int i = checkHeight(root, 0);
        return i != Integer.MIN_VALUE;
    }

    private int checkHeight(TreeNode node, int k) {
        if (node == null) {
            return k;
        }
        k++;
        int left = checkHeight(node.left, k);
        if (left == Integer.MIN_VALUE) {
            return Integer.MIN_VALUE;
        }
        int right = checkHeight(node.right, k);
        if (right == Integer.MIN_VALUE) {
            return Integer.MIN_VALUE;
        }
        int highAbs = Math.abs(left - right);
        if (highAbs > 1) {
            return Integer.MIN_VALUE;
        }
        return left > right ? left : right;
    }
```

## LCCI.04.05 Legal Binary Search Tree

题目：

> Implement a function to check if a binary tree is a binary search tree.

这题有两个关键点，第一点是要理解清楚二叉搜索树的定义。二叉搜索树要求：**所有**左边的节点小于或等于当前节点，而当前节点必须小于**所有**右边的节点。

有了这个条件，我们就可以写出满足二叉搜索树的条件：`currentNode.left.max<=currentNode<currentNode.right.min`

我先参考了书本上的题解，是一种自顶而下的方法。我自己写了个自底向上的解法，需要将返回数据都封装在一个类里，进行结果返回给上一层处理：

```java

    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;//测试用例中根节点为null时返回true
        }
        PacValue res = isBST(root, null, root.val);
        return res.isRes();
    }
	/**
	 * 用于封装返回结果数据的类
	 */
    class PacValue {
        private int max;
        private int min;
        private boolean res = true;

        public int getMax() {
            return max;
        }

        public void setMax(int max) {
            this.max = max;
        }

        public int getMin() {
            return min;
        }

        public void setMin(int min) {
            this.min = min;
        }

        public boolean isRes() {
            return res;
        }

        public void setRes(boolean res) {
            this.res = res;
        }
    }

    PacValue isBST(TreeNode node, Boolean isMax, Integer preNodeVal) {
        PacValue value = new PacValue();
        if (node == null) {
            return null;//递归返回条件
        }
        PacValue left = isBST(node.left, true, node.val);
        if (left != null && !left.isRes()) {
            value.setRes(false);//左子树已不满足二叉搜索树时，直接向上传递结果
            return value;
        }
        PacValue right = isBST(node.right, false, node.val);
        if (right != null && !right.isRes()) {
            value.setRes(false);//右子树已不满足二叉搜索树时，直接向上传递结果
            return value;
        }
        //<editor-fold desc="对于基线条件的处理">
        if (left == null && right == null) {
            value.setMax(node.val);//左右子树都为空，则最大最小值为当前节点的值
            value.setMin(node.val);
            value.setRes(true);
        }
        if (left == null && right != null) {
            /**
             * 左子树为空，右子树不为空，若满足当前node小于右子树的最小值，则仍满足二叉搜索树的属性
             * 此时则最大值为右子树的最大值，最小值为当前节点值
             * 注意：此时若不满足属性，则最大值填了右子树最大值是错的，
             * 但是这样没有关系，因为一旦不满足属性递归不会再做处理直接层层向上传递false的结果
             */
            value.setMax(right.getMax());
            value.setMin(node.val);
            value.setRes(node.val < right.getMin());
        }
        if (left != null && right == null) {//类似上一种情况
            value.setMax(node.val);
            value.setMin(left.getMin());
            value.setRes(node.val > left.getMax());
        }
        if (left != null && right != null) {
            /**
             * 左右子树都不为空，则按照当前节点小于右子树的最小值，并大于左子树的最大值来进行判断是否
             * 满足二叉搜索树的属性
             * 此时，若满足属性则最小值为左子树的最小值，最大值为右子树的最大值。
             * 若不满足属性，此时的最大最小值设置是错的，但是没有关系，不满足属性的结果会直接向上传递。
             */
            value.setRes(node.val > left.getMax() && node.val < right.getMin());
            value.setMin(left.getMin());
            value.setMax(right.getMax());
        }
        //</editor-fold>
        return value;
    }
```







## 经验总结

- 最重要的是要理解清楚题目的意思！！！
- 字符串的调换位置、去重、判断奇偶操作这种类型的题目可以考虑将采取哈希映射为数组，或者bit位。用位操作来完成最后的识别判断。注意，当有顺序要求，不能去重统计时则不适用于哈希映射法。
- 双指针类问题，需要仔细分清不同 代码分支情况，一条条的梳理清楚。
- 对字符数组遍历，边界问题的测试需要考虑到字符长度为1，为0，遍历到尾部最后一个字符的处理逻辑；可以尝试通过在字符的最后补了一位来规避遍历时处理最后一个字符的特殊情况。
- 链表的删除可以考虑使用后续节点代替当前节点
- 在两个引用指向相同对象的，若想对两个对象内的数值进行互换不能采用异或操作。否则会导致结果为0。
- 遇到栈相关的问题，一般可以考虑使用临时的另一个栈来完成题目对于栈的顺序要求。
- ArrayList和LinkedList的toArray方法并不能直接转换为数组，否则会报错。可以新建个数组，在使用`toArray(新数组)`的方法来实现转换为数组。
- 递归可以通过返回特定值来向上传递某种结果退出递归。
- **递归题目，首要条件是要找到基准条件。找到之后可以分为自底向上和自顶而下两种写法，自底向上是将每一层的处理结果返回给上一层处理，每层在调用完递归后处理自己本层逻辑再返回。自顶而下则是先对自己本层的逻辑进行处理，然后再将数据封装到参数中递交给下一层处理。不管是自底向上还是自顶而下，都需要对返回条件进行特殊处理！**