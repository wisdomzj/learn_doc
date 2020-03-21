# AJAX 学习总结（仅供参考）

### 快速上手

#### 工作流程

```js
//1.相当安装浏览器（用户代理）
var xhr = new XMLHttpRequest();  //xhr对象发送请求接受响应

//2.相当于打开浏览器,输入网址
xhr.open('GET','http://localhost/ajax/time.php'); //打开网址之间的连接

//3.相当于敲回车
xhr.send(); //通过连接发送请求

//4.等待响应 (不只是在响应时触发除此之外状态改变就触发)
xhr.onreadystatechange = function(){ 
    
    //5.看结果
    if(this.readyState !== 4) return;
    console.log(this.responseText);
}
```



#### readyState 状态变化

| readyState |     状态描述     |                         说明                          |
| :--------: | :--------------: | :---------------------------------------------------: |
|     0      |      UNSENT      |      代理（XHR）被创建，但尚未调用 open() 方法。      |
|     1      |      OPENED      |          open() 方法已经被调用，建立了连接。          |
|     2      | HEADERS_RECEIVED | send() 方法已经被调用并且已经可以获取状态行和响应头。 |
|     3      |     LOADING      |  响应体下载中responseText 属性可能已经包含部分数据。  |
|     4      |       DONE       |     响应体下载完成，可以直接使用 responseText 。      |

```js
//1.相当安装浏览器（用户代理）
var xhr = new XMLHttpRequest();  //xhr对象发送请求接受响应
console.log(xhr.readyState); ==>0 //代理（XHR）被创建，但尚未调用open() 方法

//2.相当于打开浏览器,输入网址
xhr.open('GET','http://localhost/ajax/time.php'); //打开网址之间的连接
console.log(xhr.readyState); ==>1 //open()方法被调用

//3.相当于敲回车
xhr.send(); //通过连接发送请求
console.log(xhr.readyState); ==>1 //建立连接

//4.等待响应 (不只是在响应时触发除此之外状态改变就触发)
xhr.onreadystatechange = function(){
    
    //5.看结果
    //if(this.readyState !== 4) return;
    //console.log(this.responseText);
    
    switch(this.readyState){
        case 2:
        console.log(this.readyState) ==>2 //获取到响应头
        //console.log(this.getAllResponseHeaders())
        //console.log(this.getResponseHeader('server')) 
        break;
        case 3:
        console.log(this.readyState) ==>3 //响应体下载中,可能也不完整
        //console.log(this.responseText);
        break;
        case 4:
        console.log(this.readyState) ==>4 //响应报文下载完成
        //console.log(this.responseText);
        break;
    }
}
```



#### 遵守http协议

本质上 XMLHttpRequest 就是 JavaScript 在 Web 平台中发送 HTTP 请求的手段，所以我们发送出去的请求任然是HTTP 请求，同样符合 HTTP 约定的格式

```js
var xhr = new XMLHttpRequest();
xhr.open('POST','http://localhost/ajax/time.php'); //设置请求地址

xhr.setRequestHeader('foo','bar'); //设置一个请求头

xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded') ;//我们请求头的Content-Type格式随着请求报文的请求体的格式变化而变化

xhr.send('key=value'); //设置请求体  

xhr.onreadystatechange = function(){
    if(this.readyState == 4){
        console.log(this.status) //获取响应体状态码
        console.log(this.statusText) //获取响应状态描述
        
      	//获取响应头
        console.log(this.getResponseHeader('server')) //获取指定响应头
        console.log(this.getAllResponseHeaders()) //获取全部响应头信息
        
        //获取响应体
        console.log(this.responseXML) //获取响应体xml形式 可以不用了解即可
        console.log(this.responseText) //获取响应体文本形式
        console.log(this.response); //获取响应体
    }
}
```



### 具体用法

#### 接口（api）

对于返回数据的地址一般我们称之为接口又可以叫api（形式上是 Web 形式）

