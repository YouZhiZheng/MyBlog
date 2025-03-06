---
title: 41. 缺失的第一个正数
subtitle:
date: 2025-03-06T16:19:50+08:00
slug: d0633e2
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

## [题目](https://leetcode.cn/problems/first-missing-positive/description/?envType=study-plan-v2&envId=top-100-liked)

![图1](/PostsImgs/LeetCode/41/question.png)

## 思路

这道题的难点就在于要求时间复杂度为$O(n)$，空间复杂度为$O(1)$。根据题目可知我们要找到的数必定在区间 $[1, len + 1]$里，当且仅当 $1$ 到 $len$ 都出现过，要寻找的数才为$len + 1$。所以，我们可以先判断$1$ 到 $len$ 的数是否都出现过了。

由于空间复杂度必须为$O(1)$，所以借用$nums$数组本身来记录$1$ 到 $len$的出现情况，如果$[1, len]$中的某个数 x 出现过，我们就用 $nums[x - 1]$ 这个空间来记录其已经出现过了。最后再遍历 nums 数组，查看哪个正整数没有出现过，即看区间$[1, len]$中的哪个正整数没有出现过，如果  $1$ 到 $len$ 都出现过，那么缺失的第一个正整数就是 $len + 1$。

具体来说，我采用的记录方式为 若区间$[1, len]$中的 x 出现过那么将 $nums[x - 1]$ 变为 $-nums[x - 1]$(注意不要重复处理，比如两次相反就变回正数了)，后续遍历时，遍历到下标为 $i$时，若 $nums[i]$ 为负值，则表明 $i + 1$ 这个值出现过。所以，我们要先处理负值，将数组中的 **全部负值** 变为 $len + 1$(变为 0 或大于 len的任意数都行)。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution
{
 public:
  int firstMissingPositive(vector<int>& nums)
  {
    int len = nums.size();
    // 处理负数和0的情况
    for (int i = 0; i < len; ++i)
    {
      if (nums[i] <= 0)
      {
        nums[i] = len + 1;
      }
    }

    // 标记区间 [1,len] 里的整数
    for (int i = 0; i < len; ++i)
    {
      int abs_num = abs(nums[i]); // 这里取绝对值是因为这个位置可能被标记了
     
     //  nums[abs_num - 1] > 0 是为了避免重复处理的情况, 比如数组中 3 出现次数大于1次这种情况
      if (abs_num <= len && nums[abs_num - 1] > 0)
      {
        nums[abs_num - 1] = -nums[abs_num - 1];
      }
    }

    // 寻找没有出现过的最小正整数
    for (int i = 0; i < len; ++i)
    {
      // 判断这个位置对应的值，即 i + 1， 是否出现过
      if (nums[i] > 0)
      {
        return i + 1;
      }
    }

    return len + 1; // 前面没有return 说明 1 ~ len 都出现过
  }
};
```
