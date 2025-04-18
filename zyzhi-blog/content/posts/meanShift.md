---
title: MeanShift算法
subtitle:
date: 2025-02-16T10:46:23+08:00
slug: 60e3fcc
draft: false
author:
  name: zyz
  link:
  email:
  avatar:
description: 详细介绍了 meanShift 算法
keywords:
license:
comment: true
weight: 0
tags:
  - meanshift
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

## 前言

最近在看的一篇布局论文，发现其将 **Mean Shift** 算法扩展来进行聚类操作，而想要读懂文章就必须先理解此算法，故此文章记录的就是我对该算法的理解，若有理解错误的地方，欢迎留言指正。

## 简介

**Mean Shift** 是一种「基于密度」的聚类算法，它是一种非参数聚类方法（无需开始前指定簇数），可以自动从数据点的分布中推断出簇的数量。该算法会让数据点朝着密度更高的地方进行快速移动，直到达到该数据点所在区域的局部密度峰值地（即簇中心）。

通俗来说，将密度峰值比作山顶，数据点视为登山者，该算法就是让每个登山者沿着最快的路径登上离自己最近的山顶，登上同一个山顶的登山者就属于同一个簇。

## 带宽

带宽(bandwidth)是Mean Shift 算法唯一需要指定的参数，它用于确定每个点的**邻域**。以当前点为圆心，带宽为半径画圆，所画出的圆就是该点的邻域，在邻域内的点就是该点的**邻居**。

带宽非常重要，它会影响簇的大小：

- **小带宽**。这会形成许多小簇，因为只有近距离的点才会形成一个群集。
- **大带宽**。这会形成较少的大簇，因为距离远的点也可以形成一个群集。

## 核函数

在 Mean Shift 算法中，核函数的作用就是用来衡量当前移动点的邻居对该移动点的下一个位置的**影响程度**(由于该点是不断移动的，所以其邻居也会不断变化)。

### 为什么使用核函数？

核函数有助于平滑数据并确保远离中心的点不会过度影响结果。它使得 Mean Shift 算法关注数据中的局部情况而不是全局，有助于进行分类。

### 核函数类型

核函数有非常多的类型，比如**Uniform Kernel、Epanechnikov Kernel**等，其中最常用的的是**高斯核函数(Gaussian Kernel)**：
$$
K\left(\frac{x - x_i}{h}\right) = \exp\left(-\frac{\|\|x - x_i\|\|^2}{2h^2}\right)
$$

- $x$: 当前点坐标
- $x_i$: 当前点的邻居坐标
- $h$: 带宽
- **物理意义:** 距离$x$越近的$x_i$，权重越大；超过带宽$h$的点权重为0  

## 核密度估计(Kernel Density Estimation, KDE)

在Mean Shift算法中，**KDE** 的作用是用来估算数据点的**局部密度分布**。它通过核函数将每个点的邻域内的其他点进行加权，提供一个平滑的、连续的密度估计，帮助识别局部**密度峰值**(即告诉我们山顶在哪)，<span style="color:red;">确定点的移动方向</span>。KDE具体公式如下:
$$
f(x) = \frac{1}{nh^d} \sum_{i=1}^{n} K\left(\frac{x - x_i}{h}\right)
$$

- $n$: 数据集中的点数
- $h$: 带宽
- $d$: 数据的维度
- $K\left(\frac{x - x_i}{h}\right)$: 核函数，加权每个点 $x_i$ 对当前点的密度贡献

## 均值漂移向量(Mean Shift Vector)

在使用KDE确定了移动方向后，就需要使用**均值漂移向量**来将该数据点移动到当前的山顶。

### 怎么得出均值漂移向量的公式？

实际上我们要寻找的局部密度峰值就是KDE的局部极大值。所以，可以通过**梯度上升法**来将点移动到局部极大值对应的点位置(即利用导数来进行移动)。

#### 步骤1: 写出密度梯度表达式

对$f(x)$求梯度：
$$
\nabla f(x) = \frac{1}{nh^d} \sum_{i=1}^{n} \nabla K\left( \frac{x - x_i}{h} \right)
$$

#### 步骤2: 选择核函数（以高斯核为例）

高斯核函数对应的梯度为：
$$
\nabla K\left( \frac{x - x_i}{h} \right) = K\left( \frac{x - x_i}{h} \right) \cdot \left( - \frac{x - x_i}{h^2} \right)
$$

#### 步骤3: 代入梯度表达式

将高斯核函数的梯度代入$\nabla f(x)$：
$$
\nabla f(x) = \frac{1}{nh^d} \sum_{i=1}^{n} K\left( \frac{x - x_i}{h} \right) \cdot \left( - \frac{x - x_i}{h^2} \right)
$$

