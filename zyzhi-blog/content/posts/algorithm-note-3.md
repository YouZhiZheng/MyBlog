---
title: 322. 零钱兑换
subtitle:
date: 2024-07-29T21:01:40+08:00
slug: bd6092d
draft: false
author:
  name: zyz
  link:
  email:
  avatar:
description: 使用动态规划解决零钱兑换问题
keywords: 动态规划
license:
comment: true
weight: 0
tags:
  - 动态规划
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
**题目描述([力扣](https://leetcode.cn/problems/coin-change/description/)):**  

![图1](/PostsImgs/algorithm_note_3_imgs/picture1.png)

**什么是最优子结构**
>最优子结构指的是，问题的最优解包含子问题的最优解。反过来说就是，可以通过子问题的最优解，推导出问题的最优解。要符合「最优子结构」，子问题间必须互相独立。
>
>比如说，假设你考试，每门科目的成绩都是互相独立的。你的原问题是考出最高的总成绩，那么你的子问题就是要把语文考到最高，数学考到最高…… 为了每门课考到最高，你要把每门课相应的选择题分数拿到最高，填空题分数拿到最高…… 当然，最终就是你每门课都是满分，这就是最高的总成绩。
>
>得到了正确的结果：最高的总成绩就是总分。因为这个过程符合最优子结构，「每门科目考到最高」这些子问题是互相独立，互不干扰的。
>
>但是，如果加一个条件：你的语文成绩和数学成绩会互相制约，不能同时达到满分，数学分数高，语文分数就会降低，反之亦然。
>
>这样的话，显然你能考到的最高总成绩就达不到总分了，按刚才那个思路就会得到错误的结果。因为「每门科目考到最高」的子问题并不独立，语文数学成绩户互相影响，无法同时最优，所以最优子结构被破坏。


**思路:** 题目说明了每种硬币数量无限，所以该问题具有最优子结构，可以用动态规划来解决。确定好用动态规划来解决后，先确定dp数组的意义，这里dp[i]存储的值就是总金额为i的最优解。接下来最重要的事情就是找到状态转移方程，dp[0] = 0，当amount大于0时，我们只需遍历给出的coins数组找到 dp[amount - coin_value] + 1 中的最小值即可(避免越界，需要先判断coin_value是否小于amount)，加一是因为要加上一个面值为coin_value的硬币。


**代码:**
```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) 
    {
        vector<int> dp(amount + 1, INT_MAX - 1); //初始化dp数组，初始值为INT_MAX - 1是因为后面有dp[i - value] + 1操作，需要避免整形溢出
        dp[0] = 0; //已知的最优解
        for(int i = 1; i <= amount; i++)
        {
            for(int value : coins) //遍历每种硬币
            {
                if(i - value < 0) continue; //进行判断是为了避免越界
                dp[i] = min(dp[i], dp[i - value] + 1); //找到总金额为 i 时的最优解
            }
        }
        return dp[amount] == (INT_MAX - 1) ? -1: dp[amount];
    }
};
```
