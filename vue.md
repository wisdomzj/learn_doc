#  Vue学习总结

[TOC]

### 起步

#### 安装VUE

##### 直接引入

直接下载并用 `<script>` 标签引入，`Vue` 会被注册为一个全局变量

开发版：包含完整的警告和调试模式 ，下载地址：https://cn.vuejs.org/js/vue.js

生产版：删除了警告，33.30KB min+gzip 下载地址：https://cn.vuejs.org/js/vue.min.js



##### cdn 获取

```js
// 对于制作原型或学习，你可以这样使用最新版本
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> 

// 对于生产环境，我们推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的破坏   
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script> 

// 使用原生 ES Modules，这里也有一个兼容 ES Module 的构建文件
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.esm.browser.js'
</script>
```



##### npm 安装

在用 Vue 构建大型应用时推荐使用 NPM 安装，NPM 能很好地和诸如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用

```shell
# 最新稳定版
$ npm install vue
```



#### 环境搭建（vue-cli）

##### 需要node环境支持

下载地址： <https://nodejs.org/en/download>

![1571845560490](http://demo.bestzhangjun.com/node/1571845560490.png)



##### 基于脚手架开发

###### 脚手架安装

Vue 提供了一个[官方的 CLI](https://github.com/vuejs/vue-cli)，为单页面应用 (SPA) 快速搭建繁杂的脚手架，它为现代前端工作流提供了 batteries-included 的构建设置，只需要几分钟的时间就可以运行起来并带有热重载、保存时 lint 校验，以及生产环境可用的构建版本

```shell
# 全局安装 vue-cli
$ npm install --global vue-cli //2.x
$ npm install -g @vue/cli  or yarn global add @vue/cli //3.x
```



###### 向下兼容版本

Vue CLI >= 3 和旧版使用了相同的 `vue` 命令，所以 Vue CLI 2 (`vue-cli`) 被覆盖了，如果你仍然需要使用旧版本的 `vue init` 功能，你可以全局安装一个桥接工具

```shell
npm install -g @vue/cli-init
# vue init 的运行效果将会跟 vue-cli@2.x 相同
vue init webpack my-project
```



##### 创建Vue项目

```shell
# 创建一个基于 webpack 模板的创建新项目
$ vue init webpack my-project //2.x
$ vue create my-project //3.x  

# 另一种创建项目的方式（简洁版本）
$ vue init webpack-simple projectName //2.x 目录结构简单些
$ vue ui //3.x gui方式创建项目,可视化创建

# 安装依赖（3.x不需要手动安装 但创建项目的时候需要配置）
$ cd my-project
$ npm install 

# 启动项目
$ cd projectdir
$ npm run dev //2.x
$ npm run serve //3.x
```



##### 命令行初始化配置

选择Manually select features自定义功能配置

![](https://img-blog.csdnimg.cn/20190517164544362.png)

选择自定义功能配置 Babel编译、Vue路由、Vue状态管理器、CSS预处理器、代码检测和格式化、以及单元测试

![](https://img-blog.csdnimg.cn/20190517164753186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21hZ2dpZV9saXZl,size_16,color_FFFFFF,t_70)

选择css预处理器，具体看选择技术栈

![](https://img-blog.csdnimg.cn/20190517164937760.png)

选择ESLint的代码规范

![](https://img-blog.csdnimg.cn/20190517165120908.png)

进行代码检测，选择在保存时进行检测

![](https://img-blog.csdnimg.cn/20190517165120908.png)

选择单元测试解决方案，此处选择jest

![](https://img-blog.csdnimg.cn/2019051716522953.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21hZ2dpZV9saXZl,size_16,color_FFFFFF,t_70)

选择Babel、PostCSS、ESLint等配置文件存放位置推荐单独保存在各自的配置文件

![](https://img-blog.csdnimg.cn/20190517165425266.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21hZ2dpZV9saXZl,size_16,color_FFFFFF,t_70)

是否将以上这些将此保存为未来项目的预配置

```js
? save this as a preset for future projects? Yes
//是否将以上这些将此保存为未来项目的预配置吗？
? save preset as:vue-test
//描述项目 
```



##### GUI界面初始化配置

通过 `vue ui` 命令以图形化界面创建和管理项目

```shell
$ vue ui
```

上述命令会打开一个浏览器窗口，并以图形化界面将你引导至项目创建的流程

![1573017833721](C:\Users\25561\AppData\Roaming\Typora\typora-user-images\1573017833721.png)



##### 项目目录解析

```js
├── build // webpack的基本配置、开发环境配置、生产环境配置
├── config // 路径、端口以及反向代理配置
├── dist // webpack打包后的静态资源
├── node_modules // npm安装的依赖包
├── src // 前端主文件
│   ├── assets // 静态资源
│   │   ├── font
│   │   ├── img
│   │   └── scss
│   ├── components // 单个组件
│   │   ├── xxx.vue // 单文件组件
│   ├── router // 路由配置
│   ├── store // 状态管理
│   ├── App.vue //App主组件
│   ├── main.js //主入口文件
├── public // 静态文件
├── .babelrc  // babel的配置项
├── .editorconfig  // 编辑器的配置项
├── .gitignore  // 会忽略语法检查的目录
├── index.html // 入口页面
├── package.json // 项目的描述和依赖 
```



##### 项目加载过程

![](https://user-gold-cdn.xitu.io/2018/2/9/1617b04b5c587539?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 核心学习

#### 指令学习

##### 原生指令

###### 插值表达式 

插值表达式 ==> 只会替换自己的这个占位符 会产生闪烁问题，但不会覆盖标签的内容

```vue
<template>
	<div id="app">
    	<span>{{msg}}</span>
    	<span>my name is {{name}}</span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                msg:"I am zhangjun",
                name:"zhangjun"
            }
        }
    }
</script>
```



###### v-html&v-text

v-text ==> 会覆盖元素原本的内容，不会产生闪烁问题

v-html ==> 内容中如果出现标签就将其转义成html标签形式展示，没有标签就展示原本内容

```vue
<template>
	<div id="app">
    	<p v-text="text">要被覆盖的内容</p>
    	<p v-html="html"></p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                text:"hello vue",
                html:"<span>hello vue</span>"
            }
        }
    }
</script>
```



###### v-bind&v-on

v-bind ==> 用于设置属性 简写为 :属性

v-on ==> 用于给事件源绑定事件 简写 @事件名

```vue
<template>
	<div id="app">
    	<input v-bind:value="value1">
    	<input :value="value2">
    	<button v-on:click="foo1">点击我</button>
    	<button @click="foo2">点击我</button>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                value1:"this is value1",
                value2:"this is value2",  
            }
        },
        methods: {
            foo1(){
                alert("我是一个方法！！！");
            },
            foo2(){
                alert("我是一个方法！！！");
            }
        }
    }
</script>
```



###### 双向绑定

v-model ==> 可以实现表单元素和 Model 中数据的双向数据绑定，v-model 运用在 表单元素中才合适

```vue
<template>
	<div id="app">
    	<input type="text" v-model="m">
    	<select v-model="icon">
        	<option>+</option>
        	<option>-</option>
        	<option>*</option>
        	<option>/</option>
    	</select>
        <input type="text" v-model="n">
    	<input type="text" v-model="result">
    	<input type="button" value="calc" @click="calc">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                m:"0",
                n:"0",
                icon:"+",
                result:"0",
            }
        },
        methods: {
            calc(){
                this.result = eval("parseInt(this.m)"+this.icon+"parseInt(this.n)");
            }
        }
    }
</script>
```



###### 条件渲染

1. v-if&v-show

   （1）v-if 的特点：每次都会重新删除或创建元素    v-show 的特点： 每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式 

   （2）v-if 有较高的切换性能消耗    v-show 有较高的初始渲染消耗 

   （3）如果元素涉及到频繁的切换，最好不要使用    v-if, 而是推荐使用 v-show 如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if

   ```vue
   <template>
   	<div id="app">
       	<input type="button" value="toggle" @click="toggle">
       	<h1 v-if="flag">测试v-if</h1>
       	<h1 v-show="flag">测试v-show</h1>
       </div>
   </template>
   
   <script>
       export default {
           name: 'app',
           data () {
               return {
                   flag:true
               }
           },
           methods: {
               toggle(){
                   this.flag = !this.flag;
               }
           }
       }
   </script>
   ```

   

2. v-else&v-else-if

   兄弟元素进行使用，同时注意 `v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别 

   ```vue
   <template>
   	<div id="app">
           <p v-if="flag === 1">测试v-if</p>
       	<p v-else-if="flag === 2">测试v-else-if</p>
       	<p v-else>测试v-else</p>
       </div>
   </template>
   
   <script>
       export default {
           name: 'app',
           data () {
               return {
                   flag:1
               }
           }
       }
   </script>
   ```

   

###### 列表渲染

1. 迭代数组

   ```vue
   <!-- 迭代内部元素为对象的数组 -->
   <ul v-for="(item,index) in list" :key="item.id">
       <li>姓名:{{item.name}}--年纪:{{item.age}}--下标:{{index}}</li>
   </ul>
   
   <!-- 迭代普通数组 -->
   <p v-for="(item,index) in arr" :key="item">{{item}}--{{index}}</p>
   ```

   

2. 迭代对象

   ```vue
   <!-- 迭代对象 -->
   <p v-for="(value,key,index) in student" :key="key">{{value}}--{{key}}--{{index}}</p>
   ```

   

3. 迭代数字

   ```vue
   <!-- 迭代数字 -->
   <div v-for="(count,index) in 10" :key="count">{{count}}--{{index}}</div>
   ```



4. key属性

   vue 用v-for正在更新的元素列表它默认使用"就地复用"策略 如果数据项的顺序被改变，vue将不是移动dom元素来匹配数据项的顺序，而是简单用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素，为了给vue一个提示以便他能跟踪每个节点的身份，从而重用和重新排序现有元素，需要为每项提供一个唯一key属性

   ```vue
   <template>
   	<div id="app">
       	<input type="text" v-model="id">
       	<input type="text" v-model="name">
       	<button @click="add">添加</button>
       	<p v-for="item in test" :key="item.id">
           	<input type="checkbox"> 姓名:{{item.name}}
       	</p>
       </div>
   </template>
   
   <script>
       export default {
           name: 'app',
           data () {
               return {
                   test:[
                       {
                           id:1,
                           name:"wisdomzj"
                       },
                       {
                           id:2,
                           name:"smartzj"
                       }
                   ],
               }
           },
           methods:{
               add(){
                   this.test.unshift({
                       id:this.id,
                       name:this.name
                   })
               } 
           } 
       }
   </script>
   ```



##### 自定义指令

###### 钩子函数

|     函数名称     |                           功能描述                           |
| :--------------: | :----------------------------------------------------------: |
|       bind       |          只调用一次，在指令第一次绑定到元素上时调用          |
|      update      | 在 bind 之后立即以初始值为参数第一次调用，之后每当绑定值变化时调用，参数为新值与旧值 |
|      unbind      |             只调用一次，在指令从元素上解绑时调用             |
| componentUpdated |     指令所在组件的 VNode **及其子 VNode** 全部更新后调用     |
|     inserted     | 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中) |



###### 参数解析

|   属性值   |             属性描述             |
| :--------: | :------------------------------: |
|     el     |          指令绑定的元素          |
|     vm     |   拥有该指令的上下文 ViewModel   |
| expression | 指令的表达式，不包括参数和过滤器 |
|    arg     |            指令的参数            |
|    name    |      指令的名字，不包含前缀      |
| modifiers  |    一个对象，包含指令的修饰符    |
| descriptor |   一个对象，包含指令的解析结果   |



###### 全局指令

```js
//全局自定义指令
Vue.directive("changebg",{
    //虚拟dom渲染之前执行
    bind:function(el,binding){
        el.style.background = binding.value;
        console.log("绑定值为:",binding.value);
        console.log("绑定的指令名为:",binding.name);
    }
})

Vue.directive("focus",{
    //表示被绑定元素插入父节点时调用
    inserted:function(el){
        el.focus();
    }
})
```



###### 组件指令

```vue
<template>
	<div id="app">
        <input type="text" v-focus>
    	<p v-changebg="'red'">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
               
            }
        },
        directives:{
            "focus":{
                inserted:function(el){
                    el.focus();
                }
            },
            "changebg":{
                bind:function(el,binding){
                    el.style.background = binding.value;
                }
            }
        }
    }
</script>
```



#### 运用修饰符

##### 键盘修饰符

###### 默认键盘修饰符

默认提供的键盘修饰符有 `enter` `tab` `delete` `esc` `space` `up` `down` `left` `right`

```vue
<template>
	<div id="app">
    	输入姓名:<input type="text" v-model="name">
    	<br>
    	输入密码:<input type="password"  v-model="password" @keyup.enter="login">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                name:"",
                password:""  
            }
        },
        methods: {
            login(){
                if(this.name == 'zj' && this.password == "123" ){
                    alert("登陆成功!!!")
                }else{
                    alert("登陆失败!!!")
                }
            }
        }  
    }
</script>
```



###### 自定义键盘修饰符

通过`Vue.config.keyCodes.名称 = 按键值`来自定义按键修饰符的别名

```js
Vue.config.f2 = 113

new Vue({
  el: '#app',
  render: h => h(App)
})
```

```vue
<template>
	<div id="app">
    	输入姓名:<input type="text" v-model="name">
    	<br>
    	输入密码:<input type="password"  v-model="password" @keyup.f2="login">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                name:"",
                password:""  
            }
        },
        methods: {
            login(){
                if(this.name == 'zj' && this.password == "123" ){
                    alert("登陆成功!!!")
                }else{
                    alert("登陆失败!!!")
                }
            }
        }  
    }
</script>
```



##### 事件修饰符

###### stop 

阻止冒泡 阻止inner冒泡触发了outer 和 box的事件发生

```vue
<template>
	<div id="app">
    	<div class="box" @click="foo3">
        	<div class="outer" @click="foo2">
            	<div class="inner" @click.stop="foo1"></div>
        	</div>
    	</div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {

            }
        },
        methods: {
            foo1(){
                console.log("foo1");
            },
            foo2(){
                console.log("foo2");
            },
            foo3(){
                console.log("foo3");
            },
        }  
    }
