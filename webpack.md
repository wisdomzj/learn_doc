# webpack学习总结

[toc]



### 起步

#### Node环境支持

下载地址： <https://nodejs.org/en/download>

![](http://demo.bestzhangjun.com/node/1571845560490.png)



#### Webpack安装

##### 全局安装（不推荐）

```shell
$ npm install webpack webpack-cli -g
$ webpack -v
```



##### 本地安装（推荐）

```shell
$ npm install webpack webpack-cli --save-dev
$ npx webpack -v
```



##### 找不到命令？

###### 解决方案1

使用npx webpack npx命令发现本地找不到相关命令文件自动下载直到完成，如果发现本地项目有可执行文件就执行命令

```shell
$ npx webpack -v
```



###### 解决方案2

在packjson文件里面配置scripts项，相当于将命令行语句保存下来，之后只要运行简单的命令类似：npm run build，就可以实现了

```js
# package.json
"scripts": {
    "version": "webpack -v"
}
```

```shell
$ npm run version
```



##### 安装历史版本

```shell
$ npm install webpack@版本号
$ npm webpack -v
$ npx webpack -v
```



#### Webpack是什么

本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle

![](C:\Users\25561\AppData\Roaming\Typora\typora-user-images\1576836870816.png)



### 基础学习

#### 基础概念

 ##### 开发模式（mode）

提供 mode 配置选项，配置 webpack 相应模式的内置优化

```js
// production生产环境  development开发环境
module.exports = {
    mode: 'production'  
}
```



##### 入口文件（entry）

入口文件，类似于其他语言的起始文件，比如：c 语言的 main 函数所在的文件，入口起点指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始，进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的，可以在 webpack 的配置文件中配置入口，配置节点为： `entry`,当然可以配置一个入口，也可以配置多个

```js
module.exports = {
    mode: 'production',
    entry:{
        main:'./src/index.js',
        test:'./src/test.js'
    }
}
```



##### 输出文件（output）

output属性告诉webpack 在哪里输出它所打包后的文件目录，以及如何命名这些文件

```js
const path = require('path');

module.exports = {
    mode: 'production',
    entry:{
        main:'./src/index.js',
        test:'./src/test.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'), 
        filename: '[name]_[hash].js', // 按键命名各自打包输出
        publicPath: "http://www.bestzhangjun.com" // 添加公共头地址
    }
}
```



##### 转换有效模块（loader）

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript），loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理

```js
const path = require('path');

module.exports = {
    mode: 'production',
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.jpg$/,
                use:{
                    loader:'file-loader'
                }
            }
        ]
    },
    output: {
        path: path.resolve(__dirname, 'dist'), 
        filename: 'bundle.js'
    }
}
```



##### 插件（plugins）

loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务，插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量，插件接口功能极其强大，可以用来处理各种各样的任务

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode: 'production',
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.jpg$/,
                use:{
                    loader:'file-loader'
                }
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output: {
        path: path.resolve(__dirname, 'dist'), 
        filename: 'bundle.js'
    }
}
```



#### 图片处理

##### 静态图片

###### 源文件

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
```

```js
# index.js
import avatar from './avatar.jpg'
var img = new Image()
img.src = avatar
var root = document.getElementById('root')
root.append(img)
```



###### 安装file-loader

```shell
$ npm install file-loader -D
```



###### 使用file-loader

不管图片多大，都会以静态资源形式打包处理

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.(jpg|png|gif)$/,
                use:{
                    loader:'file-loader',
                    options:{
                        name:'[name]_[hash].[ext]',
                        outputPath:'img/'
                    }
                }
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### Base64

###### 源文件

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
```

```js
# index.js
import avatar from './avatar.jpg'
var img = new Image()
img.src = avatar
var root = document.getElementById('root')
root.append(img)
```



###### 安装url-loader

```shell
$ npm install url-loader -D
```



###### 使用url-loader

考虑到图片很小，不必要的进行http请求，可以考虑使用url-loader转换base64，如果超出限制字节作用和file-loader一样

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.(jpg|png|gif)$/,
                use:{
                    loader:'url-loader',
                    options:{
                        name:'[name]_[hash].[ext]',
                        outputPath:'img/',
                        limit:2048 //表示字节
                    }
                }
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



#### 样式处理

##### css处理

###### 源文件

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
```

