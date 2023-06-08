---
title: web-terminal-trzsz
tags: [web-terminal, terminal, xterm, trzsz]
date: 2023-05-22 15:11:38
update: 2023-06-08 11:30:03
---
#web-terminal #terminal #xterm #trzsz

# web-terminal-trzsz

## 集成 trzsz

使用 [trzsz.js](https://github.com/trzsz/trzsz.js)，并进行部分修改

可参考：ttyd

## 功能调整

修改 TrzszFilter，增加 onDetect，由外部检测到上传或者下载的行为。

- 利用相关暴露的 api 手动实现 `handleDownloadFiles`，`handleUploadFiles`
- 限制上传和下载文件大小
- 限制上传和下载目录
- 替换 File System Access API，使用 `input[type='file']` 代替上传，使用 `a.click` 代替下载
