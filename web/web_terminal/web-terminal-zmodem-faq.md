---
title: ZMODEM FAQ
tags: 
date: 2023-05-25 15:53:11
update: 2023-05-25 16:07:54
---

# ZMODEM FAQ

## 上传没有进度条

`send_files` 提供了 `on_progress` 方法，但是没有效果

zmodem.js 使用 `FileReader` 读取，`process` 事件只会触发一次，怀疑是读取本地文件速度太快了。

后续，在 `on_offer_response` 中的 `xfer` 中传递 `progress` 事件，参考的是 <https://github.com/lyanmay/zmodemjs/commit/9e2fbf792bd5a8a11fa0482fb656c7c2f2db21c0#diff-7a7fc95217346d6b69029b36e6d6f524fc35617c87731f3eb003b8d98ba1ee98>

虽然能够实时返回 progress 事件，但是写入 terminal 后不起作用，只有等待传输完毕之后才会一起写入。
![[Pasted image 20230525160108.png]]
怀疑是使用了` while true` 阻塞了，这块没有理解。

后续，将文件切片读取

<https://github.com/leffss/gowebssh/blob/master/example/html/zmodem/zmodem.devel.js#LL922C1-L922C5>

同时解决了一个问题：参考 <https://github.com/FGasper/zmodemjs/issues/11>

## Uncaught TypeError: Cannot read property 'ZRPOS' of null

大文件会出现，使用 `rz -O` 可以解决

参考：<https://github.com/FGasper/zmodemjs/issues/11>

最后使用文件切片读取解决。引入新问题，上传过程中取消会导致 terminal 卡住
