---
title: VsCode推荐插件
subtitle:
date: 2024-08-12T10:30:17+08:00
slug: c0ef113
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
  - vscode插件
categories:
  - VsCode
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
## Better C++ Syntax

>该插件主要作用是提供 C++ 语法高亮

## Bookmarks

> 该插件主要作用是允许用户在代码中添加书签，以便快速跳转到这些特定位置。这个插件对于需要在大型项目中导航和跟踪重要代码段的开发者来说非常有用。

该插件常用快捷键为：  
1. 添加书签：`Ctrl+Alt+K`
1. 删除书签：将光标放在带有书签的行上，然后使用 `Ctrl+Alt+J`
1. 切换书签：`Ctrl+Alt+Q`可以在书签之间切换，如果当前行有书签，则会删除它。
1. 跳转到下一书签：`Ctrl+Alt+L`
1. 跳转到上一书签：`Shift+Ctrl+Alt+L`

## C/C++
>该插件主要作用是提供了一系列的工具和功能，例如代码分析功能、代码格式化功能、代码提示等。

## CMake
>该插件主要作用是CMake语法高亮、CMake代码自动补全。

## CMake Tools
>该插件主要作用是提供各种CMake编译相关的小工具，包括在底部状态栏显示一些快捷工具。

## cmake-format
>该插件的主要作用是格式化 CMakeLists.txt 文件，使其保持一致和可读性。安装此插件前，需要先安装cmake-format工具。

## Code Runner
>该插件的主要作用是运行选定的代码片段。  

该插件常用快捷键为：  
1. 运行选定代码 `Ctrl+Alt+N` 或 直接点击右上角的三角形 或 点击右键菜单中的 `Run Code`
2. 停止正在运行的代码 `Ctrl+Alt+M` 或 点击右键菜单中的 ` Stop Run Code`

插件详细说明[地址](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)

## Diff
>该插件的主要作用是比较两个文件的不同之处，直接在资源管理窗口中选择两个要比较的文件，将会直接显示比较结果。

## Error Lens
>该插件的主要作用是将代码中存在的问题突出显示(包括错误、警告和语法问题)，它不仅在代码行尾显示问题，而且会在整行进行高亮，使得诊断信息更加明显。

## GitLens
>该插件的主要作用是可以更方便地在VsCode中进行Git相关操作，该插件的使用可看[此视频](https://www.bilibili.com/video/BV1AS4y1V7PG/?spm_id_from=333.337.search-card.all.click&vd_source=744dd2bfd43a3b6a0d6a04beeeb1f108)了解。

**PS:** 建议能够熟练使用Git命令后再使用此插件，Git的学习可看[此教程](https://zyzhi.top/posts/a538cc4/)。

## GitHub Copilot
>该插件的主要作用是辅助编码。该插件是GitHub的AI编码工具，能根据注释、函数名、函数参数编写代码。该插件的详细介绍可看[此文章](https://blog.csdn.net/Hyl_Aa/article/details/131520129)。

**PS:** 该插件需要进行GitHub学生认证才能免费使用，可参考[此教程](https://mdnice.com/writing/9e9fe24b16234c28a460aa45b99655ae)。

## Include Autocomplete
>该插件的主要作用是提供编写 C++ `#include` 语句时自动补全功能

## Markdown All in One 和 Markdown Preview Enhanced
>这两个插件都是用于帮助编写Markdown文件

## SVG Viewer
>该插件的主要作用是在VsCode中编辑和预览 SVG 文件

## Todo Tree
>该插件的主要作用是用于展示工作区中所有的待办事项，点击对应事项后可以跳转到对应位置

主要有以下几种注释格式：  
![图1](/PostsImgs/VscodeRecomPlugin_imgs/picture1.png)  

该插件的配置代码如下，直接粘贴进 **`setting.json`** 文件中即可。使用 `Ctrl + ,` 命令，再点击右上角的打开设置，即可打开 **`setting.json`** 文件。  

```json
  "todo-tree.tree.showScanModeButton": false,
  "todo-tree.filtering.excludeGlobs": ["**/node_modules", "*.xml", "*.XML"],
  "todo-tree.filtering.ignoreGitSubmodules": true,
  "todohighlight.keywords": [
  ],
  "todo-tree.tree.showCountsInTree": true,
  "todohighlight.keywordsPattern": "TODO:|FIXME:|NOTE:|\\(([^)]+)\\)",
  "todohighlight.defaultStyle": {

  },
  "todohighlight.isEnable": false,
  "todo-tree.highlights.customHighlight": {
    "BUG": {
      "icon": "bug",
      "foreground": "#F56C6C",
      "type": "line"
    },
    "FIXME": {
      "icon": "flame",
      "foreground": "#FF9800",
      "type":"line"
    },
    "TODO":{
      "foreground": "#FFEB38",
      "type":"line"
    },
    "NOTE":{
      "icon": "note",
      "foreground": "#67C23A",
      "type":"line"
    },
    "INFO":{
      "icon": "info",
      "foreground": "#909399",
      "type":"line"
    },
    "TAG":{
      "icon": "tag",
      "foreground": "#409EFF",
      "type":"line"
    },
    "HACK":{
      "icon": "versions",
      "foreground": "#E040FB",
      "type":"line"
    },
    "XXX":{
      "icon": "unverified",
      "foreground": "#E91E63",
      "type":"line"
    }
  },
  "todo-tree.general.tags": [
    "BUG",
    "HACK",
    "FIXME",
    "TODO",
    "INFO",
    "NOTE",
    "TAG",
    "XXX"
  ],
  "todo-tree.general.statusBar": "total",
```

## Doxygen Documentation Generator
>该插件的主要作用是生成Doxygen能够读取的注释风格，配合[Doxygen](https://blog.17lai.site/posts/1acb0edb/)软件使用，可自动生成代码的说明文档。

默认使用方法为：在要生成注释的位置输入 `/**`后直接回车即可。

**PS:** 一般使用默认格式即可，如果要修改生成注释风格模板可直接在设置(`Ctrl + ,`即可打开)➡扩展➡Doxygen Documentation Generator 里自行调式

## Remote-SSH
>该插件的主要作用是允许通过 SSH 连接到远程服务器，并在 VSCode 环境中无缝地进行远程开发。

相关学习资料：  
1. [Remote-SSH的使用](https://www.cnblogs.com/qiuhlee/p/17729647.html)
2. [Ubuntu下安装ssh服务](https://blog.csdn.net/sdnuwjw/article/details/109786245)
3. [Remote-SSH的使用(视频)](https://www.bilibili.com/video/BV1s44y1G7E2/?spm_id_from=333.337.search-card.all.click&vd_source=744dd2bfd43a3b6a0d6a04beeeb1f108)