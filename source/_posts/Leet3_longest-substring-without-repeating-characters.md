---
title: LeetCode 3. Longest Substring Without Repeating Characters
date: 2019-07-06 16:00:21
categories:

- LeetCode
tags:
- String
- Two Pointers
- Hash Table
- Sliding Window
mathjax: true
---

* 本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 协议，转载请注明出处。
* [LeetCode](https://leetcode-cn.com/) 练习

---
## LeetCode 3. Longest Substring Without Repeating Characters [无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**示例 1**:
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**
```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
题干很容易看懂，直接用暴力法查找所有子串中最长的无重复字符子串这种方法就不写了，时间复杂度是$O(n^3)$，肯定是会超时的。暴力法中对所有子串进行检测会做很多重复的操作，如果我们能记录下$i$到$j-1$中没有重复的字符， 那么只要$s[j]$不在$i$到$j-1$中，那么$i$到$j$的子串中就没有重复的字符。这里我们就使用滑动窗口来记录$i$到$j-1$中的字符。



Java题解1：

按照上述思路可以得到正确的答案，但是耗时太长。使用HashSet记录字符，可以把查找的效率提高到$O(1)$。整个算法的时间复杂度为$O(n)$。

```java
/**
 * 使用set来一个新的字符判断是否存在，在的话就从这个窗口的头部开始把字符从set中一个个
 * 删去，不在的话就从添加进这个set
 * 
 * @param s
 * @return
 */
public int lengthOfLongestSubstring(String s) {
    int n = s.length();
    Set<Character> set = new HashSet<>();
    int ans = 0, i = 0, j = 0;
    while (i < n && j < n) {
        if (!set.contains(s.charAt(j))){
            set.add(s.charAt(j++)); // 添加当前字符
            ans = Math.max(ans, j - i);
        }
        else {
            set.remove(s.charAt(i++)); // 删除最前面的字符
        }
    }
    return ans;
}
```



Java题解2：

优化题解

优化点：

使用HashMap来记录某个字符串最后出现的位置，这样删除的时候就不需要一个个删除，$left$可以直接跳到与$s[i]$重复的那个字符串的位置。

```java
/**
 * 使用map记录字符最后一次出现的位置
 * 
 * @param s
 * @return
 */
public int lengthOfLongestSubstring2(String s) {
    int max = 0;
    int left = -1;
    HashMap<Character, Integer> hashMap = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        char charitem = s.charAt(i);
        if (hashMap.containsKey(charitem)) {
            left = Math.max(left, hashMap.get(charitem));
        }
        max = Math.max(i - left, max);
        hashMap.put(charitem, i);
    }
    return max;
}
```