</script>
```



###### prevent  

阻止默认事件 阻止a标签等等  跳转事件发生

```vue
<template>
	<div id="app">
    	<a href="https://www.baidu.com" @click.prevent="goTobaidu">前往百度</a>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {

            }
        },
        methods: {
            goTobaidu(){
                console.log("阻止跳转...")
            }
        }  
    }
</script>
```



###### capture 

添加事件侦听器时使用事件捕获模，给元素添加一个监听器，当元素发生冒泡时，先触发带有该修饰符的元素。若有多个该修饰符，则由外而内触发,就是谁有该事件修饰符，就先触发谁

```vue
<template>
	<div id="app">
    	<div class="box" @click.capture ="foo4">
        	<div class="outer" @click.capture ="foo3">
            	<div class="inner" @click="foo2">
                	<div class="mininner" @click="foo1"></div>
    			</div>
    		</div>
    	</div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {

            }
        },
        methods: {
            foo1(){
                console.log("foo1");
            },
            foo2(){
                console.log("foo2");
            },
            foo3(){
                console.log("foo3");
            },
            foo4(){
                console.log("foo4");
            },
        }  
    }
</script>
```



###### self 

只当事件在该元素本身触发时才被调用冒泡和捕获的触发都不能（主动或被动）触发该元素绑定的事件 ，self 只会阻止自己被冒泡行为触发本身的事件，并不会真正阻止事件冒泡 这是和stop的最大区别

```vue
<template>
	<div id="app">
    	<div class="box" @click="foo3">
        	<div class="outer" @click="foo2">
            	<div class="inner" @click.self.capture="foo1"></div>
    		</div>
    	</div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {

            }
        },
        methods: {
            foo1(){
                console.log("foo1");
            },
            foo2(){
                console.log("foo2");
            },
            foo3(){
                console.log("foo3");
            },
        }  
    }
