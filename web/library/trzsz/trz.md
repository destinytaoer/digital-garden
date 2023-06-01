---
title: trz 上传文件
tags: [trzsz]
date: 2023-05-31 14:34:19
update: 2023-05-31 14:43:12
---
#trzsz 

# trz 上传文件

trz 过程：

1. 识别到 `..7.::TRZSZ:TRANSFER:R:1.1.2:8550900567700...`，格式 `::TRZSZ:TRANSFER:[mode]:[version][uniqueId]`，其中 uniqueId 包含 : 号。开始执行 `handleTrzszUploadFiles`，先选择文件，选完则确认发送，否则拒绝
2. 确认发送，发送 `#ACT:xxxx`, 调用的是 `transfer.sendAction(true, remoteIsWindows)`
	1. 接收配置，`#CFG:xxx`，配置为
	2. 发送数量，`#NUM:1`，文件数量(包含目录)，接收方返回 `#SUCC`确认
	3. 发送文件大小，`#SIZE:34402289`，接收发方返回 `#SUCC`确认
	4. 开始发送文件，`#DATA:xxx`，接收方返回 `#SUCC`确认
	5. 确认文件完整性，发送 `#MD5:xxx`，接收方返回 `#SUCC`确认
	6. 成功退出，发送 `#EXIT:xxx`
	7. 中断，调用 `transfer.stopTransferring`，发送 `#fail:xxx`，返回 Stopped 回显
3. 拒绝，发送 `#ACT:xxxxx`，调用的是 `transfer.sendAction(false, remoteIsWindows)`，返回 Cancelled
4. 报错，返回 `#FAIL:xxx`，回显内容
