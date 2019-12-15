---
title: LeetCode 4. Median Of Two Sorted Arrays
date: 2019-07-16 20:00:21
categories:

- LeetCode
tags:
- Arrays
- Binary Search
- Divide And Conquer
mathjax: true
---

* 本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 协议，转载请注明出处。
* [LeetCode](https://leetcode-cn.com/) 练习

---
## LeetCode 4. Median Of Two Sorted Arrays [寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)
给定两个大小为 m 和 n 的有序数组 `nums1` 和 `nums2`。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 $O(log(m + n))$。

你可以假设 `nums1` 和 `nums2` 不会同时为空。

**示例 1:**

```
nums1 = [1, 3]
nums2 = [2]
```

则中位数是 2.0

**示例 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]
```

则中位数是 (2 + 3)/2 = 2.5



看到题目最容易想到的解决办法就是把这两个有序数组合并为一个有序数组，这样根据这一个有序数组就能很方便的找到中位数。

Java题解1：

按照上述思路可以得到下面的解法，合并为一个数组时间复杂度为$O(m+n)$。

```java
/**
 * 先把两个有序数组合并为一个有序数据，然后取中位数
 * 
 * @param s
 * @return
 */
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    int[] nums = new int[nums2.length + nums1.length];
    for (int i = 0, j = 0, k = 0; i < nums2.length + nums1.length; i++) {
        if (j < nums1.length && k < nums2.length) {
            if (nums1[j] > nums2[k]) {
                nums[i] = nums2[k];
                k++;
            } else {
                nums[i] = nums1[j];
                j++;
            }
        } else {
            if (j >= nums1.length) {
                nums[i] = nums2[k];
                k++;
            } else {
                nums[i] = nums1[j];
                j++;
            }
        }
    }
    return getMid(nums);
}

public double getMid(int[] nums) {
    int len = nums.length;
    double nums1mid;
    if (len % 2 == 0) {
        nums1mid = ((double) nums[len / 2 - 1] + nums[len / 2]) / 2;
    } else {
        nums1mid = nums[(int) Math.floor(len / 2)];
    }
    return nums1mid;
}
```



显然合并两个有序数组并不能被称为一道难度为Hard的题目，在$O(n)$级别下如果还有更好的，那看来就要寻找$O(log(n))$级别的更优解法。

Java题解2：

1.首先，我们在任一位置 i 将 A(长度为m) 划分成两个部分：

| leftA | rightA |
| ----- | ------ |
| A[0],A[1],... A[i-1] | A[i],A[i+1],...A[m - 1] |

2.同样，在任一位置 i 将 B(长度为n) 划分成两个部分：

| leftB | rightB |
| ----- | ------ |
| B[0],B[1],... B[j-1] | B[j],B[j+1],..B[n - 1] |

3.把leftA和leftB组合，把rightA和rightB组合得到：

| leftA+leftB | rightA+rightB |
| ----- | ------ |
| A[0],A[1],... A[i-1] | A[i],A[i+1],...A[m - 1] |
| B[0],B[1],... B[j-1] | B[j],B[j+1],...B[n - 1] |

4.这个时候如果能够满足以下两个条件：
  4.1 $i + j = m - i + n - j$(或$m - i + n - j + 1$)  如果$n >= m$。只需要使$i = 0 ~ m, j = (m+n+1)/2-i$ =====> 该条件在m+n为奇数/偶数时，该推理都成立；
  4.2 $B[j] >= A[i-1]$ 并且 $A[i] >= B[j-1]$。
这时就能找出中位数，$median = (max(leftPart) + min(rightPart)) / 2$

官方的提醒：
特殊情况处理：临界值 i=0,i=m,j=0,j=n，此时 A[i-1],B[j-1],A[i],B[j] 可能不存在。

在循环搜索中，我们只会遇到三种情况：

1.$(j = 0 or i = m or B[j−1]≤A[i])$ 或是$(i=0 or j = n or A[i−1]≤B[j])$，这意味着 $i$ 是完美的，我们可以停止搜索。
2.$j > 0 and i < m and B[j−1]>A[i]$ 这意味着 $i$ 太小，我们必须增大它。
3.$i > 0 and j < n and A[i−1]>B[j]$ 这意味着 $i$ 太大，我们必须减小它。


```java
/**
 * 官方解答 理解： 假设有A=[1,9,10,11],B=[2,3,4,8,12] m=4, n=5 确保m>n, m=5,n=4 把A分成两部分A1&A2
 * A1<A2 把B分成两部分B1&B2 B1<B2 把A1和B1放在一起 A2和B2放在一起 因为A1<A2,B1<B2,
 * 如果能够满足A1<B2,B1<A2,那么 {A1,B1}就小于 {A2,B2} 同时如果两边元素个数相同，或者相差1就找到了
 * 
 * iMin=0 iMax=5 halflen=5 i=2 j=3 
 * 把A1: 1,9 &B1: 2,3,4放在了一起 A2: 10,11 & B2: 8,12放在一起
 * 发现A1并不是小于B2(9>8) i-- i=1,j=4 
 * A1:1 B1:2,3,4,8 A2:9,10,11 B2:12 
 * 发现A1总是小于B2(1<12)，B1总是小于A2(8<9)
 * 这时leftPart有5，rightPart有4
 * 所以是左边的最后一个就是中位数 就是8
 * 
 * @param A
 * @param B
 * @return
 */
public double findMedianSortedArrays2(int[] A, int[] B) {
    int m = A.length;
    int n = B.length;
    if (m > n) { // to ensure m<=n
        int[] temp = A;
        A = B;
        B = temp;
        int tmp = m;
        m = n;
        n = tmp;
    }
    int iMin = 0, iMax = m, halfLen = (m + n + 1) / 2;
    while (iMin <= iMax) {
        int i = (iMin + iMax) / 2;
        int j = halfLen - i;
        if (i < iMax && B[j - 1] > A[i]) {
            iMin = i + 1; // i is too small
        } else if (i > iMin && A[i - 1] > B[j]) {
            iMax = i - 1; // i is too big
        } else { // i is perfect
            int maxLeft = 0;
            if (i == 0) {
                maxLeft = B[j - 1];
            } else if (j == 0) {
                maxLeft = A[i - 1];
            } else {
                maxLeft = Math.max(A[i - 1], B[j - 1]);
            }
            if ((m + n) % 2 == 1) {
                return maxLeft;
            }

            int minRight = 0;
            if (i == m) {
                minRight = B[j];
            } else if (j == n) {
                minRight = A[i];
            } else {
                minRight = Math.min(B[j], A[i]);
            }

            return (maxLeft + minRight) / 2.0;
        }
    }
    return 0.0;
}
```

