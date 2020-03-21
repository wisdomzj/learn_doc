# es6学习总结

[toc]

### 搭建环境

#### 安装webpack

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



#### es6转换配置方案

##### 安装loader

```shell
$ npm install babel-loader @babel/core --save-dev
$ npm  install @babel/preset-env --save-dev
$ npm install @babel/polyfill --save
```



##### preset-env转换语法

```js
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
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### polyfill 补充缺少语法

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



##### 按需引进语法配置

```js
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
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### presets其他配置

```js
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
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    }
}
```



##### .babelrc配置文件

```js
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



### 核心学习

#### 新式声明方式

##### 作用域

###### 全局作用域

变量在函数或者代码块{ } 外定义，即为全局作用域，不过在函数或者代码块{ } 内未定义的变量也是拥有全局作用域

```js
var name = 'zhangjun' 

function Myfunc(){
    carName = 'wisdomzj'
    console.log(name)
}

Myfunc()
console.log(carName)
```



在函数内部或代码中没有定义的变量实际上是作为window/global的属性存在，而不是全局变量，没有使用var定义的变量虽然拥有全局作用域但是它能被delete掉的，而全局变量不可以

```js
var aaa = '123'
delete aaa //false

bbb = '123'
delete bbb //true
```



###### 函数作用域

在函数内部定义的变量，就是局部作用域，函数作用域内，对外是封闭的，对外层的作用域无法直接访问函数内部的作用域！

```js
function Myfunc(){
    var value = 'test' 
}

console.log(value)  //Uncaught ReferenceError: value is not defined
```



使用return读取函数内部变量

```js
function Myfunc(){
    var value = 'test'
    return value
}

console.log(Myfunc())
```



使用闭包读取函数内部变量

```js
function Myfunc(){
    var value = 'test'
    function Inner(){
        return value
    }
    return Inner()
}

console.log(Myfunc())
```



###### 块级作用域

{ } 一对花括号就可以理解为块级作用

```js
if(true){
   let a = 1
   console.log(a)
}

{
    let b = 2
    console.log(b)
}

console.log(a,b) 
```



##### 块级作用域变量

###### 变量定义

在 ES6之前，我们都是用 var 来声明变量，而且 JS 只有函数作用域和全局作用域，没有块级作用域，所以`{}`限定不了 var 声明变量的访问范围

```js
function Func(){
    var a = 3
    if(a === 3){
       var b = 6 // 不能封锁数据在块中被访问
       let c = 6 // 能数据封锁智在块中被访问
    }
    console.log(b) // 4
    console.log(c) // ReferenceError: c is not defined
}

====================================================
    
{
    var a = 123
    let b = 123
    const c = 123 //初始化必须进行赋值否则报错
}
console.log(a); // 123
console.log(b); // ReferenceError: b is not defined
console.log(c); // ReferenceError: c is not defined
```



###### 禁止声明之前访问

用`let`声明的变量，不存在变量提升，而且要求必须 等`let`声明语句执行完之后，变量才能使用，否则报错，暂时性死区==>当前作用域顶部到该变量声明位置中间的部分都是死区

```js
{
    console.log(a) // Uncaught ReferenceError: Cannot access 'a' before initialization
    let a = 123 
}

let s = 1
if(true){
    console.log(s) // Uncaught ReferenceError: Cannot access 's' before initialization
    let s = 2
}
```



###### 不能重复声明

let&const 不允许在相同作用域内，重复声明同一个变量，否则报错

```js
{
    let a = '123'
    let a = '345'
    console.log(a) // SyntaxError: redeclaration of let a
    
    const  b = '123'
    const  b = '345'
    console.log(b) // SyntaxError: redeclaration of const b
} 
```



###### 不进行变量提升

 `var`命令会发生“变量提升”现象，即变量可以在声明之前使用，值为`undefined`，这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用 

```js
// var 的情况
console.log(foo); // undefined
var foo = 2;

// let 的情况
console.log(bar); // ReferenceError
let bar = 2;
```



###### 不属于顶层对象的属性

为了保持兼容性，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性；let 命令、const 命令、class 命令声明的全局变量，不属于顶层对象的属性

```js
 {
     let a = 123
     console.log(window.a) //undefined 
     
     const b = 123
     console.log(window.b) //undefined
     
     var c = 123
     console.log(window.c) //123
 }
```



#### 新式数据类型

##### 声明Symbol

######  Symbol函数

 ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：`undefined`、`null`、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object） 

```js
let s1 = Symbol()
let s2 = Symbol()
let s3 = Symbol()

console.log(s1 === s2) //false

let n1 = 123
let n2 = 123

console.log(n1 === n2) //true

console.log(typeof s3)  // "symbol"
```



######  Symbol描述

`Symbol`函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分 

```js
let s1 = Symbol('foo')
let s2 = Symbol('bar')
let s3 = Symbol('foo') //描述相同,但返回的值是不一样

console.log(s1 === s3) //false
console.log(s1) // Symbol(foo)
console.log(s2) // Symbol(bar)
console.log(s3) // Symbol(foo)
```



###### Symbol转换

symbol也是可以进行一定的转换

```js
let sym = Symbol('My symbol')

