# Js 学习总结

[toc]



### 语法规则

#### 变量

##### 定义变量

###### 先定义后赋值

```js
var name
name = 'zhangjun'
console.log(name)
```



###### 边定义边赋值

```js
var name = 'zhangjun'
console.log(name)
```



###### 批量定义后赋值

```js
var a,b,c
a = '1'
b = '2'
c = '3'
```



###### 批量定义直接赋值

```js
var a = '1',b = '2',c = '3'
```



##### 变量特点

###### 保留

```js
// 变量被重新声明但没赋值它还保留以前被赋予的值
var test = 'test';
var test;
console.log(test)
```



###### 覆盖

```js
// 变量被重新声明而且也赋值了它还不再保留以前被赋予的值	
var test = 'foo';
var test = 'bar';
console.log(test)
```



##### 输出工具

###### 弹出框

```js
alert('我是张军,我为自己代言')
```



###### 页面输出

```js
document.write('<p>我是张军,我为自己代言</p>')
```



######  提示框

```js
prompt('亲,输入你的姓名：')
```



###### 控制台打印

```js
console.log('我是张军,我为自己代言')
```



#### 运算符

##### 算术运算符

###### 加减乘除

```js
var n1 = 1+2;
var n2 = 2-1;
var n3 = 2*1;
var n4 = 4/2;
console.log(n1,n2,n3,n4)
```



###### 取余

```js
var num = prompt("请输入数字进行判断");
if(num%2 == 0){
    console.log("我是偶数")
}else{
    console.log("我是奇数")
}
```



###### ++&&--

```js
var n1 = 6,n2 = 6
var res1 = 5 * ++n1 
console.log(res1,n1) // 35 7
var res2 = 5 * n2++
console.log(res2,n2) // 30 7
```



##### 逻辑运算符

###### 或

```js
if(4>3 || 2<3){
    console.log('我是对的');
}else{
    console.log('我是错的');
}
```



###### 与

```js
if(1<2 && 3<4){
    console.log('我是对的');
}else{
    console.log('我是错的');
}
```



###### 非

```js
if(!(3<4)){
    console.log('我是对的');
}else{
    console.log('我是错的');
}
```



##### 关系运算符

###### >&>=

```js
var a = 5>3
var b = 6>=6
console.log(a,b)
```



###### <&<=

```js
var a = 3<5
var b = 6<=6
console.log(a,b)
```



###### ==&===

```js
// ==表示:相等 ，内容比较相等,类型不比较相等与否
// ===表示：全等，内容进行比较相等，类型也比较相等与否
var a = 3
var b = '3'
console.log(a == b) // true
console.log(a === b) // false
```



###### !=&!==

```js
var a = 3
var b = '3'
console.log(a != b) // false
console.log(a !== b) // true
```



##### 其他运算符

###### 赋值运算符

```js
// +=:表示：加上原来的变量在加后面的值
var a = 5
a += 5
console.log(a) // 10

// *=:表示：乘上原来的变量在加后面的值
var b = 9
b *= 2
console.log(b) // 18
```



###### 三元表达式

```js
var age = prompt('亲,输入你的姓名：')
var res = (age>18)?'成年了':'未成年'
console.log(res)
```



#### 数据类型

##### 数值

###### 数值类型

```js
var num1 = 123, num2 = 12.26
console.log(typeof(num1)) // number
console.log(typeof num2)  // number
```



###### 非数值

```js
var age = 12,name = 'zj'
// 非数值是一个特殊的数值类型
console.log(age - '11abc') // NaN
console.log(typeof(age - '11abc')) // number

// 任何涉及NaN的操作都会返回NaN
console.log(NaN / 10) // NaN
console.log((age - '11abc') / 10) // NaN

// NaN与任何值都不相等,包括NaN本身
console.log((age - '11abc') == NaN) //false 
console.log(NaN === NaN) //false

// 检测是否是非数值
console.log(isNaN(age)) //false
console.log(isNaN(name)) //true
```



###### 数值转换

```js

```



##### 字符串

###### 字符串类型

###### 字符串转换



##### "空"&"无"&布尔

###### undefined

```js
// undefined类型只有一个值,即特殊的undefined,变量定义了但没赋值
var test1;
console.log(test) // undefined

// 一般不需要显示的将变量设置为undefined,也不建议这么做
var test2 = undefined;
console.log(test2) // undefined
```



###### null

```js
// 表示一个空对象指针,如果定义的变量准备用于保存对象,最好将改变量初始化为null而不是其他值
var test = null
if(true){
    test = 'test'
}
console.log(test)

// undefined值派生自null值,所以 undefined == null 的返回结果为true
console.log(undefined == null)
```



###### boolean

```js

```



##### 对象



#### 流程控制

#### 循环结构

#### 函数运用



### 内置对象

#### 日期对象

#### 数学对象

#### 数组对象

#### 字符串对象





### 文档对象模型（DOM）

#### DOM节点

##### 创建节点

###### 创建节点API

###### 高效创建节点



##### 查找节点

##### 删除节点

##### 操作节点

##### 获取节点

 



#### DOM属性

#### DOM事件



### 浏览器对象模型（BOM）



### 错误处理（Error）

