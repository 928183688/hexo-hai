---
title: webpack4笔记
cover: http://sora3.coding.me/imgs/preview/preview4.jpg
---
## 安装webpack
- 通过npm install webpack webpack-cli -g安装全局包
- 输入命令：webpack ./src/app.js,会将指定的app.js打包转换为./dist/main.js

添加一个webpack.config.js文件，注意文件名称绝对不能修改
- entry:设置你的入口文件，说白了就是你想打包转换的文件
- output:设置目标文件的全路径
```js
const path = require('path')
module.exports = {
    // 设置文件的入口：一开始就解析这个文件
    entry:'./src/app.js',
    // 将入口文件解析为目标文件的配置
    output:{
        // 目标文件的输出路径文件夹名称
        path:path.join(__dirname,"dist",),
        // 目标文件的文件名称
        filename:'main.js'
    }
}
```

## 开启webpack
- 下载包：npm i webpack-dev-server -g
- 在webpack.config.js中进行服务器开发的配置
```js
module.exports = {
    // 设置文件的入口：一开始就解析这个文件
    entry:'./src/app.js',
    // 将入口文件解析为目标文件的配置
    output:{
        // 目标文件的输出路径文件夹名称
        path:path.join(__dirname,"dist"),

        // publicPath: '/dist',
        // 目标文件的文件名称
        filename:'main.js'
    },
    // 建议使用这个配置，新版本建议这样配置,默认会生成main.js
    devServer:{
        publicPath: '/dist'
    }
}
```
运行 webpack-dev-server --open


## 解析css
- npm install css-loader style-loader --save-dev
- css-loader：css解析器，将css解析为浏览器可以识别的类型
- style-loader：自动的在指定文件中添加style标签，同时添加指定的样式代码
在webpack.config.js文件中添加配置
```js
// 下面这个成员就是不同类型的文件的解析加载规则
module: {
    rules: [
        // 配置的是用来解析.css文件的loader(style-loader和css-loader)
        {
            // 1.0 用正则匹配当前访问的文件的后缀名是  .css
            test: /\.css$/,
            use: ['style-loader', 'css-loader'] //webpack底层调用这些包的顺序是从右到左
        }
    ]
}
```

## 解析less和scss
- npm install less less-loader sass-loader node-sass --save-dev
- less less-loader：处理less文件
- sass-loader node-sass：处理scss文件
```js
// 配置less解析
{
       test: /\.less$/,
        use: [{
            loader: 'style-loader'
        }, {
            loader: 'css-loader'
        }, {
            loader: 'less-loader'
        }]
},
// 配置scss解析
{
        test: /\.scss$/,
         use: [{
                loader: 'style-loader'
            }, {
                loader: 'css-loader'
            }, {
                loader: 'sass-loader'
         }]
}
```
## 解析图标+图片
- npm install file-loader url-loader --save-dev
```js
{
    test: /\.(png|jpg|gif|eot|svg|ttf|woff)/,
    use: [{
        loader: 'url-loader',
        options: {
          // limit表示如果图片大于50000byte，就以路径形式展示，小于的话就用base64格式展示
            limit: 50000
        }
    }]
}
```

## 解析ES6 转换为ES5
一些老版的浏览器可能不支持ES6，这个babel的作用就是能够将ES6转换ES5，达到兼容的目的
- npm install babel-loader @babel/core @babel/preset-env --save-dev
```js
{
      test: /\.js$/,
      // Webpack2建议尽量避免exclude，更倾向于使用include
      // exclude: /(node_modules)/, // node_modules下面的.js文件会被排除
      include: [path.resolve(__dirname, 'src')],
      use: {
        loader: 'babel-loader',
        // options里面的东西可以放到.babelrc文件中去
        options: {
            presets: ['@babel/preset-env']
          }
      }
}
```
## 解析Vue 
- npm vue-loader
```js
const VueLoaderPlugin = require('vue-loader/lib/plugin');
{
    test: /\.vue$/,
    loader: 'vue-loader'
},
plugins: [
    new VueLoaderPlugin()
]
```
## 打包压缩
- npm html-webpack-plugin
- npm uglifyjs-webpack-plugin
```js
const htmlWebpackPlugin = require('html-webpack-plugin');
   plugins: [
        new htmlWebpackPlugin({
            template: //指定要打包的html路径和文件名,
            filename: //指定输出路径和文件名,
        })
        new uglifyjs(), //压缩js
    ]
```

## 自动清理构建目录
避免构建前每次都要手动删除dist，使用clean-webpack-plugin，默认会删除output指定的输出目录
```bash
module.exports = {
    entry: {
        app: './src/app.js',
        search: './src/search.js'
    },
    output: {
        filename: '[name][chunkhash:8].js',
        path: __dirname + '/dist'
    },
    plugins: [
    +     new CleanWebpackPlugin()
    ]
};
```
## 构建速度和体积优化
使用内置的stats分析。stats:构建的统计信息
```js
"scripts":{
    "build:stats":"webpack --env production --json > stats.json"
}
```