# 转换字符串
console.log(String(sym))  // 'Symbol(My symbol)'
console.log(sym.toString()) // 'Symbol(My symbol)'

# 转换布尔
console.log(Boolean(sym))  // true
console.log(!sym)  // false

# 转换数值??
console.log(Number(sym)) // Uncaught TypeError: Cannot convert a Symbol value to a number
console.log(sym + 2) // Uncaught TypeError: Cannot convert a Symbol value to a number
```



##### Symbol的Api

###### for

需要使用同一个Symbol 值的时候被使用到，它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值，如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局 

```js
let s1 = Symbol.for('foo')
let s2 = Symbol.for('foo')

console.log(s1 === s2) // true
```



###### keyfor

 `Symbol.keyFor()`方法返回一个已登记的 Symbol 类型值的`key` 

 ```js
let s1 = Symbol.for("foo")
console.log(Symbol.keyFor(s1)) // "foo"

let s2 = Symbol("foo")
console.log(Symbol.keyFor(s2)) // undefined
 ```



##### 应用场景

###### 作为属性名

由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性，这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖 

```js
let sym = Symbol() 
let obj = {
    [sym]:'123',
    'a':'345',
    'b':'678'
}

console.log('obj==>',obj)
```



###### 遍历属性名

 Symbol 作为属性名，遍历对象的时候，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回 

```js
let sym = Symbol() 
let obj = {
    [sym]:'123',
    'a':'345',
    'b':'678'
}

# 拿到非Symbol属性名的情况
for(let [key,value] of Object.entries(obj)){
    console.log(key,value)
}

# 只拿到Symbol属性名的情况
Object.getOwnPropertySymbols(obj).forEach((item)=>{
    console.log(item,obj[item])
})

# 拿到全部属性名的情况
Reflect.ownKeys(obj).forEach((item)=>{
    console.log(item,obj[item])	
})
```



#### 新式数据结构

##### Set

###### set实例

初始化大的参数必须式可遍历的，可以是数组或者自定义遍历的数据结构

```js
let s1 = new Set() //未初始化
let s2 = new Set([1,2,3,4]) //初始化
console.log(s1,s2)
```



###### 添加数据

set数据结构不允许数据重复，所以添加重复的数据是无效的

```js
let s = new Set()
s.add('hello')
s.add('goodbye')

s.add('html').add('css') // 可以进行链式调用
s.add('es6').add('es6') // 无效，只添加重复中的一个

console.log(s)
```



###### 删除数据

可以删除指定的数据，删除全部数据

```js
let s = new Set()
s.add('hello')
s.add('goodbye')
s.add('es6')
console.log(s)

s.delete('hello') //删除特定
console.log(s)

s.clear() //清空全部
console.log(s)
```



###### 数据遍历

`keys`键名的遍历器，`values`键值的遍历器，`entries` 键值对的遍历器

```js
let s = new Set()
s.add('hello')
s.add('goodbye')

console.log(s.keys()) 
console.log(s.values())
console.log(s.entries())

s.forEach((item)=>{
    console.log(item)
})

for(let item of s){
    console.log(item)
}
```



###### 统计数据

set 可以快速进行统计数据，如数据是否存在，数据的总数

```js
let s = new Set()
s.add('hello')
s.add('goodbye')

console.log(s.has('hello')) // true
console.log(s.size) // 2
```



##### Map

###### map实例

iterable 可以是一个数组或或者iterable对象，其元素为键值对（两个元素的数组），每个键值对都会添加到新的map

```js
let m1 = new Map([]) // 未初始化
let m2 = new Map([[1,'1'],[2.'2']]) // 初始化
console.log(m1,m2)
```



###### 添加数据

与对象不同，键值可以添加任意值，对象，数值，函数... 

```js
let m = new Map([])
let keyObj = {}
let keyFunc = function(){}
let keyString = 'a string'

m.set(keyObj,'和键 keyObj 关联的值')
m.set(keyFunc,'和键 keyFunc 关联的值')
m.set(keyString,'和键 keyString 关联的值')
console.log(m)
```



###### 删除数据

可以删除指定的数据，删除全部数据

```js
let m = new Map([])
let keyObj = {}
let keyFunc = function(){}
let keyString = 'a string'

m.set(keyObj,'和键 keyObj 关联的值')
m.set(keyFunc,'和键 keyFunc 关联的值')
m.set(keyString,'和键 keyString 关联的值')
console.log(m)

m.delete(keyFunc) //删除特定
console.log(m)

m.clear() //清空全部
console.log(m)
```



###### 数据遍历

`keys`键名的遍历器，`values`键值的遍历器，`entries` 键值对的遍历器，`get` 返回某个map对象中的一个指定元素

```js
let m = new Map([])
let keyObj = {}
let keyFunc = function(){}
let keyString = 'a string'

map.set(keyObj,'和键 keyObj 关联的值')
map.set(keyFunc,'和键 keyFunc 关联的值')
map.set(keyString,'和键 keyString 关联的值')

console.log(m.get(keyString))
console.log(m.keys())
console.log(m.values())
console.log(m.entries())

m.forEach((value,key,item)=>{
    console.log(value,key,item)
})