```css
/* index.css */ 
.avatar {
    width: 150px;
    height: 150px;
}
```

```js
# index.js
import './index.css'
import avatar from './avatar.jpg'
var img = new Image()
img.src = avatar
img.classList.add('avatar')
var root = document.getElementById('root')
root.append(img)
```



###### 安装css-loader

```shell
$ npm install css-loder style-loder -D
```



###### 使用css-loader

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.css$/,
                use:['style-loader','css-loader'] // css-loader分析css文件关系合并成一段css style-loader 挂载style样式在html页面中
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### scss/less处理

###### 源文件

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
```

```scss
/* index.scss */ 
body{
    .avatar {
        width: 150px;
        height: 150px;
    } 
}
```

```js
# index.js
import './index.scss'
import avatar from './avatar.jpg'
var img = new Image()
img.src = avatar
img.classList.add('avatar')
var root = document.getElementById('root')
root.append(img)
```



###### 安装sass-loader

```shell
$ npm install css-loder style-loder -D
$ npm install sass-loader node-sass --save-dev
```



###### 使用sass-loader

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.scss$/,
                use:['style-loader','css-loader','sass-loader'] // 从下到上 从右到左
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### 样式前缀处理

###### 源文件

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
```

```css
/* index.css */ 
.avatar {
    width: 150px;
    height: 150px;
    transform: translate(100px, 100px);
}
```

```js
# index.js
import './index.css'
import avatar from './avatar.jpg'
var img = new Image()
img.src = avatar
img.classList.add('avatar')
var root = document.getElementById('root')
root.append(img)
```



###### 安装postcss-loader

```shell
$ npm install postcss-loader --save-dev
```



###### postcss-loader配置

```shell
$ npm install autoprefixer --save-dev
```

```js
module.exports = {
    plugins:[require('autoprefixer')]
}
```



###### 使用postcss-loader

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.css$/,
                use:['style-loader','css-loader','postcss-loader']
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### 字体文件

###### 源文件

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
```

```css
/* index.css */
@font-face {
    font-family: 'iconfont';
    src: url("./font/iconfont.eot");
    src: url("./font/iconfont.eot?#iefix") format("embedded-opentype"), url("./font/iconfont.woff2") 	 format("woff2"), url("./font/iconfont.woff") format("woff"), url("./font/iconfont.ttf") 		     format("truetype"), url("./font/iconfont.svg#iconfont") format("svg");
}		

.iconfont {
    font-family: "iconfont" !important;
    font-size: 16px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
```

```js
# index.js
import './index.css'
var root = document.getElementById('root')
root.innerHTML = "<div class='iconfont'>&#xe623;</div>"
```



###### 安装file-loder

```shell
$ npm install file-loader -D
```



###### 字体文件处理

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.css$/,
                use:['style-loader','css-loader'] 
            },
            {
                test:/\.(eot|ttf|svg|woff2|woff)$/,
                use:{
                    loader:'file-loader',
                    options:{
                        name:'[name]_[hash].[ext]',
                        outputPath:'font/'
                    }
                }              
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### css-loader配置

###### 源文件

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
</html>
```

```css
/* index.scss */ 
@import './test.scss';
body{
    .avatar {
        width: 150px;
        height: 150px;
        transform: translate(100px, 100px);
    } 
}
```

```js
# index.js
import avatar from './avatar.jpg'
import style from './index.scss'
import createAvatar from './createAvatar'

createAvatar()

var img = new Image()
img.src = avatar
img.classList.add(style.avatar)
var root = document.getElementById('root')
root.append(img)
```

```js
# createAvatar.js
import avatar from './avatar.jpg';
import style from './index.scss'

function createAvatar(){
    var img = new Image();
    img.src = avatar;
    img.classList.add(style.avatar)
    var root = document.getElementById('root');
    root.append(img);
}

export default createAvatar;
```



###### 相关配置

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            {
                test:/\.css$/,
                use:[
                    'style-loader',
                    {
                        loader:'css-loader',
                        options:{
                            importLoaders:2, // 解决scss/less内部导入不解析问题
                            modules:true // 开启模块化
                        }
                    },
                    'postcss-loader'
                ]
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



 #### 处理ES6语法

##### 编写业务代码配置方案

###### 安装loader

```shell
$ npm install babel-loader @babel/core --save-dev
$ npm  install @babel/preset-env --save-dev
$ npm install @babel/polyfill --save
```



###### preset-env转换部分语法

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader",
                options:{
                    presets: ["@babel/preset-env"]
                } 
            },
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



###### polyfill 补充缺少语法

```js
import "@babel/polyfill";

const arr = [
    new Promise(() => {}),
	new Promise(() => {})
]

arr.map(item => {
	console.log(item);
})

other es6 code ...
```



###### 按需引进语法配置

```js
const arr = [
    new Promise(() => {}),
	new Promise(() => {})
]

arr.map(item => {
	console.log(item);
})

other es6 code ...
```

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader",
                options:{
                    presets: [["@babel/preset-env",{
                        useBuiltIns:"usage"
                    }]]
                } 
            },
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



