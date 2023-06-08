---
title: sz 流程
tags: 
date: 2023-06-08 11:33:20
update: 2023-06-08 11:34:31
---

# sz 流程

1. 发送 ZRQINIT “pre-header” 信号，让终端变成 Zmodem 接收者
2. 终端在识别出 ZRQINIT 报头后，用 ZRINIT header 进行响应
3. 终端要么 accept 要么 skip
4. 没有跳过的情况下，发送方开始发送内容，最终以 ZEOF header 标记结束
5. 接收方会发送额外的 ZRINIT header，标识接收了完整的文件

- 重复 2-5，直到没有文件发送
- 当没有文件发送时，发送方将发送 ZEOF header，收件人回复后，最终发送方发送 `00`(“over and out”) 结束 session
![[sz.png]]
