# webKit


### web sql

虽然Html5已经提供了功能强大的localStorage和sessionStorage，但是他们两个都只能提供存储简单数据结构的数据
对于复杂的Web应用的数据却无能为力。逆天的是Html5提供了一个浏览器端的数据库支持，
允许我们直接通JS的API在浏览器端创建一个本地的数据库，而且支持标准的SQL的CRUD操作，
让离线的Web应用更加方便的存储结构化的数据。接下里介绍一下本地数据的相关API和用法。

&nbsp;

操作本地数据库的最基本的步骤是：

* 第一步：openDatabase方法：创建一个访问数据库的对象。

* 第二步：使用第一步创建的数据库访问对象来执行transaction方法，
  通过此方法可以设置一个开启事务成功的事件响应方法，在事件响应方法中可以执行SQL.

* 第三步：通过executeSql方法执行查询，当然查询可以是：CRUD。
接下来分别介绍一下相关的方法的参数和用法。


  
##### （1）openDatabase方法
```
//Demo：获取或者创建一个数据库，如果数据库不存在那么创建之
var dataBase = openDatabase("student", "1.0", "学生表", 1024 * 1024, function () { });
```

openDatabase方法打开一个已经存在的数据库，如果数据库不存在，它还可以创建数据库。几个参数意义分别是：

* 1，数据库名称。
* 2，数据库的版本号，目前来说传个1.0就可以了，当然可以不填；
* 3，对数据库的描述。
* 4，设置分配的数据库的大小（单位是kb）。
* 5，回调函数(可省略)。



初次调用时创建数据库，以后就是建立连接了。



######（2）db.transaction方法可以设置一个回调函数，此函数可以接受一个参数就是我们开启的事务的对象。然后通过此对象可以进行执行Sql脚本，跟下面的步骤可以结合起来。



```
db.transaction(function (tx) {
    tx.executeSql("INSERT INTO mytable (mytitle, timestamp) values(?, ?)", 
    ["str", timeStamp], null, null);
});
```



#####（3）通过executeSql方法执行查询。

```
ts.executeSql(sqlQuery,[value1,value2..],dataHandler,errorHandler)
```

#####参数说明：

* qlQuery：需要具体执行的sql语句，可以是create、select、update、delete；
* [value1,value2..]：sql语句中所有使用到的参数的数组，在executeSql方法中，将s>语句中所要使用的参数先用“?”代替，然后依次将这些参数组成数组放在第二个参数中
* dataHandler：执行成功是调用的回调函数，通过该函数可以获得查询结果集；
* 4,errorHandler：执行失败时调用的回调函数；
下面是一个综合的例子，可以看一下：