```php
//用于测试 user.php
$data = array(
    array(
        'id' => 1,
        'name' => '张军',
        'age' => 23
    ),
    array(
        'id' => 2,
        'name' => '张小军',
        'age' => 19
    ),
    array(
        'id' => 3,
        'name' => '雪茹',
        'age' => 18
    )
);

if(empty($_GET['id'])){
    //因为 HTTP 中约定报文的内容就是字符串，而我们需要传递给客户端的信息是一个有结构的数据这种情况下我们一般采用 JSON 作为数据格式。
    echo json_encode($data);
}else{
    foreach($data as $item){
        if($item['id'] != $_GET['id']) continue;
        echo json_encode($item);
    }
}

===================================================================================

//用于测试 login.php   
if(empty($_POST['username'])||empty($_POST['password'])){
	exit("请提交用户名或密码");
}

$usrname = $_POST['username'];
$password = $_POST['password']; 

if($usrname == 'root' && $password == '1226'){
    exit('登陆成功');
}

exit('用户名或密码错误!!!');

===================================================================================
    
//用于测试 xml.php
<?xml version="1.1" encoding="utf-8"?>
<person>
  <name>雪茹</name>
  <age>18</age>
  <sex>女神</sex>
</person>   
```



#### GET请求

通常在一次 GET 请求过程中，参数传递都是通过 URL 地址中的 ? 参数传递

```js
var xhr = new XMLHttpRequest();
xhr.open('GET','http://localhost/ajax/user.php?id=3');
xhr.send(null);
xhr.onreadystatechange = function(){
    if(this.readyState != 4) return;
    console.log(this.responseText);
}
```



#### POST请求

POST 请求过程中，都是采用请求体承载需要提交的数据

```js
xhr.open('post','http://localhost/ajax/login.php');
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
xhr.send('username=' + username + '&password=' + password);
//xhr.send(`username=${username}&password=${password}`)
xhr.onreadystatechange = function(){
    if(this.readyState !=4 ) return;
    console.log(this.responseText);
}
```



#### 异步&同步

> 同步：一个人在同一个时刻只能做一件事情，在执行一些耗时的操作不去做别的事，只是等待。
> 异步：在执行一些耗时的操作去做别的事，而不是等待。
>
> 区别：
>
> （1）在于 send 方法会不会出现等待情况 同步模式 ajax 操作会有楞等的情况
>
> （2）异步情况下拿不到响应内容，同步等send方法发出请求完成的情况下，能拿到响应内容
>
> （3）onreadystatechange事件的注册时间前后获取状态码也有区别  一定在发送请求 send() 之前注册 readystatechange （不管同步或者异步）为了让这个事件可以更加可靠（一定触发），一定是先注册

```js
//异步 open 方法的第三个参数是 async 可以传入一个布尔值true。
console.time('async');
var xhr_async = new XMLHttpRequest();
xhr_async.open('GET','./time.php',true);
xhr_async.send(null);
console.log(xhr_async.responseText); //空值
console.log(xhr_async.readyState); //1
xhr_async.onreadystatechange = function(){
    console.log(this.readyState); //2,3,4 
}
console.timeEnd('async'); //1.062255859375ms

//同步 open 方法的第三个参数是 async 可以传入一个布尔值false
console.time('sync');
var xhr_sync = new XMLHttpRequest();
xhr_sync.open('GET','./time.php',false);
xhr_sync.send(null);
console.log(xhr_sync.responseText); //响应内容
console.log(xhr_sync.readyState); //4
xhr_sync.onreadystatechange = function(){
    console.log(this.readyState); //空值  
}
console.timeEnd('sync'); //9.89306640625ms
```



#### 响应数据格式

>  XML ：一种数据描述手段老掉牙的东西，简单演示一下，不在这里浪费时间，基本现在的项目不用了
> 淘汰的原因,数据冗余太多
>
>  JSON ：也是一种数据描述手段，类似于 JavaScript 字面量方式服务端采用 JSON 格式返回数据，客户端按照 JSON 格式解析数据
>
> 不管是 JSON 也好，还是 XML，只是在 AJAX 请求过程中用到，并不代表它们之间有必然的联系，它们只是 数据协议罢了



```js
//xml
var xhr_xml = new XMLHttpRequest();
xhr_xml.open('GET','./xml.php',true);
xhr_xml.send(null);
xhr_xml.onreadystatechange = function(){
    if(this.readyState!=4) return;
    console.log(this.responseXML.documentElement.children[0].innerHTML); //操作document
}
   
//json
var xhr_json = new XMLHttpRequest();
xhr_json.open('GET','./user.php',true);
xhr_json.send(null);
xhr_json.onreadystatechange = function(){
    if(this.readyState!=4) return;
    console.log(JSON.parse(this.responseText)[0].age); //操作对象
}
```



#### 兼容方案

XMLHttpRequest 在老版本浏览器（IE5/6）中有兼容问题，可以通过另外一种方式代替

