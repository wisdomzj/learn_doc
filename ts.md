# TS学习总结



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



#### 全局安装Ts环境

 ```SHELL
$ npm install typescript@[版本号] -g
 ```



#### 体验Ts（hello world）

```JS
interface Point {
  x: number
  y: number
}

function tsDemo(data: Point) {
  console.log("hello ts")
  return Math.sqrt(data.x ** 2 + data.y ** 2);
}

tsDemo({ x: 1, y: 2 })
```



