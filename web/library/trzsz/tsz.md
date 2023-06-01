---
title: tsz
tags: [trzsz]
date: 2023-05-31 14:34:27
update: 2023-05-31 14:42:54
---
#trzsz 

# tsz

tsz 过程：

1. 识别到 `..7.::TRZSZ:TRANSFER:S:1.1.2:8544343035300...`标志，`findTrzszMagicKey`，开始执行 `handleTrzszDownloadFiles`，先选保存路径，选中则发送接收，否则发送拒绝
2. 接受，发送 `#ACT:xxxxx`，调用的是 `transfer.sendAction(true, remoteIsWindows)`
	1. 接收配置，`#CFG:xxx`，配置为
		1. binary
		2. directory
		3. bufsize
		4. timeout
		5. overwrite
		6. escape_chars
		7. tmux_output_junk
	2. 接收数量，`#NUM:4`，文件数量(包含目录)，接收方发送接收成功 `#SUCC`（测试发现 NUM 会跟在 CFG 后面，同一条消息）
	3. 接收文件或目录名，`#NAME:xxx`，如果是目录则不处理，如果是文件则要继续下一步，每次都执行 `browser.openSaveFile` 接收完都会发送 `#SUCC`确认
	4. 接收文件大小，`#SIZE:34402289`，接收完发送 `#SUCC`确认
	5. 开始接收文件，`#DATA:xxx`，接收完都会发送 `#SUCC`确认
	6. 接收完成，发送 `#Exit:xx`，退出，可传回显信息：``transfer.clientExit(`Saved ${filename}`)``
	7. 中断，调用 `transfer.stopTransferring`，发送 `#fail:xxx`，返回 Stopped 回显
3. 拒绝，发送 `#ACT:xxxxx`，调用的是 `transfer.sendAction(false, remoteIsWindows)`，返回 Cancelled
4. 报错，返回 `#FAIL:xxx`，回显内容
