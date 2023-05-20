---
title: WebTerminal 流程设计
tags: [web-terminal, terminal, xterm]
date: 2023-05-20 20:33:13
update: 2023-05-20 20:44:11
---
#web-terminal #terminal #xterm  

# WebTerminal 流程设计

![[web-terminal.png]]

1. init：初始化 terminal
	1. new Terminal
	2. loadAddon：加载一些插件，fitAddon/weblinkAddon
	3. renderer：设置渲染器，dom/canvas/webgl
	4. terminal.open
2. connect socket：连接 WebSocket，处理前后端交互
	1. 连接鉴权
	2. 界面交互
	3. 消息处理
	4. 错误处理
3. destroy：清理定时器、解绑事件、关闭 socket 等等
