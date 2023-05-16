---
title: xterm 用法
layout: post
category: 
tags: [xterm, javascript]
date: 2023-05-16 17:51:39
update: 2023-05-16 18:54:07
comments: true
copyright: true
---

# xterm 用法

#xterm #javascript 

[文档](https://xtermjs.org/)
[Github](https://github.com/xtermjs/xterm.js)

## 初始化

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

##  渲染类型

v4 版本支持 `rendererType` 配置项，支持 dom 和 canvas 两种类型，默认使用 canvas

```javascript
const terminal = new Terminal({
	rendererType: 'dom'
})
```

v5 版本废弃了这个配置项，xterm 将 canvas 渲染器单独抽离出来，封装成了一个插件 [xterm-addon-canvas](https://github.com/xtermjs/xterm.js/tree/master/addons/xterm-addon-canvas)，默认使用 dom 进行渲染。另外 v5 版本推荐使用 webgl 来进行渲染，而 canvas 作为一种备用的回退方案。

```javascript
try {  
	// https://github.com/xtermjs/xterm.js/issues/3357  
	// safari < 16 报错, 需要 try catch
	if (detectWebGLContext()) {
		this.webglAddon = new WebglAddon()
		this.webglAddon.onContextLoss((e) => {
			log.error('something error: lost webgl context', e)
			this.webglAddon?.dispose()
		})
		this.loadAddon(this.webglAddon)
		log.info('render xterm use webgl')
	} else {
		throw new Error('current browser does not support webgl')
	}
} catch (e) {
	log.warn('WebGL renderer could not be loaded, falling back to canvas renderer')
	disposeWebglRenderer()
	enableCanvasRenderer()
}
```

## 主题配置
