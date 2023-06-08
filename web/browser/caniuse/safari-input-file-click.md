---
title: Safari 编程式打开文件选择器失效
tags: [浏览器, safari]
date: 2023-06-08 10:54:48
update: 2023-06-08 11:14:44
---
#浏览器 #safari 

# Safari 编程式打开文件选择器失效

使用的方式：

```typescript
export function selectFile(multiple = false): Promise<FileList> {  
	return new Promise((resolve, reject) => {  
		const inputDom = document.createElement('input')  
		inputDom.type = 'file'  
		inputDom.multiple = multiple
		
		inputDom.addEventListener('change', (e: any) => {  
			const files = e.target?.files
			console.log('files', files)
			resolve(files)
		})

		// inputDom.addEventListener('cancel', () => {  
		// 	reject(new Error('cancel select'))
		// })
		
		inputDom.click()
	})  
}
```

<https://discussions.apple.com/thread/252051341?answerId=253911617022#253911617022>
<https://stackoverflow.com/questions/60121933/file-input-javascript-click-not-working-in-safari>

在 Safari 上失效。

尝试插入到 body 中 设置为 `display:none`，也尝试了设置为 `visibility:hidden` ，并没有效果。

最终只能在 UI 界面上增加一个步骤，显示出 `input[type='file']` 让用户手动去点击触发文件选择器。
