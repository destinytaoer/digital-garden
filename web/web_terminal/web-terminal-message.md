---
title: 消息处理
tags: 
date: 2023-05-20 20:44:40
update: 2023-05-20 20:44:57
---

# 消息处理

terminal 和 socket 交互中，我们通常需要处理这几种消息：

- input：terminal 的输入消息，分为 data 和 binary
- output：tty 反馈的消息，需要回显到 terminal 中
- resize：尺寸消息，保持 terminal 尺寸一致
- heartbeat：心跳消息，保证长连接

消息的处理需要分为编码处理和格式定义：
- 编码处理就是消息以什么格式传输：二进制、base64、json 字符串
- 格式定义就是怎么去区分这些消息

以目前的 k8s exec api 为例。

首先是消息的编码方式，exec 接口提供了两个子协议，在建立 WebSocket 连接时，通过 protocols 选项来传递：
- base64.channel.k8s.io：base64
- channel.k8s.io：二进制


```javascript
new WebSocket('/exec', ['base64.channel.k8s.io'])
```

exec 接口提供的消息通道有：
- StdIn：0
- StdOut：1
- StdError: 2,  
- ServiceError: 3, 
- Resize: 4

前端发送的消息处理：

```javascript
export const K8sExecMsgChannel = {  
  StdIn: '0',  
  StdOut: '1',  
  StdError: '2',  
  ServiceError: '3',  
  Resize: '4',  
}
// input
socket.send(K8sExecMsgChannel.StdIn + util.base64.encode(data))
// resize
socket.send(K8sExecMsgChannel.Resize + util.base64.encode(JSON.stringify({  
  Width: cols,  
  Height: rows,  
})))
// heartbeat
socket.send(K8sExecMsgChannel.StdIn)
```


服务端返回的消息处理：

```javascript
const base64Msg = data.slice(1)  
const type = data.slice(0, 1)  
const msg = util.base64.decode(base64Msg).toString()
// stdout/stderror
switch (type === ) {
  case K8sExecMsgChannel.StdOut:  
  case K8sExecMsgChannel.StdError:  
    return terminal.write(msg)
  case K8sExecMsgChannel.ServiceError:  
     // 错误处理
}
```