整理后：
$$
\nabla f(x) = \frac{1}{nh^{d+2}} \sum_{i=1}^{n} K\left( \frac{x - x_i}{h} \right) (x_i - x)
$$

即：
$$
\nabla f(x) \propto \sum_{i=1}^{n} K\left(\frac{x-x_{i}}{h}\right)\left(x_{i}-x\right)
$$
该表达式的结构可以拆解为：

- $\sum_{i=1}^{n} K\left(\frac{x-x_{i}}{h}\right) \cdot x_i$：所有邻居点的加权位置和（邻居点的加权平均）
- $\sum_{i=1}^{n} K\left(\frac{x-x_{i}}{h}\right) \cdot x$：当前点的加权和（当前位置的加权平均）
- 结果是两者的差值，表示密度梯度的方向

#### 步骤4: 得出均值漂移向量

上面的梯度表达式只是给出了移动方向，并没有告诉我们应该移动多少，**它的数值大小未必适合直接作为移动步长**。此外，数值大小依赖于**数据的尺度**，比如数据分布的密度和带宽会影响梯度的绝对值大小，导致步长难以控制。

这时就需要进行<span style="color:red;">**归一化操作**</span>，让梯度表达式的结果转换成 **加权平均位置**（表示在当前点邻域内，密度中心的位置），而不是一个与数据规模、密度等因素相关的数值。

利用加权平均位置即可得到**均值漂移向量**(表示了移动方向和移动距离)：
$$
m(x) = \frac{\sum_{i=1}^{n} K\left( \frac{x - x_i}{h} \right) x_i}
{\sum_{i=1}^{n} K\left( \frac{x - x_i}{h} \right)} - x
$$
即:
$$
m(x)=加权平均位置−当前位置
$$

这样就可以得到**新位置的计算公式**了：
$$
x_{new} = x + m(x) = \frac{\sum_{i=1}^{n} K\left( \frac{x - x_i}{h} \right) x_i}
{\sum_{i=1}^{n} K\left( \frac{x - x_i}{h} \right)}
$$

## 算法流程

### Step 1: 初始化

- 选择合适的带宽$h$，比如通过**estimate_bandwidth**函数、经验法等方式来选择合适的带宽
- 选择合适的收敛阈值$\varepsilon$

### Step 2: 迭代更新

对于数据点$x$，利用均值漂移向量迭代的更新其位置。

### Step 3: 收敛判断

判断数据点$x$是否收敛，即判断下面不等式是否成立：
$$
||x_{new} - x||< \varepsilon
$$

- $\varepsilon$ 为指定阈值，比如为0.001

### Step 4: 重复上述步骤

对于每个数据点依次重复**step2**和**step3**，使得全部的数据点都收敛(即都达到山顶)，移动到同一 "山顶" 的数据点属于同一类。

### Step 5: 合并过近的簇

对于簇中心过近的簇进行合并，比如簇中心距离小于 $h/2$ 的两个簇就会被合并成一个簇。

## 具体例子

假设有一组二维数据点，这些点的分布如下：

| 点编号 | 坐标  |
| :----: | :---: |
|   A    | (1,2) |
|   B    | (2,3) |
|   C    | (3,3) |
|   D    | (5,6) |
|   E    | (6,7) |
|   F    | (6,5) |
|   G    | (7,6) |

这些点大致形成了两个聚类区域：

- (A, B, C) 形成一个密度较高的区域（簇1）
- (D, E, F, G) 形成另一个密度较高的区域（簇2）

假设选择带宽$h = 2.5$，核函数为高斯核。

**对于点A：**

1. **找到邻域内的点：**
    - 带宽 $h = 2.5$，以 $(1,2)$ 为圆心的邻域包含点：$A(1,2)$、$B(2,3)$、$C(3,3)$。  
2. **计算新位置：**
   - 根据公式计算加权后的新位置：
   $$
   x_{\text{new}} = \frac{1\cdot(1,2) + 1\cdot(2,3) + 1\cdot(3,3)}{3} = (2,2.67)
   $$
3. **重复这个过程，直到收敛：**
   - 继续迭代，点 A 可能会进一步移动到 $(2.5,3)$，最终收敛到密度峰值附近，例如 $(2.5,3)$。

对于剩下的点采用同样的方式将其进行收敛，**最终结果**为：

- 点 A、B、C 最终都收敛到"山顶" $(2.5,3)$，形成一个簇。
- 点 D、E、F、G 最终都收敛到"山顶" $(6,6)$，形成另一个簇。
