---
title: LeetCode 1.Two Sum
date: 2019-06-07 20:00:21
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
## LeetCode 1. Two Sum [两数之和](https://leetcode-cn.com/problems/two-sum/)
给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



LeetCode解题写博客的旅程开始了，虽然网上各个论坛中写LeetCode题解的博主相当多，但是总觉得如果一道题不能给别人说明白，那么自己对这道题的理解就还是有不到位的地方，所以在此写出自己的理解。

Two-sum作为Leetcode中的经典，暴力解法是很容易想到的，当然想要通过是不可能的，试了下果然是超出时间限制。那么如何把时间从O(n^2)变为O(n)呢？用HashMap。

HashMap的数据结构是数组＋链表（过长使用红黑树）。每个key值经过哈希算法寻找到数组对应位置，如果有多个key值hash值相同那么就会在这个位置上使用链表来记录value值。这样HashMap是常数级的查找效率，我们每次遍历给定数组nums的时候查找`target-nums[i]`是否在HashMap中，如果在就找到的解，不在就把该值放到HashMap中。

Java题解：

```java
private int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> hashMap = new HashMap<>();
    int[] result = new int[2];
    for (int i = 0; i < nums.length; i++) {
        int num2 = target - nums[i];
        Integer index;
        if ((index = hashMap.get(num2)) != null) {
            result[1] = i;
            result[0] = index;
            break;
        } else {
            hashMap.put(nums[i], i);
        }
    }
    return result;
}
```