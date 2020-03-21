# MongoDB学习总结

[TOC]

### 数据库操作

#### 查看数据库列表

```shell
show dbs
```

![1565530168652](<http://demo.bestzhangjun.com/imgs/node/1565530325689.png>)



#### 使用指定数据库

```shell
use [DataBasename]
```

![1565530585094](<http://demo.bestzhangjun.com/imgs/node/1565530585094.png>)



#### 创建或查看数据库

```js
db //查看当前使用数据
use test //如果没有则创建一个新数据库 
show dbs // 此时不会显示出来，需要我们插入数据才能显示出来
db.stuinfo.insert({name:"lxr",age:18,sex:"女",hobby:"旅游"}) //尝试插入一条数据
show dbs //此时执行命令会显示出来
```

![1565530886289](http://demo.bestzhangjun.com/imgs/node/1565530886289.png)



#### 删除数据库

```js
db //先查看当前使用的是那个数据库
use [数据库名] //使用指定数据库
db.dropDatabase() //删除指定数据库
show dbs //查看是否删除成功
```

![1565531231448](http://demo.bestzhangjun.com/imgs/node/1565531231448.png)

### 集合操作

#### 创建集合

```js
db.createCollection(name, {capped: <Boolean>, autoIndexId: <Boolean>, size: <number>, max <number>})

db.createCollection("test_01") //创建一个名字为test_01的集合，没有任何的大小，数量限制

db.createCollection("test_02",{capped:true,size:1024}) //创建一个名字为test_02集合，限制它的空间大小为1M，如果超过1M的大小，则会删除最早的记录

db.createCollection("test_03",{capped:true,size:1024,max:1024}) //创建一个名字为test_03集合，最大条数为1024条，超过1024再插入数据的话会删除最早的一条记录
```

![1565531470617](http://demo.bestzhangjun.com/imgs/node/1565531470617.png)



#### 查看集合

```js
show collections
```

![1565531731981](http://demo.bestzhangjun.com/imgs/node/1565531731981.png)



#### 删除集合

```js
db.[collectionName].drop()
```

![1565531903222](http://demo.bestzhangjun.com/imgs/node/1565531903222.png)



#### 参数说明（只列举部分，更多属性参照官网）

|    属性     |  类型   |                             描述                             |
| :---------: | :-----: | :----------------------------------------------------------: |
|   capped    | boolean | 可选的，要创建[上限集合](https://docs.mongodb.com/manual/reference/glossary/#term-capped-collection)，请指定`true`，如果指定`true`，则还必须在`size`字段中设置最大大小 |
| autoIndexId | boolean | 可选的。指定`false`禁用在`_id`字段上自动创建索引在MongoDB中4.0开始，你不能设置选项`autoIndexId` ，以`false`在比其他数据库创建集合时 `local`数据库 |
|    size     |   int   | 可选的。为封顶集合指定最大大小（以字节为单位），一旦上限集合达到其最大大小，MongoDB将删除旧文档以为新文档腾出空间，该`size`字段是加盖集合所必需的，对其他集合则是忽略的 |
|     max     |   int   | 可选的。上限集合中允许的最大文档数。该 `size`限制的优先级高于这个限度。如果上限集合`size`在达到最大文档数之前达到限制，MongoDB将删除旧文档。如果您更喜欢使用`max`限制，请确保`size`限制集合所需的限制足以包含最大数量的文档 |
|    name     | string  |                           集合名称                           |



### 文档操作

#### 数据插入

##### 插入一条记录

```js
db.collection.insertOne()

db.stuinfo.insertOne(
	{
		name:"lxr",
		age:18,
		sex:"女",
		hobby:"旅游"
	}
);
```



##### 插入多条记录

```js
db.collection.insertMany()

db.stuinfo.insertMany(
	[
		{name:"zj",age:23,sex:"男",hobby:"运动"},
		{name:"test",age:35,sex:"男",hobby:"音乐"}
	]
);
```



##### 插入记录

```js
db.collection.insert() 

db.stuinfo.insert(
    {
        name:"zhansan",
     	age:56,sex:"女",
        hobby:"游戏"
    }
);
```



#### 数据修改

##### 修改第一条记录

```js
// 如果有多条语句只修改第一条，会覆盖原有数据，未开启多选，如果想开启，加上第三个属性multi为true
db.stuinfo.update(
    {
        "name":"test"
    },
    {
        "name":"t123"
    },
    {
        multi:true
    }
);

```



##### 修改某个key的value使用$set

```js
db.collection.update() //更新或替换与指定过滤器匹配的单个文档，或更新与指定过滤器匹配的所有文档

db.stuinfo.update(
    {
        "name":"zj"
    },
    {
        $set:{"name":"张军"}
    }
);
```



##### 修改多条记录

```js
db.collection.updateMany() //更新与指定过滤器匹配的所有文档

db.stuinfo.updateMany(
    {
        "name":"zhansan"
    },
    {
        $set:{"name":"张三"}
    }
);
```



##### 修改一条记录

```js
db.collection.updateOne() //即使多个文档可能与指定的过滤器匹配，也最多更新与指定过滤器匹配的单个文档

db.stuinfo.updateOne(
    {
        "name":"lxr"
    },
    {
        $set:{"name":"雪茹"}
    }
);
```



##### 替换一条记录

```js
db.collection.replaceOne() //即使多个文档可能与指定的过滤器匹配，也最多替换与指定过滤器匹配的单个文档

db.stuinfo.replaceOne(
    {
        "name":"t123"
    },
    {
        "name":"test"
    }
);
```



#### 数据删除

#####  删除满足条件的所有记录

```js
db.collection.remove()	//删除单个文档或与指定过滤器匹配的所有文档

db.stuinfo.remove(
    {
        "name":"zhansan"
    }
);
```



##### 只删除一条记录

```js
db.collection.remove()	//删除单个文档或与指定过滤器匹配的所有文档,第二参数为true 则设置只删除一条
db.stuinfo.remove(
    {
        "name":"zhansan"
    }
    ,true
);

```



##### 删除所有数据记录

```js
db.stuinfo.remove();  //不传任何参数
```



##### 语义删除一条记录

```js
db.collection.deleteOne()	//即使多个文档可能与指定的过滤器匹配，也最多删除与指定过滤器匹配的单个文档版本3.2中的新功能

db.stuinfo.deleteOne(
    {
        "name":"test"
    }
);
```



##### 语义删除多条记录

```js
db.collection.deleteMany()	//删除与指定过滤器匹配的所有文档,版本3.2中的新功能

db.stuinfo.deleteMany(
    {
        "name":"zhansan"
    }
);
```



#### 数据查询

##### 查询全部

```js
db.stuinfo.find(); 
```



##### 查询指定记录

```js
db.stuinfo.find(
    {
        "name":"zhangsan"
    }
); 
```



##### 查询一条记录

```js
db.stuinfo.findOne(
    {
        "name":"zhangsan"
    }
); 
```



##### 条件与

```js
db.stuinfo.find(
    {
        "name":"zhangsan",
        "age":37
    }
); 
```



##### 条件或

```js
db.stuinfo.find(
    {
        "name":{$in:["张军","雪茹"]
    }
); 
```



#####  条件非

```js
db.stuinfo.find(
    {
        "name":{$nin:["张军","雪茹"]}
	}
); 
```



##### 格式化显示

```js
db.stuinfo.find().pretty(); 
```



##### 获取结果的行数

```js
db.stuinfo.find().count(); 
```



##### 按照sort里面key的值排序

```js
db.stuinfo.find().sort({"age":-1}); 
```



##### 读取指定数量的数据记录

```js
db.stuinfo.find({name:"zhangsan"}).limit(1);
```



##### 读取时跳过指定数量的数据记录

```js
db.stuinfo.find({name:"zhangsan"}).skip(1);
```



##### 条件操作符

```js
db.stuinfo.find({age:{$gt:23}});
```



##### 类型操作符

```js
db.stuinfo.find({name:{$type:2}});
```

### 操作符

#### 常见的操作符

|   符号    |              表示               |
| :-------: | :-----------------------------: |
| >  and <  |     大于 - $gt  小于 - $lt      |
| <= and >= | 小于等于 - $lte  大于等于 -$gte |
|    !=     |           不等于 -$n            |
| $addToSet |     将文档指定字段的值去重      |
|   $max    |      文档指定字段的最大值       |
|   $min    |      文档指定字段的最小值       |
|   $sum    |        文档指定字段求和         |
|   $avg    |       文档指定字段求平均        |



#### 类型操作符

|    符号    | 表示 |
| :--------: | :--: |
|  双精度型  |  1   |
|   字符串   |  2   |
|    对象    |  3   |
|    数组    |  4   |
| 二进制数据 |  5   |
|   对象ID   |  7   |
|  布尔类型  |  8   |
|    数据    |  9   |
|     空     |  10  |

### 认识索引

#### 索引基础

##### 创建索引

```js
// 创建索引
db.collectionename.ensureIndex({"name":1});
```



##### 获取索引

```js
// 获取当前集合的索引
db.collectionename.getIndexes();

```



##### 删除索引

```js
// 删除索引 复合和唯一索引都能删除
db.collectionename.dropIndex({"name":1});
```



##### 指定索引名

```js
// 创建索引时为其指定索引名 否则设置为默认索引名
db.collectionename.ensureIndex({"name":1},{"name":"indexname"});
```



##### 后台创建索引

```js
// 后台创建索引
db.collectionename.ensureIndex({"username":1},{"background":true});
```



#### explain的使用

```js
// 查询具体耗费时间
db.collectionename.find().explain("executionStats");
```



#### 唯一索引

```js
// 缺省情况下创建的索引均不是唯一索引,需要传入第二设置项
db.collectionename.ensureIndex({"userid":1},{"unique":true});

// 我们同样可以创建复合唯一索引，即保证复合键值唯一即可
db.collectionename.ensureIndex({"userid":1,"age":1},{"unique":true}); 
```



### 聚合管道

#### 管道操作符

| 管道操作符 |                         描述                         |
| :--------: | :--------------------------------------------------: |
|  $project  |                增加、删除、重命名字段                |
|   $match   |      条件匹配，只满足条件的文档才能进入下一阶段      |
|   $limit   |                    限制结果的数量                    |
|   $skip    |                    跳过文档的数量                    |
|   $sort    |                       条件排序                       |
|   $group   |                  条件组合结果 统计                   |
|  $lookup   | $lookup 操作符 用以引入其它集合的数据 （表关联查询） |



#### 与sql进行比较

|   sql    |  mongo   |
| :------: | :------: |
|  WHERE   |  $match  |
| GROUP BY |  $group  |
|  HAVING  |  $match  |
|  SELECT  | $project |
| DESC ASC |  $sort   |
|  LIMIT   |  $limit  |
|  SUM()   |   $sum   |
|   JOIN   | $lookup  |

#### 模拟数据

```js
// 订单表
db.order.insert({"order_id":"1","uid":10,"trade_no":"111","all_price":100,"all_num":2})
db.order.insert({"order_id":"2","uid":7,"trade_no":"222","all_price":90,"all_num":2})
db.order.insert({"order_id":"3","uid":9,"trade_no":"333","all_price":20,"all_num":6})

// 订单详情表
db.order_item.insert({"order_id":"1","title":"商品鼠标 1","price":50,num:1})
db.order_item.insert({"order_id":"1","title":"商品键盘 2","price":50,num:1})
db.order_item.insert({"order_id":"1","title":"商品键盘 3","price":0,num:1})

db.order_item.insert({"order_id":"2","title":"牛奶","price":50,num:1})
db.order_item.insert({"order_id":"2","title":"酸奶","price":40,num:1})

db.order_item.insert({"order_id":"3","title":"矿泉水","price":2,num:5})
db.order_item.insert({"order_id":"3","title":"毛巾","price":10,num:1})
```



#### 详细使用

##### 修改文档结构

```js
// 修改文档的结构，可以用来重命名、增加或删除文档中的字段
db.order.aggregate(
    [
    	{
    		$project:{ trade_no:1, all_price:1 }
    	}
	]
)
```



##### 过滤文档

```js
// 用于过滤文档，用法类似于 find() 方法中的参数
db.order.aggregate(
    [
        {
        	$match:{"all_price":{$gte:90}}
        }
	]
)
```



##### 文档分组

```js
// 将集合中的文档进行分组，可用于统计结果 如果按照id分组需要_id 不能更改字段名
db.order_item.aggregate(
    [
        {
        	$group: {_id: "$order_id", total: {$sum: "$num"}}
        }
    ]
)

```



##### 文档排序

```js
// 将集合中的文档进行排序 1升序 -1降序
db.order.aggregate(
  	[
        {
			$sort:{"all_price":-1}
		}
	]
)
```



##### 限制文档数量

```js
// 限制集合中的文档数量
db.order.aggregate(
    [
        {
        	$project:{ trade_no:1, all_price:1 }
        },
        {
        	$match:{"all_price":{$gte:90}}
        }, 
        {
        	$sort:{"all_price":-1}
        },
        {
        	$limit:1
        }
	]
)
```



##### 跳过文档数量

```js
// 跳过集合中的文档数量
db.order.aggregate(
    [
        {
        	$project:{ trade_no:1, all_price:1 }
        }, 
        {
        	$match:{"all_price":{$gte:90}}
        }, 
        {
        	$sort:{"all_price":-1}
        },
        {
        	$skip:1
        }
	]
)
```



##### 集合关联

```js
// 集合关联
db.order.aggregate(
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



### 安全管理

#### 账户权限配置

##### 创建超级管理员

```js
use admin

db.createUser({
	user:'zj', 
    pwd:'happy365', 
    roles:[{role:'root',db:'admin'}]
})

db.createUser({
    user:'bihu', 
    pwd:'happy365', 
    roles:[{role:'dbOwner',db:'bihudb'}]
})
```

![1565533832084](http://demo.bestzhangjun.com/imgs/node/1565533832084.png)



##### 修改配置文件

```js
# 安全起见 修改配置文件复制一份好还原源文件
copy file

# 注意的地方
修改的地方严格按照mongod.cfg格式修改 空格需要注意下，否则服务启动不起来

# 路径 具体安装目录
C:\Program Files\MongoDB\Server\4.0\bin\mongod.cfg

# 配置
security: 
    authorization: enabled 
```



##### 重启MongoDB Serve 服务

![1565533973005](http://demo.bestzhangjun.com/imgs/node/1565533973005.png)



##### 测试直连是否成功

![1565534027675](http://demo.bestzhangjun.com/imgs/node/1565534027675.png)



##### 测试超管连接是否成功

```js
mongo admin -u zj -p happy365 //连接本地数据库 
mongo 192.168.1.200:27017/admin -u zj -p happy365 //连接远程数据库
```

![1565534119891](http://demo.bestzhangjun.com/imgs/node/1565534119891.png)



##### 测试数据库创建角色

```js
 mongo Student -u stu -p happy365 //连接本地
 mongo 192.168.1.200:27017/Student -u stu -p happy365 //连接远程
```

![1565534227551](http://demo.bestzhangjun.com/imgs/node/1565534227551.png)



#### 配置中的常用命令

##### 查看用户

```js
show users;
```



##### 删除用户

```js
db.dropUser(username);
```



##### 修改密码

```js
db.updateUser(databasename,{pwd:password});
```



##### 密码认证

```js
db.auth(databasename,password);
```



#### 数据库角色

|    角色分配    |                           角色名称                           |
| :------------: | :----------------------------------------------------------: |
| 数据库用户角色 |                       read，readWrite                        |
| 数据库管理角色 |                 dbAdmin，dbOwner，userAdmin                  |
|  集群管理角色  |  clusterAdmin，clusterManager，clusterMonitor，hostManager   |
|  备份恢复角色  |                       backup，restore                        |
| 所有数据库角色 | readAnyDatabase，readWriteAnyDatabase，userAdminAnyDatabase，dbAdminAnyDatabase |
|  超级用户角色  |                             root                             |

### 备份&还原

#### 备份操作

```js
mongodump -h dbhost -d dbname -o dbdirectory
参数说明：
    -h： MongDB所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017
    -d： 需要备份的数据库实例，例如：test
    -o： 备份的数据存放位置
```

![1565625393424](http://demo.bestzhangjun.com/imgs/node/1565625393424.png)



#### 还原操作

```js
mongorestore -h dbhost -d dbname --drop --dir dbdirectory
参数或名：
    -h： MongoDB所在服务器地址 例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017
    -d： 需要恢复的数据库实例，例如：test，当然这个名称也可以和备份时候的不一样，比如test2
    --dir： 备份数据所在位置，例如：/home/mongodump/itcast/
    --drop： 恢复的时候，先删除当前数据，然后恢复备份的数据,就是说，恢复后，备份后添加修改的数据都会被删
除，慎用！
```

![](http://demo.bestzhangjun.com/imgs/node/2019-08-15_003913.png)







 