```
<head>
 <script src="Scripts/jquery-1.5.1.js" type="text/javascript"></script>
    <script type="text/javascript">
        function initDatabase() {
            var db = getCurrentDb();//初始化数据库
            if(!db) {alert("您的浏览器不支持HTML5本地数据库");return;}
            db.transaction(function (trans) {//启动一个事务，并设置回调函数
                //执行创建表的Sql脚本
                trans.executeSql("create table if not exists Demo(uName text null,title text null,words text null)", [], function (trans, result) {
                }, function (trans, message) {//消息的回调函数alert(message);});
            }, function (trans, result) {
            }, function (trans, message) {
            });
        }
        $(function () {//页面加载完成后绑定页面按钮的点击事件
            initDatabase();
            $("#btnSave").click(function () {
                var txtName = $("#txtName").val();
                var txtTitle = $("#txtTitle").val();
                var txtWords = $("#txtWords").val();
                var db = getCurrentDb();
                //执行sql脚本，插入数据
                db.transaction(function (trans) {
                    trans.executeSql("insert into Demo(uName,title,words) values(?,?,?) ", [txtName, txtTitle, txtWords], function (ts, data) {
                    }, function (ts, message) {
                        alert(message);
                    });
                });
                showAllTheData();
            });
        });
        function getCurrentDb() {
            //打开数据库，或者直接连接数据库参数：数据库名称，版本，概述，大小
            //如果数据库不存在那么创建之
            var db = openDatabase("myDb", "1.0", "it's to save demo data!", 1024 * 1024); ;
            return db;
        }
        //显示所有数据库中的数据到页面上去
        function showAllTheData() {
            $("#tblData").empty();
            var db = getCurrentDb();
            db.transaction(function (trans) {
                trans.executeSql("select * from Demo ", [], function (ts, data) {
                    if (data) {
                        for (var i = 0; i < data.rows.length; i++) {
                            appendDataToTable(data.rows.item(i));//获取某行数据的json对象
                        }
                    }
                }, function (ts, message) {alert(message);var tst = message;});
            });
        }
        function appendDataToTable(data) {//将数据展示到表格里面
            //uName,title,words
            var txtName = data.uName;
            var txtTitle = data.title;
            var words = data.words;
            var strHtml = "";
            strHtml += "<tr>";
            strHtml += "<td>"+txtName+"</td>";
            strHtml += "<td>" + txtTitle + "</td>";
            strHtml += "<td>" + words + "</td>";
            strHtml += "</tr>";
            $("#tblData").append(strHtml);
        }
    </script>
</head>
    <body>
        <table>
            <tr>
                <td>用户名：</td>
                <td><input type="text" name="txtName" id="txtName" required/></td>
            </tr>
               <tr>
                <td>标题：</td>
                <td><input type="text" name="txtTitle" id="txtTitle" required/></td>
            </tr>
            <tr>
                <td>留言：</td>
                <td><input type="text" name="txtWords" id="txtWords" required/></td>
            </tr>
        </table>
        <input type="button" value="保存" id="btnSave"/>
        <hr/>
        <input type="button" value="展示所哟数据" onclick="showAllTheData();"/>
        <table id="tblData">
        </table>
    </body>
</html>
```

# Gecko

### indexedDB

在HTML5本地存储——Web SQL Database提到过Web SQL Database实际上已经被废弃，而HTML5的支持的本地存储实际上变成了

Web Storage（Local Storage和Session Storage）与IndexedDB。Web Storage使用简单字符串键值对在本地存储数据，方便灵活，但是对于大量结构化数据存储力不从心，IndexedDB是为了能够在客户端存储大量的结构化数据，并且使用索引高效检索的API。

异步API
在IndexedDB大部分操作并不是我们常用的调用方法，返回结果的模式，而是请求——响应的模式，比如打开数据库的操作

```
var request=window.indexedDB.open('testDB');
```

这条指令并不会返回一个DB对象的句柄，我们得到的是一个IDBOpenDBRequest对象，而我们希望得到的DB对象在其result属性中

除了result，IDBOpenDBRequest接口定义了几个重要属性

onerror: 请求失败的回调函数句柄
onsuccess:请求成功的回调函数句柄
onupgradeneeded:请求数据库版本变化句柄
 

所谓异步API是指并不是这条指令执行完毕，我们就可以使用request.result来获取indexedDB对象了，就像使用ajax一样，语句执行完并不代表已经获取到了对象，所以我们一般在其回调函数中处理。

####创建数据库
刚才的语句已经展示了如何打开一个indexedDB数据库，调用indexedDB.open方法就可以创建或者打开一个indexedDB。看一个完整的处理

```
function openDB (name) {
    var request=window.indexedDB.open(name);
    request.onerror=function(e){
        console.log('OPen Error!');
    };
    request.onsuccess=function(e){
        myDB.db=e.target.result;
    };
}
 var myDB={
     name:'test',
     version:1,
     db:null
 };
 openDB(myDB.name);
 ```
 
 代码中定义了一个myDB对象，在创建indexedDB request的成功毁掉函数中，把request获取的DB对象赋值给了myDB的db属性，这样就可以使用myDB.db来访问创建的indexedDB了。

