---
title: 53. 最大子数组和
subtitle:
date: 2025-03-01T10:14:12+08:00
slug: 3930252
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
  - 动态规划
  - 前缀和
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

## [题目](https://leetcode.cn/problems/maximum-subarray/?envType=study-plan-v2&envId=top-100-liked)

![图1](/PostsImgs/LeetCode/53/question.png)

## 思路

看到题目说寻找**最大和的连续子数组**，想到了 **「560. 和为 K 的子数组」** 中的思路 **前缀和**。对于任意一个区间$[i, j]$（$i \le j$）来说其和为：
$$
sum[i, j] = preSum[j] - preSum[i - 1]
$$
我们的目标是**找到一个和最大的连续子数组**，即：
$$
max(preSum[j] - preSum[i - 1])，j \in [0, nums.size() - 1]
$$
我们只需计算出每一个$preSum[j] - preSum[i - 1]$的值，取其中最大值作为答案。但对$preSum[j]$来说我们需要让其减去每一个$preSum[i - 1]$吗？并不需要，因为我们需要的是**差值的最大值**，所以只需要用一个变量$preSumMin$来记录最小的$preSum[i - 1]$，然后只需计算每一个 $preSum[j] - preSumMin$，取其中的最大差值为答案即可。

## 代码

```cpp
class Solution
{
 public:
  int maxSubArray(vector<int>& nums)
  {
    int preSumMin = 0, preSum = 0, ans = -1e5;

    for (int i = 0; i < nums.size(); ++i)
    {
      preSum += nums[i]; // 计算当前前缀和
      ans = max(ans, preSum - preSumMin); // 更新最大和
      preSumMin = min(preSumMin, preSum); // 更新最小的 preSum[i - 1]
    }
    return ans;
  }
};
```
