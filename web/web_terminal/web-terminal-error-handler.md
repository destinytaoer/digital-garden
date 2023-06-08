---
title: 错误处理
tags: [web-socket, web-terminal]
date: 2023-05-20 20:45:58
update: 2023-06-08 11:42:48
---
#web-socket #web-terminal 

# 错误处理

WebSocket 中有个 error 事件，但是返回的事件对象中并没有包含任何有用的信息，反而是 close 事件中，会包含 close code 和 reason，可以用于一定的错误判断。

一个是可以与后端协商能捕获到的错误，在关闭时返回定义好的 code 和 reason。

另一个就是类似 k8s exec api 中的 ServiceError，通过一个消息来返回错误信息。

目前还不是很完善，后面持续优化。

大家有做过 WebSocket 错误处理的，也可以给我一点建议。

## k8s exec 接口 ServiceError channel

连接 k8s exec 接口时，有一个 ServiceError Channel：以 3 开头，我们对这类消息当做是错误来处理

返回的消息中，包含消息原因和 exit code。

k8s exit code：还待完善

- Exit code 137 - Pods terminated，pod 销毁了
- 127：