for(let [key,value] of m){
    console.log(key,value)
}
```



###### 统计数据

map 可以快速进行统计数据，如数据是否存在，数据的总数

```js
let m = new Map([])
let keyObj = {}
let keyFunc = function(){}
let keyString = 'a string'

map.set(keyObj,'和键 keyObj 关联的值')
map.set(keyFunc,'和键 keyFunc 关联的值')
map.set(keyString,'和键 keyString 关联的值')

console.log(m.has(keyObj)) // true
console.log(m.size) // 3
```



#### 数组扩展

##### 数组初始化

###### of()

 将一组值转换成数组，进行数组初始化

```js
let arr1 = Array.of(1,2,3,4,5)
let arr2 = Array.of(1)
console.log(arr1,arr2)
```



###### fill()

 使用给定的值，填充一个数组，填充完后会改变原数组

```js
let arr1 = Array(5).fill(1)
let arr2 = [1,2,3,4,5].fill(8,2,4)
console.log(arr1,arr2)
```



###### from()

 将伪数组变成数组，有length属性的可以转成数组

```js
let arr = Array.from({ length:5 }, () => {
	return 1
})
console.log(arr)
```



##### 数组遍历

###### for... of（es6）

```js
let arr = [1,2,3,4,5]
for(let item of arr){
    console.log(item)
}
```



###### map（es5）

map和forEach等遍历方法不同，在forEach中return语句是没有任何效果的，而map则可以改变当前循环的值，返回一个新的被改变过值之后的数组，然而map需return，一般用来处理需要修改某一个数组的值 

```js
let arr1 = [1,2,3,4,5]
let arr2 = arr1.map((value,key) => {
    console.log(value)    
    console.log(key)      
    return value * value
})
console.log(arr1) // [ 1, 2, 3, 4, 5 ]
console.log(arr2) // [ 1, 4, 9, 16, 25 ]
```



###### filter（es5）

filter函数可以看成是一个过滤函数，返回符合条件的元素的数组，filter需要在循环的时候判断一下是true还是false，是true才会返回这个元素

```js
let arr1 = [1,2,3,4,5]
let arr2 = arr1.filter((value,key) => {
    console.log(value)    
    console.log(key)            
    return value >= 4 ? false : true     
})
console.log(arr1); // [ 1, 2, 3, 4, 5 ]
console.log(arr2); // [ 1, 2, 3]
```



##### 数组转换

###### 什么是伪数组

如果一个对象的所有键名都是按索引建立，并且有length属性，那么这个对象就很像数组，语法上称为类似数组的对象(array-like object),即为伪数组 

```js
let arrLikeobject = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3
}

obj[0] // 'a'
obj[1] // 'b'
obj.length // 3
obj.push('d') // TypeError: obj.push is not a function
```



###### 伪数组转换

数组的slice方法可以将类似数组的对象变成真正的数组， 尽管这种方法可以实现将类数组转变为数组的目的，但并不直观，ES6新增Array.from()方法来提供一种明确清晰的方式以解决这方面的需求，更推荐后者的办法 

```js
// 转换map
const map = new Map()
map.set('k1', 1)
map.set('k2', 2)
map.set('k3', 3)
console.log(Array.from(map))

// 转换set
const set = new Set()
set.add(1).add(2).add(3)
console.log(Array.from(set))

// 转换字符串
const str = 'hello world' 
console.log(Array.from(str))

// 类数组对象
let arrLikeobject = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3
}
console.log(Array.from(arrLikeobject))

// Nodelist
console.log(Array.from(document.querySelectorAll('img')))
```



###### 参数解析

 Array.from接受三个参数，但只有input是必须的 input==> 你想要转换的类似数组对象和可遍历对象，map==>类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组，context==>绑定map中用到的this

```js
let arr1 = Array.from({ length:5 }, () => {
	return 1
})

console.log(arr1)

let obj = {
    handle(n){
        return n + 2
    }
}

let arr2 = Array.from([1, 2, 3, 4, 5], function(x){
    return this.handle(x)
}, obj)

console.log(arr2)
```



##### 数组查找

###### find

找到第一个符合条件的数组成员

```js
let value = [1,2,3,4,5].find((value, index) => value > 2)
console.log(value)
```



###### findIndex

找到第一个符合条件的数组成员的索引值

```js
let index = [1,2,3,4,5].findIndex((value, index) => value > 2)
console.log(index)
```



#### 对象扩展

##### 简洁表示法

###### 属性简写

```js
const getUserInfo = ()=>{
    const name = 'zj'
    const age = '22'
    const key = 'sex'
    return {
        name,
        age,
        [key]:"test",
        ['abc' + 'def']:"test"
        [`${key}123`]:'test'
    }
}

console.log(getUserInfo())
```



###### 方法简写

```js
const getUserInfo = ()=>{
    const name = 'zj'
    const age = '22'
    return {
        name,
        age,
        say(){
            console.log(this.name,this.age)
        },
        * hello(){
            console.log("我是异步函数")
        }
    }
}

console.log(getUserInfo().say())
console.log(getUserInfo().hello())
```



##### 扩展运算符

###### 复制对象

```js
let obj = {
    a:1,
    b:2,
    c:{
        name:"zj",
        age:"123"
    }
}

let copyobj = { ...obj }
console.log(obj.c.name) // zj
console.log(copyobj.c.name) // zj 

