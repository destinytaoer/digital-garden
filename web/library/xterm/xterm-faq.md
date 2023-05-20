---
title: FAQ
tags: [xterm, javascript]
date: 2023-05-17 10:32:42
update: 2023-05-17 10:33:11
---

# FAQ

#xterm #javascript 

## Safari 15 兼容性

Safari 15 中，使用 webgl 渲染 xterm 会报错：Webgl2 is only supported on Safari 16 and above

目前查到是 safari 15 版本是支持 webgl2 的，所以代码检测能通过。

https://caniuse.com/?search=webgl

但是 safari 15 的 webgl2 是实验性功能，实现上存在一些问题，在 xterm 的 issue 上也有记录，所以 xterm 自身做了兼容性报错，浏览器上的这个报错信息是 xterm 提示的。
  
- https://github.com/xtermjs/xterm.js/issues/3357  
- https://github.com/xtermjs/xterm.js/pull/4255/files

现在打算使用的解决方案是在浏览器兼容性检测的基础上，再 try catch xterm 加载 webgl 的过程，如果有报错就回退到 canvas

>最早处理 safari 兼容性处理是在 webglAddon 的 `activate` 方法中，也就是在 `terminal.load(webglAddon)` 时触发。但是后来改到了 `constructor` 里面，所以在 `new WebglAddon` 时就会报错，需要注意 try catch 的范围

