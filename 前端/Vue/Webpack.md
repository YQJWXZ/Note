# Webpack

前端项目工程化的具体解决方案

## 1. Webpack基础

### 1.1 安装webpack
> npm install webpack@版本号 webpack-cli@版本号 -D

### 1.2 在项目中配置webpack
1. 项目根目录中常见名为webpack.config.js的webpack配置文件，并初始化
```js
module.exports = {
    mode: 'development'  // mode用来指定构建模式，可选值有development 和 production
}
```

2. 在package.json的scripts节点下，新增dev脚本如下
```json
"scripts": {
    "dev": "webpack"  // scripts节点下的脚本，可以通过npm run执行
}
```

3. 在终端中运行npm run dev命令，启动webpack进项项目的打包构建

### 1.3 webpack中的默认约定

在webpack4.x和5.x的版本中，有如下的默认约定：
* 默认的打包入口文件为src -> index.js
* 默认的输出文件路径为dist -> main.js

自定义打包的入口与出口：
* 在webpack.config.js配置文件中，通过entry节点指定打包的入口。通过output节点指定打包的出口
```js
const path = require('path')  // 导入node.js中专门操作路径的模块

module.exports = {
    entry: path.join(__dirname, './src/index/js'),  // 打包入口文件的路径
    output: {
        path: path.join(__dirname, './dist'),  // 输出文件的存放路径
        filename: 'bundle.js'  // 输出文件的名称
    }
}
```

### 1.4 webpack中的插件

1. webpack-dev-server(会启动一个实时打包的http服务器)

> npm install webpack-dev-server@版本号 -D

配置webpack-dev-server：
* 修改package.json -> scripts中的dev命令：
```json
"scripts": {
    "dev": "webpack serve",  
}
```
* 再次运行npm run dev命令，重新进行项目的打包(放内存里面去了)

2. html-webpack-plugin
> npm install html-webpack-plugin@5.3.2 -D

```js
const HtmlPlugin = require('html-webpack-plugin')

// 创建HTML插件的实例对象
const htmlPlugin = new HtmlPlugin({
    template: './src/index.html',  // 指定原文件的存放路径
    filname: './index.html'  // 指定生成文件的存放路径
})

module.exports = {
    mode: 'development',
    plugins: [htmlPlugin]  // 通过plugin节点，是htmlPlugin插件生效
    devServer: {
        open: true,  // 初次打包完成后，自动打开浏览器
        host: '127.0.0.1',
        port: 80,
    }
}
```

### 1.5 webpack中的loader

loader加载器的作用：协助webpack打包处理特定的文件模块
* css-loader
* less-loader
* babel-loader 可以打包处理webpack无法处理的高级js语法

1. 打包处理css文件
> npm i style-loader@3.0.0 css-loader@5.2.6 -D
* 在webpack.config.js的 module -> rules数组中，添加loader规则：
```js
module: {  // 所有第三方文件模块的匹配规则
    rules: [  // 文件后缀名的匹配规则
        { 
            test: /\.css$/,  // 文件的类型
            use: ['style-loader', 'css-loader']  // 对应要调用的loader, 写的顺序是固定的，调用顺序由后往前
        }
    ]
}
```

2. 打包处理less文件
> npm i less-loader@10.0.1 less@4.1.1 -D
* 在webpack.config.js的 module -> rules数组中，添加loader规则：
```js
module: {
    rules: [
        {
            test: /.less$/,
            use: ['style-loader', 'css-loader', 'less-loader']
        }
    ]
}
```

3. 打包处理样式表中与url路径相关的文件
> npm i url-loader@4.1.1 file-loader@6.2.0 -D
* 在webpack.config.js的 module -> rules数组中，添加loader规则：
```js
module: {
    rules: [
        {
            test: /\.jpg|png|gif$/,
            use: 'url-loader?limit=22229'  // limit用来指定图片的大小，单位是字节,<=limit大小的图片，才会被转成base64格式的图片
        }
    ]
}
```

4. 打包处理js文件中的高级语法
```js
// 定义了名为info的装饰器
function info(target){
    // 为目标添加静态属性
    target.info = 'Person info'
}

// 为Person类应用info装饰器
@info
class Person{}

// 打包Person的静态属性info
console.log(Person.info)
```
> npm i babel-loader@8.2.2 @babel/core@7.14.6 @babel/plugin-proposal-decorators@7.14.5 -D
* 在webpack.config.js的 module -> rules数组中，添加loader规则：
```js
{
    test: /\./js$/,
    use: 'babel-loader',
    exclude: /node_modules/    
}
```

配置babel-loader:
在项目根目录下，创建名为babel.config.js的配置文件，定义Babel的配置项如下：
```js
module.exports = {
    // 声明babel可用的插件
    plugins: [['@babel/plugin-proposal-decorators', { legacy: true}]]
}
```




### 1.6 打包

1. 配置webpack的打包发布
在package.json文件的scripts节点，新增build命令：
```js
"scripts": {
    "dev": "webpack serve",  // 开发环境中，运行dev命令
    "build": "webpack --mode production"  // 项目发布时，运行build命令
}
``` 

2. 优化图片和js文件的存储路径
* 修改webpack.config.js的url-loader配置项，新增outputPath选项即可指定图片文件的输出路径
```js
module: {
    rules: [
        {
            test: /\.jpg|png|gif$/,
            use: 'url-loader?limit=22228&outputPath=images'  // 明确指定把打包生成的图片文件，存储到dist目录下的 images文件夹中
        },
    ]
}
```

3. 配置和使用clean-webpack-plugin插件自动删除dist目录
> npm i clean clean-webpack-plugin@3.0.0 -D
```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

const webpackConfig = {
    pugins: [ new CleanWebpackPlugin() ],
};

module.exports = webpackConfig;
```

## 2. Source Map

Source Map就是一个信息文件，里面储存着位置信息
> 在发布项目的时候出于安全性考虑建议把devtool的值设置为nosources-source-map或者关闭Source Map

开发环境下，推荐在webpack.config.js中添加配置：
```js
module.exports = {
    mode: 'development',
    devtool: 'eval-source-map',  // 此选项生成的Source Map能够保证"运行时报错的行数"与"源代码的行数"保持一致
}
```

1. webpack中使用@表示位置信息
```js
resolve: {
    alias: {
        // 告诉webpack, 程序员写的代码中，@符号表示src这一层目录
        '@': path.join(__dirname, './src/')
    }
}
```