```js
var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP'
```



### 原生封装

#### 版本一

基础请求

```js
function ajax (methods,url,params){
    var xhr = new XMLHttpRequest();
    xhr.open(methods,url);
    xhr.send(params);
    xhr.onreadystatechange = function(){
        if(this.readyState != 4) return;
        console.log(this.responseText);
    }
}
```



####  版本二

根据参数不同参数呈现不同的传送方式 ，改进 可以选择get post方式

```js
function ajax (methods,url,params){
    var xhr = new XMLHttpRequest();
    methods = menubar.toUppercase();
    var date = null;
    if(methods == 'GET'){
        url += '?'+params;
    }
    xhr.open(methods,url);
    if(methods == 'POST'){
        xhr.setRequestHeader('Content-Type','application/x-www-from-urlencoded');
        date = params;
    }
    xhr.send(date);
    xhr.onreadystatechange = function(){
        if(this.readyState != 4) return;
        console.log(this.responseText);
    }
}
```



#### 版本三

继续改进 当我们传进去的参数为对象时的情况

```js
function ajax (methods,url,params){
    var date = null;
    methods = methods.toUpperCase();
    var xhr = new XMLHttpRequest();
    if(typeof params == 'object'){
        var temarr = [];
        for(var key in params){
            var value = params[key];
            temarr.push(key+'='+value);
        }
        params = temarr.join('&');
    }
    if(methods == 'GET'){
        url += '?' + params;
    }
    xhr.open(methods,url);
    if(methods == 'POST'){
        xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
        date = params;
    }
    xhr.send(date);
    xhr.onreadystatechange = function(){
        if(this.readyState != 4) return;
        console.log(this.responseText);
    }
}
```



#### 版本四

思考我们异步回来的返回值怎么处理？封装者不可太主观的认为是打印到控制台，如果直接返回也是拿不到结果，封装函数里面还有一个函数此时的返回结果是内部函数的返回值。我们的处理方式是委托一个函数帮我们完成，调用者直接把拿到结果的处理方式告诉封装者，封装者采用回调函数的方式同样能达到效果，将响应的内容也能呈现给我们

```js
function ajax (methods,url,params,done){
    var date = null;
    methods = methods.toUpperCase();
    var xhr = new XMLHttpRequest();
    if(typeof params == 'object'){
        var temarr = [];
        for(var key in params){
            var value = params[key];
            temarr.push(key+'='+value);
        }
        params = temarr.join('&');
    }
    if(methods == 'GET'){
        url += '?' + params;
    }
    xhr.open(methods,url);
    if(methods == 'POST'){
        xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
        date = params;
    }
    xhr.send(date);
    xhr.onreadystatechange = function(){
        if(this.readyState != 4) return;
        done(this.responseText);
    }
}
```



### Jq中的ajax使用

#### 底层接口

```js
$.ajax({
    // 请求地址
    url:'./user.php',
    // 请求方式
    type:'get',
    // 需要传递到服务端的数据，如果 GET 则通过 URL 传递，如果 POST 则通过请求体传递 
    data:{id:3},
    // 请求体内容类型，默认 application/x-www-form-urlencoded 
    contentType:'x-www-form-application/urlencoded',
    // 服务端响应体数据类型
    dataType:'json',
    // 请求发起之前触发 
    beforeSend:function(){
        console.log('requset beforeSend');
    },
    // 请求成功之后触发
    success:function(res){
        console.log(res.name);
    },
    // 请求失败触发 
    error:function(err){
        console.log('错误信息:'+err);
    },
    // 请求完成触发（不管成功与否）
    complete:function(){
        console.log('requset complete');
    }
})
```



#### 快捷方法

```js
//get
$.get('./user.php',{id:3},res=>{
    console.log(res);
});

//post
$.post('./login.php',{name:username,pwd:password},res=>{
    console.log(res);
});
```



#### 全局事件

```js
//当我们需要发很多的ajax请求时我们每个ajax肯定会有部分相同的操作那我们把这些相同的操作统一放到全局事件中这样减少代码的冗余，提高复用性。

$(document).ajaxStart(function(){
    NProgress.start();
}).ajaxStop(function(){
    NProgress.done();
})
```



#### 局部刷新

```js
$('.list-group-item').on('click',function(){
    var url = $(this).attr('href');
    $('#main').load(url+' #main>*'); //注意有个空格
    return false;
})
```