####version
我们注意到除了onerror和onsuccess，IDBOpenDBRequest还有一个类似回调函数句柄——onupgradeneeded。这个句柄在我们请求打开的数据库的版本号和已经存在的数据库版本号不一致的时候调用。

indexedDB.open()方法还有第二个可选参数，数据库版本号，数据库创建的时候默认版本号为1，当我们传入的版本号和数据库当前版本号不一致的时候onupgradeneeded就会被调用，当然我们不能试图打开比当前数据库版本低的version，否则调用的就是onerror了，修改一下刚才例子

```
function openDB (name,version) {
    var version=version || 1;
    var request=window.indexedDB.open(name,version);
    request.onerror=function(e){
        console.log(e.currentTarget.error.message);
    };
    request.onsuccess=function(e){
        myDB.db=e.target.result;
    };
    request.onupgradeneeded=function(e){
        console.log('DB version changed to '+version);
    };
}
var myDB={
    name:'test',
    version:3,
    db:null
};
openDB(myDB.name,myDB.version);
```

由于刚才已经创建了版本为1的数据库，打开版本为3的时候，会在控制台输出：DB version changed to 3

####关闭与删除数据库
关闭数据库可以直接调用数据库对象的close方法

```
function closeDB(db){
    db.close();
}
```
####删除数据库使用indexedDB对象的deleteDatabase方法
```
function deleteDB(name){
    indexedDB.deleteDatabase(name);
}
```
#### 简单调用
```
var myDB={
    name:'test',
    version:3,
    db:null
};
openDB(myDB.name,myDB.version);
setTimeout(function(){
    closeDB(myDB.db);
    deleteDB(myDB.name);
},500);
```

由于异步API愿意，不能保证能够在closeDB方法调用前获取db对象（实际上获取db对象也比执行一条语句慢得多），所以用了setTimeout延迟了一下。当然我们注意到每个indexedDB实例都有onclose回调函数句柄，用以数据库关闭的时候处理，有兴趣同学可以试试，原理很简单，不演示了。

####object store
有了数据库后我们自然希望创建一个表用来存储数据，但indexedDB中没有表的概念，而是objectStore，一个数据库中可以包含多个objectStore，objectStore是一个灵活的数据结构，可以存放多种类型数据。也就是说一个objectStore相当于一张表，里面存储的每条数据和一个键相关联。


我们可以使用每条记录中的某个指定字段作为键值（keyPath），也可以使用自动生成的递增数字作为键值（keyGenerator），也可以不指定。选择键的类型不同，objectStore可以存储的数据结构也有差异

<table><tr><td>键类型</td><td>存储数据</td></tr>
<tr><td>不使用</td><td>任意值，但是没添加一条数据的时候需要指定键参数</td></tr>
<tr><td>keyPath</td><td>Javascript对象，对象必须有一属性作为键值</td></tr>
<tr><td>keyGenerator</td><td>任意值</td></tr>
<tr><td>都使用</td><td>Javascript对象，如果对象中有keyPath指定的属性则不生成新的键值，如果没有自动生成递增键值，填充keyPath指定属性</td></tr></table>
 
#####事务
在对新数据库做任何事情之前，需要开始一个事务。事务中需要指定该事务跨越哪些object store。

######事务具有三种模式

只读：read，不能修改数据库数据，可以并发执行
读写：readwrite，可以进行读写操作
版本变更：verionchange
 

`var transaction=db.transaction([students','taecher']);`  //打开一个事务，使用students 
和teacher object store
`var objectStore=transaction.objectStore('students');` //获取students object store
给object store添加数据
 

调用数据库实例的createObjectStore方法可以创建object store，方法有两个参数：store name和键类型。
调用store的add方法添加数据。有了上面知识，我们可以向object store内添加数据了

 

keyPath

因为对新数据的操作都需要在transaction中进行，而transaction又要求指定object store，所以我们只能在创建数据库的时候初始化object store以供后面使用，这正是onupgradeneeded的一个重要作用，修改一下之前代码