</script>
```



###### once 

事件只触发一次

```vue
<template>
	<div id="app">
    	<a href="https://www.baidu.com" @click.prevent.once="goTobaidu">前往百度</a>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {

            }
        },
        methods: {
            goTobaidu(){
                console.log("只能阻止跳转一次...")
            }
        }  
    }
</script>
```



##### v-model修饰符

###### lazy

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步，可以添加 `lazy` 修饰符，从而转变为使用 `change` 事件进行同步 

```vue
<input v-model.lazy="msg" >
```



###### trim

 如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符 

```vue
<input v-model.trim="msg">
```



###### number

 如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符 

```vue
<input v-model.number="age" type="number">
```



#### 样式控制

##### class样式

###### 展示多种样式类 

```vue
<template>
	<div id="app">
    	<p :class="['cl','fs','bg']">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {

            }
        }
    }
</script>

<style lang="scss">
    .cl{
        color: red;
    }
    .fs{
        font-size: 32px;
    }
    .bg{
        background: lightcoral;
    }
</style>  
```



###### 使用三元表达式

```vue
<template>
	<div id="app">
    	<p :class="['cl','fs',isbg?'bg':'']">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                isbg:true
            }
        }
    }
</script>

<style lang="scss">
    .cl{
        color: red;
    }
    .fs{
        font-size: 32px;
    }
    .bg{
        background: lightcoral;
    }
</style>  
```



###### 嵌套对象形式

```vue
<template>
	<div id="app">
    	<p :class="['cl','fs',{'bg':isbg}]">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                isbg:false
            }
        }
    }
</script>

<style lang="scss">
    .cl{
        color: red;
    }
    .fs{
        font-size: 32px;
    }
    .bg{
        background: lightcoral;
    }
</style>  
```



###### 直接使用对象

```vue
<template>
	<div id="app">
    	<p :class="style">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                style:{
                    cl:false,
                    fs:true,
                    bg:false,
                }
            }
        }
    }
</script>

<style lang="scss">
    .cl{
        color: red;
    }
    .fs{
        font-size: 32px;
    }
    .bg{
        background: lightcoral;
    }
</style>  
```



##### style样式

###### 手写书写样式对象

```vue
<template>
	<div id="app">
    	<p :style="{'color':'red','font-size':'48px','background':'lightcoral'}">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                
            }
        }
    }
</script>
```



###### 样式对象定义到 `data` 中

```vue
<template>
	<div id="app">
    	<p :style="style">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                style:{
                    "color": "red",
                    "font-size": "48px",
                    "background": "lightcoral"
                }
            }
        }
    }
</script>
```



###### 数组引用多个 `data` 上的样式对象

```vue
<template>
	<div id="app">
    	<p :style="[cl','fs','bg']">我是p标签</p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                cl:{
                    "color": "red",
                },
                fs:{
                    "font-size": "48px",
                },
                bg:{
                    "background": "lightcoral"	
                }
            }
        }
    }
</script>
```



#### 动画&过渡

##### 过渡效果

```vue
<template>
	<div id="app">
    	<transition>
        	<p v-show="show">测试动画</p>
    	</transition>
    	<button @click="toggle">点击</button>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                show:true
            }
        },
        methods:{
            toggle(){
                this.show = !this.show 
            }
        }
    }
</script>

<style lang="scss">
    .v-enter,.v-leave-to{
        opacity: 0;
    }
   
    .v-enter-active,.v-leave-active{
        transition: opacity 2s;
    }
</style>  
```



##### 帧动画

```vue
<template>
	<div id="app">
    	<transition>
        	<p v-show="show">测试动画</p>
    	</transition>
    	<button @click="toggle">点击</button>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                show:true
            }
        },
        methods:{
            toggle(){
                this.show = !this.show 
            }
        }
    }
</script>

<style lang="scss">
    @keyframes run{
        0% {
            transform: scale(0);
        }
        50% {
            transform: scale(1.5);
        }
        100%{
            transform: scale(1);
        }
    }
    .v-enter-active{
        transform-origin:left center; 
        animation: run 1s;
    }
    .v-leave-active{
        transform-origin:left center; 
        animation: run 1s reverse;
    }
</style>  
```



##### 第三方库

```vue
<template>
	<div id="app">
    	<transition     
                enter-active-class="animated bounce"
                leave-active-class="animated jello">
        	<p v-show="show">测试动画</p>
    	</transition>
    	<button @click="toggle">点击</button>
    </div>
</template>

<script>
    import "./assets/css/animate.css"
    export default {
        name: 'app',
        data () {
            return {
                show:true
            }
        },
        methods:{
            toggle(){
                this.show = !this.show 
            }
        }
    }
</script>

<style lang="scss">
    
</style>  
```



##### 属性解析

name：自定义前缀
appear&appear-active-class：页面开始加载显示入场动画
type：以哪种动画方式为（出场和入场）动画结束时间
duration ：自定义（出场和入场）动画结束时间 ==>  {leave："出场动画时间",enter："入场动画时间"}

```vue
<template>
	<div id="app">
    	<transition
                name = "zj"
                :duration="{enter:10000,leave:5000}"
                type ="transition"
                appear 
                appear-active-class="animated bounce zj-enter-active"
                enter-active-class="animated bounce zj-enter-active"
                leave-active-class="animated jello zj-leave-active">
        >
        	<p v-show="show">测试动画</p>
    	</transition>
    	<button @click="toggle">点击</button>
    </div>
</template>

<script>
    import "./assets/css/animate.css"
    export default {
        name: 'app',
        data () {
            return {
                show:true
            }
        },
        methods:{
            toggle(){
                this.show = !this.show 
            }
        }
    }
</script>

<style lang="scss">
    .zj-enter-active,.zj-leave-active{
        transition: opacity 2s;
    }
</style> 
```



##### JS控制动画

```vue
<template>
	<div id="app">
    	<transition     
                @before-enter="handleBeforeenter"
                @enter="handleEenter"
                @after-enter="handleAfterenter"
                @before-leave="handleBeforeleave"
                @leave="handleLeave"
                @after-leave="handleAfterleave">
        	<p v-show="show">测试动画</p>
    	</transition>
    	<button @click="toggle">点击</button>
    </div>
</template>

<script>
    import "./assets/js/velocity.min.js"
    export default {
        name: 'app',
        data () {
            return {
                show:true
            }
        },
        methods:{
            toggle(){
                this.show = !this.show 
            },
            handleBeforeenter(el){
                el.style.opacity = 0;
            },
            handleEenter(el,done){
                Velocity(el,{opacity:1},{duration:2000,complete:done})
            },
            handleAfterenter(el){
                console.log("入场动画结束..")
            },
            handleBeforeleave(el){
                el.style.opacity = 1;
            },
            handleLeave(el,done){
                Velocity(el,{opacity:0},{duration:2000,complete:done})
            },
            handleAfterleave(el){
                console.log("出场动画结束..")
            }
        }
    }
