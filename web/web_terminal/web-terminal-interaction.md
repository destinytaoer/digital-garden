---
title: 前后端交互
tags: [web-terminal, terminal, xterm, web-socket]
date: 2023-05-20 20:39:19
update: 2023-05-20 20:44:07
---
#web-terminal #terminal #xterm #web-socket 

# 前后端交互

处理 terminal 与 socket 的交互

## terminal 输入

```javascript
// 输入
terminal.onData((data) => socket.send(process('data', data)))
// 特殊的鼠标事件输入
terminal.onBinary((data) => socket.send(process('binary', data)))
```

## socket 输出

```javascript
// 输出
socket.addEventListener('message', (e) => {
  const msg = e.data
  // 解析 msg
  // 区分消息进行处理
  if(isOutput(msg)) {
	terminal.write(msg)
  }
})
```

## 尺寸变化

必须保持前后端的 terminal 尺寸是一致的，不然显示会有问题
- 首先需要在初始化时就设置好，一般跟随鉴权信息一起携带
- 其次每次尺寸变化都需要同步给后端
- 尺寸变化不能太频繁

```javascript
const resizeCb = debounce(() => {
  fitAddon.fit()
}, 500)
window.addEventListener('resize', resizeCb)
// 尺寸变化
terminal.onResize(({cols, rows})) => socket.send(process({cols, rows})))
```

## 心跳

- 网关超时 60s，为了保证长连接需要加心跳
- [chrome 浏览器非活动标签页对定时器的优化](https://developer.chrome.com/blog/timer-throttling-in-chrome-88/)

```javascript
// 直接用 setInterval 会有问题
setInterval(() => {
  socket.send(heartbeatMsg)
}, 30 * 1000)
```