###### polyfill舍弃后的配置

```shell
npm install --save core-js@3
```

 ```js
import "core-js/stable"
import "regenerator-runtime/runtime"

const arr = [
    new Promise(() => {}),
	new Promise(() => {})
]

arr.map(item => {
	console.log(item);
})

other es6 code ...
 ```

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader",
                options:{
                    presets: [["@babel/preset-env",{
                        //useBuiltIns:"usage",
                        corejs: 3
                    }]]
                } 
            },
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



###### presets其他配置

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader",
                options:{
                    presets: [["@babel/preset-env",{
                        useBuiltIns:"usage",
                        targets: {
                            edge: "17",
                            firefox: "60",
                            chrome: "67", //大于谷歌67版本以上支持es就忽略es6转es5
                            safari: "11.1"
                        }
                    }]]
                } 
            },
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### 编写组件库/第三方模块配置方案

###### 安装插件

```shell
$ npm install @babel/plugin-transform-runtime --save-dev
$ npm install @babel/runtime --save
$ npm install --save @babel/runtime-corejs2
```



###### 插件配置

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader",
                options:{
                    plugins: [
                        [
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime": false,
                                "corejs": 2,
                                "helpers": true,
                                "regenerator": true,
                                "useESModules": false,
                                "version": "7.0.0-beta.0"
                            }
                        ]
                    ]
                } 
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### .babelrc配置文件

###### 创建.babelrc文件

```js
{          
    plugins: [
        [
            "@babel/plugin-transform-runtime",
            {
                "absoluteRuntime": false,
                "corejs": 2,
                "helpers": true,
                "regenerator": true,
                "useESModules": false,
                "version": "7.0.0-beta.0"
            }
        ]
    ]
} 

=====================================================

{          
    presets: [
        [
            "@babel/preset-env",
            {
                useBuiltIns:"usage",
                targets: {
                    edge: "17",
                    firefox: "60",
                    chrome: "67", //大于谷歌67版本以上支持es就忽略es6转es5
                    safari: "11.1"
                }
    		}
        ]
    ]
}    
```



###### webpack配置

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader"
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



### 进阶学习

#### DevServer（自动化工具）

##### WebpackDevServer 

###### 定义命令

```js
# package.json
"scripts": {
    "start": "webpack-dev-server"
}
```



###### 安装工具

```shell
$ npm install webpack-dev-server --save-dev 
```



