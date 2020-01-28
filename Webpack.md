![webpack打包](D:\notes\Vue\img\12.webpack\webpack打包.png)



# `webpack`配置文件

先`npm init`，在项目中生成一个`package.json`。

再取一个名叫`webpack.config.js`的文件，内容如下：

```js
const path = require('path') // 用于第6行自动获取输出文件的绝对路径做铺垫

module.exports = {
    entry: './src/main.js', // 入口
    output: { // 出口,有别于入口,有两个配置
        path: path.resolve(__dirname, 'dist'), 
        // 输出路径,使用绝对路径,使用上述引入的path中的resolve方法,来自动找到当前的 webpack.config.js 存在的根路径,后面的 dist 是指定的要输出的文件夹名称
        filename: 'bundle.js' // 指定文件夹中具体的文件名
    }
}
```

有了上述配置之后，我们只要敲击`webpack`就可以将我们的代码更快捷地打包了。:happy:

但是感觉单纯敲一个`webpack`有点呆，我们还是习惯于使用打包命令`npm run build`进行打包，所以：

```js
// 回到package.json文件中,处理一下scripts字段
"scripts": {
    "build": "webpack" // 加上这么一条,当我们敲击npm run build的时候,就会来这里找到后面的webpack命令,并且执行该命令; 还有就是加上这条指令之后,就会优先执行本地的webpack进行打包,而非全局版本的webpack进行打包
}
```



# 本地安装`webpack`

为什么要在本地进行安装？

因为上述方式通过`script`将打包命令给改写，优先会从本地进行查找。这样有什么好处呢？

当我们下载项目到本地的时候，如果我们全局`webpack`版本不同于项目中配置的`webpack`版本，那么在进行打包的时候就会报错。但是只要经过上述处理后，优先使用本地的`webpack`，而非全局`webpack`进行打包，会少掉很多报错。

```shell
npm install webpack@3.6.0 --save-dev // 开发时依赖
```

执行上述代码会在`package.json`中多出一个`devDependencies`字段，用来存储所有我们在开发过程中需要使用到的东西。

`package.json`中的`scripts`脚本在执行的时候，会按照一定的顺序寻找命令对应的位置，首先会寻找本地的`node_modules/.bin`路径中对应的命令，如果没有找到，才会去全局的环境变量中寻找。



# `loader`的配置

我们主要使用`webpack`来处理`js`代码，并且`webpack`会自动处理`js`之间的相互依赖。

但是在开发中我们还需要处理加载`css`、图片、也包括一些高级的将`ES6`转换成`ES5`代码，将`TypeScript`转成`ES5`代码，将`scss`、`less`转成`css`，将`.jsx`、`.vue`文件转成`js`文件等等。

`webpack`本身是不支持这些功能的，但只要给`webpack`扩展对应的`loader`就可以了。

`loader`使用过程：

- 通过`npm`安装需要使用的`loader`
- 在`webpack.comfig.js`中的`modules`关键字下进行配置

## 打包`css`文件的`loader`配置

安装：

![loader-css](D:\notes\Vue\img\12.webpack\loader-css.png)