copyobj.c.name = "wisdomzj"
console.log(obj.c.name) // wisdomzj
console.log(copyobj.c.name) // wisdomzj
```



###### 合并对象

```js
let obj1 = {
    a:1,
    b:2
}

let obj2 = {
    c:3,
    a:4
}

let obj = {
    ...obj1,
    ...obj2
}

console.log(obj)
```



##### 新增方法

###### 对象复制

###### 获取对象属性名

###### 获取对象属性值

###### 获取对象键值对形式



#### 字符串扩展

##### 模板字符串

###### 换行&拼接处理

字符串不是静态内容，往往需要加载变量变量或者表达式，这个是常见的需求，之前是的做法是字符串拼接，如果有大量的变量和表达式，拼接会很头疼

```js
const str1 = 'In JavaScript this is \n not legal.'

const str2 = `In JavaScript this is
not legal.`

console.log(str1,str2) 
```



###### 包含变量处理

可以插入变量或者表达式，只需要${ } 包裹变量即可

```js
const a = 20
const b = 10
const c = 'javascript'

const str1 = 'my age is'+ (a + b) + 'i love' + c  // es5
const str2 = `my age is ${a + b} i love ${c}` // es6

console.log(str1,str2)
```



###### 包含复杂逻辑处理

对于包含复杂逻辑的字符串并不是简单的表达式能搞定，所以需要另一中解决方案Tag Literals，定义一个Tag函数，用这个函数充当一个模板引擎

```js
function Price(strings,type){
    let s1 = strings[0]
    const retailPrice = 20
    const wholeSalePrice = 16
    let showTxt;
    if(type === 'retail'){
       showTxt = `购买单价是 ${retailPrice}`
    }else{
       showTxt = `购买单价是 ${wholeSalePrice}`
    }
    return `${s1}${showTxt}`
}

let showTxt = Price`您此次的${'retail'}`
console.log(showTxt)
```



##### 新增方法

######  查找字符串

startsWith() ==> 返回布尔值，表示参数字符串是否在原字符串的头部 ， endsWith() ==> 返回布尔值，表示参数字符串是否在原字符串的尾部，include() ==>  返回布尔值，表示是否找到了参数字符串  

```js
let s = 'Hello world!'

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true

// 使用第二个参数n时，endsWith的行为与其他两个方法有所不同,它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束  一种向前数位置  一种向后数位置
s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```



###### 字符串补全

如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全 

```js
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'

//如果省略第二个参数，默认使用空格补全长度
'x'.padStart(4) // '   x'
'x'.padEnd(4) // 'x   '
```



###### 消除前后空格

它们的行为与`trim()`一致，`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格，它们返回的都是新字符串，不会修改原始字符串 

```js
const str = '  abc  '

console.log(str.trim())  // "abc"
console.log(str.trimStart()) // "abc  "
console.log(str.trimEnd()) // "  abc"
console.log(str) // "  abc  "
```



###### 字符串重复

`repeat`方法返回一个新字符串，表示将原字符串重复`n`次 

```js
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'test'.repeat(0) // ""
'test'.repeat(2.9) // "testtest"
'test'.repeat(Infinity)// RangeError
'test'.repeat(-1)// RangeError
'test'.repeat(-0.9) // ""
'test'.repeat(NaN) // ""
'test'.repeat('3') // "testtesttest"
```



#### 数值扩展

##### 新增方法

###### 判断是否有限

```js

```



###### 判断是否为NaN

```js

```



###### 判断是否是整数

```js

```



###### 限制数范围

```js

```



###### 安全整数

```js

```



###### 获取整数部分

```js

```



###### 判断数的正负情况

```js

```



##### 方法调整

###### parseInt()

```js

```



###### parseFloat()

```js

```



#### 函数扩展

##### 传参方式增强

###### 参数默认值

函数而言，参数是会被进场用到的，es5中通常写在函数体内，es6改变了这一做法

```js
function Func1(x,y,z){
    if(y === undefined){
       y = 7
    }
    if(z === undefined){
       z = 5
    }
    
    return x + y + z
}

function Func2(x,y=7,z=5){ 
    return x + y + z
}

Func1(1) // 12
Func2(2) // 12
```



###### 不确定参数

在函数体内，有时候需要判断函数有几个参数，es5中会用arguments，es6总废弃了这一用法，使用rest参数来接受不确定参数

```js
function Sum1(){
    let num = 0
    Array.forEach.call(arguments,(item)=>{
        num += item*1
    })
    return num
}

function Sum2(...nums){
    let num = 0
    nums.forEach((item)=>{
        num += item*1
    })
    return num
}

console.log(Sum1(1,2,3,4,5))
console.log(Sum2(1,2,3,4,5))
```



###### rest逆运算

rest参数是把不定的的参数收敛到一个数组中，而逆运算是具有相反意义的操作符，是把固定的数组内容打散到对应的参数中

```js
function sum(x=1,y=2,z=3){
    return x + y + z
}
console.log(sum(...[4])) //9
console.log(sum(...[4,5])) //12
console.log(sum(...[4,5,6])) //15
```



##### 箭头函数

###### 基本用法

es6中简化了声明函数形式，而且语法也很优雅

