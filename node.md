# node学习总结（仅供参考）

[TOC]



### 起步

#### 安装Node.js环境

下载地址： <https://nodejs.org/en/download>

![1571845560490](http://demo.bestzhangjun.com/imgs/node/1571845560490.png)



#### 查看Node版本号

```shell
$ node -v 
$ node --version 
```

![1571846083144](http://demo.bestzhangjun.com/imgs/node/1571846083144.png)



#### NVM多版本管理

##### 简介

nvm就是一个可以让你在同一台机器上安装和切换不同版本node的工具



 ##### 怎么下载

github上下载最新版本 <https://github.com/coreybutler/nvm-windows/releases>

+ nvm-noinstall.zip： 这个是绿色免安装版本，但是使用之前需要配置
+ nvm-setup.zip：这是一个安装包，下载之后点击安装，无需配置就可以使用，方便
+ Source code(zip)：zip压缩的源码
+ Sourc code(tar.gz)：tar.gz的源码，一般用于*nix系统



##### 安装步骤

按照提示完成安装即可，安装完成后可以检测一下是否安装成功在命令行输入`nvm`，如果出现nvm版本号和一系列帮助指令，则说明nvm安装成功；否则，可能会提示nvm: command not found

![1571846245289](http://demo.bestzhangjun.com/imgs/node/1571846245289.png)



##### 修改配置文件

在你安装的目录下找到settings.txt文件，打开后加上 
node_mirror: <https://npm.taobao.org/mirrors/node/> 
npm_mirror: <https://npm.taobao.org/mirrors/npm/>



##### 基础使用

+ nvm list 查看目前已经安装的版本
+ nvm list available 显示可下载版本的部分列表
+ nvm install 版本号 安装指定的版本的nodejs
+ nvm uninstall 版本号 卸载指定的版本的nodejs
+ nvm use 版本号 使用指定版本的nodejs



#### 适应REPL环境（控制台）

+ read 读取（读取用户输入的js代码）
+ exec 执行（执行用户输入的js代码）
+ print 打印（打印用户输入的js代码）
+ loop 循环 （后续所有代码执行训话以上命令）
+ 退出 crtl+c （强制退出程序）

![1571846628191](http://demo.bestzhangjun.com/imgs/node/1571846628191.png)



#### 体验node（hello word）

+ 创建编写好的js脚本文件
+ 打开中终端，定位文件所述目录
+ 输入命令 `node filename` 执行命令
+ 查看执行结果

![1571847469143](http://demo.bestzhangjun.com/imgs/node/1571847469143.png)



### 模块系统

#### 自定义模块

##### 模块通信交互规则

+ 一个 JavaScript 文件就是一个单独的模块
+ 模块天生就是一个私有的作用域，默认模块内定义的变量等成员只能被模块内部访问
+ 每一个模块中都有一个  `module.export` 编程接口对象，默认是一个空对象 
+ 可以通过给 `module.exports` 编程接口对象添加成员向外暴露内部成员



 ##### 加载和导出的认识

###### 加载require使用规则

1. 语法

   ```js
   var variablename = require("Module name")  
   ```

   

2. 作用

   + 执行被加载模块中的代码
   + 得到被加载模块中`module.exports` 导出的接口对象



###### 导出exports使用规则

1. 导出多个成员

   ```js
   exports.a = 123;
   exports.b = "hello";
   exports.c = function(){
       console.log("this is foo method");
   }
   exports.d = {
       foo:"bar"
   }
   ```

   

2. 导出单个成员

   ```js
   //导出单个属性
   module.exports = "hello";
   
   //导出单个方法
   module.exports = function(){
       console.log("this is a method");
}
   ```
   
   

###### exports和module.exports的区别

+ exports是module.exports的一个引用，两者指向同一个对象地址 可以理解为module.exports简写
+ 改变exports的指向，添加exports.xxx 都是无效，因为require返回是module.exports
+ 改变module.exports的指向，exports添加的属性和方法不存在于module.exports指向的新对象中
+ 如果只用来添加属性和方法，exports与module.exports完全可以混用



##### 使用自定义模块

###### 自定义模块文件

```js
// 定义一个 tools.js 的模块
var tools = {
    sayHello: function () {
        console.log('hello NodeJS');
    },
    add: function (x, y) {
        return x + y;
    }
}

// 模块接口的暴露
module.exports = tools;

// exports.sayHello = tools.sayHello;
// exports.add = tools.add;
```



###### 加载自定义模块

```js
const tools = require("./tools")

tools.sayHello()

let result = tools.add(2,3)
console.log(result)
```



#### 内置核心模块

##### os模块（操作系统信息）

###### 引入os模块 

```js
const os = require('os')
```



###### 常用的api使用

```js
const os = require('os')
const G = 1024 * 1024 * 1024;
const system = {
  uptime: os.uptime(),
  platform: os.platform(),
  hostname: os.hostname(),
  release: os.release(),
  type: os.type(),
  arch: os.arch(),
  eol: os.EOL, // 换行符
  endianness: os.endianness(), // 字节次序
  loadavg: os.loadavg(), // 平均负载
  network: os.networkInterfaces() // 网络
}
const memory = {
  freemem: os.freemem(),
  totalmem: os.totalmem()
}
const dir = {
  homedir: os.homedir(),
  tmpdir: os.tmpdir()
}
const cpus = os.cpus();
const userInfo = os.userInfo();
const constants = os.constants;

console.log('系统版本：%s %s %s', system.type, system.release, system.arch);
console.log('主机名称：%s', system.hostname);
console.log('开机时长：%sh', (system.uptime/3600).toFixed(1));
console.log('总内存：%s', `${(memory.totalmem/G).toFixed(2)}G`);
console.log('可用内存：%s', `${(memory.freemem/G).toFixed(2)}G`);
console.log('HOME目录：%s', dir.homedir);
console.log('TEMP目录：%s', dir.tmpdir);
console.log('CPU：%s %s核处理器', cpus[0].model, cpus.length);
console.log(`用户名：${userInfo.username}`);
```



##### fs模块（文件处理）

###### 引入fs模块

```js
const http = require('fs')
```



###### 常見的api使用

```js
// 检测是文件还是目录 err:错误信息  stats：检测信息
fs.stat("test",(err,stats)=>{
    if(err){
        console.log(err);
    }else{
        console.log(stats);
        console.log(`文件:${stats.isFile()}`);
        console.log(`文件夹:${stats.isDirectory()}`);
    }
})

// 创建目录  err：错误信息
fs.mkdir("Files",(err)=>{
    if(err){
        console.log(err);
    }else{
        console.log("创建文件成功");
    }
})

// 删除目录
fs.rmdir("test",(err)=>{
    if(err){
        console.log(err);
    }else{
        console.log("删除目录成功");
    }
})

// 读取目录 err：错误信息 files：该目录下的文件或目录 
fs.readdir("public",(err,files)=>{
    if(err){
        console.log(err);
    }else{
        console.log(files);
    }
})

// 写文件 err:	错误信息 没有文件就新创建文件
fs.writeFile("zj.txt","my name is zhangjun!",(err)=>{
    if(err){
        console.log(err);
    }else{
        console.log("文件写入成功")
    }
})

// 追加文件 err：错误信息 没有文件就新创建文件
fs.appendFile("lxr.txt","my name is lixueru!",(err)=>{
    if(err){
        console.log(err);
    }else{
        console.log("追加成功！！！");
    }
})

// 读文件 err: 错误信息 data：文件内容 读取的十六进制数据 需要使用toString()方法
fs.readFile("zj.txt",(err,data)=>{
    if(err){
        console.log(err);
    }else{
        console.log(data.toString());
    }
})

// 删除文件 err: 错误信息
fs.unlink("lxr.txt",(err)=>{
    if(err){
        console.log(err);
    }else{
        console.log("删除成功");
    }
})

// 重命名 err: 错误信息  目录或文件的重命名  剪切文件
fs.rename("test/test.css","upload/index.css",(err)=>{
    if (err) {
        console.log(err);
    } else {
        console.log('重命名成功');
    }  
})
```



###### fs流的认识

```js
// 从文件流中读取数据
let ReadStream = fs.createReadStream("tt.txt")
let WriteStream = fs.createWriteStream("ts.txt")
let str = "";
let count = 0;
let data = "我是要被写入的数据!!";

ReadStream.on("data",(chunk)=>{
    count++; 
    str += chunk; 
})

ReadStream.on("end",()=>{
    console.log("读取次数："+count);
    console.log("文件内容："+str);
})

ReadStream.on("error",(err)=>{
    console.log(err);
})


// 写入文流到文件中
WriteStream.write(data,"utf-8");
        
WriteStream.end();

WriteStream.on("finish",()=>{
    console.log("写入完成！！！");
})

WriteStream.on("error",(err)=>{
    console.log(err.stack);
})

// 管道流（拷贝文件）
// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');
// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);
```



###### fs使用场景

```js
// 判断是否存在upload文件夹 如果没有则创建upload
fs.stat("upload",(err,stats)=>{
    if(err){
        fs.mkdir("upload",(error)=>{
            if(error) return;
        })
    }else{
        console.log('目录已经存在');
    }
})

// 递归遍历文件
let ergfile = (dir)=>{
    let arr = [];
    if(fs.existsSync(dir)){
        fs.readdirSync(dir).forEach((filename)=>{
            let path = dir + "/" + filename;
            if(fs.statSync(path).isDirectory()){
               arr = arr.concat(ergfile(path));
            }else{
                arr.push(filename);
            }
        })
    }else{
        console.log("目录不存在,请传入正确的目录");
    }
    return arr;
}

let route_erg = __dirname+"/test";
ergfile(route_erg).forEach((x)=>{
	console.log("文件：" + x);
})

// 递归删除文件
let delfile = (dir)=>{
    if(fs.existsSync(dir)){
        fs.readdirSync(dir).forEach((filename)=>{
            let path = dir + "/" + filename;
            if(fs.statSync(path).isDirectory){
                delfile(path);     
            }else{
                fs.unlinkSync(filename);
            }
        })
        fs.rmdirSync(dir);
    }
}

let route_del = __dirname+"/test";
delfile(route_del);
console.log("已经删除test文件夹");
```



 ##### path模块（路径处理）

###### 引入path模块

```js
const path = require('path')
```



###### 常用的api使用

```js
let p = "C:/Users/zj/Desktop/test/test.html";

//dirname(path)：返回尾缀
path.dirname(p) // C:/Users/zj/Desktop/test

//basename(path)：返回目录名
path.basename(p) // test.html

//extname(path)：返回拓展名
path.extname(p) // html

//isAbsolute(path) 判断是不是绝对路径
path.isAbsolute(p) // true

//join([...paths]) 用平台特定的分隔符把全部给定的path片段连接到一起，并规范化
path.join('/foo', 'bar', 'baz/abcd')  // \foo\bar\baz\abcd

//resolve([...paths]) 将路径或路径片段的序列解析为一个绝对路径
path.resolve(__dirname,"foo/bar")  // C:\Users\zj\Desktop\test\foo\bar
```



##### url模块（网址处理）

###### 引入url模块

```js
const http = require('url');
```



###### parse 的用法

```js
const http = require("http")
const url = require("url")

const server = http.createServer()

server.on("request",(req,res)=>{
    let urls = req.url;
    if(urls != "/favicon.ico"){
        /*
        	第一个参数是地址
       		第二个参数是 true 的话表示把 get 传值转换成对象 
        */
        let result = url.parse("http://www.bestzhangjun.com?name=zj&pwd=123",true); 
        console.log(result);
        console.log(result.query.name);
        console.log(result.query.pwd);
    }
    res.writeHeader(200,"success",{"content-Type":"text/html;charset=utf-8"});
    res.end("url 模块下 parse 函数");
})

server.listen(3000,()=>{
    console.log("this is running...");
})
```



###### format逆向 parse

```js
const http = require("http")
const url = require("url")

const server = http.createServer()

server.on("request",(req,res)=>{
    let urls = req.url;
    if(urls != "/favicon.ico"){
        console.log(url.format({
            protocol: 'http:', //协议
            hostname:'www.bestzhangjun.com', //域名
            port:'80', //端口
            pathname :'/news', //路由
            query:{page:1} //get参数
        }));
    }
    res.writeHeader(200,"success",{"content-Type":"text/html;charset=utf-8"});
    res.end("url 模块下 format 函数");
})

server.listen(3000,()=>{
    console.log("this is running...");
})
```



###### resolve的认识

 ```js
const http = require("http")
const url = require("url")

const server = http.createServer()

server.on("request",(req,res)=>{
    let urls = req.url;
    if(urls != "/favicon.ico"){
        // 追加或替换
        console.log(url.resolve("127.0.0.1:3000/?name=zj&pwd=123", "/?userName=wisdomzj&pwd=123"));
        console.log(url.resolve("127.0.0.1:3000", "/?userName=lxr&pwd=123"));
    }
    res.writeHeader(200,"success",{"content-Type":"text/html;charset=utf-8"});
    res.end("url 模块下 resolve 函数");
})

server.listen(3000,()=>{
    console.log("this is running...");
})
 ```



##### http模块（构建http服务器）

###### 引用http模块

```js
const http = require('http')
```



###### 简易http服务功能

```js
//1.加载http核心模块
const http = require("http")

//2.创建一个web服务器
var server = http.createServer()

//3.接受处理请求并响应
server.on("request",(req,res)=>{
    console.log("收到请求路径为"+req.url);
})

//4.启动服务
server.listen(3000,()=>{
    console.log("server is running...")
})
```



###### 尝试响应一个html页面

```js
const http = require("http")
const fs = require("fs")

const server = http.createServer()

server.on("request",(req,res)=>{
    let url = req.url;
    if(url === "/"){
        fs.readFile("./test.html",(err,data)=>{
            if(err) throw err;
            res.writeHead(200,{"Content-Type":"text/html;charset=utf-8"});
            res.end(data);
        })
    }
})

server.listen(3000,()=>{
    console.log("this server is running...")
})
```



###### 模板引擎替换html页面（art-template）

```js
const http = require("http")
const fs = require("fs")
var template = require('art-template')

const server = http.createServer()

server.on("request",(req,res)=>{
    let url = req.url;
    if(url === "/"){
        fs.readFile(__dirname+"/view/test.html",(err,data)=>{
            if(err) throw err;
            res.writeHead(200,{"Content-Type":"text/html;charset=utf-8"});
            var htmlStr = template.render(data.toString(),{
                name:"zj",
                age:22,
                sex:"男",
                hobby:"台球"
            })
            res.end(htmlStr);
        })
    }
})

server.listen(3000,()=>{
    console.log("this server is running...")
})
```



###### 请求不同种类的资源

```js
const http = require("http")
const fs = require("fs")
const path = require("path")
const url = require("url")
const mimeModel = require("./mime")

let server = http.createServer()

server.on("request",(req,res)=>{
    //获取网址(去除路由参数); 
    let pathname = url.parse(req.url).pathname;
    
    //获取文件后缀名
    let extname = path.extname(pathname);
    
    //根路由 默认显示首页
    if(pathname=="/"){
        pathname = "/index.html";
    }
    
    //其他路由 显示静态资源
    if(pathname!="/favicon.ico"){
        fs.readFile("static/"+pathname,(error,data)=>{
            if(error){
                fs.readFile("./static/404.html",(err,data)=>{
                    if(err) return;
                    res.writeHead(400,"faile",{"content-Type":"text/html;charst='utf-8'"});
                    res.end(data);
                })
            }else{
                let type = mimeModel.getMimes(extname); 
                res.writeHead(200,"success",{"content-Type":""+type+";charst='utf-8'"});
                res.end(data);
            }
        })
    }
})

server.listen(3000,()=>{
    console.log("this is port 3000");
})
```



###### 请求对象&响应对象

```js
const http = require("http")

const server = http.createServer()

server.on("request",(req,res)=>{
    console.log("请求头信息对象形式:"+req.headers);
    console.log("请求头信息数组格式:"+req.rawHeaders);
    console.log("http版本:"+req.httpVersion);
    console.log("请求方法:"+req.method);
    console.log("请求路径:"+req.url);
    
    res.statusCode = 200; //响应状态码
    res.statusMessage = "成功"; //状态信息
    res.setHeader("Content-Type","text/html,charset=utf-8") //设置响应头
    res.writeHead(200,"成功",{"Content-Type":"text/html,charset=utf-8"}) //三者的综合写法
})

server.listen(3000,()=>{
    console.log("this server is running....")
})

```



##### events模块（事件驱动）

###### 非阻塞io事件驱动

​		在 Java、PHP 或者.net 等服务器端语言中，会为每一个客户端连接创建一个新的线程。
而每个线程需要耗费大约 2MB 内存。也就是说，理论上，一个 8GB 内存的服务器可以同时
连接的最大用户数为 4000 个左右。要让 Web 应用程序支持更多的用户，就需要增加服务器
的数量，而 Web 应用程序的硬件成本当然就上升了
​		Node.js 不为每个客户连接创建一个新的线程，而仅仅使用一个线程。当有用户连接了，
就触发一个内部事件，通过非阻塞 I/O、事件驱动机制，让 Node.js 程序宏观上也是并行的。
使用 Node.js，一个 8GB 内存的服务器，可以同时处理超过 4 万用户的连接



###### node.js回调处理机制

```js
const fs = require("fs")

function getMimes(done){
    fs.readFile("./mime.json",(err,data)=>{
        if(err) return;
        done(data.toString());
    })
}

getMimes((msg)=>{
    console.log(msg);
})
```



###### events 模块处理异步

```js
const event = require("events")
const fs = require("fs")

function getMimes(){
    fs.readFile("./mime.json",(err,data)=>{
        if(err) return;
        //done(data.toString());
        EventEmitter.emit("mime",data.toString());
    })
}

getMimes()

EventEmitter.on("mime",(res)=>{
    console.log(res);
})
```



#### 第三方模块

##### npm包管理器

###### 简介

+ 一个命令行工具 是英文node package manager 的缩写
+ 下载node所需要的第三方模块
+ 安装node.js 自带npm无需单独安装



###### 基本语法

1. 命令

   + 安装：`npm install 模块 或 模块@版本号【安装可选参数】` 
   + 卸载：`npm uninstall 模块` 
   

   
2. 安装参数

   + --save（-S） ：记录生产环境所需模块（默认，项目目录）
   + --save-dev（-D）：记录开发环境所需模块
   + -g ：该模块可在命令行运行（全局可用）



###### 模块版本控制（了解）

1. 版本含义

   + 主版本：功能模块有大的改动，比如架构，或者增加多个模块
   + 次版本：次版本号的升级对应的只是局部的变动
   + 修改版本：bug修复或功能的扩展

   

2. 符号含义

   + ~：用户使用该版本后，最多升级到【修改版】最新
   + ^：用户使用该版本后，最多升级到【次版本】最新
   + *：用户使用该版本后，最多升级到最新版本



###### 源管理

1. 简介

   nrm是资源管理工具，可以切换国内服务器下载

   

2. 命令

   + 安装：`npm install nrm -g`
   + 查看源列表：`nrm ls`
   + 查看当前使用源：`nrm current`
   + 切换：`nrm use 服务器名`  
   + 测速：`nrm test` 

     

3. 使用cnpm命令

   + 安装：`npm install -g cnpm --registry=https://registry.npm.taobao.org`

   + 查看版本：`cnpm -v`

   + 使用：`cnpm install [name]`

     

###### 自定义脚本命令

1. package.json 配置

   ```json
   {
     "name": "es6",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "build": "babel src/test.js -o dist/test.js"
     },
     "keywords": [],
     "author": "",
     "license": "ISC",
     "devDependencies": {
       "babel-cli": "^6.26.0",
       "babel-preset-es2015": "^6.24.1"
     }
   }
   ```

   

2. 使用自定义脚本运行命令

   ```shell
   npm run build
   ```



###### npm生态市场

网址：https://www.npmjs.com

![1571879623262](http://demo.bestzhangjun.com/node/1571879623262.png)



##### 使用第三方模块（更多根据需求对应的模块包即可）

###### 自动重启服务

+ 作用：检测文件修改自动重启服务
+ 安装：`npm install nodemon -g`
+ 使用：`nodemon [filename]`

![1571879763646](http://demo.bestzhangjun.com/node/1571879763646.png)



###### 日期处理模块

+ 作用：格式化时间

+ 安装：`npm install moment`

+ 使用：查看官网文档手册

  ```js
  //示范demo
  var moment = require('moment')
  moment().format()
  ```

  

+ 在线中文网：http://momentjs.cn

![1571879867714](http://demo.bestzhangjun.com/node/1571879867714.png)



##### Socket.io（实现即时通讯）

###### 简单认知

1. 如何让服务端主动推送数据

   ● 长轮询：客户端每隔很短的时间，都会对服务器发出请求，查看是否有新的消息，只要
   轮询速度足够快，例如 1 秒，就能给人造成交互是实时进行的印象。这种做法是无奈之举，
   实际上对服务器、客户端双方都造成了大量的性能浪费

   ● 长连接：浏览器和服务器只需要要做一个握手的动作，在建立连接之后，双方可以在任
   意时刻，相互推送信息。同时，服务器与客户端之间交换的头信息很小

   

2. WebSocket 和 Socket.io

   ● 支持 WebSocket 协议的浏览器有：Chrome 4、火狐 4、IE10、Safari5
   ● 支持 WebSocket 协议的服务器有：Node 0、Apach7.0.2、Nginx1.3

   ● Node.js 从诞生之日起，就支持 WebSocket 协议。不过，从底层一步一步搭建一个 Socket 服
   务器很费劲，所以，有大神帮我们写了一个库 Socket.IO

   ● WebScoket 是一种让客户端和服务器之间能进行双向实时通信的技术。它是 HTML 最新标准
   HTML5 的一个协议规范，本质上是个基于 TCP 的协议，它通过 HTTP/HTTPS 协议发送一条特
   殊的请求进行握手后创建了一个 TCP 连接，此后浏览器/客户端和服务器之间便可以通过此
   连接来进行双向实时通信

   ● WebSocket 是 HTML5 最新提出的规范，虽然主流浏览器都已经支持，但仍然可能有不兼容的情况，
   为了兼容所有浏览器，给程序员提供一致的编程体验，SocketIO 将 WebSocket、AJAX 和其它的
   通信方式全部封装成了统一的通信接口，也就是说，我们在使用 SocketIO 时，不用担心兼容问题，
   底层会自动选用最佳的通信方式。因此说，WebSocket 是 SocketIO 的一个子集

   

3. 学习网址

   网址：https://socket.io

   ![1571879949691](http://demo.bestzhangjun.com/node/1571879949691.png)



###### 安装 Socket.io

```shell
npm install socket.io --save
```



###### 引入socket.io

```js
// 引入并挂载app
let io = require("socket.io")(app)
```



###### 服务端监听客户端连接

```js
const app = require("http").createServer()
const fs = require("fs")

app.on("request",(req,res)=>{
    fs.readFile("./views/app.html",(err,data)=>{
        if(err) return;
        res.writeHead(200,{"Content-Type":"text/html;charset='utf-8'"});
        res.end(data);
    })
})

let io = require("socket.io")(app);

io.on("connection",(socket)=>{
    console.log("监听客户端连接...");
})

app.listen(3000,()=>{
    console.log("this serve is successful...");
})
```



###### 客户端和服务端建立连接

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!-- 引入socket.io.js -->
    <script src="http://localhost:3000/socket.io/socket.io.js"></script> 
    <title>Document</title>
</head>
<body>
    <script>
        var socket = io('http://localhost:3000/');  /*和服务器建立连接*/
    </script>
    客户端<button id="button">给服务器发送数据</button>
</body>
</html>
```



###### 服务端向客户端广播数据

```js
socket.emit("to-client",{
    msg:"测试服务端给客户端广播数据"
});
```



###### 客户端接受服务端广播过来的数据

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="http://localhost:3000/socket.io/socket.io.js"></script>
    <title>Document</title>
</head>
<body>
    客户端<button id="btn">给服务器发送数据</button>
    <script>
        var socket = io('http://localhost:3000/');  /*和服务器建立连接*/
        window.onload = function(){
            var btn = document.getElementById("btn");
            socket.on("to-client",(data)=>{
                console.log(data);
            })
        }
    </script>
</body>
</html>
```



###### 客户端向服务端广播数据

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="http://localhost:3000/socket.io/socket.io.js"></script>
    <title>Document</title>
</head>
<body>
    客户端<button id="btn">给服务器发送数据</button>
    <script>
        var socket = io('http://localhost:3000/'); 
        window.onload = function(){
            var btn = document.getElementById("btn");
            btn.onclick = function(){
                socket.emit("addcart",{
                        name:"zj",
                        msg:"我叫张军"
                })
            }
        }
    </script>
</body>
</html>
```



###### 服务端接收客户端广播过来的数据

```js
socket.on("addcart",(data)=>{
     console.log(data);
})
```



### Express框架

#### 安装使用

##### 安装express

```shell
cnpm install express --save
```



##### 引入实例化app

```js
const express = require('express')
const app = express()
```



##### 初步使用

```js
const express = require('express')
const app = express()
 
app.use('/', (req, res)=>{
  res.send('Hello World')
})
 
app.listen("3000",()=>{
    console.log("this is running...");
});
```



#### 中间件机制

##### 理解中间件

###### 认识中间件

*中间件*函数能够访问[请求对象](https://expressjs.com/zh-cn/4x/api.html#req) (`req`)、[响应对象](https://expressjs.com/zh-cn/4x/api.html#res) (`res`) 以及应用程序的请求/响应循环中的下一个中间件函数，下一个中间件函数通常由名为 `next` 的变量来表示

![1571880133477](http://demo.bestzhangjun.com/node/1571880133477.png)



###### 执行任务

+ 执行任何代码
+ 对请求和响应对象进行更改
+ 结束请求/响应循环
+ 调用堆栈中的下一个中间件



###### 简单示例

```js
//如果当前中间件函数没有结束请求/响应循环，那么它必须调用 next()，以将控制权传递给下一个中间件函数，否则，请求将保持挂起状态

var express = require('express');
var app = new express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000);
```



##### 中间件类型

###### 应用级中间件

1. 匹配任意请求

   ```js
   var express = require('express')
   var app = new express()
   
   app.use((req, res, next) => {
     console.log('Time:', Date.now());
     next();
   })
   
   app.listen(3000,()=>{
       console.log("this server is running....");
   })
   ```

   

2. 匹配任何类型的 HTTP 请求

   ```js
   var express = require('express')
   var app = new express()
   
   app.use('/user', (req, res, next) => {
     console.log('Request Type:', req.method);
     next();
   })
   
   app.listen(3000,() => {
       console.log("this server is running....");
   })
   ```

   

3. 匹配指定url&指定请求方式请求

   ```js
   var express = require('express')
   var app = new express()
   
   app.get('/a', (req, res) => {
   	res.send('Got a GET request');
   })
   
   app.post('/b', (req, res) => {
   	res.send('Got a POST request');
   })
   
   app.put('/c', (req, res) => {
   	res.send('Got a PUT request');
   })
   
   app.delete('/d', (req, res) => {
   	res.send('Got a DELETE request');
   })
   
   app.all('/all',(req,res)=>{
       res.send('Got a all request');
   })
   
   app.listen(3000,() => {
       console.log("this server is running....");
   })
   ```

   

4. 中间件子堆栈

   ```js
   var express = require('express')
   var app = new express()
   
   // 传入多个中间件函数
   app.get('/user', (req, res, next) => {
     console.log('Request URL:', req.originalUrl);
     next();
   }, (req, res, next) => {
     console.log('Request Type:', req.method);
     next();
   })
   
   // 传入一个数组
   var cb0 = (req, res, next) => {
     console.log('CB0');
     next();
   }
   
   var cb1 = (req, res, next) => {
     console.log('CB1');
     next();
   }
   
   var cb2 = (req, res) => {
     res.send('Hello from C!');
   }
   
   app.get('/user', [cb0, cb1, cb2]);
   
   // 混合传参
   app.get('/user', [cb0, cb1], (req, res, next) => {
     console.log('the response will be sent by the next function ...');
     next();
   }, (req, res) => {
     res.send('Hello from D!');
   });
   
   app.listen(3000,() => {
       console.log("this server is running....");
   })
   ```

   

5. 一个路径定义多个路由

   ```js
   var express = require('express')
   var app = new express()
   
   app.get('/user/:id', function (req, res, next) {
     console.log('ID:', req.params.id);
     next();
   })
   
   app.get('/user/:id', function (req, res, next) {
     res.end(req.params.id);
   })
   
   app.listen(3000,() => {
       console.log("this server is running....");
   })
   ```

   

6. 中间件跳转

   ```js
   var express = require('express')
   var app = new express()
   
   app.get('/user/:id', (req, res, next) => {
     if (req.params.id == 0) next('route');
     else next(); 
   }, (req, res, next) => {
     res.render('regular');
   })
   
   app.get('/user/:id', (req, res, next) => {
     res.render('special');
   })
   
   app.listen(3000,() => {
       console.log("this server is running....");
   })
   ```

   

###### 路由级中间件

1. 绑定 `express.Router()` 实例

   ```js
   var express = require('express')
   var app = new express()
   var router = express.Router()
   ```

   

2. 基本使用

   ```js
   var express = require('express')
   var app = new express()
   var router = express.Router()
     
   router.use((req, res, next) => {
     console.log('Time:', Date.now());
     next();
   })
   
   router.use('/user/:id', (req, res, next) => {
     console.log('Request URL:', req.originalUrl);
     next();
   }, (req, res, next) => {
     console.log('Request Type:', req.method);
     next();
   })
   
   router.get('/user/:id', (req, res, next) => {
     if (req.params.id == 0) next('route');
     else next(); 
   }, (req, res, next) => {
     res.render('regular');
   })
   
   router.get('/user/:id', (req, res, next) => {
     console.log(req.params.id);
     res.render('special');
   })
   
   module.exports = router  
   ```

   

3. 装载路由器模块（模块化处理路由）

   ```js
   var express = require('express')
   var app = new express()
   var admin = require("./router/admin")   
   app.use("/admin",admin)
   
   app.listen(3000,() => {
       console.log("this server is running....");
   })
   ```

   

###### 内置中间件

1. 可选的 `options` 对象

   | 属性         | 描述                                                         | 类型     | 默认值       |
   | :----------- | :----------------------------------------------------------- | :------- | ------------ |
   | dotfiles     | 是否对外输出文件名以点（.）开头的文件。可选值为 “allow”、“deny” 和 “ignore” | String   | "ignore"     |
   | etag         | 是否启用etag生成                                             | Boolean  | true         |
   | extensions   | 设置文件扩展名备份选项                                       | Array    | [ ]          |
   | index        | 发送目录索引文件，设置为 false 禁用目录索引                  | mixed    | "index.html" |
   | lastModified | 设置 Last-Modified 头为文件在操作系统上的最后修改日期        | Boolean  | true         |
   | maxAge       | 毫秒或者其字符串格式设置 Cache-Control 头的 max-age 属性     | Number   | 0            |
   | redirect     | 当路径为目录时，重定向至"/"                                  | Boolean  | true         |
   | setHeaders   | 设置HTTP头以提供文件的函数                                   | Function |              |

   

2. 演示案例

   ```js
   var options = {
     dotfiles: 'ignore',
     etag: false,
     extensions: ['htm', 'html'],
     index: false,
     maxAge: '1d',
     redirect: false,
     setHeaders: function (res, path, stat) {
       res.set('x-timestamp', Date.now());
     }
   }
   
   app.use(express.static('public', options))
   ```

   

3. 设置多个静态目录

   ```js
   app.use(express.static('public'))
   app.use(express.static('uploads'))
   app.use(express.static('files'))
   ```

   

###### 错误处理中间件

1. 与其他中间件的区别

   ```js
   // 以与其他中间件函数相同的方式定义错误处理中间件函数，除了使用四个参数而不是三个参数
   
   app.use(function (err, req, res, next) {
     console.error(err.stack)
     res.status(500).send('Something broke!')
   })
   ```

   

2. 统一处理错误

   ```js
   const express = require('express');
   const fs = require('fs');
   
   let app = new express();
   
   app.get('/a',(req,res,next)=>{
       fs.readFile("/aaa",(err,data)=>{
           if(err) {
               // return res.status(500).send("serve error");
               next(err);
           }
       })
   })
   
   app.get('/a',(req,res,next)=>{
       fs.readFile("/aaa",(err,data)=>{
           if(err) {
               console.log("到我这里来...");
               return res.status(500).send("serve error");
           }
       })
   })
   
   app.get('/b',(req,res,next)=>{
       fs.readFile("/bbb",(err,data)=>{
           if(err) {
               // return res.status(500).send("serve error");
               next(err);
           }
       })
   })
   
   app.use((err,req,res,next)=>{
       console.log("出错了")
       return res.status(500).json({
           code:500,
           msg:err.message
       })
   })
   
   app.listen(3000,()=>{
       console.log("this serve is runnning...");
   })
   ```

   

###### 第三方中间件

1. express middleware

   | Middleware module                                            | Description                                                  | Replaces built-in function (Express 3) |
   | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------- |
   | [body-parser](http://www.expressjs.com.cn/en/resources/middleware/body-parser.html) | Parse HTTP request body. See also: [body](https://github.com/raynos/body), [co-body](https://github.com/visionmedia/co-body), and [raw-body](https://github.com/stream-utils/raw-body). | express.bodyParser                     |
   | [compression](http://www.expressjs.com.cn/en/resources/middleware/compression.html) | Compress HTTP responses.                                     | express.compress                       |
   | [connect-rid](http://www.expressjs.com.cn/en/resources/middleware/connect-rid.html) | Generate unique request ID.                                  | NA                                     |
   | [cookie-parser](http://www.expressjs.com.cn/en/resources/middleware/cookie-parser.html) | Parse cookie header and populate `req.cookies`. See also [cookies](https://github.com/jed/cookies) and [keygrip](https://github.com/jed/keygrip). | express.cookieParser                   |
   | [cookie-session](http://www.expressjs.com.cn/en/resources/middleware/cookie-session.html) | Establish cookie-based sessions.                             | express.cookieSession                  |
   | [cors](http://www.expressjs.com.cn/en/resources/middleware/cors.html) | Enable cross-origin resource sharing (CORS) with various options. | NA                                     |
   | [csurf](http://www.expressjs.com.cn/en/resources/middleware/csurf.html) | Protect from CSRF exploits.                                  | express.csrf                           |
   | [errorhandler](http://www.expressjs.com.cn/en/resources/middleware/errorhandler.html) | Development error-handling/debugging.                        | express.errorHandler                   |
   | [method-override](http://www.expressjs.com.cn/en/resources/middleware/method-override.html) | Override HTTP methods using header.                          | express.methodOverride                 |
   | [morgan](http://www.expressjs.com.cn/en/resources/middleware/morgan.html) | HTTP request logger.                                         | express.logger                         |
   | [multer](http://www.expressjs.com.cn/en/resources/middleware/multer.html) | Handle multi-part form data.                                 | express.bodyParser                     |
   | [response-time](http://www.expressjs.com.cn/en/resources/middleware/response-time.html) | Record HTTP response time.                                   | express.responseTime                   |
   | [serve-favicon](http://www.expressjs.com.cn/en/resources/middleware/serve-favicon.html) | Serve a favicon.                                             | express.favicon                        |
   | [serve-index](http://www.expressjs.com.cn/en/resources/middleware/serve-index.html) | Serve directory listing for a given path.                    | express.directory                      |
   | [serve-static](http://www.expressjs.com.cn/en/resources/middleware/serve-static.html) | Serve static files.                                          | express.static                         |
   | [session](http://www.expressjs.com.cn/en/resources/middleware/session.html) | Establish server-based sessions (development only).          | express.session                        |
   | [timeout](http://www.expressjs.com.cn/en/resources/middleware/timeout.html) | Set a timeout period for HTTP request processing.            | express.timeout                        |
   | [vhost](http://www.expressjs.com.cn/en/resources/middleware/vhost.html) | Create virtual domains.                                      | express.vhost                          |

   

2. additional middleware

   | Middleware module                                            | Description                                                  |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | [connect-image-optimus](https://github.com/msemenistyi/connect-image-optimus) | Optimize image serving. Switches images to `.webp` or `.jxr`, if possible. |
   | [express-debug](https://github.com/devoidfury/express-debug) | Development tool that adds information about template variables (locals), current session, and so on. |
   | [express-partial-response](https://github.com/nemtsov/express-partial-response) | Filters out parts of JSON responses based on the `fields` query-string; by using Google API’s Partial Response. |
   | [express-simple-cdn](https://github.com/jamiesteven/express-simple-cdn) | Use a CDN for static assets, with multiple host support.     |
   | [express-slash](https://github.com/ericf/express-slash)      | Handles routes with and without trailing slashes.            |
   | [express-stormpath](https://github.com/stormpath/stormpath-express) | User storage, authentication, authorization, SSO, and data security. |
   | [express-uncapitalize](https://github.com/jamiesteven/express-uncapitalize) | Redirects HTTP requests containing uppercase to a canonical lowercase form. |
   | [helmet](https://github.com/helmetjs/helmet)                 | Helps secure your apps by setting various HTTP headers.      |
   | [join-io](https://github.com/coderaiser/join-io)             | Joins files on the fly to reduce the requests count.         |
   | [passport](https://github.com/jaredhanson/passport)          | Authentication using “strategies” such as OAuth, OpenID and many others. See <http://passportjs.org/> for more information. |
   | [static-expiry](https://github.com/paulwalker/connect-static-expiry) | Fingerprint URLs or caching headers for static assets.       |
   | [view-helpers](https://github.com/madhums/node-view-helpers) | Common helper methods for views.                             |
   | [sriracha-admin](https://github.com/hdngr/siracha)           | Dynamically generate an admin site for Mongoose.             |



3. 更多中间件npmjs上查找

```js
$ npm install cookie-parser

var express = require('express')
var app = express()
var cookieParser = require('cookie-parser')

// load the cookie-parsing middleware
app.use(cookieParser())
```



##### 开发中间件

###### 自定义中间件函数

```js
var myLogger = function (req, res, next) {
  console.log('LOGGED')
  next()
}

var requestTime = function (req, res, next) {
  req.requestTime = Date.now()
  next()
}

...
```



###### 加载中间件函数

```js
var express = require('express')
var app = express()

var requestTime = (req, res, next) => {
  req.requestTime = Date.now()
  next()
}

app.use(requestTime)

app.get('/', (req, res) => {
  var responseText = 'Hello World!<br>'
  responseText += '<small>Requested at: ' + req.requestTime + '</small>'
  res.send(responseText)
})

app.listen(3000)
```



###### 中间件模块化

```js
module.exports = (options) => {
  return (req, res, next) => {
    console.log("my-middleware")
    next()
  }
}

============================================
    
var mw = require('./my-middleware.js')
app.use(mw({ option1: '1', option2: '2' }))    
```



#### 路由处理

##### 匹配任意路由

```js
app.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
})

============================================
    
router.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
})    
```



##### 匹配指定url任意的路由

```js
app.use("/user/:id",(req, res, next) => {
  console.log('Request originalUrl:', req.originalUrl);
  next();
})

============================================
    
router.use("/user/:id",(req, res, next) => {
  console.log('Request originalUrl:', req.originalUrl);
  next();
})    
```



##### 匹配指定url&指定请求方式的路由

```js
app.get("/user",(req, res, next) => {
  console.log('Request method:', req.method);
  next();
})

============================================
    
router.get("/user",(req, res, next) => {
  console.log('Request method:', req.method);
  next();
}) 
```



##### 动态路由

```js
const express = require("express")
let app = new express()

app.get("/user/:id",(req,res)=>{
    let id = req.params.id;
    res.send(id);
})

app.listen(3000,()=>{
    console.log("this is running...");
})
```



##### 请求方式

| 方式             | 解析                                           |
| ---------------- | ---------------------------------------------- |
| GET（SELECT）    | 从服务器取出资源（一项或多项）                 |
| POST（CREATE）： | 在服务器新建一个资源                           |
| PUT（UPDATE）    | 在服务器更新资源（客户端提供改变后的完整资源） |
| DELETE（DELETE） | 在服务器更新资源（客户端提供改变后的完整资源） |
| HEAD             | 获取资源的元数据                               |
| OPTIONS          | 获取信息，关于资源的哪些属性是客户端可以改变的 |
| PATCH（UPDATE）  | 在服务器更新资源（客户端提供改变的属性）       |



##### 响应方式

| Method                                                       | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [res.download()](http://www.expressjs.com.cn/en/4x/api.html#res.download) | Prompt a file to be downloaded.                              |
| [res.end()](http://www.expressjs.com.cn/en/4x/api.html#res.end) | End the response process.                                    |
| [res.json()](http://www.expressjs.com.cn/en/4x/api.html#res.json) | Send a JSON response.                                        |
| [res.jsonp()](http://www.expressjs.com.cn/en/4x/api.html#res.jsonp) | Send a JSON response with JSONP support.                     |
| [res.redirect()](http://www.expressjs.com.cn/en/4x/api.html#res.redirect) | Redirect a request.                                          |
| [res.render()](http://www.expressjs.com.cn/en/4x/api.html#res.render) | Render a view template.                                      |
| [res.send()](http://www.expressjs.com.cn/en/4x/api.html#res.send) | Send a response of various types.                            |
| [res.sendFile()](http://www.expressjs.com.cn/en/4x/api.html#res.sendFile) | Send a file as an octet stream.                              |
| [res.sendStatus()](http://www.expressjs.com.cn/en/4x/api.html#res.sendStatus) | Set the response status code and send its string representation as the response body. |



##### 路由模块化

```js
const express = require("express")
const router = express.Router()

router.get("/",(req,res)=>{
    res.send("管理员首页");
})

router.get("/login",(req,res)=>{
    res.send("管理员登陆");
})

router.get("/reg",(req,res)=>{
    res.send("管理员注册");
})

module.exports = router

============================================
    
const admin = require("./router/admin")   
app.use("/admin",admin)
```



#### 请求参数处理

##### 获取GET请求数据

```js
const express = require('express')
const app = new express()

app.use((req,res)=>{
    res.send(req.query);
})

app.listen(3000,()=>{
    console.log("this is running...");
})
```



##### 获取POST请求数据

```js
const express = require('express')
const bodyParser = require('body-parser')
 
const app = new express()
 
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
 
// parse application/json
app.use(bodyParser.json())
 
app.use((req, res) => {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})

app.listen(3000,()=>{
    console.log("this is running...");
})
```



#### ejs模板引擎

##### 安装ejs

```shell
cnpm install ejs --save
```



##### 使用ejs

```js
const express = require("express")
const path = require('path')
const app = new express()

app.set('views', path.join(__dirname,'views'))
app.set("view engine","ejs")

app.use((req,res) => {
	res.render("news",{
 		"msg" : "测试数据"
 	});
})

app.listen(3000,()=>{
    console.log("this is running...");
})
```



##### 修改模板文件夹位置

```js
app.set('views', path.join(__dirname,'pages'))
```



##### 修改文件后缀

```js
//引入ejs
const ejs = require("ejs")

//注册html模板
app.engine("html",ejs.__express)

//设置转换
app.set("view engine","html")
```



##### ejs基础语法

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
    <!-- 子模板 -->
    <% include header.html islog%>

    <!-- 变量输出 -->
    <% var a = 123 %>
    <%= msg %>
    <%- '<div>这是html内容</div>' %>

    <!-- 条件判断 -->
    <%if(true){%>
        <div>true</div>
    <%}else{%>
        <div>false</div>
    <%}%>

    <!-- 循环 -->
    <%for(var i=0;i<list.length;i++){%>
        <li><%= list[i].name %></li>
    <%}%>
</body>
</html>
```



#### art-template模板引擎

##### 安装art-template

```shell
npm install art-template --save
npm install express-art-template --save
```



##### 引入art-template

```js
const template = require('express-art-template')
```



##### 使用art-template

```js
const express = require('express')
const path = require('path')
const template = require('express-art-template')

let app = new express()

app.engine("art",template) //express兼容art-template

app.set('views', path.join(__dirname,'views')) //设置模板文件夹位置 
app.set("view engine","art") // 设置模板后缀名

app.use((req,res) => {
	res.render("test",{
 		"msg" : "测试数据"
 	});
})

app.listen('3000',()=>{
    console.log("this serve is runnning...");
})
```



#### 托管静态资源

##### 静态资源中间件

```js
// 配置属性
let options = {
  dotfiles: 'ignore',
  etag: false,
  extensions: ['htm', 'html'],
  index: false,
  maxAge: '1d',
  redirect: false,
  setHeaders: (res, path, stat) => {
    res.set('x-timestamp', Date.now());
  }
}

// 设置目录
app.use(express.static('public'),options)
```



##### 设置静态资源虚拟目录

```js
app.use('/static', express.static('public'))
```



##### 指定多个静态资源目录

```js
app.use(express.static('public'))
app.use(express.static('uploads'))
app.use(express.static('files'))
```



#### 会话管理

##### cookie（客户端）

###### cookie简介

● cookie 是存储于访问者的计算机中的变量。可以让我们用同一个浏览器访问同一个域
名的时候共享数据

● HTTP 是无状态协议。简单地说，当你浏览了一个页面，然后转到同一个网站的另一个页
面，服务器无法认识到这是同一个浏览器在访问同一个网站。每一次的访问，都是没有任何
关系的

● Cookie 是一个简单到爆的想法：当访问一个页面的时候，服务器在下行 HTTP 报文中，
命令浏览器存储一个字符串; 浏览器再访问同一个域的时候，将把这个字符串携带到上行
HTTP 请求中。第一次访问一个服务器，不可能携带 cookie。 必须是服务器得到这次请求，
在下行响应报头中，携带 cookie 信息，此后每一次浏览器往这个服务器发出的请求，都会
携带这个 cookie



###### cookie特点

● ccookie 保存在浏览器本地
● 正常设置的 cookie 是不加密的，用户可以自由看到;
● 用户可以删除 cookie，或者禁用它
● cookie 可以被篡改
● cookie 可以用于攻击
● cookie 存储量很小，未来实际上要被 localStorage 替代，但是后者 IE9 兼容



###### cookie使用

1. 基本使用

   ```js
   // 安装 
   cnpm instlal cookie-parser --save
   
   // 引入
   const cookieParser = require('cookie-parser')
   
   // 使用中间件
   app.use(cookieParser())
   
   //设置 cookie
   res.cookie("name",'zhangsan',{
       maxAge: 900000, 
       httpOnly: true
   })
   
   //获取 cookie
   req.cookies.name
   ```

   

2. cookie属性

   |    属性    |  种类   |                             描述                             |
   | :--------: | :-----: | :----------------------------------------------------------: |
   |   domain   | String  |                             域名                             |
   | name=value |  Date   | 键值对，可以设置要保存的 Key/Value，注意这里的 name 不能和其他属性项的名字一样 |
   |  Expires   | Boolean | 过期时间（秒），在设置的某个时间点后该 Cookie 就会失效，如 expires=Wednesday,09-Nov-99 23:12:40 GMT |
   |   maxAge   |  Date   |            最大失效时间（毫秒），设置在多少后失效            |
   |   secure   | String  | 当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效 |
   |    Path    | String  | 表示 cookie 影响到的路径，如 path=/，如果路径不能匹配时，浏览器则不发送这个 Cookie |
   |  httpOnly  | Boolean | 是微软对 COOKIE 做的扩展。如果在 COOKIE 中设置了“httpOnly”属性，则通过程序（JS脚本、applet 等）将无法读取到 COOKIE 信息，防止 XSS 攻击产生 |
   |   singed   | Boolean | 表示是否签名 cookie, 设为 true 会对这个 cookie 签名，这样就需要用res.signedCookies 而不是 res.cookies 访问它。被篡改的签名 cookie 会被服务器拒绝，并且 cookie值会重置为它的原始值 |

   

3. 设置cookie

   ```js
   const express = require("express")
   const cookieParser = require('cookie-parser')
   
   let app = new express()
   
   app.use(cookieParser())
   
   app.get("/set",(req,res)=>{
       res.cookie("name","雪茹",{
           maxAge:60000,
           secure:false,
           path:"/get",
           httpOnly:true
       });
       res.send("设置cookie完毕");
   })
   
   app.listen("3000",()=>{
       console.log("this server is running...");
   })
   ```

   

4. 获取cookie

   ```js
   const express = require("express")
   const cookieParser = require('cookie-parser')
   
   let app = new express()
   
   app.use(cookieParser())
   
   app.get("/get",(req,res)=>{
       let name = req.cookies.name;
       res.send("姓名:"+name);
   })
   
   app.listen("3000",()=>{
       console.log("this server is running...");
   })
   ```

   

5. 删除cookie

   ```js
   const express = require("express")
   const cookieParser = require('cookie-parser')
   
   let app = new express()
   
   app.use(cookieParser())
   
   app.get("/set",(req,res)=>{
       res.cookie("name","雪茹",{maxAge:0});
   })
   
   app.get("/get",(req,res)=>{
       let name = req.cookies.name;
       res.send("姓名:"+name);
   })
   
   app.listen("3000",()=>{
       console.log("this server is running...");
   }) 
   ```




6. 加密cookie

   ```js
   const express = require("express")
   const cookieParser = require('cookie-parser')
   let app = new express()
   
   app.use(cookieParser('123456')) //使用的时候需要传参可以是任意字符
   
   app.get("/set",(res,req)=>{
   	res.cookie('userinfo','hahaha',{
       	domain:'demo.ccc.com',
       	maxAge:900000,
       	httpOnly:true,
       	signed:true //signed 属性需要设置为true
   	})
   })
   
   app.get("/get",(req,res)=>{
       console.log(req.signedCookies);	
   })
   
   app.listen("3000",()=>{
       console.log("this server is running...");
   }) 
   ```

   

##### session（服务端）

###### 简单介绍

session 是另一种记录客户状态的机制，不同的是 Cookie 保存在客户端浏览器中，而 session 保存在服
务器上



###### 工作流程

当浏览器访问服务器并发送第一次请求时，服务器端会创建一个 session 对象，生成一个类似于
key,value 的键值对，然后将 key(cookie)返回到浏览器(客户)端，浏览器下次再访问时，携带 key(cookie)，
找到对应的 session(value)，客户的信息都保存在 session 中



###### 简单使用

```js
// 安装 
cnpm install express-session

// 引入
const session = require("express-session")

//session中间件配置
const app = express()
app.set('trust proxy', 1) // trust first proxy
app.use(session({
	secret: 'keyboard cat',
    resave: false,
    saveUninitialized: true,
    cookie: { secure: true }
}))

//设置
app.get("/set",(req,res)=>{
    req.session.name = "zj";
    res.send("设置session完毕");
})

//获取
app.get("/get",(req,res)=>{
    res.send(req.session.name);
})

app.listen("3000",()=>{
    console.log("this server is running...");
}) 
```



###### 常用参数

|       参数        |                             作用                             |
| :---------------: | :----------------------------------------------------------: |
|      secret       |  一个 String 类型的字符串，作为服务器端生成 session 的签名   |
|       name        | 返回客户端cookie的 name ，默认为 connect.sid,也可以自己设置  |
|      resave       | 强制保存 session 即使它并没有变化，默认为 true，建议设置成 false |
| saveUninitialized | 强制将未初始化的 session 存储。当新建了一个 session 且未设定属性或值时，它就处于未初始化状态。在设定一个 cookie 前，这对于登陆验证，减轻服务端存储压力，权限控制是有帮助的。（默认：true），建议手动添加 |
|      cookie       | 设置返回到前端 key 的属性，默认值为{ path: ‘/’, httpOnly: true, secure: false, maxAge: null } |
|      rolling      | 在每次请求时强行设置 cookie，这将重置 cookie 过期时间（默认：false） |



###### 常用方法

```js
req.session.destory((err) => { /*销毁 session*/
	console.log(err);
})

req.session.cookie.maxAge=0; //重新设置 cookie 的过期时间

req.session.username='张三'; //设置 session

req.session.username //获取 session
```



###### 负载均衡配置session

```js
//安装
cnpm install express-session --save
cnpm install connect-mongo --save

//引入
const session = require("express-session")
const MongoStore = require('connect-mongo')(session)

//配置session中间件
app.use(session({
	secret: 'keyboard cat',
	resave: false,
	saveUninitialized: true,
	rolling:true,
	cookie:{
		maxAge:100000
	},
	store: new MongoStore({
		url: 'mongodb://127.0.0.1:27017/test',
		touchAfter: 24 * 3600 // time period in seconds
	})
}))

app.listen("3000",()=>{
    console.log("this server is running...");
}) 
```



##### cookie&session区别

+ cookie 数据存放在客户的浏览器上，session 数据放在服务器上
+ cookie 不是很安全，别人可以分析存放在本地的 COOKIE 并进行 COOKIE 欺骗
  考虑到安全应当使用 session
+ session 会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
  考虑到减轻服务器性能方面，应当使用 COOKIE
+ 单个 cookie 保存的数据不能超过 4K，很多浏览器都限制一个站点最多保存 20 个 cookie



#### 文件上传处理

##### 安装multiparty 中间件

```shell
npm install multiparty --save
```



##### 引入multiparty 中间件

```js
const multiparty = require('multiparty')
```



##### 配置multiparty 中间件

```js
let form = new multiparty.Form()
form.uploadDir="uploads"   //上传图片保存的地址
form.parse(req, (err, fields, files) => {
    console.log(fields); //普通表单数据
    console.log(files); //文件数据
})
```



##### 使用multiparty 中间件

```js
const multiparty = require('multiparty')
let app = new express();

app.post('/doProductAdd',function(req,res){
    var form = new multiparty.Form();
    form.uploadDir = "uploads";  
    form.parse(req, function(err, fields, files) {
        DB.insert('product',{
            title:fields.title[0],
            price:fields.price[0],
            fee:fields.fee[0],
            description:fields.description[0],
            pic:files.pic[0].path
        },(err,data)=>{
            if(!err){
                res.redirect('/product');
            }
        })
    });
})

app.listen('3000',()=>{
    console.log("this serve is runnning...");
})
```



#### 即时通讯（socket.io）

##### 安装socket.io中间件

```shell
npm install --save socket.io
```



##### 引入并配置socket.io中间件

```js
const app = require('express')()
const server = require('http').Server(app)
const io = require('socket.io')(server)
```



##### 服务端监听客户端连接

```js
var app = require('express')()
var http = require('http').createServer(app)
var io = require('socket.io')(http)
var template = require('express-art-template')
var path = require('path')

app.engine("html",template) 
app.set('views', path.join(__dirname,'views')) 
app.set("view engine","html") 

app.use(express.static('public'))

app.get('/', function(req, res){
    res.render('test')
})

io.on('connection', function(socket){
  console.log("a user connected")
})

http.listen(3000, function(){
  console.log('listening on *:3000')
})
```



##### 客户端和服务器建立连接

```js
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();
</script>
```



##### 单用户通讯

###### 在线智能机器人原理分析

```js
io.on('connection', (socket)=>{
  console.log("a user connected")
  socket.on("msg",(data)=>{
    socket.emit("info","服务端回应消息"+data);
  })
})
```



###### 实现在线智能机器人

```js
# 服务端
io.on("connection", socket => {
    socket.on("client",(data)=>{
        chatModel.find({"title":{$regex:new RegExp(data.msg)}},(err,data)=>{
            if(err) return;
            socket.emit("serve",{
                result:data
            })  
        })
    })
})

# 客户端
socket.on("serve",(data)=>{
    let newp = document.createElement("p");
    if(data.result.length){
        newp.innerHTML = data.result[0].content;
    }else{
        newp.innerHTML = "主人我的功能还有待提高,暂无服务";
    }
    newp.setAttribute("class","rpsty")
    list.appendChild(newp);
    footer.scrollIntoView();
})
send.onclick = function(){	
    socket.emit("client",{
        msg:textarea.value
    })
    let newp = document.createElement("p");
    newp.innerHTML = textarea.value;
    newp.setAttribute("class","lpsty")
    list.appendChild(newp);
    txtarea.value = "";	
}	
```



##### 多用户通讯

###### 聊天室原理分析

```js
io.on('connection', function(socket){
  console.log("a user connected")
  socket.on("msg",(data)=>{
    io.emit("info","服务端回应消息"+data)
  })
})
```



###### 实现聊天室

```js
# 服务端
io.on("connection", socket => {
    socket.on("client",(data)=>{
        socket.broadcast.emit("serve",data.msg)
        //io.emit("serve",data.msg)
    })
})

# 客户端
socket.on("serve",(data)=>{
    let newp = document.createElement("p");
    newp.innerHTML = data;
    newp.setAttribute("class","rpsty")
    list.appendChild(newp);
    footer.scrollIntoView();
})
send.onclick = function(){	
    socket.emit("client",{
        msg:textarea.value
    })
    let newp = document.createElement("p");
    newp.innerHTML = textarea.value;
    newp.setAttribute("class","lpsty")
    list.appendChild(newp);
    txtarea.value = "";	
}	
```



##### 分组用户通讯

###### 分组用户通讯分析

```js
io.on('connection', (socket)=>{
  let roomid = url.parse(socket.request.url,true).query.roomid
  socket.join(roomid)
  socket.on("addcart",(data)=>{
    io.to(roomid).emit("serve","Server AddCart Ok") ///通知分组内的所有用户
    socket.broadcast.to(roomid).emit('serve','Server AddCart Ok') //通知分组内的用户不包括自己
  })
})
```



###### 每桌无人点餐案例

```js
...code
```



### Koa2.x框架

#### 安装使用

##### 安装koa

```shell
cnpm install koa --save
```



##### 引入实例化app对象

```js
const Koa = require('koa')
const app = new Koa()
```



##### 初步运用

```js
const Koa = require('koa')
const app = new Koa()
 
app.use(ctx => {
  ctx.body = 'Hello Koa';
})
 
app.listen("3000",()=>{
    console.log("this server is running...");
}) 
```



#### 中间件机制

##### koa&express 中间件的区别

###### 异步差异

| Express | callback           |
| ------- | ------------------ |
| 框架    | 异步               |
| Koa1    | generator/yield+co |
| Koa2    | Async/Await        |



###### 写法区别

```js
//express
var express = require('express')
var app = express()

app.use((req,res,next)=>{
    next()
})

app.use((req,res,next)=>{
    res.send('Hello Express!')
})

app.listen(3000,()=>{
    console.log("Server Start Successful Listening Port 3001");
})

===============================================================
    
//koa
const koa = require("koa");
let app = new koa();

app.use(async (ctx, next) => {
  const start = new Date();
  await next();
  const ms = new Date() - start;
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
})

app.use(ctx => {
  ctx.body = 'Hello Koa';
})

app.listen(3000,()=>{
    console.log("Server Start Successful Listening Port 3001");
})   
```



###### 执行顺序区别

express内部维护一个函数数组，这个函数数组表示在发出响应之前要执行的所有函数，也就是中间件数组，每一次use以后，传进来的中间件就会推入到数组中，执行完毕后调用next方法执行函数的下一个函数，如果没用调用，调用就会终止

![](https://user-gold-cdn.xitu.io/2018/7/29/164e57423cae6c7b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



Koa会把多个中间件推入栈中，与express不同，koa的中间件是所谓的洋葱型模型

![](https://user-gold-cdn.xitu.io/2018/7/29/164e57423c9f040b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



##### koa中间件函数

```js
const Koa = require('koa')
const app = new Koa()

const logger = (ctx,next) => {
    console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`)
    next()
}

const main = ctx => { 
    ctx.body='hello koa'
}

app.use(logger)
app.use(main)

app.listen(3000)
```



##### koa异步中间件

```js
const Koa = require('koa')
const app = new Koa()
const fs = require('fs.promised') 

app.use(async (ctx,next)=>{
    ctx.body = await fs.readFile('./views/test.html','utf-8')
})

app.listen(3000,()=>{
    console.log("successful...")
})
```



##### koa中间件的合成

```js
const Koa = require('koa')
const compose = require('koa-compose')
const app = new Koa()

const logger = (ctx,next) => {
    console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`)
    next()
}

const main = (ctx,next) => {
    ctx.body='hello world'
}

const middleWare = compose([logger, main])

app.use(middleWare)

app.listen(3000,()=>{
    console.log("successful...");
})
```



##### koa中间件类型

###### 应用级中间件

```js
const koa = require("koa")
const app = new koa()

const timer = (ctx,next)=>{
    conso.log('Time',Date.now())
    next()
}

app.use(timer())

app.listen(3000,()=>{
    console.log("this server is running....")
})
```



###### 路由中间件

1. 匹配任意路由

   ```js
   
   ```

   

2. 匹配任意请求方式的确定路由

   ```js
   
   ```

   

3. 匹配指定请求方式的严格匹配路由

   ```js
   
   ```

   

###### 错误处理中间件

1. 500错误
2. 404错误
3. 错误处理中间件       
4. 监听error事件
5. 释放error事件



###### 第三方中间件

| 名称             | 作用                    |
| ---------------- | ----------------------- |
| koa-router       | 路由中间件              |
| koa-art-template | 模板引擎                |
| koa-static       | 静态资源处理            |
| koa-bodyparser   | 处理post数据的中间件    |
| koa-session      | session处理中间件       |
| mongoose         | 连接MongoDB数据的中间件 |
| koa-multer       | 文件上传数据处理        |
| js-xss           | 防xss攻击模块           |



#### 请求参数处理

##### GET请求参数的获取

```js
const Koa = require('koa')
const app = new Koa()
 
app.use(async(ctx)=>{
    let reqGetStr = {
        //获取request请求参数对象
        ctxrq:ctx.request.query,
        ctxq:ctx.query,
       
        //获取request请求参数字符串
        ctxrqs:ctx.request.querystring,
        ctxqs:ctx.querystring
    }
    ctx.body = reqGetStr;
})

app.listen(3000,()=>{
    console.log("this server is success...")
})
```



##### 	POST请求参数的获取

###### 手动封装post请求参数

```js
let parsePostData = (ctx)=>{
    return new Promise((resolve,reject)=>{
        try {
            let postdata = "";
            ctx.req.on("data",(data)=>{
                postdata += data;
            })
            ctx.req.addListener("end",()=>{
                let parseData = parseQueryStr(postdata);
                resolve(parseData);
            })
        } catch (error) {
            reject(error)
        }
    })
}

let parseQueryStr = (str)=>{
    let obj = {};
    let strarr = str.split("&");
    for (let [index,params] of strarr.entries()) {
        let paramsarr = params.split("=");
        obj[paramsarr[0]] = decodeURIComponent(paramsarr[1]); 
    }
    return obj;
}

module.exports = parsePostData;
```



###### 测试是否获取成功

```js
const Koa = require('koa')
const app = new Koa()
const parsePostData = require("./tools/tools")
 
app.use(async(ctx)=>{
    ctx.body = await parsePostData(ctx);
})

app.listen(3000,()=>{
    console.log("this server is success...")
})
```



##### 处理post请求数据中间件

1. 安装koa-bodyparser

   ```shell
   npm install koa-bodyparser --save
   ```

   

2. 引入koa-bodyparser

   ````js
   const bodyParser = require('koa-bodyparser')
   ````

   

3. 使用koa-bodyparser

   ```js
   const Koa = require('koa')
   const bodyParser = require('koa-bodyparser')
    
   let app = new Koa()
   app.use(bodyParser())
    
   app.use(async ctx => {
       ctx.body = ctx.request.body;
   });
   
   app.listen(3000,()=>{
       console.log("this server is success...")
   })
   ```

   

#### 路由处理

##### 安装路由中间件

```shell
npm install koa-router --save
```



##### 引入并实例化路由对象

```js
const Router = require('koa-router')
const router = new Router()
```



##### 使用路由中间件

```js
const Koa = require('koa')
const Router = require('koa-router')
 
const app = new Koa()
const router = new Router()
 
router
    .get('/get', (ctx, next) => {
     	ctx.body = "this is get router"
    })
	.post('/post', (ctx, next) => {
      	ctx.body = "this is post router"
    });
 
app
  .use(router.routes())
  .use(router.allowedMethods())

app.listen(3000,()=>{
    console.log("this serve is runnning...")
})
```



##### 动态路由

```js
const Koa = require('koa')
const Router = require('koa-router')
 
const app = new Koa()
const router = new Router()
 
router.get('/pagelist/:id', (ctx, next) => {
    ctx.body = ctx.request.query.id
})
 
app
  .use(router.routes())
  .use(router.allowedMethods())

app.listen('3000',()=>{
    console.log("this serve is runnning...")
})
```



##### 路由重定向

```js
const Koa = require('koa')
const app = new Koa()
const route = require('koa-route')

router.get('/',(ctx)=>{
  ctx.redirect('/')
})

router.get('/about',(ctx)=>{
  ctx.body='<h1>这是首页</h1>'
})

app
  .use(router.routes())
  .use(router.allowedMethods())

app.listen(3000)
```



##### 路由模块化

```js
const Koa = require("koa")
const Router = require("koa-router")
const admin = require("./routers/admin")
const api = require("./routers/api")
const index = require("./routers/index") 

const app = new Koa()
const router = new Router()

//层级路由 ，一层一层划分 注意：当检测路由是/开头 它下面的模块无需加/

router.use("/",index)
router.use("/admin",admin)
router.use("/api",api)

app
  .use(router.routes())
  .use(router.allowedMethods())

app.listen(3000,()=>{
    console.log("this server is running....")
})
```



#### 托管静态资源

##### 安装koa-static中间件

```shell
npm install koa-static --save
```



##### 引入koa-static中间件

```js
const static = require('koa-static')
```



##### 设置静态资源目录

+ 在根目录创建`/public/img`  `/public/css` `/public/js`目录接口，实现可以通过链接访问图片资源
+ 只有指定的静态目录中的静态资源可以访问

```js
app.use(static(path.join(__dirname,"./public")))
```



##### 使用koa-static中间件

```js
const Koa = require('koa')
const Router = require('koa-router')
const static = require('koa-static')
const render = require('koa-art-template')
const path = require('path')

const router = new Router()
const app = new Koa()

app.use(static(path.join(__dirname,"./public")))

render(app, {
  root: path.join(__dirname, 'views'),
  extname: '.html',
  debug: process.env.NODE_ENV !== 'production'
})

app.listen(3000,()=>{
    console.log("this is server is running....");
})
```



#### 模板引擎渲染

##### 安装 koa-art-template中间件

```shell
npm install koa-art-template --save
```



##### 引入 koa-art-template中间件

```js
const render = require('koa-art-template')
```



##### 使用高效率模板引擎

###### 基本使用

```js
const Koa = require('koa')
const render = require('koa-art-template')
const app = new Koa()

render(app, {
  root: path.join(__dirname, 'view'),
  extname: '.html',
  debug: process.env.NODE_ENV !== 'production'
})
 
app.use(async (ctx) => {
  await ctx.render('test',{
      msg:"测试渲染数据"
  });
})
 
app.listen(3000,()=>{
    cosole.log("this server is running... ")
})
```



###### 循环数据

```js
{{each target}}
    {{$index}} {{$value}}
{{/each}}
```



###### 判断

```js
{{if value}} ... {{/if}}
{{if v1}} ... {{else if v2}} ... {{/if}}
```



###### 输出

```js
{{value}}
{{data.key}}
{{data['key']}}
{{a ? b : c}}
{{a || b}}
{{a + b}}
```



###### 子模板

```js
{{include './header.art'}}
{{include './header.art' data}}
```



###### 继承模板

```js
{{extend './layout.art'}}
{{block 'head'}} ... {{/block}}
```



#### 会话管理

##### koa-cookie的认识

###### cookie简介

+ cookie是存储与访问者计算机的变量，可以让我们用同一个浏览器访问同一个域名的时候共享数据
+ http是无状态协议，简单的说浏览了一个页面，然后转到同一个网站的另一个页面，服务器无法然识别这是同一个浏览器在访问同一个网站，每次的访问都是没有关系的



###### koa-cookie的使用

1. 设置koa-cookie值

   ```js
   const Koa = require('koa')
   const Router = require('koa-router')
   const app = new Koa()
    
   router.get('/set', (ctx, next) => {
       ctx.cookies.set("username","zhangjun",{
             maxAge:60*60*1000, //设置过期最大时间
             path:"/get", //设置允许访问的路径
             httpOnly:false //客户端和服务都访问到cookie
        })
   })
   
   app.listen(3000,()=>{
       console.log("this server is success...")
   })
   ```
   

   
2. 配置参数说明

   | options 名称 |                       options 值                        |
   | :----------: | :-----------------------------------------------------: |
   |    maxage    |            一个数字表示Date.now() 得到毫秒数            |
   |   expires    |                    cookie 过期的Date                    |
   |     path     |         cookie 路径，默认是 / 全部路径可以访问          |
   |    domain    |                        设置域名                         |
   |    secure    | 安全的cookie 默认false 设置成true表示只有https 可以访问 |
   |   httpOnly   |         是否只是服务器可以访问cookie 默认是true         |
   |  overwrite   |      一个布尔值，表示是否覆盖以前设置的同名cookie       |

   

3. 获取koa-cookie值

   ```js
   const Koa = require('koa')
   const Router = require("koa-router")
   
   const app = new Koa()
   const router = new Router()
   
   router
       .get('/setcookies', (ctx, next) => {
       	ctx.cookies.set("username","zhangjun",{
               maxage:60*1000
           })
       })
       .get('/getcookies', (ctx, next) => {
           let uname = ctx.cookies.get("username");
           ctx.body = uname; 
       })
   
   app.listen(3000,()=>{
       console.log("this server is success...")
   })
   ```

   

##### koa-session的认识

###### session简介

session 是一种记录客户端状态的机制，不同的是cookie保存在客户端浏览器中，而session保存在服务器上



###### session工作流程

当浏览器访问服务器并发送第一次请求时，服务器端会创建一个session对象，生成一个类似key，value的键值对，然后key（cookie）返回到客户端，浏览器下次在访问时，携带key（cookie）找到对应session（value），客户端的信息都保存在session中



###### koa-session的使用

1. 安装koa-session

   ```shell
   npm install koa-session --save
   ```

   

2. 引入koa-session

   ```js
   const session = require('koa-session')
   ```

   

3. 演示demo

   ```js
   const Koa = require('koa')
   const Router = require("koa-router")
   const session = require('koa-session')
   
   const app = new Koa()
   const router = new Router()
    
   app.keys = ['some secret hurr']
    
   const CONFIG = {
     key: 'koa:sess',  //cookie key (defaulg is koa:sess)
     maxAge: 86400000, //响应给客户端的cookie过期时间 【需要修改】
     autoCommit: true, 
     overwrite: true,  //是否可以重新设置
     httpOnly: true,  //cookie是否只有服务器可以访问
     signed: true,  //签名默认true
     rolling: false,  //在每次请求时强行设置cookie 这将重置cookie过期时间 【需要修改】
     renew: false, //cookie即将过期强行设置cookie 这将重置cookie过期时间 需要修改】
   };
    
   app.use(session(CONFIG, app))
   
   router
       .get('/set', (ctx, next) => {
       	ctx.session.username = "zhangjun";
       })
       .get('/get', (ctx, next) => {
          console.log(ctx.session.username);
       })
    
   app.listen(3000,()=>{
   	console.log('listening on port 3000');    
   })
   ```

   

##### cookie&session区别

+ 存储数据的位置不同，cookie在访问者计算机中 ，session在服务端
+ 安全性不同，cookie的数据源在客户端能获取到不是很安全，session在服务端相对于cookie较安全
+ 存储容量不同，cookie存储少量不重要的数据，session可以存储大量数据
+ 工作流程不同，cookie直接把数据源返给客户端，session基于cookie工作，每次访问服务端只需要将返给客户端的唯一标识sessionid（cookie key）做对比，如果相同则可以获取对应的session对象中的数据，不同则获取不到



#### 文件上传处理

##### 安装koa-multer中间件

```shell
npm install koa-multer --save
```



##### 引入koa-multer中间件

```js
const multer = require('koa-multer')
```



##### 配置koa-multer中间件

```js
const storage = multer.diskStorage({
	destination: (req, file, cb) => {
		cb(null, 'public/uploads/') //注意路径必须存在
	},	
    filename: (req, file, cb) => {
		let fileFormat = (file.originalname).split(".");
		cb(null,Date.now() + "." + fileFormat[fileFormat.length - 1]);
	}
})

const upload = multer({ storage: storage })
```



##### 使用koa-multer中间件

```js
const koa = require("koa");
const Router = require("koa-router");
const multer = require("koa-multer");

let app = new koa();
let router = new Router();

const storage = multer.diskStorage({
	destination: (req, file, cb) => {
		cb(null, 'public/uploads/') //注意路径必须存在
	},	
    filename: (req, file, cb) => {
		let fileFormat = (file.originalname).split(".");
		cb(null,Date.now() + "." + fileFormat[fileFormat.length - 1]);
	}
})

const upload = multer({ storage: storage })

router.post('/doAdd',upload.single('pic'),async (ctx, next) => {
    ctx.body = {
        filename: ctx.req.file.filename,
        body:ctx.req.body
    }
});

app.listen('3000',()=>{
    console.log("this serve is runnning...");
})
```



#### 即时通讯（socket.io）

##### 安装koa-socket中间件

```shell
cnpm i -S koa-socket
```



##### 引入并配置koa-socket中间件

```js
const app = new koa();
const IO = require("koa-socket"); // 引入
const io = new IO(); // 实例化
io.attach(app); // 附加app
```



##### 服务端监听客户端连接

```js
const koa = require("koa");
const static = require("koa-static");
const render = require("koa-art-template");
const Router = require("koa-router");
const IO = require("koa-socket");
const path = require("path");

const app = new koa();
const io = new IO();
const router = new Router();
io.attach(app);

app.use(static(path.join(__dirname,"./public")));

render(app, {
    root: path.join(__dirname, 'views'),
    extname: '.html',
    debug: process.env.NODE_ENV !== 'production'
})

router.get("/", async(ctx)=>{
    ctx.render("test");
})

app._io.on("connection", socket => {
    console.log("a user connected")
})

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(3001,()=>{
    console.log("Server Start Successful Listening Port 3001");
})
```



##### 客户端和服务器建立连接

```js
// 引入对应文件 
<script src="http://localhost:3001/socket.io/socket.io.js"></script>  

// 和服务器建立连接
var socket = io('http://localhost:3001/'); 
```



##### 单用户通讯

###### 在线智能机器人原理分析

```js
// 原理： 谁发送数据给服务端，服务端就响应给谁
app._io.on("connection", socket => {
    // 服务端监听客户端广播过来的数据
    socket.on("client",(data)=>{
        console.log(data.msg);
        // 服务端给客户端我广播数据
        socket.emit("serve",{
            msg:"我是服务端数据"
        })
    })
})
```



###### 实现在线智能机器人

```js
# 服务端
app._io.on("connection", socket => {
    socket.on("client",(data)=>{
        chatModel.find({"title":{$regex:new RegExp(data.msg)}},(err,data)=>{
            if(err) return;
            socket.emit("serve",{
                result:data
            })  
        })
    })
})

# 客户端
socket.on("serve",(data)=>{
    let newp = document.createElement("p");
    if(data.result.length){
        newp.innerHTML = data.result[0].content;
    }else{
        newp.innerHTML = "主人我的功能还有待提高,暂无服务";
    }
    newp.setAttribute("class","rpsty")
    list.appendChild(newp);
    footer.scrollIntoView();
})

send.onclick = function(){	
    socket.emit("client",{
        msg:textarea.value
    })
    let newp = document.createElement("p");
    newp.innerHTML = textarea.value;
    newp.setAttribute("class","lpsty")
    list.appendChild(newp);
    txtarea.value = "";	
}	
```



##### 多用户通讯

###### 聊天室原理分析

```js
app._io.on('connection', function(socket){
  console.log("a user connected")
  socket.on("msg",(data)=>{
    io.emit("info","服务端回应消息"+data) //通知所有连接的用户（客户端）  
  })
})
```



###### 实现聊天室

```js
# 服务端
app._io.on("connection", socket => {
    socket.on("client",(data)=>{
        socket.broadcast.emit("serve",data.msg)
        //io.emit("serve",data.msg)
    })
})

# 客户端
socket.on("serve",(data)=>{
    let newp = document.createElement("p");
    newp.innerHTML = data;
    newp.setAttribute("class","rpsty")
    list.appendChild(newp);
    footer.scrollIntoView();
})
send.onclick = function(){	
    socket.emit("client",{
        msg:textarea.value
    })
    let newp = document.createElement("p");
    newp.innerHTML = textarea.value;
    newp.setAttribute("class","lpsty")
    list.appendChild(newp);
    txtarea.value = "";	
}	
```



##### 分组用户通讯

###### 分组用户通讯分析

```js
app._io.on('connection', (socket)=>{
  let roomid = url.parse(socket.request.url,true).query.roomid
  socket.join(roomid)
  socket.on("addcart",(data)=>{
    io.to(roomid).emit("serve","Server AddCart Ok") //通知分组内的所有用户
    socket.broadcast.to(roomid).emit('serve','Server AddCart Ok') //通知分组内的用户不包括自己
  })
})
```



###### 每桌无人点餐案例

```js
...code
```



### 数据库交互

#### Mongodb

##### 安装驱动

```shell
npm i mongodb --save
```



##### 引入驱动

```js
const MongoClient = require('mongodb').MongoClient;
```



##### 数据库连接封装

```js
connect(){
    return new Promise((resolve,reject)=>{
        if(this.dbClient == null){
            //获得连接对象 这里只获取一次
            MongoClient.connect(Config.dbUrl,(err,client)=>{
                if (err){
                    reject(err);
                }else{
                    //获得db对象
                    this.dbClient = client.db(Config.dbName);
                    resolve(this.dbClient);
                }
            })
        }else{
            resolve(this.dbClient);
        }
    })
```



##### CRUD封装

```js
//查询
find(collectionname,condition){
    return new Promise((resolve,reject)=>{
        this.connect().then((db)=>{
            let res = db.collection(collectionname).findOne(condition);
            res.toArray((err,docs)=>{
                if(err) {
                    reject(err);
                    return;
                }else{
                    resolve(docs);  
                }
            })
        })
    })
}

//增加
insert(collectionname,condition){
    return new Promise((resolve,reject)=>{
        this.connect().then((db)=>{
            db.collection(collectionname).insertOne(condition,(err,res)=>{
                if(err){
                    reject(err);
                    return;
                }else{
                    resolve(res);
                }
            });
        })
    })
}

//修改
update(collectionname,old,ori){
    return new Promise((resolve,reject)=>{
        this.connect().then((db)=>{
            db.collection(collectionname).updateOne(old,{$set:ori},(err,res)=>{
                if(err){
                    reject(err);
                    return;
                }else{
                    resolve(res);
                }
            });
        })
    })
}

//删除
remove(collectionname,condition){
    return new Promise((resolve,reject)=>{
        this.connect().then((db)=>{
            db.collection(collectionname).deleteOne(condition,(err,res)=>{
                if(err){
                    reject(err);
                    return;
                }else{
                    resolve(res);
                }
            });
        })
    })
}
```



##### 封装完整类

```js
// 根据需求自己扩展其他方法
class Db {
    // 单例模式多次实例化实例不共享的问题
    static getInstance(){
        if(!Db.instance){
            Db.instance = new Db();
        }
        return DB.instance;
    }

    constructor(){
        this.dbClient = null; //获得连接对象 这里只返回获取一次连接对象
        this.connect();
    }

    // 数据库连接
    connect(){
       ...
    }

    //查询
    find(){
        ...
    }

    //增加
    insert(){
        ...
    }

    //修改
    update(){
        ...
    }

    //删除
    remove(){
        ...
    }
    
    //获取ObjectId 
    getObjectId(id){
        return new ObjectID(id);
    }  
        
    ....
}
```



##### 测试性能

```js
//返回单例
var db1 = Db.getInstance();
var db2 = Db.getInstance();

//返回同一个连接对象
setTimeout(()=>{
    console.time("start1");
    db1.find("userinfo",{}).then((data)=>{
        console.timeEnd("start1");
    });
},100)

setTimeout(()=>{
    console.time("start2");
    db1.find("userinfo",{}).then((data)=>{
        console.timeEnd("start2");
    });
},3000)

setTimeout(()=>{
    console.time("start3");
    db2.find("userinfo",{}).then((data)=>{
        console.timeEnd("start3");
    });
},5000)

setTimeout(()=>{
    console.time("start4");
    db2.find("userinfo",{}).then((data)=>{
        console.timeEnd("start4");
    });
},7000)
```



#### Mongoose

##### 安装模块

```js
npm i mongoose --save
```



##### 引入模块

```js
const mongoose = require('mongoose')
```



##### 数据库连接

###### 初始化连接

```js
mongoose.connect('mongodb://localhost/test') //无密码
mongoose.connect('mongodb://test:happy365@localhost:27017/test') //有密码
```



###### 监听连接状态

```js
mongoose.connection.on("error", (err) => {
	console.log("数据库连接失败：" + err);
});

mongoose.connection.on("open", () => {
	console.log("数据库连接成功");
});
```



##### 结构骨架

###### 定义结构

```js
//Schema主要用于定义MongoDB中集合Collection里文档document的结构,可以理解为mongoose对表结构的定义(不仅仅可以定义文档的结构和属性，还可以定义文档的实例方法、静态模型方法、复合索引等)，每个schema会映射到mongodb中的一个collection，schema不具备操作数据库的能力

let stuSchema = mongoose.Schema({
    name:{type:String},
    age:{type:Number},
    sex:{type:String},
    hobby:{type:String}
})
```



###### 设置默认值

```js
let stuSchema = mongoose.Schema({
    name:{type:String},
    age:{
        	type:Number,
        	default:0
    },
    sex:{
        	type:String,
        	default:'男'
    },
    hobby:{type:String}
})
```



###### 数据格式化

1. 预定义模式修饰符

   ```js
   //lowercase、uppercase 、trim.....
   //mongoose 提供的预定义模式修饰符，可以对我们增加的数据进行一些格式化
   
   let stuSchema = mongoose.Schema({
       name:{
           type:String,
           trim:true //去除左右空格
       },
       age:{
           	type:Number,
           	default:0
       },
       sex:{
           	type:String,
           	default:'男'
       },
       hobby:{type:String}
   })
   ```

   

2. Getters 与 Setters 自定义修饰符

   ```js
   // Setters 修饰符在设置数据的时候对数据进行格式化
   let stuSchema = mongoose.Schema({
       name:{
           type:String,
           trim:true //去除左右空格
       },
       age:{
           	type:Number,
           	default:0
       },
       sex:{
           	type:String,
           	default:'男'
       },
       hobby:{
           type:String,
           set(hobby){
               if(!hobby) return hobby; //排除默认值情况
               if(hobby.indexOf("你的爱好是:")!=0){
                   hobby = "你的爱好是:" + hobby;
               }
               return hobby;
           }
       }
   })
   
   //Getters
   let stuSchema = mongoose.Schema({
       name:{
           type:String,
           trim:true //去除左右空格
       },
       age:{
           	type:Number,
           	default:0
       },
       sex:{
           	type:String,
           	default:'男'
       },
       hobby:{
           type:String,
           get(hobby){
               if(!hobby) return hobby; //排除默认值情况
               if(hobby.indexOf("你的爱好是:")!=0){
                   hobby = "你的爱好是:" + hobby;
               }
               return hobby;
           }
       }
   })
   
   //区别：set是数据在修改或者被赋值的时候进行逻辑操作返回处理后的数据存到数据库，能影响数据库的数据，get则影响不到数据库数据只是在获取数据的时候进行逻辑操作在返回处理后的值
   ```

   

###### 数据校验

1. 内置校验参数

   |   参数    |                          描述                          | 应用类型 |
   | :-------: | :----------------------------------------------------: | :------: |
   | required  |                  表示这个数据必须传入                  |   any    |
   |    max    |              用于 Number 类型数据，最大值              |  number  |
   |    min    |              用于 Number 类型数据，最小值              |  number  |
   |   enum    | 枚举类型，要求数据必须满足枚举值 enum: ['0', '1', '2'] |  string  |
   |   match   |         增加的数据必须符合 match（正则）的规则         |  string  |
   | maxlength |                         最大值                         |  string  |
   | minlength |                         最小值                         |  string  |

   ```js
   let stuSchema = mongoose.Schema({
       sid:{
           type:String,
           required:true //sid字段必须输入
       },
       name:{
           type:String,
           maxlength:20, //输入的最大name长度为:20 
           minlength:5, //输入的最小name长度:5
       },
       age:{
           type:Number,
           max:200, //输入的最大age为200
           min:0 //输入的最小age为0
       },
       sex:{
           type:String,
           enum:["男","女"] //输入的sex必须在这个枚举数组中
       },
       hobby:{
           type:String, 
           match:/^喜欢(.*)/ //输入的hobby开头必须包含喜欢
       }
   })
   ```

   

2. 自定义校验器

   ```js
   let stuSchema = mongoose.Schema({
       sid:{
           type:String,
           required:true
       },
       name:{
           type:String,
           maxlength:20,
           minlength:5,
       },
       age:{
           type:Number,
           // 自定义的验证器，如果通过验证返回 true，没有通过则返回 false
           validate:(age)=>{
               return age>0 && age<200;
           }
       },
       sex:{
           type:String,
           enum:["男","女"]
       },
       hobby:{
           type:String, 
           match:/^喜欢(.*)/
       }
   })
   ```




###### 建立索引

1. 基础索引

   ```js
   let stuSchema = mongoose.Schema({
       name:{
           type:String,
           index:true
       },
       age:{type:Number,default:0},
       sex:{type:String,default:'男'},
       hobby:{type:String}
   })
   ```

   

2. 唯一索引

   ```js
   let stuSchema = mongoose.Schema({
       name:{type:String},
       age:{type:Number,default:0},
       sex:{type:String,default:'男'},
       hobby:{
           type:String,
           unique: true
       }
   })
   ```

   

###### 扩展静态，实例方法

1. 扩展静态方法

   ```js
   stuSchema.statics.findBysid = function(sid,done){
       this.find(sid,function(err,docs){
           done(err,docs);
       })
   }
   ```

   

2. 扩展实例方法

   ```js
   stuSchema.methods.print = ()=>{
       console.log('这是一个实例方法');
   }
   ```

   

##### 模型运用

###### 生成模型

```js
//Model是由Schema编译而成的假想（fancy）构造器，具有抽象属性和行为。Model的每一个实例（instance）就是一个document，document可以保存到数据库和对数据库进行操作。简单说就是model是由schema生成的模型，可以对数据库的进行操作

//如果传入 2 个参数的话这个模型会和模型名称相同的复数的数据库建立连接

//如果传入 3 个参数的话:模型默认操作第三个参数定义的集合名称
let stuModel = mongoose.model("stu", stuSchema); //stus
let stuModel = mongoose.model("stu", stuSchema,"stuinfo"); //stuinfo
```



###### CRUD（增删改查）

```js
# 增
let stuEntity = new stuModel({
    name : "雪茹",
    age : 18,
    sex: "女",
    hobby: "旅游"
});

stuEntity.save((err,res)=>{
    if(err) return;
    console.log(res); 
    console.log("添加成功!");
})

# 删
stuModel.remove({name:"test"},(err,res)=>{
    if(err) return;
    console.log(res);
    console.log('删除成功!');
});

# 改
stuModel.updateOne({name:"zj"},{name:"张军"},(err,res)=>{
    if(err) return;
    console.log(res);
    console.log("修改成功!");
});

# 查
stuModel.find({},(err,docs)=>{
    if(err) return;
    console.log(docs);
})
```



###### 聚合管道（复杂查询）

```js
# 修改文档结构，可以用来重命名、增加或删除文档中的字段
stuModel.aggregate(
    [
    	{
    		$project:{ trade_no:1, all_price:1 }
    	}
	]
)

# 过滤文档，用法类似于 find() 方法中的参数
stuModel.aggregate(
    [
        {
        	$match:{"all_price":{$gte:90}}
        }
	]
)

# 文档分组，可用于统计结果 如果按照id分组需要_id 不能更改字段名
stuModel.aggregate(
    [
        {
        	$group: {_id: "$order_id", total: {$sum: "$num"}}
        }
    ]
)

# 文档排序 1升序 -1降序
stuModel.aggregate(
  	[
        {
			$sort:{"all_price":-1}
		}
	]
)

# 限制文档数量
stuModel.aggregate(
    [
        {
        	$project:{ trade_no:1, all_price:1 }
        },
        {
        	$limit:1
        }
	]
)

# 跳过文档数量
stuModel.aggregate(
    [
        {
        	$project:{ trade_no:1, all_price:1 }
        }, 
        {
        	$skip:1
        }
	]
)

# 多集合关联
stuModel.aggregate(
 [
        {
        	$lookup:
            {
                from: "order_item",
                localField: "order_id",
                foreignField: "order_id", 
                as: "items"
            }
        }
    ]
)
```



###### 多集合关联查询（多表关联查询）

1. 聚合管道关联多集合

   ```js
   ArticleModel.aggregate(
      [
         {
           $lookup: {
             from: "articlecate", //文章分类
             localField: "cid",
             foreignField: "_id",
             as: "cate"
           }
         },
         {
           $lookup: {
             from: "user", //用户
             localField: "author_id",
             foreignField: "_id",
             as: "user"
           }
         }
   	],
       (err,docs){
   	if(err) return;
           console.log(docs);
     		console.log(JSON.stringify(docs));
   	}
   )
   ```

   

2. populate多集合关联查询

   ```js
   let articleSchema = mongoose.Schema({
       title:{type:String},
       content:{type:String},
       thumb:{type:String},
       uid:{type:String,ref:'users'},
       ctime:{type:Date}
   })
   
   articleModel.find({}).populate("uid").exec((err,docs)=>{
       if(err) return;
       console.log(docs);
   })
   ```

   

##### 模块化

###### 配置项模块

```js
const config = {
    dbUrl: "mongodb://stu:happy365@localhost:27017/student",
    dbName: "student"
}

module.exports = config;
```



###### 数据库连接模块

```js
const mongoose = require('mongoose');
const config = require('./config');

mongoose.connect(config.dbUrl,{useNewUrlParser: true});

mongoose.connection.on("error", (err) => {
	console.log("数据库连接失败：" + err);
});

mongoose.connection.on("open", () => {
	console.log("数据库连接成功!");
});
```



###### 自定义模型模块

```js
const mongoose = require("mongoose");

let stuSchema = mongoose.Schema({
    name:{type:String},
    age:{type:Number,default:0},
    sex:{type:String,default:'男'},
    hobby:{type:String}
})

let StuModel = mongoose.model("stu", stuSchema,"stuinfo");

module.exports = StuModel;
```



###### 性能测试

```js
const db = require("./dbconnect");
const stuModel = require("./stuModel");
const userModel = require("./userModel");

setTimeout(()=>{
    console.time("t1");
    stuModel.find({},(err)=>{
        if(err) return;
        console.timeEnd("t1");
    })
},300)

setTimeout(()=>{
    console.time("t2");
    stuModel.find({},(err)=>{
        if(err) return;
        console.timeEnd("t2");
    })
},2000);

setTimeout(()=>{
    console.time("t3");
    userModel.find({},(err)=>{
        if(err) return;
        console.timeEnd("t3");
    })
},5000);

setTimeout(()=>{
    console.time("t4");
    userModel.find({},(err)=>{
        if(err) return;
        console.timeEnd("t4");
    })
},7000);
```



### 专题系列

#### RESTful API

##### 认识RESTful API

##### RESTful API最佳实践

###### 设计指南

- 协议：建议使用更安全的 https 协议
- 域名：尽量部署在专属域名下面，比如 https://api.bestzhangjun.com
- 应该将 api 的版本号放入 URl 中，比如 https://api1.bestzhangjun.com
- API 中的名词也应该使用复数
- http 请求数据的方式的支持
- 过滤方式、请求数据方式、返回数据、安全



###### Json接口

express版本

```js
const express = require('express')
const Db = require('./dbModel/db')
const app = express()
const db = new Db()

app.get('/api', async (req,res,next)=>{
  const res = await db.find('stuinfo',{})
  res.json(res)
})

app.listen(3000,()=>{
  console.log("this serve is running....");
})
```

koa2.x版本

```js
const koa = require("koa")
const Router = require("koa-router")
const Db = require('./dbModel/db')
const app = new koa()
const router = new Router()
const db = new Db()

router.get('/api', async (ctx,next)=>{
  let result = await db.find('stuinfo',{})
  ctx.body = result
})

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(3000,()=>{
  console.log("this serve is running....");
})
```



###### Jsonp接口

express版本

```js
const express = require('express')
const Db = require('./dbModel/db')
const app = express()
const db = new Db()

app.get('/api', async (req,res,next)=>{
  let arr = await db.find('stuinfo',{});
  res.jsonp(arr)
})

app.listen(3000,()=>{
  console.log("this serve is running....")
})
```

koa2.x版本

```js
const koa = require("koa")
const Router = require("koa-router")
const Db = require('./dbModel/db')
const jsonp = require('koa-jsonp')
const app = new koa()
const router = new Router()
const db = new Db()

app.use(jsonp())

router.get('/api', async (ctx,next)=>{
  let result = await db.find('stuinfo',{})
  ctx.body = result
})

app
  .use(router.routes())
  .use(router.allowedMethods())

app.listen(3000,()=>{
  console.log("this serve is running....")
})
```



###### 跨域问题

express版本

```js
const express = require('express')
const Db = require('./dbModel/db')
let app = express()
let db = new Db()

app.use('*', (req, res, next) => {
    res.header('Access-Control-Allow-Origin', '*')
    res.header('Access-Control-Allow-Headers', 'Content-Type')
    res.header('Access-Control-Allow-Methods', '*')
    res.header('Content-Type', 'application/json;charset=utf-8')
    next()
})

app.get('/api', async (req,res,next)=>{
  let arr = await db.find('stuinfo',{})
  res.json(arr)
})

app.listen(3000,()=>{
  console.log("this serve is running....")
})
```

koa2.x版本

```js
const koa = require("koa")
const Router = require("koa-router")
const Db = require('./dbModel/db')
const cors = require('koa2-cors')

const app = new koa()
const router = new Router()
const db = new Db()

app.use(cors({
    origin: ['http://localhost:9528'],
    credentials: true
}))

router.get('/api', async (ctx,next)=>{
  let result = await db.find('stuinfo',{})
  ctx.body = result
})

app
  .use(router.routes())
  .use(router.allowedMethods());

app.listen(3000,()=>{
  console.log("this serve is running....")
})
```



##### 用户的认证与授权



##### 仿知乎API 设计定制

###### 个人资料模块

###### 关注与粉丝模块

###### 话题模块

###### 问题模块

###### 答案模块



#### JWT

##### Node.js使用jwt

###### 生成token

###### 获取token

###### 验证token



##### Axios访问基于Jwt的接口

###### 单独访问添加token

###### 拦截器添加token



##### token过期处理

###### 前端自动刷新

###### 后台进行判断



#### 异步IO 模型

#### 爬虫（Pupeteer）

#### 调试（debug）

#### 测试（Test）



















​	