---
title: 基础用法
tags: [xterm, javascript]
date: 2023-05-16 17:51:39
update: 2023-05-17 14:35:46
---

# 基础用法

#xterm #javascript 

```javascript
import { Terminal } from 'xterm'
// 样式文件
import 'xterm/css/xterm.css'

const terminal = new Terminal({
	cursorBlink: true
})

// 将 terminal 挂载到某个 DOM 节点上就完成了
const element = document.getElementById('terminal-container')
terminal.open(element)
```

xterm 的文档比较简单，配置项可直接查看[类型文件](https://github.com/xtermjs/xterm.js/blob/5.1.0/typings/xterm.d.ts#L26)

此时在 terminal 中输入是没有任何效果的，我们可以通过 write 方法和 onData 事件来实现简单的交互

```javascript
terminal.onData((data: string) => {
	terminal.write(data)
})
```
