---
title: 解决Git的ssh超时问题
subtitle:
date: 2025-01-03T11:14:33+08:00
slug: 4d5a726
draft: false
author:
  name: zyz
  link:
  email:
  avatar:
description: 解决无法通过ssh连接git问题
keywords:
license:
comment: true
weight: 0
tags:
  - git
  - ssh
  - timeout
categories:
  - Git
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
## 问题发现

最近在使用 **`git push`** 命令时发现连接超时，因为我使用的是用公钥走ssh协议进行连接，输入命令 **`ssh -T git@github.com`** 发现果然是ssh连接超时。在网上搜索了一圈，找到问题是主域名 **`github.com`** 的 IP 被彻底墙了。

## 解决办法

通过Google大法，找到供 ssh 连接的子域名 `ssh.github.com` IP 还活着。因此，解决办法就是往 **`~/.ssh/config`** 文件 中添加下面的配置：

```bash
Host github.com
  Hostname ssh.github.com
```

至此，问题成功解决。

## 参考资料

1. https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port
2. https://ww-rm.top/posts/2024/01/17/githubssh-timeout/

<!--more-->
