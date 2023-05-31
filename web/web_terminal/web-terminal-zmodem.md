---
title: 支持 zmodem
tags: [web-terminal, terminal, zmodem, lrzsz]
date: 2023-05-22 15:07:34
update: 2023-05-31 14:31:48
---
#web-terminal #terminal #zmodem #lrzsz 

# 支持 zmodem

[[zmodem]] 协议一种文件传输协议，支持 zmodem 实际上是为了支持类似 [[lrzsz]] 这样的工具来进行远程的文件上传和下载。

## 局限性

- 一些ZMODEM数据包（例如 ZACK，ZRPOS）在传输的文件中嵌入一个字节的32位无符号整数，作为偏移量。 此设计限制了ZMODEM只能**可靠地传输大小不超过4GB的文件**。
- 即使协议允许它，引用（l）rzsz实现也不能编码任何非控制字符（例如'〜'），这些字符经常被TCP / IP连接程序（如telnet和ssh）用作客户端“终端转义”字符。 **用户必须禁用终端转义功能才能在这些类型的链接上实现可靠的传输**，例如ssh -e none user@hostname。

## 前端实现

直接使用 zmodem.js 库，参考 [ttyd](https://github.com/tsl0922/ttyd) 开源实现

- Zmodem 是 二进制协议，所以首先会从 base64 消息转换成二进制消息
- 检测到 rz 命令回传的特殊标识，弹出系统选择文件弹窗，选择文件进行上传
- 检测到 sz 命令回传的特殊标识，会将终端传输过来的二进制数据合成一个 blob 进行下载，系统弹窗选择保存目录
- 在上传和下载过程中，不允许用户在终端进行输入
- 进度条：文件数据传输过程需要包含进度条（可选）
- 取消上传/下载：在上传或者下载传输大文件的过程中，可以利用快捷键中断，如：Ctrl + C（可选）
- 文件大小限制（可选）
- 文件传输速度限制（可选）

## 测试

- 基本功能正常
		- 常用操作
		- tree
		- vim 操作
		- ...
- 在 pod 中安装 lrzsz
		- rz 上传文件
		- sz 下载文件
		- 进度条显示正常
		- 可取消
		- 测试文件传输的大小极限
- 测试上传文件
	- rz 上传文件
	- 单个文件上传
	- 多个文件上传
	- 进度条显示正常
	- 取消上传
	- 异常中断上传：断网
	- 测试文件传输大小限制
- 测试下载文件
	- sz 下载文件，只支持单个文件下载
	- 进度条显示正常
	- 取消下载
	- 异常中断下载
	- 测试下载文件的大小限制

下面是 ttyd 的 web terminal，可以本地安装 ttyd 和 lrzsz 尝试一下
![[lrzsz.gif]]

## 参考

zmodem 库 [https://github.com/FGasper/zmodemjs](https://github.com/FGasper/zmodemjs)

如何支持 zmodem.js 文章

[https://blog.ops-coffee.cn/tag/webssh/](https://blog.ops-coffee.cn/tag/webssh/)

[https://www.xkblogs.com/index.php/archives/28/](https://www.xkblogs.com/index.php/archives/28/)

[https://blog.csdn.net/weixin_45717984/article/details/130004395](https://blog.csdn.net/weixin_45717984/article/details/130004395)

<https://juejin.cn/post/6935621453400244260#heading-2>

现成实现方案

- [ttyd 的实现](https://github.com/tsl0922/ttyd/blob/397b24f138/html/src/components/terminal/index.tsx)：包含 lrzsz 和 trzsz
- zmodem.js 给的 demo：[https://github.com/FGasper/xterm.js/blob/1324_move_zmodem_demo/demo/zmodem/main.js](https://github.com/FGasper/xterm.js/blob/1324_move_zmodem_demo/demo/zmodem/main.js)
- gotty: [https://github.com/sorenisanerd/gotty/tree/master/js/src](https://github.com/sorenisanerd/gotty/tree/master/js/src)

xterm 对 zmodem 的支持：
[https://github.com/xtermjs/xterm.js/issues/279](https://github.com/xtermjs/xterm.js/issues/279)

[https://github.com/xtermjs/xterm.js/issues/4471](https://github.com/xtermjs/xterm.js/issues/4471)
