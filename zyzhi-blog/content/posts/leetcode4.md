---
title: 4. 寻找两个正序数组的中位数
subtitle:
date: 2025-04-28T10:27:02+08:00
slug: 6567d9a
draft: false
author:
  name: zyz
  link:
  email:
  avatar:
description:
keywords:
license:
comment: true
weight: 0
tags:
  - 数组
  - 二分查找
  - LeetCodeHot100
categories:
  - 算法
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRss: false
hiddenFromRelated: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: true
lightgallery: force
password:
message:
repost:
  enable: false
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

## [题目](https://leetcode.cn/problems/median-of-two-sorted-arrays/description/?envType=study-plan-v2&envId=top-100-liked)

![图1](/PostsImgs/LeetCode/394/question.png)

## 思路

暴力法，先计算出中位数是第几个元素，然后使用双指针分别指向两个数组的开头元素，然后不断将指向较小元素的指针往后移，直到移到中位数即可，时间复杂度为$O(m + n)$，不符合题目要求。

进一步思考中位数的作用是什么？中位数的作用就是:
> 将一个集合划分为两个等长的子集，且其中一个子集的元素总是小于另一个子集的元素。

所以，对于这道题来说，我们需要分别在两个数组中找到一条分割线，使得这两条分割线左边的元素个数等于右边的元素个数，且左边集合的最大值小于等于右边集合的最小值。

切割好后，如果两数组长度和为偶数，那么中位数为：
$$
median = \frac{max(left\\_part) + min(right\\_part)}{2}
$$

长度和为奇数（为了便于处理，我们将中位数划分给**左边**部分。当然也可以划分给右边部分），那么中位数为：
$$
median = max(left\\_part)
$$

确定好怎么计算中位数后，我们只需要考虑怎么去寻找这两条切割线了。首先，设数组A的分割线位置为$i$，数组B的分割线位置为$j$（即数组A中下标为$[0, i)$的元素属于其左部分，$[i, lenA)$属于其右部分，数组B同理），根据上面的分析可知：
$$
\begin{cases}
i + j = m - i + n - j & \text{当 } m+n \text{ 为偶数} \\\
i + j = m - i + n - j + 1 & \text{当 } m+n \text{ 为奇数}
\end{cases}
\Rightarrow
\begin{cases}
i + j = \frac{m + n}{2} & \text{当 } m+n \text{ 为偶数} \\\
i + j = \frac{m + n + 1}{2} & \text{当 } m+n \text{ 为奇数}
\end{cases}
$$

又$m$和$n$均是整数，由整除可知 $\frac{m + n}{2} = \frac{m + n + 1}{2}$，故$i + j = \frac{m + n + 1}{2}$。只要我们确定了$i$值就能确定$j$值，需要注意的是当**数组A的长度大于数组B的长度**（即$m > n$）时，$j$可能不是有效值。所以，当$m > n$时，我们需要交换两个数组来确保$j$的取值**一定是有效**的（可通过数学放缩法来证明$j$一定在$[0, n]$内）。

这道题时间复杂度要求为$O(log(m+n))$，所以我们只需在数组A上通过二分法找到$i$的位置，就能确定$j$的位置，从而解决这道题。因为我们通过上面的公式已经确保了$left\\_part = right\\_part$（$left\\_part = left\\_A + left\\_B$），接下来只需要使用二分法不断移动$i$，使得下面的不等式成立即可：
$$
\begin{cases}
max(left_A) \le min(right_B)  \\\
max(left_B) \le min(right_A)
\end{cases}
\Rightarrow
max(left_part) \le min(right_part)
$$

## 代码

```cpp
class Solution
{
 public:
  double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2)
  {
    int m = nums1.size();
    int n = nums2.size();

    if (m > n)
    {
      // 确保j不会越界
      return findMedianSortedArrays(nums2, nums1);
    }

    // 注意这里的 right 不能为nums1.size() - 1
    // 因为 left和right表示的是i的取值范围, i最大是能取到nums1.size()的
    int left = 0, right = nums1.size();
    int i, j, A_left_max, A_right_min, B_left_max, B_right_min;

    // 二分法查找i的位置
    while (left <= right)
    {
      i = ((right - left) >> 1) + left;
      j = (m + n + 1) / 2 - i;

      // 记得处理特殊情况, 即判断分割线左右两边是否存在元素
      A_left_max = (i == 0) ? INT_MIN : nums1[i - 1];
      A_right_min = (i == m) ? INT_MAX : nums1[i];
      B_left_max = (j == 0) ? INT_MIN : nums2[j - 1];
      B_right_min = (j == n) ? INT_MAX : nums2[j];

      // 判断是否满足 left_part <= right_part
      if (A_left_max <= B_right_min && B_left_max <= A_right_min)
      {
        if ((m + n) % 2 == 0)
        {
          // 偶数
          return (max(A_left_max, B_left_max) + min(A_right_min, B_right_min)) / 2.0;
        }
        else
        {
          // 奇数
          return max(A_left_max, B_left_max);
        }
      }
      else
      {
        if (A_left_max > B_right_min)
        {
          right = i - 1;
        }
        else
        {
          left = i + 1;
        }
      }
    }

    // 不会走到这一步
    return 0.0;
  }
};
```