###  vue-resourse使用

#### get

```js
this.$http.get('../user.php',{
     //参数项 
     params:{
         id:3
     },
     //设置请求头
     headers:{
         toke:'test'
     }
 }).then(res => {
     // 请求成功的回调
     console.log(res.body); 
 },err => {
     // 请求失败的回调
     console.log(err);
 });
```



#### post

```js
this.$http.post('../login.php',
    // 参数项
    {
        name:this.name,
        pwd:this.pwd
    },          
    // 这里需要注意下（post的data默认是以form data的形式，如果我们没走表单体提交则是request payload格式，尽管参数已经提交，但是服务端无法拿到传递过来的参数。我们设置emulateJSON属性为true将我们的data以form data形式提交）
    {
        emulateJSON:true
    }).then(res=>{
    // 请求成功的回调
    this.msg = res.body; 
},err => {
    // 请求失败的回调
    this.msg = err;
})
```



#### http

```js
// post方式传递参数有问题（已经设置了emulateJSON属性不管用）没查出来什么原因
this.$http({
    //url:'./login.php',
    //method:'post',
    url:'./user.php',
    method:'get',
    // data:{
    //     name:123,
    //     pwd:123
    // },
    params:{
        id:3
    },
    //emulateJSON:true
}).then(res=>{
    this.msg = res.body; 
},err=>{
    this.msg = err;
})
```



#### jsonp

```js
this.$http.jsonp('../user_02.php',{
    //参数项
    params:{
    	id:1
    }
}).then(res=>{
    // 请求成功的回调
	this.msg = res.data;
},err=>{
    // 请求失败的回调
	this.msg = err;
})
```



#### 全局拦截器

```js
// 一般用在loading 比较多
Vue.http.interceptors.push(function(req,next){
    NProgress.start();
    //请求完成后的回转
    next(function(res){
        NProgress.done();
        return res;
    })
})
```



### axios使用

#### get

```js
axios.get('./user.php',{
    //参数配置项 
    params:{
        id:3
    },
    //设置请求头
    headers:{
        token:'test'
    }
}).then(res => {
    // 请求成功的回调
    this.msg = res.data; 
}).catch(err => {
    // 请求失败的回调
    this.msg = err;
})
```



#### post

```js
axios.post('./login.php',
    // 需要和qs进行配合将数据格式进行序列化
    Qs.stringify({
    	name:this.name,
        pwd:this.pwd
	}),
    {
    //设置请求头
    	headers:{
        	token:'wisdom'
    	}
	}).then(res=>{
    	// 请求成功的回调
    	this.msg = res.data;
	}).catch(err=>{
    	// 请求失败的回调
    	this.msg = err;
	})
}
```



#### http

```js
 axios({
     // 接口地址
     url:'./login.php',
     url:'./user.php',
     // 传递方式
     method:'post',
     method:'get',
     //post需要qs序列化数据格式 同时需要用data属性
     data:Qs.stringify({
         name:this.name,
         pwd:this.pwd
     }),
     //get需要用params属性
     params:{
         id:1
     },
     //设置头部
     headers:{
         token:'smart'
     }
 }).then(res=>{
     // 请求成功的回调
     this.msg = res.data;
 }).catch(err=>{
     // 请求失败的回调
     this.msg = err
 })
```



#### 全局拦截器

```js
// 一般用在loading 比较多

// 发出请求之前拦截
axios.interceptors.request.use(req => {
    NProgress.start();
    return req;
});

// 请求完成之后拦截
axios.interceptors.response.use(res => {
    NProgress.done();
    return res;
})
```



### 跨域认识

#### 同源概念

同源策略是浏览器的一种安全策略，所谓同源是指域名，协议，端口完全相同，只有同源的地址才可以相互通过 AJAX 的方式请求，一个对比地址 ：http://www.bestzhangjun.com/test.html 

|                  对比地址                   | 是否同源 |        原因         |
| :-----------------------------------------: | :------: | :-----------------: |
|   http://demo.bestzhangjun.com/test.html    |  不同源  |     域名不一样      |
|   https://www.bestzhangjun.com/test.html    |  不同源  |     协议不一样      |
| http://www.bestzhangjun.com:8080/test.html  |  不同源  |    端口号不一样     |
| http://demo.bestzhangjun.com:8080/test.html |  不同源  |  端口，域名不一样   |
| https://www.bestzhangjun.com:8080/test.html |  不同源  |  端口，协议不一样   |
| http://demo.bestzhangjun.com:8080/test.html |  不同源  |  域名，协议不一样   |
|    http://www.bestzhangjun.com/task.html    |   同源   | 同域名 ，端口，协议 |