```js
function hello(){
    console.log('say hello')
}

let hello = () => {
    console.log('say hello')
}
```



###### 参数个数

如果只有一个参数，可以省略括号，如果大于一个参数不能省略括号

```js
let hello = name => {
    console.log('say hello',name)
}

let hell0 = (a,b) => {
    console.log('say hello',a,b)
}
```



###### 不同返回值

返回也分多种情况，多种情况对应的写法

```js
// 如果返回值是表达式可以省略return 和 {}
let pow = x => x*x

// 如果返回值是字面量对象,则需要用小括号包起来
let person = (name) => ({
    age:22,
    name:"zj"
})
```



###### this指向问题(有点晕)



#### 结构赋值

##### 数组解构赋值

###### 简单数组

```js
{ 
    let [a, b, c] = [ 1, 2, 3]
    console.log(a,b,c); //1,2,3
}
```



###### 嵌套数组

```js
{
    let [a, [[b], c]] = [1, [[2], 3]]
    console.log(a,b,c) //1,2,3

    let [x, ,y] = [1, 2, 3]
    console.log(x,y) //1,3
}
```



###### 默认值情况

解构赋值允许指定默认值，ES6 内部使用严格相等运算符（`===`），判断一个位置是否有值，所以，只有当一个数组成员严格等于`undefined`，默认值才会生效，默认值可以引用解构赋值的其他变量

```js
//指定默认值
{
    let [a,b=2] = [1]
    console.log(a,b) //1,2
}

//使用严格相等运算符（===）
{
    let [a=1] = [underfined]
    console.log(a) //1
    
    //null不完全等于underfined
    let [b=1] = [null]
    console.log(b) //null
}

//引用解构赋值的其他变量但该变量必须已经声明
{
    let [x = 1, y = x] = [];     // x=1; y=1
    let [x = 1, y = x] = [2];    // x=2; y=2
    let [x = 1, y = x] = [1, 2]; // x=1; y=2
    let [x = y, y = 1] = [];     // ReferenceError: can't access lexical declaration `y' before initialization
}
```



###### rest运算符

如果我们只关心指定的值，其他可以暂存到一个变量中，可以采取使用rest 运算符

```js
let [x,...y] = [1, 2, 3, 4]
console.log(x,y) // 1,[2,3,4]
```



###### 不完全结构情况

等号左边的模式，只匹配一部分的等号右边的数组

```js
{
    let [x, y] = [1, 2, 3]
    let [a, [b], d] = [1, [2, 3], 4]
    console.log(x,y,a,b,c) // 1 2 1 2 4
}
```



###### 结构不成功情况

解构不成功，变量的值就等于`undefined`

```js
{
    let [a] = []
    let [b, c] = [1]
    console.log(a,b,c)  // undefined 1 undefined 
}
```



##### 对象结构赋值

###### 简单对象

结构赋值除了可以应用在Array，也可以应用在Object

```js
let options = {
    title:'Menu',
    width:100,
    height:200
}

let { title:t,width:w,height:h } = options // 不采取简写,可以自定义变量
let { title,width,height } = options  //简写,当属性和属性值同名可以简写成一个单词

console.log(t,w,h)
console.log(title,width,height)
```



###### 嵌套对象

如果一个array或者Object比较复杂，它嵌套了Array或者Object 那只要被只要被赋值的结构和右侧赋值的元素一致就好了

```js
let options = {
    size：{
    width:100,
    height:200
}
items:['Cake','Donut'],
    extra:true    
}

let {
    size:{
        width,
        height
    },
    item:[item1,item2],
    extra
} = options
```



###### 默认值情况

赋值的过程中也是可以指定默认值

```js
let options = {
    title:'Menu'
}

let { width = 100,height = 200,title } = options

console.log(title,width,height)
```



###### rest 运算符

如果我们只关心指定的属性，其他可以暂存到一个变量中，可以采取使用rest 运算符

```js
let options = {
    title:'Menu',
    width:100,
    height:200
}

let { title,...rest } = options

console.log(title,rest.width,rest.height)
```



###### 不完全结构情况

等号左边的模式，只匹配一部分的等号右边的对象内容

```js
let options = {
    title:'Menu',
    width:100,
    height:200
}

let { width } = options

console.log(width)
```



###### 结构不成功情况

解构不成功，变量的值就等于`undefined`

```js
let options = {
    title:'Menu',
    width:100,
    height:200
}

let { test } = options

console.log(test)
```



##### 其他类型结构赋值

###### 布尔类型

布尔值的包装对象有`toString`属性，因此变量`s`都能取到值 

```js
let {toString: s} = true;
s === Boolean.prototype.toString // true
```



###### 字符串

 字符串也可以解构赋值，被转换成了一个类似数组的对象 

```js
const [a, b, c, d, e] = 'hello'
let {length : len} = 'hello'
console.log(a,b,c,d,e)
console.log(len)
```



###### 数值类型

数值包装对象有`toString`属性，因此变量`s`都能取到值 

```js
let {toString: s} = 123
s === Number.prototype.toString // true
```



###### 函数传参

当传入[ ] 或者 { } 形式的参数，可以采取解构赋值

```js
function add([x, y]){
  return x + y
}

