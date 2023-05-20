# 渲染类型

v4 版本支持 `rendererType` 配置项，支持 dom 和 canvas 两种类型，默认使用 canvas

```javascript
const terminal = new Terminal({
	rendererType: 'dom'
})
```

v5 版本废弃了这个配置项，xterm 将 canvas 渲染器单独抽离出来，封装成了一个插件 [xterm-addon-canvas](https://github.com/xtermjs/xterm.js/tree/master/addons/xterm-addon-canvas)，默认使用 dom 进行渲染。另外 v5 版本推荐使用 webgl 来进行渲染，而 canvas 作为一种备用的[回退方案](https://github.com/Tyriar/xterm.js/commit/8d57c64d29080de015d051617ad4f41d48edff70)。

```javascript
try {  
	// https://github.com/xtermjs/xterm.js/issues/3357  
	// safari < 16 报错, 需要 try catch
	if (detectWebGL2Context()) {
		this.webglAddon = new WebglAddon()
		this.webglAddon.onContextLoss((e) => {
			log.error('something error: lost webgl context', e)
			this.webglAddon?.dispose()
		})
		this.loadAddon(this.webglAddon)
		log.info('render xterm use webgl')
	} else {
		throw new Error('current browser does not support webgl')
	}
} catch (e) {
	log.warn('WebGL renderer could not be loaded, falling back to canvas renderer')
	disposeWebglRenderer()
	enableCanvasRenderer()
}
```
