---
title: lrzsz
tags: 
date: 2023-05-25 15:49:07
update: 2023-05-25 15:50:02
---

<https://github.com/UweOhse/lrzsz/tree/master>
https://www.ohse.de/uwe/software/lrzsz.html

这个库非常古老，已经 30 年了，最新版本为 0.12.20 和 0.12.21rc
# lrzsz

## 安装

```sh
apt-get install lrzsz
# Unable to locate package lrzsz
# 如果安装失败，尝试先执行 update 再执行 install
apt-get update
```

## 使用

```sh
# 上传
rz
# 取消超时限制
rz -O

# 下载
sz <package name>
```