add([1, 2]) // 3

const arr = [[1, 2], [3, 4]].map(([a, b]) => a + b)
console.log(arr) //[3,7]


//花括号是对象的结构赋值，实参当作提取对象，形参拿到提取后的值进行逻辑操作，本质根据属性名进行匹配，与顺序无关
function Computer({
    cpu,
    memory,
    software = ['ie10'],
    os = 'window 7'
}){
    console.log(cpu)
    console.log(memory)
    console.log(software)
    console.log(os)
}

Computer({
    memory:'128G',
    cpu:'80286',
    os:'window 10'
})
```



#### Class

##### "类"的定义

###### 构造函数（es5）

```js
function Animal (type){
    this.type = type
}

Animal.prototype.walk = function(){
    console.log('this is walk')
}

var dog = new Animal('dog')
var monkey = new Animal('monkey')
console.log(dog.type,monkey.type) // dog monkey
console.log(dog.walk,monkey.walk) // this is walk
```



###### class语法糖（es6）

```js
class Animal{
    constructor(type){
        this.type = type
    }
    
    work(){
        console.log('this is walk')
    }
}

let dog = new Animal('dog')
let monkey = new Animal('monkey')
console.log(dog.type,monkey.type) // dog monkey
console.log(dog.walk,monkey.walk) // this is walk
```



##### 静态方法&属性

###### 静态属性

```js
class Animal{
    static test = 'tt' //提案
    constructor(type){
        this.type = type
        Animal.totlaAni += 1
    }
    
    work(){
        console.log('this is walk')
    }
}

Animal.totlaAni = 0
Animal.config = {
    name:'Ani',
    age：0
}
new Animal()
new Animal()
new Animal()
new Animal()
new Animal()
console.log(Animal.totlaAni) // 5
console.log(Animal.config.name) // Ani
console.log(Animal.config.age) // 0
```



###### 静态方法

```js
class Animal{
    constructor(type){
        this.type = type
    }
    
    work(){
        console.log('this is walk')
    }
    
    static work(){
        console.log('this is static work')
    }
}

let dog = new Animal()
dog.work()
Animal.work()
```



##### 属性读写操作

###### 场景一

对于类中的属性，可以直接在constructor中通过this直接定义，还可以直接定义在类的顶层来定义

```js
class Test{
    constructor(age){
        this._age = age
    }
   
    get age(){
        return this._age
    }
    
    set age(val){
        if(val>22){
        	this._age = val   
        }
    }
}

let test = new Test(18)
console.log(test.age) // 18
test.age = 17
console.log(test.age) // 18
test.age = 23
console.log(test.age) // 23
```



###### 场景二

上述场景不能有效的保护属性，也能直接去访问_age属性，可以进行赋值修改操作，难免会觉得多此一举，但我们只想创建一个只读属性，我们怎么实现

```js
class Test{
    constructor(age){
        this._age = age
    }
   
    get name(){
        return 'Test' 
    }
}
let test = new Test(18)
console.log(test.name) // Test
test.name = 'Update Name'
console.log(test.name) // Test
```



###### 场景三

还可以对一些属性进行简单封装，达到一些要求等

```js
class Test{
    constructor(el){
        this.el = el
    }
    
    get html(){
        return this.el.innerHTML
    }
    
    set html(val){
        this.el.innerHTML = val
    }
}
let el = document.getElementById('#id')
let test = new Test(el)
console.log(test.html)
test.html = '111'
console.log(test.html)
```



###### 场景四

有时候我们真的需要一个私有属性，通过一定的规则来进行修改的操作，这时候使用set&get能容易实现

```js
let $age = 0
class Test{
    constructor(name){
        this.name = name
    }
   
    get age(){
        return $age
    }
    
    set age(val){
        if(val>0 && val<18){
        	$age = val   
        }
    }
}
let test = new Test('test')
console.log(test.age) // 0
test.age = 15
console.log(test.age) // 15
test.age = 66
console.log(test.age) // 15
```



##### 类的封装

###### 形变类初始版本

```js
class Transfrom{
    constructor(el){
        this.el = el
        this._queue = []
    }

    // 位移
    translate(value,time){
        return this._add('translate',value,time)
    }

    // 缩放
    scale(value,time){
        return this._add('scale',value,time)
    }

    // 形变
    rotate(value,time){
        return this._add('rotate',value,time)
    }

    // 添加动画
    _add(type,value,time){
        this._queue.push({type,value,time})
        return this
    }
}
```



###### 形变类加强版本

```js
class Transfrom{
    constructor(el){
        this.el = el
        this._queue = []
    }

    // 位移
    translate(value,time){
        return this._add('translate',value,time)
    }

    // 缩放
    scale(value,time){
        return this._add('scale',value,time)
    }

    // 形变
    rotate(value,time){
        return this._add('rotate',value,time)
    }

    // 添加动画
    _add(type,value,time = this.defaultTime){
        this._queue.push({type,value,time})
        return this
    }
    
    // 完成动画
    done(){
        this._start()
    }

    _start(){
        if(!this._queue.length) return

        setTimeout(()=>{
            const info = this._queue.shift()
            this.el.style.transition = `all ${ .3 }s`
            this.el.style.Transfrom = this._getTransform(info)
            setTimeout(()=>{
                this._start()
            },300)
        },0)

    }