###### 配置DevServer

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    devServer:{
        contentBase:'./dist', //根目录
        open:true, // 设置浏览器自动打开
        port:3000 // 设置端口号
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader",
                options:{
                    presets: [["@babel/preset-env",{
                        useBuiltIns:"usage",
                        targets: {
                            edge: "17",
                            firefox: "60",
                            chrome: "67", 
                            safari: "11.1"
                        }
                    }]]
                } 
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### 自定义DevServer

###### 安装webpackdev中间件

```shell
$ npm install webpack-dev-middleware --save-dev
```



###### 启动服务执行webpack

```js
const express = require('express')
const webpack = require('webpack')
const webpackDevMiddleware = require('webpack-dev-middleware')
const config = require('./webpack.config')
const complier = webpack(config)

const app = express()

app.use(webpackDevMiddleware(complier, {
    publicPath:config.output.publicPath
}))

app.listen(3000, () => {
	console.log('server is running')
})
```



#### HotModuleReplacement（热模块替换）

##### 开启热更新

```js
devServer:{
    contentBase:'./dist',
    open:true,
    port:3000,
    hot:true, // 开启热更新
    hotOnly:true // 即使不生效,也不刷新页面
}
```



##### 集成自带的插件

```js
const webpack = require('webpack')

plugins:[
    new webpack.HotModuleReplacementPlugin()
]
```



##### 调试css只更新css模块

```css
/* test.css */
div:nth-of-type(odd) {
	background: palegreen;
}
```

```js
# index.js
import from './test.css'

var btn = document.createElement('button')
btn.innerHTML = '新增'
document.body.appendChild(btn)
btn.onclick = function() {
	var div = document.createElement('div')
	div.innerHTML = 'item'
	document.body.appendChild(div)
}
```



##### 调试js模块互不影响

```js
# counter.js
function counter() {
	var div = document.createElement('div');
	div.setAttribute('id', 'counter');
	div.innerHTML = 1;
	div.onclick = function() {
		div.innerHTML = parseInt(div.innerHTML) + 1
	}
	document.body.appendChild(div);
}

export default counter
```

```js
# number.js
function number() {
	var div = document.createElement('div');
	div.setAttribute('id', 'number');
	div.innerHTML = 888000;
	document.body.appendChild(div);
}

export default number
```

```js
# index.js
import counter from './counter'
import number from './number'

counter()
number()

if(module.hot){
	module.hot.accept("./b",()=>{
		document.body.removeChild(document.getElementById('number'))
		number()
	})
}
```



#### SourceMap（映射源文件定位错误）

##### devtool参数解析

| devtool                        | build   | rebuild | production | quality                       |
| :----------------------------- | :------ | :------ | :--------- | :---------------------------- |
| (none)                         | fastest | fastest | yes        | bundled code                  |
| eval                           | fastest | fastest | no         | generated code                |
| cheap-eval-source-map          | fast    | faster  | no         | transformed code (lines only) |
| cheap-module-eval-source-map   | slow    | faster  | no         | original source (lines only)  |
| eval-source-map                | slowest | fast    | no         | original source               |
| cheap-source-map               | fast    | slow    | yes        | transformed code (lines only) |
| cheap-module-source-map        | slow    | slower  | yes        | original source (lines only)  |
| inline-cheap-source-map        | fast    | slow    | no         | transformed code (lines only) |
| inline-cheap-module-source-map | slow    | slower  | no         | original source (lines only)  |
| source-map                     | slowest | slowest | yes        | original source               |
| inline-source-map              | slowest | slowest | no         | original source               |
| hidden-source-map              | slowest | slowest | yes        | original source               |
| nosources-source-map           | slowest | slowest | yes        | without source content        |



##### 开发环境默认开启映射

```js
devtool:'none'
```



##### 固定参数配置项

```js
# development
devtool:'cheap-module-eval-source-map'

# production
devtool:'cheap-module-source-map'
```



##### Map映射原理





#### Tree Sharkin（按需引进模块内容）

##### 源文件

````js
# math.js
export const add = (a,b) => {
   console.log(a + b);
}


export const minus = (a,b) => {
    console.log(a - b); 
}
````

```js
# index.js
import { add } from './math'

add(1,2)
```



##### package.json文件配置

```js
{
  "name": "webpacktestpro",
  "sideEffects": false, //如果其他模块不使用treesharkin功能自行添加,则可忽略该文件使用treesharkin功能   
  // "sideEffects": ['@babel/polly-fill','*.css'],
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```



##### 开启tree sharkin

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    mode:"development",
    entry:{
        main:'./src/index.js'
    },
    devServer:{
        contentBase:'./dist', 
        open:true, 
        port:3000
    },
    module:{
        rules:[
            { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: "babel-loader",
                options:{
                    presets: [["@babel/preset-env",{
                        useBuiltIns:"usage",
                        targets: {
                            edge: "17",
                            firefox: "60",
                            chrome: "67", 
                            safari: "11.1"
                        }
                    }]]
                } 
            }
        ]
    },
    plugins:[ 
        new HtmlWebpackPlugin({
            template:"./src/index.html", 
            filename:"index.html"
        })
    ],
    optimization:{
        usedExports:true //开发环境还保留代码方便调试,线上环境中则会真正的按需打包内容
    },
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



#### Code Splitting（代码分割）



#### Lazy Loading（懒加载）





