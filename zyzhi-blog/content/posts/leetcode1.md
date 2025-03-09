---
title: 1. 两数之和
subtitle:
date: 2025-02-17T19:39:01+08:00
slug: 7d4248c
draft: false
author:
  name: zyz
  link:
  email:
  avatar:
description: leetcode第1题
keywords:
license:
comment: true
weight: 0
tags:
  - 数组
  - 哈希表
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

[题目描述](https://leetcode.cn/problems/two-sum/description/?envType=study-plan-v2&envId=top-100-liked)：

![图1](/PostsImgs/LeetCode/1/question.png)

## 思路

时间复杂度为 **$O(n^2)$** 的算法思路很简单，直接采用暴力法使用两层循环遍历每一种可能，判断是否存在答案。

这里详细说明一下时间复杂度为 **$O(n)$** 的算法，暴力法之所以慢是因为寻找 `target - x` 的时间复杂度过高。那有没有什么方法可以加速寻找元素 `target - x` 是否存在且存在时可以知道其索引呢？有的，那就是使用哈希表来做，以`target - x` 为`key`，以索引为`value`。这样，寻找`target - x`的时间复杂度就变为了 **$O(1)$**，极大地提高了效率。

具体来说，对于遍历的每个元素，先判断其`target - x`是否存在，若存在，则直接返回对应的下标；若不存在，则将`x`存入哈希表，方便后续的查找。

## 代码

```cpp
// 暴力法
class Solution
{
 public:
  vector<int> twoSum(vector<int>& nums, int target)
  {
    for (int i = 0; i < nums.size(); ++i)
    {
      for (int j = i + 1; j < nums.size(); ++j)
      {
        if ((nums[i] + nums[j]) == target)
        {
          return { i, j };
        }
      }
    }
    return {};
  }
};

// 哈希表法
class Solution
{
 public:
  vector<int> twoSum(vector<int>& nums, int target)
  {
    unordered_map<int, int> hashtable;
    for (int i = 0; i < nums.size(); ++i)
    {
      unordered_map<int, int>::iterator it = hashtable.find(target - nums[i]);
      if (it != hashtable.end())
      {
        return { it->second, i };
      }
      hashtable[nums[i]] = i;
    }
    return {};
  }
};
```
