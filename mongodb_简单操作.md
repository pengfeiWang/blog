###mongodb 简单操作

刚开始接触 mongodb 参考教程写程序, 发现教程查询时候没有根据 id 去查询, 后来查找宝典, 问网友, 有说 id 有可能会变, 后来经过测试, id 是不会改变的, 也许比较老的版本会有变化



####设置用户 密码
```
db.adUser('user','password')
```
####权限验证
```
db.auth('user','password')
> mongo admin -u admin -p wpfejqb
```
####显示所有库
```
show dbs

db.dropDatabase() 删除当前库
```
####切换库
```
use db
```
####显示集合(表)
```
show collections
```
####插入

```
db.table.insert( {"title": "mongodb 简单操作" } ) 插入一行记录 会自动生成个 id "_id" : ObjectId("52f1be517ba6f8753eba147f"), '在终端输入中文不太理想'

下边多了2个参数, 第二个参数 safe:true, 是插入 查询一次执行, 也就是说插入之后在查询一次, 是否插入成功, 之后是必须要有回调函数

db.table.insert( {"title": "mongodb 简单操作" }, {safe: true}, function (err, doc) {} )
```

####更新

```
db.table.update( { "title": "mongodb 简单操作"}, { "title": "test" } ) 更新 "title": "mongodb 简单操作" 结果为 "title": "test"
collection.update({a:996}, {$push: {b:'b'}}, function(error, bars){});
collection.update({a:996}, {$set: {a:997}}, function(error, bars){});
```
注意，这里为了代码简单，我们没有设置{safe:true}。你可以看到update的第一个参数是条件，即对a＝996的文档进行更新。第二个参数则是表示要如何更新文档，譬如第一个update是增加一个属性b，且设置其值为字符串'b'；第二个update是修改a的值为997。可以看出，第二个参数是一个对象，其属性名是一个操作符，以$开头，值为一个对象。这些操作符除了这里的$push和$set，还有其它的$inc, $unset, $pushAll等等，具体可以参考[这里](http://docs.mongodb.org/manual/tutorial/isolate-sequence-of-operations)

####查询

查询

```
db.table.find({ "_id" : ObjectId("52f1be517ba6f8753eba147f") })
```


查询 `_id` 为 `ObjectId("52f1be517ba6f8753eba147f") 这条记录 ObjectId("52f1be517ba6f8753eba147f")` 非字符串, 而是个对象

```
var ObjectID = require('mongodb').ObjectID;
var tid = new ObjectID("52f1be517ba6f8753eba147f");
//我觉得好像不 new 也可以, 不明白是什么原理, 了解的给讲解下
```
####删除

```
db.table.drop()
```
#### 备份库

```
mongodump 
```

#### 恢复库

```
mongorestore -d webDevTool –directoryperdb /data/dump/webDevTool
```

####高级

* sort
	
```
collection.find({}, {sort: [['created_at', 'desc'], ['body', 'asc']]})
其中'desc'也可以用-1表示，而'asc'可以用1表示。如果是只有一个sort列，也可以用下面的方式
collection.find({}, {sort: {'created_at': -1}})
注意：这里用一个对象表示sort的时候，排序方向必须是1（升序）或者-1（降序）。可以说这是一个很垃圾的API设计首先不应该用数组的数组来表示sort；而只用一列排序时只能用数字不能用字符串更加是API的不一致
```
* limit

```
collection.find({}, {limit: 10, skip:20})
这个可以用来做分页，表示获取从第20条（第1条记录序号为0）记录开始的10条记录.类似与Mysql的limit 20, 10.
```
* count

```
collection.count({}, function(err, count){...} )
第一个参数是query对象，可以省略。第二个参数是callback函数。
```
####授权启动 后台运行

```
mongod --fork --auth --dbpath=/data/nodejs --logpath=/data/log/2014-09-15.log
```
####更多参考

[MongoDB的基础介绍](http://mongodb.github.io/node-mongodb-native/api-articles/nodekoarticle1.html)

[高级查询](http://docs.mongodb.org/manual/reference/operator/)