    _getTransform({time,value,type}){
        switch(type){
            case 'translate':
                return `translate(${value})`
                break;
            case 'scale':
                return `scale(${value})`
                break;
            case 'rotate':
                return `rotate(${value}deg)`
                break;
        }
    }
}
```



###### 形变类完整版本

```js
class Transfrom{
    constructor(el){
        this.el = el
        this._queue = []
        this._transfrom = {
            translate:'',
            scale:'',
            rotate:''
        }
        this.defaultTime = Transfrom.config.defaultTime
    }

    // 位移
    translate(value,time){
        return this._add('translate',value,time)
    }

    // 缩放
    scale(value,time){
        return this._add('scale',value,time)
    }

    // 形变
    rotate(value,time){
        return this._add('rotate',value,time)
    }

    // 添加动画
    _add(type,value,time = this.defaultTime){
        this._queue.push({type,value,time})
        return this
    }

    done(){
        this._start()
    }

    _start(){
        if(!this._queue.length) return

        setTimeout(()=>{
            const info = this._queue.shift();
            this.el.style.transition = `all ${info.time / 1000}s`
            this.el.style.Transfrom = this._getTransform(info)
            setTimeout(()=>{
                this._start()
            },info.time)
        },0)
    }

    _getTransform({time,value,type}){
        const _tsf = this._transfrom;
        switch(type){
            case 'translate':
                _tsf.translate = `translate(${value})`
                break;
            case 'scale':
                _tsf.scale = `scale(${value})`
                break;
            case 'rotate':
                _tsf.rotate = `rotate(${value}deg)`
                break;
        }
        return `${_tsf.translate} ${_tsf.scale} ${_tsf.rotate}`
    }
}

Transfrom.config = {
    defaultTime:300
}

const tf = new Transform('#ball')

tf.translate('200px, 200px')
  .scale(1.5)
  .sleep(1000)
  .rotate(180,5000)
```



##### 类的继承

###### extends关键字

```js
class Animal{
    constructor(type){
        this.type = type
    }
    
    work(){
        console.log('this is walk')
    }
}

class Dog extends Animal{
     constructor(type,name){
        super(type)
        this.name = name
     }
    
     eat(){
         console.log('this Dog eat func')
     }
}

let dog = new Dog('type','name')
dog.work()
dog.eat()
```



###### super关键字

```js
class Animal{
    constructor(type){
        this.type = type
    }
    
    work(){
        console.log('this is work')
    }
    
    static foo(){
        console.log('this is foo')
    }
}

Animal.bar = 'bar'


class Dog extends Animal{
     constructor(type,name){
        // 作为函数使用代表父类构造函数使用,且只能在构造器中使用,否则报错
        super(type)
        this.name = name
     }
    
     eat(){
         console.log('this Dog eat func')
     }
     
     work(){
         // 作为对象在普通方法中使用,指向父类的原型对象
         super.work()
     }
    
     static test(){
         // 在静态方法中,指向父类
         console.log(super.name) 
         console.log(super.bar)  
         super.foo() 
     }
}

let dog = new Dog('type','name')
dog.work()
Dog.test() 
```



###### 使用继承

```js
class MultiTransform extends Transform {
    constructor(el){
        super(el)
    }

    // 同时执行多个动画
	multi(value, time) {
		return this._add('multi', value, time);
	}

    // 延迟执行下一个动画
	sleep(time) {
		return this._add('speep', '', time);
	}

	_getTransform({ time, value, type }) {
		const _tsf = this._transform;

		switch (type) {
			case 'translate':
				_tsf.translate = `translate(${ value })`;
				break;
			case 'scale':
				_tsf.scale = `scale(${ value })`;
				break;
			case 'rotate':
				_tsf.rotate = `rotate(${value}deg)`;
				break;
			case 'multi':
				value.forEach(item => {
					this._getTransform(item);
				});
			case 'sleep':
				break;
		}

		return `${_tsf.translate} ${_tsf.scale} ${_tsf.rotate}`;
	}
}

const mtf = new MultiTransform('#ball')

mtf.translate('250px, 250px').sleep(3000).multi([
    {
        type: 'translate',
        value: '100px,100px'
    },
    {
        type: 'scale',
        value: 2
    },
    {
        type: 'rotate',
        value: 150
    }
]).done();
```



#### Promise

##### 基本语法

###### 解决回调地狱

```js
function loadScript(src,callback){
    let script = document,cteateElement('script')
    script.src = src
    script.onload = () => callback(script) 
    document.head.append(script)
}

loadScript('1.js',(error,scipt)=>{
    if(error){
       handleError(error)
    }else{
        loadScript('2.js',(error,script)=>{
            if(error){
                handleError(error)
            }else{
                loadScript('3.js',(error,script)=>{
                    if(error){
                    	handleError(error)   
                    }else{
                        // ....假如后续有很多文件需要加载
                    }
                })
            }
        })
    }
})

// 解决方案
function loadScript(src,callback){
    return new Promise((resolve,reject)=>{
        let script = document,cteateElement('script')
    script.src = src
    script.onload = () => resolve(script)
    script.onerror = (err) => reject(err)
    document.head.append(script)
    })  
}