</script>

<style lang="scss">

</style>  
```



##### 多元素过渡

```vue

```



##### 列表过渡

```vue

```



#### VUE实例

##### 创建实例

```js
var app = new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```



##### 实用属性

```js
console.log("data数据:",app.$data);
console.log("管辖范围:",app.$el);
console.log("组件传递参数:",app.$props);
console.log("实例配置参数:",app.$options);
console.log("根对象:",app.$root === app);
console.log("子组件:",app.$children);
console.log("插槽:",app.$slots);
console.log("指定组件:",app.$refs);
console.log("服务端渲染:",app.$isServer)
```



##### 实用方法

```js
app.$watch(expOrFn, callback, { options }) // 监听
app.$on(event, callback) // 接受事件
app.$emit(event, ${[…args]}) // 广播事件
app.$once(event, callback) // 执行一次事件触发
app.$forceUpdate() // 强制组件渲染
app.$set() // 设置数据在data上挂载
app.$delete(object, key) //删除属性
app.$nextTick(callback) // 相当于window.onload
```



#### 生命周期

##### 认识生命周期

Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、销毁等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。理解组件的生命周期，有利于我们了接到 vue 在创建组件的过程，以及使用生命周期钩子赋予我们更多的能力

![](https://cn.vuejs.org/images/lifecycle.png)



##### 钩子函数解析

```vue
<template>
	<div>
    	<input type="text" v-model="message">
    </div>
</template>

<script>
    export default {
        data(){
            return {
                message:"this is msg"
            }
        },
        methods: {

        },
        /*
            beforeCreate 钩子在组件的初始化前运行， data 还没被附加上 reactvie 特型，events 也还没建立好
		   created钩子中，你能够访问 reactive data 和 events，但是模板和虚拟DOM无法访问	
        */
        beforeCreate: function() {
            console.group('------beforeCreate创建前状态------');
            console.log("%c%s", "color:red" , "el     : " + this.$el); //undefined
            console.log("%c%s", "color:red","data   : " + this.$data); //undefined 
            console.log("%c%s", "color:red","message: " + this.message) //undefined
        },
        created: function() {
            console.group('------created创建完毕状态------');
            console.log("%c%s", "color:red","el     : " + this.$el); //undefined
            console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化 
            console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
        },
        /*
            beforeMount钩子在初始渲染发生之前和模板或渲染函数被编译之后运行
		   mounted钩子，你将拥有访问组件模板能力，mounted 钩子是经常使用的生命周期钩子	
        */
        beforeMount() {
            console.group('------beforeMount挂载前状态------');
            console.log("%c%s", "color:red","el     : " + (this.$el)); //已被初始化
            console.log(this.$el);
            console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化  
            console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
        },
        mounted: function() {
            console.group('------mounted 挂载结束状态------');
            console.log("%c%s", "color:red","el     : " + this.$el); //已被初始化
            console.log(this.$el);    
            console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
            console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
        },
        /*
            beforeUpdate 钩子在组件的数据更改之后运行，更新周期开始，就在DOM修改和重新渲染之前，它允许实际渲染之前获取组件上任何反应数据的新状态
		   Update钩子在组件和DOM重新呈现数据更改后运行，如果需要在属性更改后访问DOM，这可能是最安全的做法	
        */
        beforeUpdate: function () {
            console.group('beforeUpdate 更新前状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);   
            console.log("%c%s", "color:red","data   : " + this.$data); 
            console.log("%c%s", "color:red","message: " + this.message); 
        },
        updated: function () {
            console.group('updated 更新完成状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el); 
            console.log("%c%s", "color:red","data   : " + this.$data); 
            console.log("%c%s", "color:red","message: " + this.message); 
        },
        /*
        	销毁挂钩允许您在组件销毁时执行操作，例如清理或分析发送，当组件被拆除并从DOM中删除时，它们将触发
		    beforeDestroy 在拆卸组件之前被回掉，组件仍将完全存在。 如果需要清理事件或取消订阅，则可能是Destroy可能要执行此操作
        */
        beforeDestroy: function () {
            console.group('beforeDestroy 销毁前状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);    
            console.log("%c%s", "color:red","data   : " + this.$data); 
            console.log("%c%s", "color:red","message: " + this.message); 
        },
        destroyed: function () {
            console.group('destroyed 销毁完成状态===============》');
            console.log("%c%s", "color:red","el     : " + this.$el);
            console.log(this.$el);  
            console.log("%c%s", "color:red","data   : " + this.$data); 
            console.log("%c%s", "color:red","message: " + this.message)
        }
    }
</script>
```



#### 过滤器

##### 全局过滤器

```js
// 在Vue中使用过滤器（Filters）来渲染数据是一种很有趣的方式,首先我们要知道Vue中的过滤器不能替代Vue中的methods、computed或者watch过滤器不改变真正的data,而只是改变渲染后的结果，并返回过滤后的信息
Vue.filter("toDollar",money => {
    return `$${money}`
})

new Vue({
    el: '#app',
    render: h => h(App)
})    
```

```vue
<template>
	<div id="app">
    	<span>{{ money | toDollar }}</span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                money:66
            }
        }
    }
</script>
```



##### 组件过滤器

```vue
<template>
	<div id="app">
    	<span>{{ money | toDollar }}</span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                money:66
            }
        },
        filters:{
            toDollar: money => {
                return `$${money}`
            }
        }
    }
</script>
```



##### 过滤器参数

```vue
<template>
	<div id="app">
    	<span>{{ date | formatDate("yyyy-mm-dd") }}</span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                date: new Date()
            }
        },
        filters:{
            formatDate:(dateStr,pattern="")=>{
                let dt = new Date(dateStr),
                    y = dt.getFullYear(),
                    mon = (dt.getMonth()+1).toString().padStart(2,"0"),
                    d = (dt.getDate()).toString().padStart(2,"0"),
                    h = dt.getHours(),
                    min = (dt.getMinutes().toString()).padStart(2,"0"),
                    s = (dt.getSeconds().toString()).padStart(2,"0")

                if(pattern.toLowerCase()==="yyyy-mm-dd"){
                    return `${y}-${mon}-${d}`;
                }else{
                    return `${y}-${mon}-${d} ${h}:${min}:${s}`;
                }
            }
        }
    }
</script>
```



##### 过滤器串联

```vue
<template>
	<div id="app">
    	<span>{{ money | toFixed(2) | toDollar }}</span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                money:66.5367
            }
        },
        filters:{
            toDollar: money => {
                return `$${money}`
            },
            toFixed: (money,limit) => {
                return money.toFixed(limit);
            },
        }
    }
</script>
```



#### 计算属性

##### 认识计算属性

计算属性里可以完成各种复杂的逻辑，包括运算、函数调用等，只要最终就返回一个结果

```vue
<template>
	<div id="app">
    	<span>{{ Message }}</span>
    	<span>{{ reversedMessage }}</span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                Message:"my name is zhangjun"
            }
        },
        computed: {
            reversedMessage(){
                return this.Message.split('').reverse().join('')
            }
        }
    }
</script>
```



##### 依赖多个数据

计算属性还可以依赖多个Vue 实例的data中的数据，只要其中任一数据变化，计算属性就会重新执行，视图也会随之更新

```vue
<template>
	<div id="app">
    	<input type="text" v-model="firstname">
    	<input type="text" v-model="lastname">
    	<span> {{ fullname }} </span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                firstname:"ZHANG",
                lastname:"JUN"
            }
        },
        computed: {
            fullname(){
                console.log("触发了计算属性");
                return this.firstname + " " + this.lastname
            }
        }
    }