#### 解决方案

我们先测试 img link script 这些能发出请求的标签的效果

```js
//Access to XMLHttpRequest at 'http://demo.bestzhangjun.com/test/user_01.php?id=3' from origin 'http://localhost' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.

//提示需要在服务端响应头加上Control-Allow-Origin 使用ajax失败
 ajax('get','http://demo.bestzhangjun.com/test/user_01.php',{id:3},res=>{
     console.log(r);
 })

================================================================================

// 确实能发出请求，能接受到响应的数据，无法获取。 希望返回的是二进制数据而服务端给我们的是json数据
var img = new Image();
img.src = 'http://demo.bestzhangjun.com/test/user_01.php';

================================================================================
    
// Resource interpreted as Stylesheet but transferred with MIME type application/json: "http://demo.bestzhangjun.com/test/user_01.php".

// 确实能发出请求，能接受到响应的数据，无法获取。 希望返回的是rel指定的文件格式数据而服务端给我们的是json数据
var link = document.createElement('link');
link.rel = 'stylesheet';
link.href = 'http://demo.bestzhangjun.com/test/user_01.php';
document.body.appendChild(link);

================================================================================

// GET http://demo.bestzhangjun.com/test/user_01.php net::ERR_ABORTED 408 (Request Timeout)

// 确实能发出请求，能接受到响应的数据，无法获取，希望返回的是能执行的js代码而服务端给我们的是不能执行的js的代码。
var script = document.createElement('script');
script.src = 'http://demo.bestzhangjun.com/test/user_01.php';
document.body.appendChild(script);
```



讨论后的解决方案（jsonp）

由于 XMLHttpRequest 无法发送不同源地址之间的跨域请求，所以我们必须要另寻他法，script 这种方 案就是我们终选择的方式，我们把这种方式称之为 JSONP，JSONP 需要服务端配合，服务端按照客户端的要求返回一段 JavaScript 调用客户端的函数 JSONP 只能发送 GET 请求，JSONP 用的是 script 标签，和 AJAX 提供的 XMLHttpRequest 没有任何关系

```js
//原生jsonp的封装
function jsonp(url,params,callback){
    var func_name = 'jsonp_'+Date.now()+Math.random().toString().substr(2,5)
    var head = document.getElementsByTagName('head')[0]
    var extendurl = url+'?'+params+'&callback='+func_name
    var script = document.createElement('script');
    if(typeof params === 'object'){
        var temarr = [];
        for(key in params){
            var value = params[key];
            temarr.push(key+'='+value);
        }
        params = temarr.join('&');
    }
    window[func_name] = function(data){
        callback(data);
        delete window[func_name];
        head.remove('script'); 
    }
    script.setAttribute('src', extendurl);
    head.appendChild(script); 
}

jsonp('http://demo.bestzhangjun.com/test/user_02.php',{id:3},res=>{
    console.log(res);
});

//jqery中的jsonp
 $.ajax({
     type:'GET', //提交方式
     url:'http://demo.bestzhangjun.com/test/user_02.php', //接口地址
     data:{id:3},//参数
     jsonp:'callback',//默认callback 需要传给服务端的回调键(类似key)
     jsonpCallback:'successcallback', //需要传给服务端的回调值(类似value)
     dataType:'jsonp', //注意这里必须设置jsonp（如果是jsonp跨域方式）
     success:function(r){ //客户端已经得到响应数据的回调
         console.log(r);
     }
 })

//vue-resourse中的jsonp
this.$http.jsonp('../user_02.php',{
    params:{
    	id:1
    }
}).then(res=>{
	this.msg = res.data;
},err=>{
	this.msg = err.data;
})
```



####  后端设置允许跨域（cros）

jspon 是以前的解决方法，现在只要在服务端设置响应头Control-Allow-Origin：* 即可，允许跨域请求。Cross Origin Resource Share，跨域资源共享，这种方案无需客户端作出任何变化（客户端不用改代码），只是在被请求的服务端响应的时候添加一个 AccessControl-Allow-Origin 的响应头，表示这个资源是否允许指定域请求

```js
header('Access‐Control‐Allow‐Origin: *');
```



