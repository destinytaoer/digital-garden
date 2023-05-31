---
title: lrzsz
tags: [lrzsz, zmodem]
date: 2023-05-25 15:49:07
update: 2023-05-31 14:24:29
---
#lrzsz #zmodem 

# lrzsz

lrzsz是一款在linux里可代替ftp上传和下载的程序，是 [[zmodem]] 协议的一种实现。

<https://github.com/UweOhse/lrzsz/tree/master>
<https://www.ohse.de/uwe/software/lrzsz.html>

这个库非常古老，已经 30 年了，最新版本为 0.12.20 和 0.12.21rc。已经不再维护了。

## 安装

```sh
# apt-get
apt-get install lrzsz
# Unable to locate package lrzsz
# 如果安装失败，尝试先执行 update 再执行 install
apt-get update

# yum
yum -y install lrzsz
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