</script>
```



##### get&set解析

每一个计算属性都包含一个getter 和一个setter ，都是计算属性的默认用法， 只是利用了getter来读取，在你需要时，也可以提供一个setter 函数， 当手动修改计算属性的值就像修改一个普通数据那样时，就会触发setter 函数

```vue
<template>
	<div id="app">
        <input type="text" v-model="firstname">
    	<input type="text" v-model="lastname">
    	<input type="text" v-model="fullname">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                fistname:"ZHANG",
                lastname:"JUN"
            }
        },
        computed: {
            fullname:{
                get(){
                    return this.firstname+" "+this.lastname;
                },
                set(value){
                    let names = value.split(" ");
                    this.firstname = names[0];
                    this.lastname = names[1];
                }
            }
        }
    }
</script>
```



##### 缓存机制

我们为什么需要缓存？假设我们有一个性能开销比较大的的计算属性 x，它需要遍历一个巨大的数组并做大量的计算，然后我们可能有其他的属性依赖于 x，如果没有缓存，我们将不可避免的多次执行 x 的 getter，如果不希望有缓存，可以用方法来替代

```vue
<template>
	<div id="app">
    	<span> {{ getName() }} </span>
    	<span> {{ fullname }} </span>
        <input type="text" v-model="firstname">
        <input type="text" v-model="lastname">
        <input type="text" v-model="number">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                firstname:"ZHANG",
                lastname:"JUN",
                number:0
            }
        },
        methods: {
            getName(){
                console.log("函数触发了")
                return `${this.firstname} ${this.lastname}`
            }
        }
        computed: {
            fullname(){
                console.log("计算属性触发了")
                return `${this.firstname} ${this.lastname}`
            }
        }
    }
</script>
```



#### 侦听器

##### 认识侦听器

```vue
<template>
	<div id="app">
    	<input type="text" v-model="firstname">
    	<input type="text" v-model="lastname">
    	<span> {{ fullname }} </span>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                firstname:"ZHANG",
                lastname:"JUN",
                fullname:"ZHANG JUN"
            }
        },
        watch: {
            firstname(){
                this.fullname = this.firstname + " " + this.lastname
            },
            lastname(){
                this.fullname = this.firstname + " " + this.lastname
            }
        }
    }
</script>
```



##### 参数解析

```vue
<template>
	<div id="app">
    	<input type="text" v-model="msg">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                msg:"this is msg"
            }
        },
        watch: {
            msg(newv,oldv){
                console.log("旧值:" + oldv);
                console.log("新值:" + newv);
            }
        }
    }
</script>
```



##### 解析侦听器

```vue
<template>
	<div id="app">
    	<input type="text" v-model="msg">
        <input type="text" v-model="obj.name">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                msg:"this is msg",
                obj:{
                    name:"zj"
                }
            }
        },
        watch: {
            msg:{
                handler(newv,oldv){
               		console.log("旧值:" + oldv)
                	console.log("新值:" + newv)
                	console.log("开启了首次监听")
                },
                //开启首次开始监听,而不是被监听的属性发生变化在开始监听
                immediate:true, 
            },
            obj:{
                handler(newv,oldv){
                	console.log("obj.a is change...")
                },
                //被监听的值为对象，如果修改了某个属性，监听的整个对象不会发生改变，当然属性也没改变，只有整个对象的引用被改变的话，在能被见监听到，如果非要修改对象中的某个属性值且还要被监听到这时需要开启深度观察
                deep:true 
            },
        }
    }
</script>
```



##### 缓存机制

当页面模板重新渲染，如果被监听的值没有变化则使用缓存中的值，如果发生变化就用修改后的值

```vue
<template>
	<div id="app">
    	<span> {{ getName() }} </span>
    	<span> {{ fullname }} </span>
        <input type="text" v-model="firstname">
        <input type="text" v-model="number">
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                firstname:"ZHANG",
                lastname:"JUN",
                fullname:"ZHANG JUN"
                number:0
            }
        },
        methods: {
            getName(){
                console.log("函数触发了")
                this.fullname = this.firstname+" "+this.lastname
            }
        }
        watch: {
            firstname(){
                console.log("侦听器触发了")
                this.fullname = this.firstname+" "+this.lastname
            }
        }
    }
</script>
```



#### 组件学习

##### 定义组件

###### 全局组件

全局注册组件方式

```js
//使用 Vue.extend 配合 Vue.component 方法
let mycom = Vue.extend({
	template:"<p>使用Vue.extend配合Vue.component方法</p>"
})
Vue.component("Mycom",mycom)

//直接使用 Vue.component方法
Vue.component("Mycom",{
    template:"<p>直接使用 Vue.component方法</p>"
})

//将模板字符串，定义到html中，便于代码提示
<template id="tmpl">
    <h1>将模板字符串，定义到script标签中</h1>
</template>
Vue.component("Mycom",{
    template:"#tmpl"
})
```



注册全局组件 封装一个动画组件

```js
import "./assets/js/velocity.min.js"
Vue.component("fade",{
  template:`<transition
        @before-enter="handleBeforeEnter"
        @enter="handleEenter"
        @before-leave="handleBeforeLeave"
        @leave="handleLeave"
  >
    <slot v-if="show"></slot>
  </transition>`,
  props:["show"],
  methods: {
    handleBeforeEnter(el){
        el.style.opacity = 0;
    },
    handleEenter(el,done){
        Velocity(el,{opacity:1},{duration:1500,complete:done})
    },
    handleBeforeLeave(el){
        el.style.opacity = 1;
    },
    handleLeave(el,done){
        Velocity(el,{opacity:0},{duration:1500,complete:done})
    }
  }
})    
```



使用注册好的全局组件

```vue
<template>
	<div id="app">
        <fade :show="show">
        	<p v-if="show">我是p标签</p>
    	</fade>
    <button @click="toggle">toggle</button>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                show:true
            }
        },
        methods: {
            toggle(){
                this.show = !this.show
            }
        }
    }
</script>
```



###### 子组件

vue 文件方式创建子组件

```vue
<template>
  	<div>
        <span>我是子组件</span>
    </div>
</template>

<script>
export default {
    data(){
        return {

        }
    },
    methods: {
        
    }
}
</script>

<style>

</style>
```



使用子组件

```vue
<template>
	<div>
    	<span>我是父组件</span>
    	<Son></Son>
    </div>
</template>

<script>
    import Son from "./components/son"
    export default {
        data(){
            return {

            }
        },
        methods: {

        },
        components:{
            Son
        },
    }
</script>

<style>

</style>
```



##### 动态组件

###### 方式一

使用flag（自定义的变量）标识结合v-if&v-else切换组件

```vue
<template>
	<div>
    	<a href="#" @click="flag=true">登陆</a>
    	<a href="#" @click="flag=false">注册</a>
    	<login v-if="flag"></login>
    	<register v-else="flag"></register>
    </div>
</template>

<script>
    import login from "./components/login"
    import register from "./components/register"
    export default {
        data(){
            return {
                flag:true
            }
        },
        methods: {

        },
        components:{
            login,
            register
        }
    }
</script>

<style>

</style>
```



###### 方式二

使用 :is 属性来切换不同组件 

```vue
<template>
	<div>
    	<a href="#" @click="comName='login'">登陆</a>
    	<a href="#" @click="comName='register'">注册</a>
    	<login></login>
    	<register></register>
    	<component :is="comName"></component>
    </div>
</template>

