## 建议改成: 读完这篇你还不懂Babel我给你寄口罩

### 前言

最近在学习webpack, 发现了webpack中一个重要的功能点`babel-loader`, 于是就想着学习了解一波[Babel](https://www.babeljs.cn/docs/).

我们在做一件事, 学习一个知识点的时候, 都应该是抱有一个目的去做的.

在你花了大把时间大把精力去学习这个知识的时候, 它能带给你什么 🤔️ ? 能帮助到你什么🤔️ ?

![你到底有什么软用.jpg](https://user-gold-cdn.xitu.io/2020/2/15/1704712a24e5a4c9?w=500&h=375&f=png&s=141547)

就像我学习Babel一样, 之前一直只知道它是一个`JS`编译器, 大概功能是能帮我们在旧的浏览器环境中将ES6+代码转换成向后兼容版本的`JS`代码, 但是其中重要的转换功能是靠什么实现, 以及里面到底有个什么学问是我没深入了解的, 它对我学习webpack有什么帮助?

在这一篇文章中我并没有介绍过于深入的内容, 但是如果把它当成一个入门Babel的教材来看那我相信它对你是有一定帮助的. 不信如果你读完了它之后再去看看官方的文档, 一定觉得都可以看懂了. **不然的话请评论区留下你的地址, 看我给不给你寄口罩...**

不拐弯抹角了, 嘻嘻 😁, 让我们看看通过这一章节的阅读你能学习到什么:

- @babel/cli
- plugins
- presets
- 配置Babel
- polyfill



### 前期准备

学习一个新的知识, 我还是偏向于用案例的的方式来打开讲解它.

所以在正式开始阅读之前, 让我们先来准备一个这样的案例项目:

```
mkdir babel-basic && cd babel-basic
npm init -y
mkdir src && cd src
touch index.js
```

一顿操作之后, 我们新建的项目目录为:

```
/babel-basic
	|- /src
		|- index.js
	|- package.json
```

现在`package.json`是最原始的配置, 而`index.js`暂时没有写内容.

**package.json**:

```json
{
  "name": "babel-basic",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {}
}

```

下面我都将围绕这个`babel-basic`项目来进行讲解, 我希望你也能在本地准备一个这样的项目案例, 以便你更好的理解我接下来要说的内容.



### @babel/core

我们学习Babel, 首先要了解一个叫`@babel/core` 的东西, 它是Babel的核心模块.

当然要使用它, 我们得先安装:

```
$ npm i --save-dev @babel/core
```

安装成功之后就可以在我们的代码中使用了, 你可以采用`CommonJS`的引用方式:

```
const babel = require('@babel/core');
babel.transform("code", options);
```

这里的知识点有很多, 不过你不用急于的掌握它, 只需要知道它是Babel的核心, 让我们接着往下看.

### @babel/cli

再然后就是`@babel/cli`, 它是一个终端运行工具, 内置的插件,运行你从终端使用babel的工具.

同样, 它也需要先安装:

```
$ npm i --save-dev @babel/cli @babel/core
```

让我们安装`@babel/cli`的同时再来安装一下`@babel/core`, 

现在, 让我先在`src/index.js`中写上一段简单的代码, 并来看看它的基本用法.

**src/index.js**:

```javascript
const fn = () => 1; // 箭头函数, 返回值为1
console.log(fn());
```

**用法一**: 命令行的形式(在项目根目录执行语句):

```
$ ./node_modules/.bin/babel src --out-dir lib
```

这段语句的意思是: 它使用我们设置的**解析方式**来解析`src`目录下的所有`JS`文件, 并将转换后的每个文件都输出到`lib`目录下.

但是注意了, 由于我们现在没有设置任何的解析方式, 所以你在执行了这段语句之后, 能看到项目中多了一个`lib`目录, 而且里面的`JS`代码和`src`中的是一样的. 至于我说的**解析方式**, 就是后面我要介绍的plugins和presets.

另外, 如果你是`npm@5.2.0`附带的`npm`包运行器的话, 就可以用`npx babel`来代替`./node_modules/.bin/babel`:

```
$ npx babel src --out-dir lib
```

**用法二**: 给`package.json`中配置一段脚本命令:

```diff
{
    "name": "babel-basic",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
+       "build": "babel src -d lib"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
+       "@babel/cli": "^7.8.4",
+       "@babel/core": "^7.8.4"
    }
}
```

现在运行`npm run build`效果也是一样的, `-d`是`--out-dir`的缩写...

(我们使用上面的 `--out-dir` 选项。你可以通过使用 `--help` 运行它来查看 cli 工具接受的其余选项。但对我们来说最重要的是 `--plugins` 和 `--presets`。)

```
$ npx babel --help
```



### 插件plugins

#### 基本概念

知道了Babel的基本用法之后, 让我们来看看具体的代码转换.

现在要介绍的是**插件plugins**, 它的本质就是一个`JS`程序, 指示着Babel如何对代码进行转换.

所以你也可以编写自己的插件来应用你想要的任何代码转换.

#### 插件案例(箭头函数插件)

但是首先让我们来学习一些基本的插件.

如果你是要将ES6+转成ES5, 可以依赖官方插件, 例如:

`@babel/plugin-transform-arrow-functions`:

```
$ cnpm i --save-dev @babel/plugin-transform-arrow-functions
$ npx babel src --out-dir lib --plugins=@babel/plugin-transform-arrow-functions
```

这个插件的作用是将箭头函数转换为ES5兼容的函数:

还记得我们之前的`src/index.js`吗:

```javascript
const fn = () => 1; // 箭头函数, 返回值为1
console.log(fn());
```

现在编译之后, 你再打开`lib/index.js`来看看.

 它是不是被转换为ES5的代码了呢? 😁

```javascript
const fn = function () {
  return 1;
}; // 箭头函数, 返回值为1


console.log(fn());
```

捣鼓了这么久, 终于看到了一点实际的效果, 此时有点小兴奋啊😄

表情包开心

虽然我们已经实现了箭头函数转换的功能, 但是`ES6+`其它的语法(比求幂运算符`**`)却并不能转换, 这是因为我们只使用了`@babel/plugin-transform-arrow-functions`这个功能插件, 没有使用其它的了.



### Presets:

#### 基本概念

如果想要转换ES6+的其它代码为ES5, 我们可以使用"preset"来代替预先设定的**一组插件**, 而不是逐一添加我们想要的所有插件.

**这里可以理解为一个preset就是一组插件的集合.**

presets和plugins一样, 也可以创建自己的preset, 分享你需要的任何插件组合.



#### @babel/preset-env

例如, 我们使用`env`preset:

```
cnpm i --save-dev @babel/preset-env
```

`env`preset这个preset包括支持现代JavaScript(ES6+)的所有插件.

所以也就是说你安装使用了`env`preset之后, 就可以看到其它ES6+语法的转换了.

现在让我们来用用ES7中的**求幂运算符**和**函数参数支持尾部逗号**这两个功能吧:

**src/index.js**:

```javascript
const fn = () => 1; // ES6箭头函数, 返回值为1
let num = 3 ** 2; // ES7求幂运算符
let foo = function(a, b, c, ) { // ES7参数支持尾部逗号
    console.log('a:', a)
    console.log('b:', b)
    console.log('c:', c)
}
foo(1, 3, 4)
console.log(fn());
console.log(num);
```

然后在命令行里使用这个`preset`:

```
npx babel src --out-dir lib --presets=@babel/preset-env
```

现在打开`lib/src`看看:

```javascript
"use strict";

var fn = function fn() {
  return 1;
}; // 箭头函数, 返回值为1


var num = Math.pow(3, 2);

var foo = function foo(a, b, c) {
  console.log('a:', a);
  console.log('b:', b);
  console.log('c:', c);
};

foo(1, 3, 4);
console.log(fn());
console.log(num);
```

求幂运算符被转换为成`Math.pow()`

函数参数的最后一个逗号也被去掉了.



截止到现在, 看完了`@babel/core`、`@babel/cli`、`plugins`、`presets`, 相信你对Babel的功能有一定了解了吧, 但是真正使用起来我们不可能都是靠命令行的形式吧, 没错, 接下来我要将这些功能做成配置项.



### 配置

上面👆介绍的都是一些终端传入CLI的方式, 在实际使用上, 我们更加偏向于**配置文件**.

例如我们在项目的根目录下创建一个`babel.config.js`文件:

```javascript
const presets = [
	[
    "@babel/env",
    {
      targets: {
        edge: "17",
        chrome: "64",
        firefox: "60",
        safari: "11.1"
      }
    }
  ]	
]

module.exports = { presets };
```

加上这个配置的作用是:

- 使用了`env`preset这个preset
- `env`preset只会为目标浏览器中没有的功能加载转换插件

现在你要使用这个配置就很简单了, 直接用我们前面**package.json**配置的命令行语句:

```javascript
{
	"scripts": {
		"build": "babel src -d lib"
	}
}
```

执行`npm run build`就可以了.

这个命令行语句看起来并没有修改, 那是因为它默认会去寻找跟根目录下的一个名为`babel.config.js`的文件(或者`babelrc.js`也可以, 这个在之后的`使用babel的几种方式`中会说到), 所以其实就相当于以下这个配置:

```javascript
{
	"scripts": {
		"build": "babel src -d lib --config-file ./babel.config.js"
	}
}
```

因此如果你的Babel配置文件是`babel.config.js`的话, 这两种效果是一样的.

(`--config-file`指令就类似于webpack中的`--config`, 用于指定以哪个配置文件构建)



这里我重点要说一下**只会为目标浏览器中没有的功能加载转换插件**这句话的意思.

例如我这里配置的其中一项是`edge: "17"`, 那就表示它转换之后的代码支持到`edge17`.

所以你会发现, 如果你用了我上面`babel.config.js`的配置之后生成的`lib`文件夹下的代码好像并没有发生什么改变, 也就是它并没有被转换成`ES5`的代码:

**src/index.js**:

```javascript
const fn = () => 1; // ES6箭头函数, 返回值为1
let num = 3 ** 2; // ES7求幂运算符
let foo = function(a, b, c, ) { // ES7参数支持尾部逗号
    console.log('a:', a)
    console.log('b:', b)
    console.log('c:', c)
}
foo(1, 3, 4)
console.log(fn());
console.log(num);
```

**使用`babel.config.js`配置之后构建的`lib/index.js`**:

```javascript
"use strict";

const fn = () => 1; // ES6箭头函数, 返回值为1


let num = 3 ** 2; // ES7求幂运算符

let foo = function foo(a, b, c) {
  // ES7参数支持尾部逗号
  console.log('a:', a);
  console.log('b:', b);
  console.log('c:', c);
};

foo(1, 3, 4);
console.log(fn());
console.log(num);
```

箭头函数依旧是箭头函数, 求幂运算符依旧是求幂运算符.

**这是因为在Edge17浏览器中支持ES7的这些功能, 所以它就没有必要将其转换了, 它只会为目标浏览器中没有的功能加载转换插件!!!**

如果我们将`edge17`改成`edge10`看看 🤔️?

**babel.config.js**:

```diff
const presets = [
    [
        "@babel/env",
        {
            targets: {
-               edge: "17",
+               edge: "10",
                firefox: "60",
                chrome: "67",
                safari: "11.1",
            },
        },
    ],
];

module.exports = { presets };
```

保存重新运行`npm run build`, 你就会发现`lib/index.js`现在有所改变了:

```javascript
"use strict";

var fn = function fn() {
  return 1;
}; // ES6箭头函数, 返回值为1


var num = Math.pow(3, 2); // ES7求幂运算符

var foo = function foo(a, b, c) {
  // ES7参数支持尾部逗号
  console.log('a:', a);
  console.log('b:', b);
  console.log('c:', c);
};

foo(1, 3, 4);
console.log(fn());
console.log(num);
```



### Polyfill

Plugins是提供的插件, 例如箭头函数转普通函数`@babel/plugin-transform-arrow-functions`

Presets是一组Plugins的集合.

**而Polyfill是对执行环境或者其它功能的一个补充.**

什么意思呢 🤔️?

就像现在你想在`edge10`浏览器中使用ES7中的方法`includes()`, 但是我们知道这个版本的浏览器环境是不支持你使用这个方法的, 所以如果你强行使用并不能达到预期的效果.

而`polyfill`的作用正是如此, 知道你的环境不允许, 那就帮你引用一个这个环境, 也就是说此时编译后的代码就会变成这样:

```javascript
// 原来的代码
var hasTwo = [1, 2, 3].includes(2);

// 加了polyfill之后的代码
require("core-js/modules/es7.array.includes");
require("core-js/modules/es6.string.includes");
var hasTwo = [1, 2, 3].includes(2);
```

这样说你应该就能看懂它的作用了吧 😁

表情包装逼

现在就让我们来学习一个重要的`polyfill`, 它就是`babel/polyfill`.

`babel/polyfill`用来模拟完成ES6+环境:

- 可以使用像`Promise`或者`WeakMap`这样的新内置函数
- 可以使用像`Array.from`或者`Object.assign`这样的静态方法
- 可以使用像`Array.prototype.includes`这样的实例方法
- 还有`generator`函数

为了实现这一点, Polyfill增加了**全局范围**以及像String这样的原生原型.

而`@babel/polyfill`模块包括了`core-js`和自定义`regenerator runtime`

对于**库/工具**来说, 如果你不需要像`Array.prototype.includes`这样的实例方法, 可以使用`transform runtime`插件, 而不是使用污染全局的`@babel/polyfill`.

对于**应用程序**, 我们建议安装使用`@babel/polyfill`

```
cnpm i --save @babel/polyfill
```

(注意 `--save` 选项而不是 `--save-dev`，因为这是一个需要在源代码之前运行的 polyfill。)

但是由于我们使用的是`env`preset, 这里个配置中有一个叫做 `"useBuiltIns"`的选项

如果将这个选择设置为`"usage"`, 就只包括你需要的polyfill

此时的`babel.config.js`调整为:

```diff
const presets = [
	[
		"@babel/env",
		{
			targets: {
				edge: "17",
				chrome: "64",
				firefox: "67",
				safari: '11.1'
			},
+			useBuiltIns: "usage"
		}
	]
]

module.exports = { presets }
```

安装配置了`@babel/polyfill`, Babel将检查你的所有代码, 然后查找目标环境中缺少的功能, 并引入仅包含所需的polyfill

(如果我们没有将 `env` preset 的 `"useBuiltIns"` 选项的设置为 `"usage"` ，就必须在其他代码之前 require *一次完整*的 polyfill。)

还是上面👆的那个例子, 我们来改造一下, 使用`Edge17`中没有的`Promise.prototype.finally`:

**src/index.js**:

```javascript
const fn = () => 1; // ES6箭头函数, 返回值为1
let num = 3 ** 2; // ES7求幂运算符
let hasTwo = [1, 2, 3].includes(2)
let foo = function(a, b, c, ) { // ES7参数支持尾部逗号
    console.log('a:', a)
    console.log('b:', b)
    console.log('c:', c)
}
foo(1, 3, 4)
Promise.resolve().finally();
console.log(fn());
console.log(num);
console.log(hasTwo);
```

现在执行`npm run build`之后生成的`lib/index.js`变成了:

```javascript
"use strict";

require("core-js/modules/es7.promise.finally");

const fn = () => 1; // ES6箭头函数, 返回值为1


let num = 3 ** 2; // ES7求幂运算符

let hasTwo = [1, 2, 3].includes(2);

let foo = function foo(a, b, c) {
  // ES7参数支持尾部逗号
  console.log('a:', a);
  console.log('b:', b);
  console.log('c:', c);
};

foo(1, 3, 4);
Promise.resolve().finally();
console.log(fn());
console.log(num);
console.log(hasTwo);
```

`@babel/polyfill`帮我们引入了`Edge17` 环境中没有的`promise.finally()` 



### 被deprecated的@babel/polyfill

上面我介绍了一种名为`@babel/polyfill`的polypill, 其实它在Babel7.4.0以上已经不被推荐使用了.

而是推荐使用`core-js@3`+`@babel/preset-env`然后设置`@babel/preset-env`的`corejs`选项为`3`.

因此如果你按着我文章中讲方式使用`@babel/polyfill`, 是可以实现的, 不过控制台中会抛出一个警告⚠️:

```
WARNING: We noticed you're using the `useBuiltIns` option without declaring a core-js version. Currently, we assume version 2.x when no version is passed. Since this default version will likely change in future versions of Babel, we recommend explicitly setting the core-js version you are using via the `corejs` option.

You should also be sure that the version you pass to the `corejs` option matches the version specified in your `package.json`'s `dependencies` section. If it doesn't, you need to run one of the following commands:

  npm install --save core-js@2    npm install --save core-js@3
  yarn add core-js@2              yarn add core-js@3
```

解决办法是卸载掉`@babel/polyfill`, 然后重新安装`core-js@版本号`, 然后重新配置一些babel.config.js文件.

1. 安装`core-js@3`:

```
cnpm i --save core-js@3
```

2. 添加`corejs`选项:

```diff
const presets = [
	[
		"@babel/env",
		{
			targets: {
				edge: "17",
				chrome: "64",
				firefox: "67",
				safari: '11.1'
			},
			useBuiltIns: "usage",
+			corejs: 3
		}
	]
]

module.exports = { presets }
```

`(useBuiltIns`选项还是不能去掉)

现在重新`npm run build`之后就不会有这个警告了, 而且生成的`lib`也是正确的.

(感谢掘友 [KsRyY](https://juejin.im/user/5c5ed71cf265da2dcf625f1c) 的细心提示😁 )

### 小结

- `babel/cli` 允许我们从终端运行Babel
- `env`preset 只包含我们使用的功能的转换,实现我们的目标浏览器中缺少的功能
- `@babel/polyfill`实现所有新的`JS`功能, 为目标浏览器引入缺少的环境(但是Babel7.4.0以上不推荐使用)



### 后语

哈哈😄, 不好意思开头骗了大家...寄口罩不存在的 😂 我自己也是被关在家里不敢出门...

看我为了能让大家老实呆家学习多费心啊 😂

最后...

喜欢**霖呆呆**的小伙还希望可以关注霖呆呆的公众号 `LinDaiDai` 或者扫一扫下面的二维码👇👇👇.

我会不定时的更新一些前端方面的知识内容以及自己的原创文章🎉

你的鼓励就是我持续创作的主要动力 😊.

相关推荐:

[《JavaScript进阶-执行上下文(理解执行上下文一篇就够了)》](https://juejin.im/post/5db85b866fb9a0207d4cbf92)

[《全网最详bpmn.js教材》](https://juejin.im/post/5def372af265da33c84a4818)

[《霖呆呆你来说说浏览器缓存吧》](https://juejin.im/post/5e2d7d3a6fb9a02fec665157)

[《怎样让后台小哥哥快速对接你的前端页面》](https://juejin.im/post/5d89b2a7f265da03dd3db2ca)

