---
title: LeetCode 2. Add Two Numbers
date: 2019-06-17 22:00:21
categories:

- LeetCode
tags:
- LeetCode 
- Arrays
- Hash Table
---

* 本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 协议，转载请注明出处。
* [LeetCode](https://leetcode-cn.com/) 练习

---
## LeetCode 2. Add Two Numbers [两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例:**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```



从题干中可以看出，所给链表按照 **逆序** 的方式，所以其实就是从个位开始做加法，超过10的进位。从两个链表的第一位开始，示例中2+5=7，存入返回的链表第一位；接着再做第二位相加，4+6=10，进一位；第三位相加时就是3+4+1=8。



Java题解1：

自己写的，按照上述思路可以得到正解。

```java
/**
 * 循环末尾判断，两数都取完并没有进位
 * 
 * @param l1
 * @param l2
 * @return
 */
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode result = null;
    ListNode resulttemp = null;
    ListNode temp1 = l1;
    ListNode temp2 = l2;
    int temp = 0;
    while (true) {
        int temp1val = temp1 == null ? 0 : temp1.val;
        int temp2val = temp2 == null ? 0 : temp2.val;
        int sum = temp1val + temp2val + temp;
        temp = sum / 10;
        if (result == null) {
            result = new ListNode(sum % 10);
            resulttemp = result;
        } else {
            resulttemp.val = sum % 10;
        }
        temp1 = temp1 == null ? null : temp1.next;
        temp2 = temp2 == null ? null : temp2.next;
        if (temp == 0 && temp1 == null && temp2 == null) {
            break;
        } else {
            resulttemp.next = new ListNode(0);
            resulttemp = resulttemp.next;
        }
    }
    return result;
}
```



Java题解2：

官方题解

优化点：

1.使用虚拟头结点，小技巧使代码简洁很多,并且容易写bug free的code；

2.循环的判断条件修改，省的题解1在每个循环体结束前做一次判断。

```java
/**
 * 只要两个数有一个没有取完，则循环继续，结束之后判断是否有进位，加上
 * 
 * @param l1
 * @param l2
 * @return
 */
public ListNode addTwoNumbers2(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0); // 虚拟头结点
    int sum = 0;
    ListNode cur = dummyHead;
    while (l1 != null || l2 != null) {
        if (l1 != null) {
            sum += l1.val;
            l1 = l1.next;
        }
        if (l2 != null) {
            sum += l2.val;
            l2 = l2.next;
        }
        cur.next = new ListNode(sum % 10);
        sum /= 10;
        cur = cur.next;
    }
    if (sum == 1) {
        cur.next = new ListNode(sum);
    }
    return dummyHead.next;
}
```

