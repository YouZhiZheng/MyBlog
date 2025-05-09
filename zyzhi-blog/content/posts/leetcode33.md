---
title: 33. 搜索旋转排序数组
subtitle:
date: 2025-04-25T10:12:40+08:00
slug: 8c42677
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

## [题目](https://leetcode.cn/problems/search-in-rotated-sorted-array/description/?envType=study-plan-v2&envId=top-100-liked)

![图1](/PostsImgs/LeetCode/33/question.png)

## 思路

最开始想的是先通过二分来找到最大值位置，确定$right$和$left$的值，再使用一次二分来进行查询。但仔细观察题目发现只需进行一次二分查找算法就可以解决问题了，因为 **「该旋转后的数组从中间分开后，左右部分中必定有一个部分是有序的」**。

那么我们就可以借助这个有序数组来直接进行二分查找，采用下面的方式来更新$left$和$right$的值：

- 左边部分有序
  - target在这个有序区间内，则可以直接抛弃右边部分。反之，抛弃左边部分。
- 右边部分有序
  - target在这个有序区间内，则可以直接抛弃左边部分。反之，抛弃右边部分。

## 代码

```cpp
class Solution
{
 public:
  int search(vector<int>& nums, int target)
  {
    int mid, left = 0;
    int right = nums.size() - 1;

    while (left <= right)
    {
      mid = ((right - left) >> 1) + left;
      if (target == nums[mid])
      {
        return mid;
      }

      // 判断左边是否有序, 这里是 <= 是为了处理左边只有一个元素的情况
      if (nums[left] <= nums[mid])
      {
        // 左边有序
        if (nums[left] <= target && nums[mid] > target)
        {
          // 在区间内
          right = mid - 1;
        }
        else
        {
          left = mid + 1;
        }
      }
      else
      {
        // 右边有序
        if (nums[mid] < target && target <= nums[right])
        {
          // 在区间内
          left = mid + 1;
        }
        else
        {
          right = mid - 1;
        }
      }
    }

    return -1;
  }
};
```
