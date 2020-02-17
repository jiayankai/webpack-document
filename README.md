## 前言

本质上, `webpack`是一个现代`JS`应用程序的静态模块打包器(module bundler).

而想要真正学习`webpack`这门技术也并不是那么简单的事.

网上关于`webpack`的文章教材数不胜数, 在看了几篇文章之后, 感觉自己对其掌握的其实并不扎实.

所以趁着肺炎假期的这段时间, 想静下心来阅读[《webpack中文文档》](https://www.webpackjs.com/concepts/)并将其中主要的案例都“敲透”, “吃透”...

但是当我真正打开`webpack官网文档`想要学习时, m耶, 为什么感觉这一个个什么`html-webpack-plugin`、 `@babel/plugin-xxx`就和我的脑子这么没有缘, 眼睛看过去就看过去了, 但是脑子完全没有留住它...

**webpack: "是我不够迷人吗?"**

![找不到表情包](/Users/lindaidai/Pictures/表情包/babel/找不到表情包.jpeg)



而且就算我强行记住了官网给出的那些概念, 可还是不能够啊, 不能够正在理解它每个知识点的意思.

于是我转变了一种思路, 我是一个现实的人, 我就想看到我写的代码有些实质性的东西能让人看得着摸得着.

这也是我为什么喜欢前端的原因, 你敲一行代码就能看到一个效果, 多真实啊...

于是我先抛弃文档那些“晦涩难懂”的概念, 本地创建一个案例, 按着一个个新的知识点安装配置看看效果, 然后根据**实际显示的效果**与**文档的概念**进行对比, 再来理解它其中想要表达的意思. 同时将每个知识点做好笔记, **一定要有你自己对它的理解**.

然后我发现这种学习方式对于我这样的笨鸟还是很有帮助的, 虽然花费的时间可能久了些, 但是至少能让我掌握了我想要学的这个知识点.

让自己慢慢沉淀下来... 现在对它有了一个新的认识...

之前的自己面对错中复杂的`loader`、`plugin`总是一知半解似懂非懂.

而如果你真正花了心思的去学习了解它, 那么曾经让你“躺目结舌”的配置, 代码好像也能慢慢看懂理解了.

此项目记录了我在`webpack`上的学习历程.如果你也和我一样想要好好的掌握`webpack`, 那么我认为它是对你是有一定帮助的, 因为教材中是以一名webpack小白的身份进行讲解, 案例demo也都很详细, 建议先mark再花时间来看.

最后, 编写整理教材不易, 还希望能求个Star🌟, 你的支持和鼓励是霖呆呆持续创作的主要动力, Thank you~



## 目录

关于各个知识点的教材地址以及对应的案例地址:

### [基础篇](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-基础篇.md)

**文档地址**: [《霖呆呆的webpack之路-基础篇》](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-基础篇.md)

- 基本使用
- 使用配置文件
- 资源管理: style-loader, css-loader, file-loader
- 管理输出: 多个输入/输出, HtmlWebpackPlugin, CleanWebpackPlugin

**案例地址**: [LinDaiDai/webpack-basic]()



### [构建方式篇](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-构建方式篇.md)

**文档地址**: [《霖呆呆的webpack之路-构建方式篇》](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-构建方式篇.md)

- webpack --watch
- webpack-dev-server 工具
- webpack-dev-middle 工具, 以及配合express搭建本地web服务器

**案例地址**: [LinDaiDai/webpack-basic]()

🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟

- webpack-merge 构建不同的环境
- process.env.NODE_ENV 的基本使用
- webpack.DefinePlugin 插件指定 NODE_ENV

**案例地址**: [LinDaidai/webpak-merge]()



### [优化篇](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-优化篇.md)

**文档地址**: [《霖呆呆的webpack之路-优化篇》](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-优化篇.md)

- tree shaking 与压缩输出

**案例地址**: [LinDaiDai/webpack-basic]()

🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟

- 代码分离-webpack3中的CommonsChunkPlugin

**案例地址**: [LinDaiDai/webpack3-demo]()

- 代码分离-webpack4中的SplitChunksPlugin

**案例地址**: [LinDaiDai/webpack-code-splitting]()

- 代码分离-import动态导入
- 懒加载-import动态导入

**案例地址**: [LinDaiDai/webpack-dynamic-imports]()

🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟🌟

- css代码分离

**案例地址**: [LinDaiDai/webpack-loader]()



### [loader篇](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-loader篇.md)

**文档地址**: [《霖呆呆的webpack之路-loader篇》](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-loader篇.md)

- 文件
- 转换编译
- 模版



### [配置篇](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-配置篇.md)

- mode
- output
- devtool

**文档地址**: [《霖呆呆的webpack之路-配置篇》](https://github.com/LinDaiDai/webpack-document/blob/master/霖呆呆的webpack之路-配置篇.md)

**案例地址**: 在教材中的每一项有具体说明



## 后语

喜欢**霖呆呆**的小伙还希望可以关注霖呆呆的公众号 `LinDaiDai` 或者扫一扫下面的二维码👇👇👇.

<img src="./resource/LinDaiDai公众号二维码.png" alt="LinDaiDai公众号" />

我会不定时的更新一些前端方面的知识内容以及自己的原创文章🎉

你的支持和鼓励是我持续创作的主要动力 😊.

<img src="./resource/LinDaiDai赞赏码.png" alt="LinDaiDai赞赏码" />