<script>
    import login from "./components/login"
    import register from "./components/register"
    export default {
        data(){
            return {
                comName:login
            }
        },
        methods: {

        },
        components:{
            login,
            register
        }
    }
</script>

<style>

</style>
```



##### 组件插槽

###### 匿名插槽

```vue
# child
<template>
	<div class="borsty">
        // 插槽模板是slot，它是一个空壳子,因为它显示与隐藏以及最后用什么样的html模板显示由父组件控制,但是插槽显示的位置确由子组件自身决定，slot写在组件template的哪块，父组件传过来的模板将来就显示在哪块
    	<slot></slot>
    </div>
</template>

<script>
    export default {
        data(){
            return {
               
            }
        },
        methods: {

        }
    }
</script>

<style>
    .borsty{
        border:1px solid #ccc;
        height:200px;
        width:200px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
```

```vue
# parents
<template>
	<div id="app">
    	<Child>
        	<p class="psty">自定义插槽内容</p>
    	</Child>
    </div>
</template>

<script>
    import Child from "./components/child"
    export default {
        data(){
            return {
               
            }
        },
        methods: {

        },
        components:{
            Child 
        }
    }
</script>

<style>
    .psty{
        background: red;
        height: 30px;
        text-align: center;
        line-height: 30px;
        font-size: 16px;
    }
</style>
```



###### 具名插槽

```vue
# child
<template>
	<div class="borsty">
    // 匿名插槽没有name属性,所以是匿名插槽，那么插槽加了name属性，就变成了具名插槽,具名插槽可以在一个组件中出现N次,出现在不同的位置
    	<slot name="header"></slot>
    	<slot name="footer"></slot>
    </div>
</template>

<script>
    export default {
        data(){
            return {
               
            }
        },
        methods: {

        }
    }
</script>

<style>
    .borsty{
        border:1px solid #ccc;
        height:200px;
        width:200px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
```

```vue
# parents
<template>
	<div id="app">
    	<Child>
        	<div class="header somesty" slot="header">
            	我是头部插槽内容
    		</div>
        	<div class="footer somesty" slot="footer">
            	我是脚部插槽内容
    		</div>
    	</Child>
    </div>
</template>

<script>
    import Child from "./components/child"
    export default {
        data(){
            return {
               
            }
        },
        methods: {

        },
        components:{
            Child 
        }
    }
</script>

<style>
    .somesty{
        height: 50px;
        width: 150px;
        text-align: center;
        line-height: 50px;
        font-size: 16px;
        color:#ffffff;
    }
    .header{
        background: orangered;
    }
    .footer{
        background: hotpink;
    }
</style>
```



###### 作用域插槽

```vue
# child
<template>
	<div class="borsty">
    	<slot :value="value" :msg="msg" :num="666"></slot>
    </div>
</template>

<script>
    export default {
        props:["msg"],
        data(){
            return {
               value:"this is child data"
            }
        },
        methods: {

        }
    }
</script>

<style>
    .borsty{
        border:1px solid #ccc;
        height:200px;
        width:200px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
```

```vue
# parents
<template>
	<div id="app">
    	<Child :msg="msg">
        	<p class="psty" slot-scope="props">{{props.value}}--{{props.msg}}--{{props.num}}</p>
    	</Child>
    </div>
</template>

<script>
    import Child from "./components/child"
    export default {
        data(){
            return {
               msg:"this is parents data"
            }
        },
        methods: {

        },
        components:{
            Child 
        }
    }
</script>

<style>
    .psty{
        background: red;
        height: 30px;
        text-align: center;
        line-height: 30px;
        font-size: 16px;
    }
</style>
```



##### 父子组件沟通

###### 父向子传递数据

```vue
#parents
<template>
	<div id="app">
        <span>我是父组件</span>
    	<Child :msg="msg" :foo="foo" :test="test" :parents="this"></Child>
    </div>
</template>

<script>
    import Child from "./components/child"
    export default {
        data(){
            return {
               msg:"this is parents msg"
            }
        },
        methods: {
		   foo(){
                alert("我是父组件的foo方法")
            },
            test(data){
                console.log(data)
            }
        },
        components:{
            Child 
        }
    }
</script>

<style>
    
</style>
```

```vue
#child
<template>
	<div id="app">
    	<span>我是子组件</span>
    	<span>{{ msg }}</span>
    	<button @click="foo">执行父元素方法</button>
    	<button @click="test('123')">执行父元素传参方法</button>
    	<button @click="getParents">获取父组件实例</button>
    </div>
</template>

<script>
    export default {
        props:["msg","foo","test","parents"],
        data(){
            return {

            }
        },
        methods: {
            getParents(){
                console.log(this.$props.parents)
            }
        }
    }
</script>

<style>

</style>
```



###### 子向父变相传递数据

```vue
#parents 好像测试无效
<template>
	<div id="app">
        <span>我是父组件</span>
        <span>{{ msg }}</span>
    	<Child @foo="foo"></Child>
    </div>
</template>

<script>
    import Child from "./components/child"
    export default {
        data(){
            return {
               msg:"this is parents msg"
            }
        },
        methods: {
		   foo(data){
                this.msg = data
            }
        },
        components:{
            Child 
        }
    }
</script>

<style>
    
</style>
```

```vue
#child
<template>
	<div id="app">
    	<span>我是子组件</span>
    	<button @click="carried">执行父元素方法</button>
    </div>
</template>

<script>
    export default {
        data(){
            return {
			   msg:"this is child msg"
            }
        },
        methods: {
            carried(){
                this.$emit("foo",this.msg);
            }
        }
    }
</script>

<style>

</style>
```



###### 父获取子中数据和方法

```vue
#parents 
<template>
	<div id="app">
        <span>我是父组件</span>
    	<Child ref="Child"></Child>
        <button @click="getChildData">获取子组件数据和方法</button>
    </div>
</template>

<script>
    import Child from "./components/child"
    export default {
        data(){
            return {
               
            }
        },
        methods: {
		   getChildData(){
                this.$refs.Child.carried();
      		   console.log(this.$refs.Child.msg)
            }
        },
        components:{
            Child 
        }
    }
</script>

<style>
    
</style>
```

```vue
#child
<template>
	<div id="app">
    	<span>我是子组件</span>
    </div>
</template>

<script>
    export default {
        data(){
            return {
			   msg:"this is child msg"
            }
        },
        methods: {
            carried(){
                console.log("this is child carried")
            }
        }
    }
</script>

<style>

</style>
```



###### 子获取父中的数据和方法

```vue
#parents 
<template>
	<div id="app">
    	<span>我是父组件</span>
    	<Child></Child>
    </div>
</template>

<script>
    import Child from "./components/child"
    export default {
        data(){
            return {
                msg:"this is parents msg"
            }
        },
        methods: {
            carried(){
                console.log("this is parents carried")
            }
        },
        components:{
            Child 
        }
    }
</script>

<style>

</style>
```

```vue
#child
<template>
	<div id="app">
    	<span>我是子组件</span>
    	<button @click="getParentsData">获取父组件数据和方法</button>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {
            getParentsData(){
                this.$parent.carried()
                console.log(this.$parent.msg)
            }
        }
    }
</script>

<style>

</style>
```



##### 跨层级父子组件沟通

###### provide / inject

###### provide 传递值失效原因

###### provide 传递值在父元素更新子元素不更新问题解决



##### 非父子组件沟通

###### 数据广播

###### 数据共享

###### 本地存储



#### 路由学习

##### 集成路由

###### 安装路由

直接下载cdn（提供了基于 NPM 的 CDN 链接）

```js
<script src="/path/to/vue.js"></script>
<script src="/path/to/vue-router.js"></script>
```

npm（生态市场上下载）

```shell
npm install vue-router
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

构建开发版 （GitHub 上直接 clone，然后自己 build 一个 vue-router）

```js
git clone https://github.com/vuejs/vue-router.git node_modules/vue-router
cd node_modules/vue-router
npm install
npm run build
```



###### 定义路由

```js
# routes.js
import A from "../components/a.vue"
import B from "../components/b.vue"
import C from "../components/c.vue" 

export default [
    { path: '/a', component: A },
    { path: '/b', component: B },
    { path: '/c', component: C }
]
```



###### 路由实例

```js
# router.js
import Router from 'vue-router'
import routes from './routes'

export default ()=>{
    return new Router({
        routes
    })
}
```



###### 使用路由

```js
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router'
import createRouter from './router/router'
const router = createRouter();
Vue.use(VueRouter)

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

```vue
<template>
	<div id="app">
    	<span>我是app组件</span>
    	<router-link to="/a">a组件</router-link>
    	<router-link to="/b">b组件</router-link>
    	<router-link to="/c">c组件</router-link>
    	<router-view></router-view>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {

        }
    }
</script>

<style lang="scss">

</style>
```



##### 路由实例配置

###### 自定义高亮显示

###### 设置history模式

###### scrollBehavior

###### parseQuery 

######  stringifyQuery 

###### fallback



##### 路由项配置

###### 路由嵌套

```js
# routes.js
import User from "../user.vue" 
import Login from "../components/login.vue" 
import Register from "../components/register.vue" 

export default [
    { 
        path: '/user', 
        component: User,
        children:[
            {
                path:"login",
                component:Login
            },
            {
                path:"register",
                component:Register
            }
        ]
    }
]
```



###### 命名视图

```js
# routes.js
import Header from "../components/header.vue"
import Footer from "../components/footer.vue"
import Left from "../components/left.vue"

export default [
    {
        path: '/combination', 
        components: {
            Header,
            Footer,
            Left
        }
    }
]
```



###### 具名路由

```js
# routes.js
import User from "../components/user.vue"

export default [
    {
        path: '/user',
        name: 'user',
        component: User
    }
]
```



###### 路由重定向

```js
# routes.js
import A from "../components/a.vue"
import B from "../components/b.vue"
import C from "../components/c.vue" 

export default [
    {
        path:"/",
        redirect: "/a"
    },
    { 
        path: '/a', 
        component: A
    },
    { 
        path: '/b', 
        component: B
    },
    { 
        path: '/c', 
        component: C
    }
]
```



##### 路由传递参数

###### Get传值

```js
# routes.js
import Login from "../components/login.vue"

export default [
    {
        path: '/login',
        component: Login
    }
]
```

```vue
# App
<template>
	<div id="app">
        <router-link to="/login?id=666&name=zj">登陆</router-link>
    	<router-view></router-view>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {

        }
    }
