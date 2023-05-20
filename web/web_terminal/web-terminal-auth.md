---
title: 连接鉴权
tags: [web-terminal, terminal, web-socket, auth]
date: 2023-05-20 20:35:11
update: 2023-05-20 20:44:13
---
#web-terminal #terminal #web-socket #auth 

# 连接鉴权

前端 --> API Gateway --> Lepton Service WebSocket --> k8s exec api

经过云管的网关，需要带上 Authorization 请求头才能通过校验，但是浏览器的 [WebSocket API](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket) 无法自定义请求头。

携带校验信息方案：
- cookie
- url 的 query 中带上鉴权信息
- 连接成功后，马上发送鉴权消息

鉴权消息通常是由系统的 token 交换来的一次性票据 ticket，也可以直接使用 token
