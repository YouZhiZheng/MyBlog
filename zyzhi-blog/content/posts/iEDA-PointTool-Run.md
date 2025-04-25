---
title: iEDA点工具运行前环境变量设置
subtitle:
date: 2024-08-02T10:37:33+08:00
slug: 3b24a9e
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
  - 点工具
  - 教程
categories:
  - iEDA
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
## 脚本创建

在单独运行点工具前必须配置对应的环境变量，在iEDA目录/iEDA/scripts/design/sky130_gcd下使用命令`touch`创建一个.sh脚本，内容如下:

```bash
#!/bin/bash
CURRENT_DIR="$(pwd)"

export CONFIG_DIR="$CURRENT_DIR/iEDA_config" 
export RESULT_DIR="$CURRENT_DIR/result/my_test_result"
export TCL_SCRIPT_DIR="$CURRENT_DIR/script"
export NETLIST_FILE="$CURRENT_DIR/result/verilog/gcd.v"
export FOUNDRY_DIR="$CURRENT_DIR/../../foundry/sky130"
export SPEF_FILE="$CURRENT_DIR/../../foundry/sky130/spef/gcd.spef"
export SDC_FILE="$CURRENT_DIR/../../foundry/sky130/sdc/gcd.sdc"
export DESIGN_TOP="gcd"
export DIE_AREA="0.0   0.0   149.96   150.128"
export CORE_AREA="9.996 10.08 139.964  140.048"

echo "CONFIG_DIR set to: $CONFIG_DIR"
echo "RESULT_DIR set to: $RESULT_DIR"
echo "TCL_SCRIPT_DIR set to: $TCL_SCRIPT_DIR"
echo "NETLIST_FILE set to: $NETLIST_FILE"
echo "FOUNDRY_DIR set to: $FOUNDRY_DIR"
echo "SPEF_FILE set to: $SPEF_FILE"
echo "SDC_FILE set to: $SDC_FILE"
echo "DESIGN_TOP set to: $DESIGN_TOP"
echo "DIE_AREA set to: $DIE_AREA"
echo "CORE_AREA set to: $CORE_AREA"
```

## 点工具运行

运行点工具前，在当前窗口使用命令 `source 脚本名.sh` 来设置环境变量，然后按照iEDA的用户手册来运行点工具即可

**PS：** 设置的环境变量只在当前窗口有效，即在新窗口运行点工具前需要再次运行脚本来设置环境变量，如果想要使得变量在全部窗口有效请自行谷歌。

## 参考资料

1. https://github.com/OSCC-Project/iEDA/blob/master/docs/user_guide/iEDA_user_guide.md