loadScript('1.js').then((res)=>{
    return loadScript('2.js')
}).then((res)=>{
    return loadScript('3.js')
}).catch((e)=>{
    console.log(e.message)
})
```



###### 尝试封装ajax

```js
function ajax(URL){
    return new Promise((resolve,reject)=>{
        let req = new XMLHttpRequest()
        req.open('GET',URL,true)
        req.onload = function(){
            if(req.status === 200){
               resolve(this.responseText)
            }else{
               reject(new Error(this.statusText))
            }
        }
        req.onerror = function(){
            reject(new Error(this.statusText))
        }
        req.send()
    })
}

let URL = 'http://localhost:3000/api/prolist'
ajax(URL).then((res)=>{
    console.log(res)
})
```



##### 实例方法

###### then

```js
// 参数不是回调函数情况 (如果参数不是回调,则忽略,又因为参数是表达式需要进行判断，以判断就执行返回一个promise)
loadScript('./1.js')
    .then(loadScript('./2.js'))
    .then(loadScript('./3.js'))


// 参数只有一个回调函数情况
let promise = new Promise((resolve,reject)=>{
    resolve('传递给then的值')
})

promise.then((value)=>{
    console.log(value)
})

// 参数有两个回调函数情况
promise.then((value)=>{
    console.log(value)
},(err)=>{
    console.log(err)
})
```



###### catch

```js
// then捕获错误太麻烦
loadScript('1.js').then((res)=>{
    return loadScript('2.js')
},(err)=>{
    console.log(err)
}).then((res)=>{
    return loadScript('3.js')
},(err)=>{
    console.log(err)
})

// 统一捕获处理错误
loadScript('1.js').then((res)=>{
    return loadScript('2.js')
}).then((res)=>{
    return loadScript('3.js')
}).catch((e)=>{
    console.log(e.message)
})

function test(){
    return new Promise((resovle,reject)=>{
        reject(new Error('this is error'))
    })
}

test().catch((e)=>{
    console.log(e.message)
})
```



##### 静态方法

######  resolve

```js
// 形式一
new Promise((resolve)=>{
    resolve(666)
})

// 形式二
Promise.resolve(666).then((res)=>{
    console.log(res)
})
```



###### reject

```js
// 形式一
new Promise((resolve,reject)=>{
    reject(new Error('this is a debug'))
})

// 形式二
Promise.reject(new Error('this is a debug'))
```



###### all

```js
cnost loadImg = (src) => {
    return new Promise((resvole,reject)=>{
        const img = new Image()
        img.src = src
        img.onload = () => {
            resvole(img)
        }
        img.onerror = (e) => {
            reject(e)
        }
    })
}

const imgs = ['http://image4.xyzs.com/upload/84/d2/1029/20141204/141769529289494_0.jpg','http://b-ssl.duitang.com/uploads/item/201607/19/20160719155609_kyJEe.jpeg','http://img.mp.itc.cn/upload/20170204/f2f9e6e4df3042b7bfcabb6f5ef0ac05_th.jpeg']

Promise.all(imgs.map(src => loadImg(src))).then((res)=>{
    console.log(res)
    res.forEach((img)=>{
        document.body.appendChild(img)
    })
})
```



###### race

```js
let p1 = Promise.resolve(1)
let p2 = Promise.resolve(2)
let p3 = Promise.resolve(3)

Promise.race([p1,p2,p3]).then((res)=>{
    console.log(res) // 1
})
```



#### Module

##### 导出

###### 导出变量&常量

```js
// 逐个导出
export const name = 'hello'
export let addr = 'beijing'
export var list = [1,2,3]

// 一次性导出多个
const name = 'hello'
let addr = 'beijing'
var list = [1,2,3]
export {
	name,
    addr,
    list
}
```



###### 导出函数

```js
// 逐个导出
export foo = (str) => {
    console.log(str)
}

export bar = () => {
    console.log('this is function')
}

// 一次性导出多个
export {
    foo,
    bar
}
```



###### 导出对象

```js
// 形式一
export({
    code:0,
    message:'success'
})

let data = {
    code:0,
    message:'success'
}

// 形式二
export {
	data
}
```



###### 导出class

```js
// 形式一
export class Test{
    constructor(){
        this.id = 2
    }
}

// 形式二
export {
	Test
}
```



###### 修改导出名称

```js
const name = 'hello'
let addr = 'beijing'
var list = [1,2,3]
export {
	name as cname,
    addr as caddr,
    list
}
```



###### 设置默认导出

```js
const name = 'hello'
let addr = 'beijing'
var list = [1,2,3]
export {
	name,
    addr
}
export default list
```



##### 引入

###### 直接导入

```js
# 模块文件module.js
const name = 'hello'
let addr = 'beijing'
var list = [1,2,3]
export {
	name as cname,
    addr as caddr
}
export default list

# 导入文件
import list,{ cname,caddr } from module
```



###### 修改导入名称

```js
import list,{ cname as name ,caddr } from module
```



###### 批量导入

```js
import list,* as mod from module
console.log(list)
console.log(mod.cname)
console.log(mod.caddr)
```



#### Generator

#### lterator

#### Proxy

#### Reflect
