## webpack学习：

> 学习整理来自掘金网大佬，源文章地址：https://juejin.im/post/5de87444518825124c50cd36
>
> `webpack`的使用在我看来由三部分组成，入口文件（`main.js`）、输出文件（`output.js`）和打包时的相关配置项（规则[`webpack.config.js`]）

#### 1. 入门

##### 1.1 初始化项目

新建一个空文件夹，初始化`npm`

```
npm init
```

`webpack`运行在`node`环境中，需要安装包：

```
npm i -D webpack webpack-cli
```

###### 注：

> npm i -D 为npm install --save-dev的缩写
>
> npm i -S 为npm install --save的缩写

###### webpack默认配置：

新建一个`src`文件夹，新建`main.js`文件：

```
// main.js 中随便写入点JavaScript代码
console.log("Hello world!");
```

在根目录新建`package.json`文件，对文件写入`webpack`的配置：

```
{
	"scripts":{
		"build":"webpack src/main.js"
	}
}
```

执行：

```
npm run build
```

此时在根目录创建了一个`dist`文件夹，里面为打包之后的文件`main.js`

##### 1.2 自定义配置：

新建一个`build`文件夹，在里面新建一个`webpack.config.js`文件：

```
// webpack.config.js

const path = require('path');
module.exports = {
    mode:'development', // 开发模式  production 为生产模式
    entry: path.resolve(__dirname,'../src/main.js'),    // 入口文件
    output: {
        filename: 'output.js',      // 打包后的文件名称
        path: path.resolve(__dirname,'../dist')  // 打包后的目录
    }
}
```

修改`package.json`文件：

```
{
	"scripts":{
		"build":"webpack --config build/webpack.config.js"  // 使用我们的配置文件
	}
}
```

执行`npm run build` 之后将会生成dist文件夹，文件夹内的`output.js`为打包完的文件

##### 1.3 配置html模板

> js文件打包好了,但是我们不可能每次在`html`文件中手动引入打包好的js

这里可能有的朋友会认为我们打包js文件名称不是一直是固定的嘛(`output.js`)？这样每次就不用改动引入文件名称了呀？实际上我们日常开发中往往会这样配置:

```
module.exports = {
    // 省略其他配置
    output: {
      filename: '[name].[hash:8].js',      // 打包后的文件名称（为避免浏览器缓存，输出文件名中包含了hash值）
      path: path.resolve(__dirname,'../dist')  // 打包后的目录
    }
}
```

为了避免浏览器缓存在打包后的文件名中都加入了hash值（每次打包之后的文件名都不同）但是js文件需要引入到`html`中，每次手动修改引入显得很麻烦，所以我们需要一个插件：

```
npm i -D html-webpack-plugin
```

根目录新建一个`public`文件夹，文件夹中新建一个`index.html`文件，然后编辑`webpack`的配置文件：

```
// webpack.config.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode:'development', // 开发模式
    entry: path.resolve(__dirname,'../src/main.js'),    // 入口文件
    output: {
      filename: '[name].[hash:8].js',      // 打包后的文件名称
      path: path.resolve(__dirname,'../dist')  // 打包后的目录
    },
    plugins:[
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/index.html')
      })
    ]
}
```

执行打包之后出现dist文件夹，里面有打包完的js文件和html文件，并且html文件中已经引入了那个js文件，在执行过程中可能会报错`'webpack' 不是内部或外部命令，也不是可运行的程序
或批处理文件`，这时候需要全局安装`webpack`，在安装之后执行打包还可能报错` Cannot find module 'webpack/lib/node/NodeTemplatePlugin'`，这是插件指向的问题，执行以下命令`npm link webpack --save-dev`即可解决

##### 1.3.1 多入口文件的配置

> 需要配置多个html文件，生成多个html-webpack-plugin实例来解决这个问题

配置如下：

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    mode:'development', // 开发模式
    entry: {
      main:path.resolve(__dirname,'../src/main.js'),
      header:path.resolve(__dirname,'../src/header.js')
  }, 
    output: {
      filename: '[name].[hash:8].js',      // 打包后的文件名称
      path: path.resolve(__dirname,'../dist')  // 打包后的目录
    },
    plugins:[
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/index.html'),
        filename:'index.html',
        chunks:['main'] // 与入口文件对应的模块名
      }),
      new HtmlWebpackPlugin({
        template:path.resolve(__dirname,'../public/header.html'),
        filename:'header.html',
        chunks:['header'] // 与入口文件对应的模块名
      }),
    ]
}
```

同样地，执行打包之后会在dist文件夹中生成`index.html`和`header.html`还有`js`文件并在入口文件内引入了相关模块的`js`文件

##### 1.3.2 clean-webpack-plugin

> 每次执行npm run build 会发现dist文件夹里会残留上次打包的文件，这里我们推荐一个plugin来帮我们在打包输出前清空文件夹`clean-webpack-plugin`

```
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
module.exports = {
    // ...省略其他配置
    plugins:[new CleanWebpackPlugin()]
}
```

##### 1.4 引用css

因为我们的入口文件是js，所以需要在入口文件中引入我们的css文件，如图：

![p6.png](https://user-gold-cdn.xitu.io/2019/12/11/16ef2e43d850e765?imageView2/0/w/1280/h/960/ignore-error/1)

同时需要我们安装一些插件来解析css文件：

```
npm i -D style-loader css-loader
```

如果引入了less文件，还需要安装插件：

```
npm i -D less less-loader
```

相关配置如下：

```
// webpack.config.js
module.exports = {
    // ...省略其他配置
    module:{
      rules:[
        {
          test:/\.css$/,
          use:['style-loader','css-loader'] // 从右向左解析原则
        },
        {
          test:/\.less$/,
          use:['style-loader','css-loader','less-loader'] // 从右向左解析原则
        }
      ]
    }
}
```



