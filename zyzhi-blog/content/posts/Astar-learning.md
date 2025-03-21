---
title: Astar算法
subtitle:
date: 2024-09-18T19:47:39+08:00
slug: 1d723ce
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
  - Astar
  - C++
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

## A星算法简介

A*（A星）算法是一种基于格子（Grid）的寻路算法，用于从众多路径中，寻找一条从起点到终点代价最小的路径。基于格子也就是说会把地图看作是由 `w*h` 个格子组成的，因此寻得的路径也就是由一连串相邻的格子所组成的路径。

既然是要寻找代价最小的路径，那么遍历节点时，就不能盲目地遍历，而是挑那些 **「最有可能代价很小」** 的节点来运算。如何获得这种代价很小的节点呢？这就需要使用启发函数来计算每个节点的代价了（在A*中启发函数也被称为 **估价函数** ）。

一条路径的代价无非就是路径长度（不带权重时），可是当寻找路径时，该如何计算代价呢？那就是将代价分为两个方面：

1. **已知代价。** 即从起点到当前节点的路径长度。
2. **未来可能会有的代价。** 用 "猜" 的，假设当前节点到终点之间没有障碍，代价是多少。

那么我们就可以得到估价函数为：**`f(n) = g(n) + h(n)`**，其中g(n)表示起点到格子n的代价，h(n)表示格子n到终点的代价，f(n)表示格子n的代价。

由于在EDA中，是使用[曼哈顿距离](https://zh.wikipedia.org/wiki/%E6%9B%BC%E5%93%88%E9%A0%93%E8%B7%9D%E9%9B%A2)来表示两点之间的距离，所以本文采用曼哈顿距离来计算代价。
___

知道启发函数是怎么回事之后我们就可以开始遍历节点了。从起点开始，计算其四周节点的代价，并把那些节点放到一个 **「池子」** 里面去，之后每次都从池子里面捞出来代价最小的节点，继续计算其周围节点的代价。

如果遍历到目标节点，或者池子空了之后，寻路就结束了。那么如何获得路径呢？我们从遍历的过程可以看出，节点加入到池子之前，我们知道这些加入到池子中的节点是来自于当前节点的，所以在加入池子之前将那些节点的父节点设为当前节点，等寻路到目标节点之后我们逐个节点遍历其父节点，就可以遍历完最短路径。

**对于算法，有以下几点需要注意：**

* 计算当前节点的周围节点的代价值时，如果需计算的节点原先就已经算过代价值，且当前算出来的代价值比原先的小，说明这个节点从当前节点过去路径会更短一些，应更新其父节点和代价值。
* 我们每次都是从「池子」中捞出代价最小的节点，所以用小根堆来实现「池子」是十分高效的。

___

综上所述，可以画出 **`A*`** 算法流程图：  
![A*流程图](/PostsImgs/AstarLearning_imgs/Astar.svg)

## A星算法的实现

作者采用 **`C++`** 来实现 `A*` 算法，代码已上传至[此仓库](https://github.com/YouZhiZheng/AStar)