</script>

<style lang="scss">

</style>
```

```vue
# login
<template>
	<div id="app">
        <span>login组件</span>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {

        },
        mounted(){
            console.log(this.$route.query)
        }
    }
</script>

<style lang="scss">

</style>
```



###### 动态路由

```js
# routes.js
import Login from "../components/login.vue"

export default [
    {
        path: '/login/:id/:name',
        component: Login
    }
]
```

```vue
# App
<template>
	<div id="app">
        <router-link to="/login/123/zj">登陆</router-link>
    	<router-view></router-view>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {

        }
    }
</script>

<style lang="scss">

</style>
```

```vue
# login
<template>
	<div id="app">
        <span>login组件</span>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {

        },
        mounted(){
            console.log(this.$route.params)
        }
    }
</script>

<style lang="scss">

</style>
```



##### 编程式导航

###### 声明式&编程式

```js
# routes.js
import Test1 from "../components/test1.vue"
import Test2 from "../components/test2.vue"

export default [
    {
        path: '/test1',
        component: Test1
    },
    {
        path: '/test2',
        component: Test2
    }
]
```

```vue
<template>
	<div id="app">
        <span>我是app组件</span>
        <router-link to="/test1">声明式导航</router-link>
        <button @click="navigation">编程式导航</button>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {
            navigation(){
                this.$router.push("test2")
            }
        }
    }
</script>

<style lang="scss">

</style>
```



###### 添加&替换记录

replace和push很像，写法一样.但实际效果不一样.push是向history里添加新记录.而replace是直接将当前浏览器history记录替换掉，用push方法,页面1跳转到页面2,你使用浏览器的后退可以回到页面1 ， 用replace方法,页面1被替换成页面2,你使用浏览器的后退，此时你回不到页面1，只能回到页面1的前一页,页面0 

```js
# routes.js
import A from "../components/a.vue"
import B from "../components/b.vue"
import C from "../components/c.vue"
import D from "../components/d.vue"

export default [
    { 
        path: '/a', 
        component: A
    },
    { 
        path: '/b', 
        component: B
    },
    { 
        path: '/c/:id',
        name：c
        component: C
    },
    { 
        path: '/d', 
        component: D
    }
]
```

```vue
<template>
	<div id="app">
        <span>我是app组件</span>
        <button @click="navigation1">字符串形式</button>
        <button @click="navigation2">对象形式</button>
        <button @click="navigation3">命名路由动态传参形式</button>
        <button @click="navigation4">带查询参数</button>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {
            navigation1(){
                this.$router.push("a")
            },
            navigation2(){
                this.$router.push({ path: 'b' })
            },
            navigation3(){
                this.$router.push({ name: 'c', params: { id: 123 }})
            },
            navigation4(){
                this.$router.push({ path: 'd', query: { id: 123 }})
            }
        }
    }
</script>

<style lang="scss">

</style>
```



###### 倒退&前进记录

 history记录中前进或后退多少步.类似window.history.go(n).这样就能控制页面前进或者后退多少步 

```js
# routes.js
import Test from "../components/test.vue"

export default [
    {
        path: '/test',
        component: Test
    }
]
```

```vue
<template>
	<div id="app">
        <span>我是app组件</span>
        <button @click="go">前进</button>
        <button @click="back">后退</button>
        <router-link to="/test">测试路由</router-link>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        data(){
            return {

            }
        },
        methods: {
            go(){
                this.$router.go(1)
            },
            back(){
                this.$router.go(-1)
            }
        }
    }
</script>

<style lang="scss">

