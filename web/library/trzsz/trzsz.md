---
title: trzsz
tags: [trzsz, lrzsz, tmux]
date: 2023-05-31 14:31:31
update: 2023-05-31 14:41:19
---
#trzsz #lrzsz #tmux

# trzsz

trzsz ( trz / tsz ) 是一个兼容 tmux 的文件传输工具，和 lrzsz ( rz / sz ) 类似，并且有进度条和支持目录传输。

[官网](https://trzsz.github.io/)

## 功能

- 支持 **tmux**，包括 tmux 普通模式，以及与 iTerm2 集成的 tmux 命令模式。
- 支持 **传输目录**，`trz -d` 命令上传目录，`tsz -d xxx` 命令下载 xxx 目录。
- 支持 **Windows**，不仅可在 Windows 客户端使用，也可在 Windows ssh 服务器使用。
- 支持 **原生终端**，不需要原生终端做支持，只要使用 `trzsz ssh x.x.x.x` 登录即可。
- 支持 **web 终端**，通过 web 浏览器在本地与服务器之间传输目录和文件。
- 支持 **拖动上传**，将文件和目录拖到终端窗口即可上传到远程服务器。
- 支持 **进度条**，显示当前正在传输的文件名、进度、大小、速度和剩余时间等。
- 更好的 **交互体验**，传输成功或出错时显示友好的结果，`ctrl + c` 优雅中止。

## 版本

当前是 1.1.2

- [trzsz](https://github.com/trzsz/trzsz)：python
- [trzsz-go](https://github.com/trzsz/trzsz-go)：go
- [trzsz.js](https://github.com/trzsz/trzsz.js)：nodejs 版本，包含 web terminal 实现

## 特点

- trzsz 是边传边写的过程，会有文件残留，也就是上传或下载中断后不会删除残留文件
- 使用了 File System Access api，safari 浏览器兼容性较差，并且体验较差，不能下载到系统目录，并且下载时还需要用户再点击同意修改文件

## 原理

[[trz]]
[[tsz]]
