---
title: 394. 字符串解码
subtitle:
date: 2025-05-04T16:01:45+08:00
slug: 84d7cc5
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
  - 栈
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

## [题目](https://leetcode.cn/problems/decode-string/description/?envType=study-plan-v2&envId=top-100-liked)

![图1](/PostsImgs/LeetCode/394/question.png)

## 思路

该题就是给定一个编码后的字符串，然后返回解码后的字符串。

由题可知要处理嵌套的情况，即要先解决内层的$k[encoded_string]$，再解决外层，即后进先出，很适合用**栈**来解决。而且其结构为$k[encoded_string]$，很适合用双栈来解决，一个为**数字栈**用来存储重复几次，一个为**字符串栈**用来存储每一层进入 $[$ 时，之前构造的字符串。

## 代码

```cpp
class Solution
{
 public:
  string decodeString(string s)
  {
    string cur_str, pre_str, temp;
    stack<int> s_n;     // 数字栈，存储重复几次
    stack<string> s_s;  // 字符串栈，存储前面的字符串

    for (size_t i = 0; i < s.size(); ++i)
    {
      if (s[i] >= 'a' && s[i] <= 'z')
      {
        cur_str += s[i];
      }
      else
      {
        // 处理当前层
        if (s[i] == ']')
        {
          pre_str = s_s.top();
          s_s.pop();

          int repeatTimes = s_n.top();
          s_n.pop();

          temp.clear();
          for (int i = 0; i < repeatTimes; ++i)
          {
            temp += cur_str;
          }

          cur_str = pre_str + temp; // 构造当前处理完的字符串
        }
        else
        {
          // 数字
          int t_num = 0;
          while (s[i] != '[')
          {
            t_num = t_num * 10 + (s[i] - '0');
            ++i;
          }

          s_n.push(t_num);
          s_s.push(cur_str);
          cur_str.clear();
        }
      }
    }

    return cur_str;
  }
};
```