</style>
```



##### 运用路由守卫

###### 全局守卫

###### 路由独享守卫

###### 组件内守卫



#### 数据请求

##### 集成axios

###### 单独使用

```shell
npm install axios
import axios from 'axios'
```



###### 全局使用

1. 插件形式

   ```js
   import axios from 'axios'
   import VueAxios from 'vue-axios'
   
   Vue.use(VueAxios,axios);
   
   this.axios.get('api/test').then((res)=>{
       console.log(res.data)
   }).catch((err)=>{
       console.log(err);
   })
   ```

   

2. 挂载axios

   ```js
   import axios from 'axios'
   Vue.prototype.$axios = axios
   
   this.$axios.get('api/test').then((res)=>{
       console.log(res.data)
   }).catch((err)=>{
       console.log(err);
   })
   ```

   

##### 相关配置

###### 实例配置

```js
let instance = axios.create({
    baseURL:"", //公共地址头
    timeout:1000, //超出时间
    url:"xxx", //请求地址
    method:"get", //请求方式
    // 设置请求头
    headers:{
        token:"xxxxx" 
    },
    // 拼接url参数
    params:{

    },
    // 参数放置在请求体
    data:{

    }
)  

// 修改实例配置
instance.default.timeout = 1000
instance.default.baseURL = "xxx"
...
```



###### 全局配置

```js
axios.default.timeout = 1000
axios.default.baseURL = "XXX"
....
```



##### 请求操作

###### get/delete请求

```js
axios.get('/get',{
    params:{
        id:666
    }
}).then((res)=>{
    console.log(res)
})

axios({
    method:'get',
    url:'/get',
    params:{
        id:666
    }
}).then(res=>{
    console.log(res)
})
```



###### post/put/puach请求

```js
// json
axios.post('/post',{
    name:"zj",
    age:"23"
}).then(res=>{
    console.log(res)
})

axios({
    method:'post',
    url:'/post',
    data:{
        name:"zj",
        age:"23"
    }
}).then(res=>{
    console.log(res)
})

// form-data
let formData = new FormData()
for(let key in data){
    formData.append(key,data[key])
}
axios.post('/post',formData).then(res=>{
    console.log(res)
})
```



###### 并发请求

```js
axios.all(
    [
        axios.get('/test1'),
        axios.get('/test2')
    ]
).then(
    axios.spread((res1,res2)=>{
        console.log(res1,res2) 
    })
)
```



###### 取消请求（了解）

```js
let source = axios.CancelToken.source()

axios.get('/xxx',{
    cancelToken:source.token
}).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})

// 取消请求(message可选)
source.cancel('cancel http')
```



##### 使用拦截器

###### 请求拦截

```js
instance.interceptors.request.use(config => {
    // 拦截请求的一些业务处理
    return config;
},(err)=>{
    return Promise.reject(err);
})
```



###### 响应拦截

```js
instance.interceptors.response.use(res => {
    // 拦截响应的一些业务处理
    return res
},(err)=>{
    return Promise.reject(err);
})
```



##### 请求统一管理

###### 定义请求业务

```js
# api.js
const CONTACT_API = {
    proList:{
        method:'get',
        url:'/proList'
    },
    ProDetail:{
        method:'post',
        url:'/proDetail'
    },
    addCart:{
        method:'get',
        url:'/addCart'
    },
    cartCount:{
        method:'get',
        url:'/cartCount'
    },
    cartList:{
        method:'get',
        url:'/cartList'
    },
    addcartNum:{
        method:'get',
        url:'/addcartNum'
    },
    redcartNum:{
        method:'get',
        url:'/redcartNum'
    },
    orderInfo:{
        method:'post',
        url:'/orderInfo'
    },
    selorderinfo:{
        method:'get',
        url:'/selorderinfo'
    }
}
export default CONTACT_API
```



###### 封装请求方法

```js
# http.js
import axios from 'axios'
import service from './api'
import { Toast } from 'vant'
import 'vant/lib/index.css'

let instance = axios.create({
    baseURL:'http://localhost:3000/api',
    timeout:1000
})

const Http = {}; 

for(let key in service){
    let api = service[key]; 
    Http[key] = async function(
        params, 
        isFormData=false,
        config={}
    ){
        let newParams = {}
        if(params && isFormData){
            newParams = new FormData()
            for(let i in params){
                newParams.append(i,params[i])
            }
        }else{
            newParams = params
        }
        let response = {}; 
        if(api.method === 'put'|| api.method === 'post'||api.method === 'patch'){
            try{
                response =  await instance[api.method](api.url,newParams,config)
            }catch(err){
                response = err
            }
        }else if(api.method === 'delete'|| api.method === 'get'){
            config.params = newParams
            try{
                response =  await instance[api.method](api.url,config)
            }catch(err){
                response = err
            }
        }
        return response; 
    }
}

instance.interceptors.request.use(config=>{
    Toast.loading({
        mask:false,
        loadingType:'spinner',
        duration:0,
        forbidClick:true, 
        message:'数据加载中...'
    })
    return config;
},(err)=>{
    Toast.clear()
    Toast(err)
})

instance.interceptors.response.use(res=>{
    Toast.clear()
    return res.data
},(err)=>{
    Toast.clear()
    Toast(err)
})

export default Http
```



###### 测试封装结果

```js
# main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import Http from './server/http'

Vue.prototype.$Http = Http
Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```

```VUE
<template>
  <div class="test">
    <h1>This is an test page</h1>
  </div>
</template>

<script>
export default {
  data(){
    return{

    }
  },
  methods:{
      requestData1() {
          this.$Http.getProList().then((res)=>{
              console.log(res)
          })  
      },
      async requestData2() {
          let res = await this.$Http.getProList()
          console.log(res)  
      }
  },
  mounted(){
       this.requestData1()
       this.requestData2()
  }
}
</script>

<style>

</style>
```



#### 状态管理

##### 集成vuex

###### 导出store

```js
import Vue from 'vue'
import Vuex from 'vuex'
import state from './state'
import getters from './getters'
import mutations from './mutation'
import actions from './action'
import modules from './modules'

Vue.use(Vuex)

export default new Vuex.Store({
  strict: true // 设置禁止外部修改  
  state,
  getters,
  mutations,
  actions,
  modules
})
```



###### 挂载实例

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```



##### state&getters

###### state

```js
# state.js
export default {
    count:0,
    firstname:"zhang",
    lastname:"jun"
}
```

```vue
<template>
	<div class="test">
    	<h1>This is an test page</h1>
    	{{count}}{{firstname}}{{lastname}}  
    </div>
</template>

<script>
    import { mapState } from 'vuex'
    export default {
        data(){
            return{

            }
        },
        methods:{

        },
        computed:{
            // ..mapState(['count']) State简便取值
            count(){
                return this.$store.state.count
            },
            firstname(){
                return this.$store.state.count
            },
            lastname(){
                return this.$store.state.count
            }
        }
    }
</script>
```



###### getters

```js
# getters.js
export default {
    fullname(store){
        return `${store.firstname} ${store.lastname}`
    }
}
```

```vue
<template>
	<div class="test">
    	<h1>This is an test page</h1>
    	{{fullname}}
    </div>
</template>

<script>
    import { mapGetters } from 'vuex'
    export default {
        data(){
            return{

            }
        },
        methods:{

        },
        computed:{
            // ···mapGetters(['fullname']) Getters简便取值
            fullname(){
                return this.$store.getters.fullname
            }
        }
    }
</script>
```



##### mutation&action

###### mutation

```js
# mutation.js
export default {
    updateconunt (store,data) {
        store.count += data 
    }
}
```

```vue
<template>
	<div class="test">
    	<h1>This is an test page</h1>
    	<button @click="addcount">addcount</button>
    	{{count}}
    </div>
</template>

<script>
    import { mapMutations } from 'vuex'
    export default {
        data(){
            return{

            }
        },
        methods:{
            ...mapMutations(['updateconunt'])
            addcount(){
        		// this.updateconunt(1)
        		this.$store.commit("updateconunt",1)
        	}
    	}
    }
</script>
```



###### action

```js
# action.js
export default {
    updateconuntAsync (store,data) {
        setTimeout(() => {
            store.commit('updateconunt',data.num)
        },data.time)
    }
}
```

```vue
<template>
	<div class="test">
    	<h1>This is an test page</h1>
    	{{count}}
    </div>
</template>

<script>
    import { mapActions } from 'vuex'
    export default {
        data(){
            return{

            }
        },
        methods:{
			...mapActions(['updateconuntAsync'])
        },
        mounted(){
            // this.updateconuntAsync({num:666,time:2000})
            this.$store.dispatch('updateconuntAsync',{num:666,time:2000})
        }
    }
</script>
```



##### 模块处理

##### 其他配置



#### 服务端渲染



### 项目实践

#### 无人点餐

#### Todo应用

#### 云音乐后台管理系统



