---
title: webpack4笔记
cover: http://sora3.coding.me/imgs/preview/preview4.jpg
---

## Loaders
test指定匹配规则，use指定使用的loader名称

``` bash
const path = require('path');
module.exports = {
    output: {
        filename: 'bundle.js'
    },
    module: {
        rules: [
            { test: /\.txt$/, use: 'raw-loader' }
        ]
    }
};
```

## 解析CSS
css-loader 用于加载 .css ⽂文件，并且转换成 commonjs 对象
style-loader 将样式通过style标签插入到 head 中

CSS3属性加前缀
IE：Trident(-ms) 火狐：Geko(-moz) 谷歌：Webkit(-webkit) 欧朋：Presto(-o)
```bash
.box {
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    -o-border-radius: 10px;
    border-radius: 10px;
}
```
使用PostCSS 插件 autoprefixer 自动补齐 CSS3 前缀
```bash
module.exports = {
module: {
        rules: [
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader',
                    + {
                        + loader: 'postcss-loader',
                        + options: {
                            + plugins: () => [
                                + require('autoprefixer')({
                                + browsers: ["last 2 version", "> 1%", "iOS 7"]
                                + })
                            + ]
                        + }
                    + }
                ]
            }
        ]
    }
}
```
移动端CSS px自动转换rem
使用px2rem-loader和lib-flexible
入口文件main.js要引入lib-flexible

npm i lib-flexible -S
npm i px2rem-loader -D
```bash
module.exports = {
    module: {
        rules: [
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader',
                    + {
                        + loader: "px2rem-loader",
                        + options: {
                            + remUnit: 75,
                            + remPrecision: 8
                        + }
                    + }
                ]
            }
        ]
    }
};
```
## 解析图片、字体
file-loader
url-loader:可以设置较小资源⾃自动 base64
```bash
module: {
    rules: [
    + {
        + test: /\.(png|svg|jpg|gif)$/,
        + use: [{
            + loader: 'url-loader’,
            + options: {
            +     limit: 10240
            + }
        + }]
    + }
    ]
}
```
## Plugins
插件用于 bundle ⽂文件的优化，资源管理和环境变量注⼊
作用于整个构建过程
```bash
const path = require('path');
module.exports = {
    output: {
        filename: 'bundle.js'
    },
    plugins: [
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        })
    ]
};
```
## 代码压缩
js文件的压缩
内置了uglifyjs-webpack-plugin
CSS文件的压缩
optimize-css-assets-webpack-plugin
同时使用cssnano
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
    + new OptimizeCSSAssetsPlugin({
    +     assetNameRegExp: /\.css$/g,
    +     cssProcessor: require('cssnano’)
    + })
    ]
};
```
html文件的压缩
修改html-webpack-plugin，设置压缩参数
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
    + new HtmlWebpackPlugin({
    +     template: path.join(__dirname, 'src/search.html’),
    +     filename: 'search.html’,
    +     chunks: ['search’],
    +     inject: true,
    +     minify: {
    +         html5: true,
    +         collapseWhitespace: true,
    +         preserveLineBreaks: false,
    +         minifyCSS: true,
    +         minifyJS: true,
    +         removeComments: false
    +     }
    + })
    ]
};
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
## 提取页面公共资源
使用html-webpack-externals-plugin
将react、react-dom等基础包通过cdn引入，不打入bundle中
使用html-webpack-externals-plugin
```bash
const HtmlWebpackExternalsPlugin = require('html-webpack-externals-plugin')

plugins: [
    new HtmlWebpackExternalsPlugin({
        externals:[
            {
                module:'react',
                entry:'//链接',
                global:'React'
            }, {
                module:'react-dom',
                entry:'//链接',
                global:'ReactDom'
            }
        ]
    })
]
```
使用SplitChunksPlugin
Webpack4 内置的，替代CommonsChunkPlugin插件
公共脚本分离
```bash
module.exports = {
    optimization: {
        splitChunks: {
        chunks: 'async',
        minSize: 30000,
        maxSize: 0,
        minChunks: 1,
        maxAsyncRequests: 5,
        maxInitialRequests: 3,
        automaticNameDelimiter: '~',
        name: true,
        cacheGroups: {
                vendors: {
                    test: /[\\/]node_modules[\\/]/,
                    priority: -10
                }
            }
        }
    }
};
```
chunks参数说明：

async 异步引入的库进行分离（默认）
initial 同步引入的库进行分离
all 所有引入的库进行分离（推荐）


分离基础包
test：匹配出需要分离的的包
```bash
module.exports = {
    optimization: {
        splitChunks: {
            cacheGroups: {
                commons: {
                    test: /(react|react-dom)/,
                    name: 'vendors',
                    chunks: 'all'
                }
            }
        }
    }
};
```
分离页面公共文件
```bash
module.exports = {
    optimization: {
        splitChunks: {
            minSize: 0,
            cacheGroups: {
                commons: {
                        name: 'commons',
                        chunks: 'all',
                        minChunks: 2
                    }
                }
            }
        }
    }
};
```
minChunks:设置最小引用次数为两次

minuSize：分离的包体积的大小

scope hoisting
原理：将所有模块的代码按照引用顺序放在一个函数作用域里，然后适当的重命名一些变量以防止变量名冲突

对比: 通过 scope hoisting 可以减少函数声明代码和内存开销

使用：webpack mode 为 production 默认开启，必须是 ES6 语法，CJS 不支持 
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
    +     new webpack.optimize.ModuleConcatenationPlugin()
    ]
};
```
## 代码分割
安装babel插件
npm install @babel/plugin-syntax-dynamic-import --save-dev
配置babel的配置文件：.babelrc
```bash
{
    "plugins": ["@babel/plugin-syntax-dynamic-import"],
    ...
}
```
## 构建速度和体积优化
使用内置的stats分析。stats:构建的统计信息
```bash
"scripts":{
    "build:stats":"webpack --env production --json > stats.json"
}
```
速度分析：speed-measure-webpack-plugin
```bash
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();
const webpackConfig = smp.wrap({
    plugins:[
        new MyPlugin(),
        new MyOtherPlugin()
    ]
});
```
作用：分析整个打包总耗时，每个插件和loader的耗时情况

体积分析:webpack-bundle-analyzer
```bash
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
    plugins: [
        new BundleAnalyzerPlugin()
    ]
}
```