配置：[官网]( https://webpack.js.org/loaders/css-loader/ )

![loaders](D:\notes\Vue\img\12.webpack\loaders.png)

```shell
// 安装css-loader
npm install --save-dev css-loader
// 配置,官网上有呢
```

![cssloader配置](D:\notes\Vue\img\12.webpack\cssloader配置.png)

```shell
// 安装style-loader
npm install --save-dev style-loader
// 配置和css-loader相同
```

启动`npm run bulid`，发现`css`样式文件已经成功被打包了，对应的样式文件也成功被应用到了界面上。



## `less`文件的配置

首先创建一个`less`文件，随便写一些东西，然后在入口函数中引入该文件。

![less处理](D:\notes\Vue\img\12.webpack\less处理.png)

去[官网]( https://www.webpackjs.com/loaders/less-loader/ )进行下载配置。

![less配置](D:\notes\Vue\img\12.webpack\less配置.png)



## 图片文件处理

因为我们的图片是通过`url`来进行引入的，所以我们应该找到对应的`url-loader`。

[官网]( https://www.webpackjs.com/loaders/url-loader/ )

将图片转换成`base64`格式字符串。

此处存在两种可能：关于图片大小与`8kb`之间的比较问题。

首先导入一张小于`8kb`的图片，并成功引用。

![图片配置](D:\notes\Vue\img\12.webpack\图片配置.jpg)

下载并配置：

![图片处理2](D:\notes\Vue\img\12.webpack\图片处理2.jpg)

---

这次导入一张大于`limit: 8192`的图片。

这时我们打包会报错，参考报错信息，我们应该安装`file-loader`模块去读取超过限制大小的图片。

==注意==：此时我们打包超过`limit`大小的图片会重新生成在`dist`文件中，但如果我们想要对图片进行管理（命名，导出位置）等，还需要进行以下配置：

![图片处理3](D:\notes\Vue\img\12.webpack\图片处理3.jpg)

![大尺寸图片处理](D:\notes\Vue\img\12.webpack\大尺寸图片处理.jpg)



## `ES6`语法的处理

将所有我们使用的`ES6`语法代码打包到`dist`文件夹下为`ES5`语法，成为浏览器能够支持的语法。[官网]( https://www.webpackjs.com/loaders/babel-loader/ )

```shell
// 这里我们并没有安装官网的配置来,而是根据自己的需要做了一下简单的配置
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```

![es6转为es5](D:\notes\Vue\img\12.webpack\es6转为es5.jpg)



# 配置`Vue`

```shell
// 将vue安装到我们的开发依赖中
npm install --save
```

==注意==：`Vue`有两个版本：

- `runtime-only` 代码中不可以有任何的`template`
- `runtime-compiler` 代码中可以有`template`，因为有`compiler`可用于编译`template`

所以我们在使用第一个版本的时候想做第二个版本的事情，就会报错。

解决方式：回到`webpack.config.js`中

```js
resolve: {
    // 别名
    alias: {
        // 下面这行代码的意思就是,当我们import vue的时候,他会默认去 vue/dist/vue.esm.js 里面去找能够解析template模板的vue模板
        // 也就是说将我们 import 的 vue 做一下替换,换成能够识别template的vue版本
        'vue$': 'vue/dist/vue.esm.js'
    }
}
```

![vue的配置](D:\notes\Vue\img\12.webpack\vue的配置.jpg)

![vue配置2](D:\notes\Vue\img\12.webpack\vue配置2.jpg)



# `el`和`template`之间的关系

![el和template](D:\notes\Vue\img\12.webpack\el和template.jpg)

简化上述代码：提取出`template`、`data`、`methods`

![简化](D:\notes\Vue\img\12.webpack\简化.jpg)

将定义组件部分再次提取出来

![简化2](D:\notes\Vue\img\12.webpack\简化2.jpg)

再次简化

![简化3](D:\notes\Vue\img\12.webpack\简化3.jpg)



# 配置`Vue`

我们发现，`webpack`并不能识别`.vue`结尾的文件。

我们需要去配置`vue-loader`和`vue-template-compiler`，前者是对`vue`文件进行一个加载，后者是对`vue`文件进行编译的。

```shell
// 1.安装
npm install vue-loader vue-template-compiler --save-dev
```

配置：

![配置运行vue文件](D:\notes\Vue\img\12.webpack\配置运行vue文件.jpg)

根组件`App.vue`引用其他组件

![组件的使用](D:\notes\Vue\img\12.webpack\组件的使用.jpg)

扩展名的简写配置：

![配置扩展名](D:\notes\Vue\img\12.webpack\配置扩展名.jpg)



# `plugin`插件

用于对某个现有的架构进行扩展。

`webpack`中的插件就是对`webpack`现有功能的各种扩展，比如：打包优化、文件压缩等。

`loader`和`plugin`的区别：

- `loader`主要用于转换某些类型的模块，是一个转换器
- `plugin`是插件，是对`webpack`本身的扩展，是一个扩展器 

`plugin`使用步骤：

- 通过`npm`安装需要使用的`plugins`（某些已经内置的插件就不需要安装了）
- 在`webpack.config.js`中的`plugins`中配置插件

作用：给我们打包的文件添加一些说明信息。

## `BannerPlugin`

为打包的文件添加一个版权信息。

![插件1](D:\notes\Vue\img\12.webpack\插件1.jpg)



## `HtmlWebpackPlugin` 

自动生成一个`index.html`文件（可以指定模板来生成），并将打包的`js`文件自动通过`script`标签插入到`body`中。

```shell
// 安装
npm install html-webpack-plugin --save-dev
```

配置：

![生成html的配置1](D:\notes\Vue\img\12.webpack\生成html的配置1.jpg)

![html打包最终示意图](D:\notes\Vue\img\12.webpack\html打包最终示意图.jpg)

## `uglifyjs-webpack-plugin`

对`js`进行丑化压缩：去空格，将变量名用简单的符号进行代替等。

对`js`代码进行压缩并且和`cli2`保持一致，我们选择了第三方插件`uglifyjs-webpack-plugin`，并且==版本号==指定1.1.1。

```shell
// 1.下载
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev
```

![压缩js](D:\notes\Vue\img\12.webpack\压缩js.jpg)

![注意版本号](D:\notes\Vue\img\12.webpack\注意版本号.jpg)



# 搭建本地服务器

```shell
// 下载
npm install webpack-dev-server@2.9.3 --save-dev
```

![搭建服务器](D:\notes\Vue\img\12.webpack\搭建服务器.jpg)

通过`npm run dev`就可以将项目跑起来了。



# `webpack`配置文件的分离处理

根据开发配置和发布配置的不同进行一个适当的分离处理。

将`webpack.config.js`分离成：

- 1.`base.config.js`用于存储公共的代码配置文件 

- 2.`dev.config.js`用于存储开发时候的代码 

- 3.`prod.config.js`用于存储生产上线时候的配置。

  也就是说，当我们配置分离完成之后，`webpack.config.js`文件就没有用处了。

![分散配置](D:\notes\Vue\img\12.webpack\分散配置.jpg)

```shell
// 用于将我们的配置文件进行合并处理
npm install webpack-merge --save-dev 
```

**公共部分配置**：

![公共代码部分](D:\notes\Vue\img\12.webpack\公共代码部分.jpg)

**开发部分配置**：

![开发部分](D:\notes\Vue\img\12.webpack\开发部分.jpg)

**生产部分配置**

![生产部分](D:\notes\Vue\img\12.webpack\生产部分.jpg)

**最后指令的修改**：

![指令的修改](D:\notes\Vue\img\12.webpack\指令的修改.jpg)

完事之后，我们执行`npm run bulid`会发现项目被自动打包到了

![输出路径](D:\notes\Vue\img\12.webpack\输出路径.jpg)

按照我们的规则，他会先根据`base.config.js`的位置，在其同级新建一个`dist`文件夹，然后进行打包输出。这不是我们想要得到的最终结果。

我们只要将`output`中的`path`路径，给他添加上`(__dirname,'../dist')`即可。