```
function openDB (name,version) {
    var version=version || 1;
    var request=window.indexedDB.open(name,version);
    request.onerror=function(e){
        console.log(e.currentTarget.error.message);
    };
    request.onsuccess=function(e){
        myDB.db=e.target.result;
    };
    request.onupgradeneeded=function(e){
        var db=e.target.result;
        if(!db.objectStoreNames.contains('students')){
            db.createObjectStore('students',{keyPath:"id"});
        }
        console.log('DB version changed to '+version);
    };
}
``` 
 

这样在创建数据库的时候我们就为其添加了一个名为students的object store，准备一些数据以供添加

```
var students=[{ 
            id:1001, 
            name:"Byron", 
            age:24 
        },{ 
            id:1002, 
            name:"Frank", 
            age:30 
        },{ 
            id:1003, 
            name:"Aaron", 
            age:26 
        }];
function addData(db,storeName){
            var transaction=db.transaction(storeName,'readwrite'); 
            var store=transaction.objectStore(storeName); 
            for(var i=0;i<students.length;i++){
                store.add(students[i]);
            }
        }
openDB(myDB.name,myDB.version);
        setTimeout(function(){
            addData(myDB.db,'students');
        },1000);
```
这样我们就在students object store里添加了三条记录，以id为键，在chrome控制台看看效果


keyGenerate

```
function openDB (name,version) {
            var version=version || 1;
            var request=window.indexedDB.open(name,version);
            request.onerror=function(e){
                console.log(e.currentTarget.error.message);
            };
            request.onsuccess=function(e){
                myDB.db=e.target.result;
            };
            request.onupgradeneeded=function(e){
                var db=e.target.result;
                if(!db.objectStoreNames.contains('students')){
                    db.createObjectStore('students',{autoIncrement: true});
                }
                console.log('DB version changed to '+version);
            };
        }
```



剩下的两种方式有兴趣同学可以自己摸索一下了

######查找数据
可以调用object store的get方法通过键获取数据，以使用keyPath做键为例

```
function getDataByKey(db,storeName,value){
            var transaction=db.transaction(storeName,'readwrite'); 
            var store=transaction.objectStore(storeName); 
            var request=store.get(value); 
            request.onsuccess=function(e){ 
                var student=e.target.result; 
                console.log(student.name); 
            };
}
```
######更新数据
可以调用object store的put方法更新数据，会自动替换键值相同的记录，达到更新目的，没有相同的则添加，以使用keyPath做键为例

```
function updateDataByKey(db,storeName,value){
            var transaction=db.transaction(storeName,'readwrite'); 
            var store=transaction.objectStore(storeName); 
            var request=store.get(value); 
            request.onsuccess=function(e){ 
                var student=e.target.result; 
                student.age=35;
                store.put(student); 
            };
}
```
#####删除数据及object store
 

#####调用object store的delete方法根据键值删除记录
```
function deleteDataByKey(db,storeName,value){
    var transaction=db.transaction(storeName,'readwrite'); 
    var store=transaction.objectStore(storeName); 
    store.delete(value); 
}
```
调用object store的clear方法可以清空object store

```
function clearObjectStore(db,storeName){
            var transaction=db.transaction(storeName,'readwrite'); 
            var store=transaction.objectStore(storeName); 
            store.clear();
}
```
调用数据库实例的deleteObjectStore方法可以删除一个object store，这个就得在onupgradeneeded里面调用了

```
if(db.objectStoreNames.contains('students')){ 
                    db.deleteObjectStore('students'); 
}
```
最后
 

这就是关于indexedDB的基本使用方式，很多同学看了会觉得很鸡肋，和我们正常自己定义个对象使用没什么区别，也就是能保存在本地罢了，这是因为我们还没有介绍indexedDB之所以称为indexed的杀器——索引，这个才是让indexedDB大显神通的东西，下篇我们就来看看这个杀器。