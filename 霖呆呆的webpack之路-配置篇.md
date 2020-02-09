# 霖呆呆的webpack之路-配置篇

## mode

**类型**:  String

**默认值**: `"none"`

**功能**: 设置用到的模式

其它模式:

- development 开发模式(本地开发)
- production 生成模式(发布到线上环境)

在`webpack4`中, 如果将`mode`设置成`production`就会启用`webpack`内部的`uglifyjs`插件, 进入**压缩输出**.此时的`bundle`是经过了`tree shaking`和代码压缩的.

(关于`tree shaking`和压缩输出可以查看《霖呆呆的webpack之路-优化篇》)



## 输出(output)

### publicPath

**类型**: String | Function

**默认值**: 空字符串`""`

**功能**: 指定输出目录对应的公开URL, 影响的主要是**外部资源的引用**和`webpack-dev-server`的`publicPath`

**官网地址**: [output.publicPath](https://www.webpackjs.com/configuration/output/#output-publicpath)

**案例一**🌰:

例如我们使用`webpack`打包生成的`dist`文件目录为:

```
/dist
	|- bundle.js
	|- index.html
	|- icon.png
```

默认对于资源的引用, 比如`icon.png`的引用是这样的:

```css
.box {
	background: url('icon.png') 
}
```

如果设置了`output.publicPath`之后:

**webpack.config.js**

```javascript
...
module.exports = {
	output: {
		...
		publicPath: '/assets/'
	}
}
```

浏览器就会在资源文件的引用加上一个前缀, 变成:

```
.box {
	background: url('/assets/icon.png') 
}
```

此时, 图片就会加载失败, 因为`dist`文件夹下并没有`assets`这个文件夹.

**案例二**🌰:

影响了`webpack-dev-server`或者`webpack-dev-middleware`中的`publicPath`

如果了解过`webpack-dev-server`的小伙应该都知道, `webpack-dev-server`的内部就是使用了`webpack-dev-middleware`.

而我们在配置`webpack-dev-middleware`的时候, 是有一个可选项参数的.

比如本地编写的一个`server.js`:

```diff
// server.js
const express = require('express')
const webpack = require('webpack')
const webpackDevMiddleware = require('webpack-dev-middleware')

const app = express()
const config = require('./webpack.config')
const compiler = webpack(config)
// 把webpack 处理后的文件传递给一个服务器
app.use(webpackDevMiddleware(compiler 
+	,{
+		publicPath: config.output.publicPath
+	}
))

app.listen(3000, function() {
    console.log('Example app listening on port 3000!\n');
})
```

如果指定了`output.publicPath`, 那就必须也指定一下这里的`publicPath`与`output`的一致, 不然,重新执行`npm run server`, 打开`localhost:3000` 会发现页面显示的是:

```
Cannot GET /
```

你需要打开`localhost:3000/assets/`才能看到正确的页面.

关于此案例具体的可以查看: 《霖呆呆的webpack之路-》



## devtool

**类型**: String | false

**默认值**: none

**作用**:选择一种 [source map](http://blog.teamtreehouse.com/introduction-source-maps) 格式来增强调试过程。不同的值会明显影响到构建(build)和重新构建(rebuild)的速度。

常用的选项:

- `inline-source-map` 开发环境一般设置为这个, 可以使得报的错具体到报错的文件代码里
- `source-map` 生产环境可以设置成这个,  整个 source map 作为一个单独的文件生成。它为 bundle 添加了一个引用注释，以便开发工具知道在哪里可以找到它。