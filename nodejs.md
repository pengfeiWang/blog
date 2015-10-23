#nodejs 全局对象
```
在浏览器中,顶级作用域为全局作用域,在全局作用域下通过 var something 即定义了一个全局变量。但是在 Node 中并不如此,顶级作用域并非是全局作用域,在 Node 模块中通过 var something 定义的变量仅作用于该模块。
```

##process```
进程对象。
```

##__filename
```
当前正在执行的脚本的文件名。这是一个绝对路径,可能会和命令行参数中传入的文件名不同。
```

##__dirname```
当前正在执行脚本所在的目录名。```
#util 工具模块

##util.inspect```
var object = {"a": 1, "b": 2}util.inspect(object, showHidden=false, depth=2)
```以字符串形式返回 `￼￼￼object` 对象的结构信息。
如果 `showHidden` 参数设置为 `true`, 则此对象的不可枚举属性也会被显示。可使用 `depth` 参数指定 `inspect` 函数在格式化对象信息时的递归次数。
这对分 析复杂对象的内部结构非常有帮助。默认情况下递归两次,如果想要无限递归可将 ￼ ￼ `depth`￼ 参数设为 null。##util.inherits```util.inherits(constructor, superConstructor)
```
将一个 `构造函数` 的原型方法继承到另一个构造函数中。`constructor` 构造函数的原型将被设置为使用 `superConstructor` 构造函数所创建的一个新对象。此方法带来的额外的好处是,可以通过 `constructor.super_` 属性来访问 `superConstructor` 构造函数。
```
var util = require("util");var events = require("events");function MyStream() {    events.EventEmitter.call(this);}
util.inherits(MyStream, events.EventEmitter);MyStream.prototype.write = function(data) { 	this.emit("data", data);}
var stream = new MyStream();console.log(stream instanceof events.EventEmitter); // true```

#Events 事件模块
Node 引擎中很多对象都会触发事件:例如 `net.Server` 会在每一次有客户端连￼￼￼接到它时触发事件

又如 `￼￼fs.readStream` 会在文件打开时触发事件。所有能够触发事件的对象都是 `events.EventEmitter` 的实例。你可以通过 `require("events");` ￼访问这个模块。

#Streams 流
所有流都是 EventEmitter 的实例


#File System 文件系统模块

#
#Path 路径模块

##path.normalize(p)
该方法用于标准化一个字符型的路径,请注意 `..` 与 ￼`.`￼ 的使用。
当发现有多个斜杠 `(/)` 时,系统会将他们替换为一个斜杠;如果路径末尾中包 含有一个斜杠,那么系统会保留这个斜杠。在 Windows 中,上述路径中的斜杠 `(/)` 要换成反斜杠 `(\)`。

```
path.normalize('/foo/bar//baz/asdf/quux/..') // returns
'/foo/bar/baz/asdf'
```
##path.join([path1], [path2], [...])
该方法用于合并方法中的各参数并得到一个标准化合并的路径字符串。

```
require('path').join('/foo', 'bar', 'baz/asdf', 'quux', '..');
'/foo/bar/baz/asdf'```
##path.resolve([from ...], to)
如果参数 `to` 当前不是绝对的,系统会将 `from` 参数按从右到左的顺序依次前缀到 `to` 上,直到在 `from` 中找到一个绝对路径时停止。如果遍历所有 `from` 中 的路径后,系统依然没有找到一个绝对路径,那么当前工作目录也会作为参数 使用。最终得到的路径是标准化的字符串,并且标准化时系统会自动删除路径 末尾的斜杠,但是如果获取的路径是解析到根目录的,那么系统将保留路径末 尾的斜杠。
你也可以将这个方法理解为 Shell 中的一组 cd 命令。
```path.resolve('foo/bar', '/tmp/file/', '..', 'a/../subfile')类似于:cd foo/barcd /tmp/file/cd ..cd a/../subfilepwd
```
该方法与 cd 命令的区别在于该方法中不同的路径不一定存在,而且这些路径 也可能是文件。

```
path.resolve('/foo/bar', './baz') 
returns '/foo/bar/baz'path.resolve('/foo/bar', '/tmp/file/') 
returns '/tmp/file'path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif') // if currently in /home/myself/node, it returns '/home/myself/node/wwwroot/static_files/gif/image.gif'```
##path.dirname(p)
该方法返回一个路径的目录名,类似于 Unix 中的 dirname 命令。
```
path.dirname('/foo/bar/baz/asdf/quux')// returns'/foo/bar/baz/asdf'
```
##path.basename(p, [ext])
该方法返回一个路径中最低一级目录名,类似于 Unix 中的 basename 命令。

```
path.basename('/foo/bar/baz/asdf/quux.html') 
// returns'quux.html'path.basename('/foo/bar/baz/asdf/quux.html', '.html')// returns'quux'
```
#path.extname(p)
该方法返回路径中的文件扩展名,即路径最低一级的目录中'.'字符后的任何字 符串。如果路径最低一级的目录中'没有'.' 或者只有'.',那么该方法返回一 个空字符串。

```
path.extname('index.html')// returns'.html'￼path.extname('index')// returns''
```
##path.exists(p, [callback])
该方法用于测试参数 `p` 中的路径是否存在。然后以 `true` 或者 `false` 作为参数调用 ￼`callback`￼ 回调函数。示例:```
path.exists('/etc/passwd', function (exists) {util.debug(exists ? "it's there" : "no passwd!");});```
##path.existsSync(p)net 模块为你提供了一种异步网络包装器,它包含创建服务器和客户端(称为 streams)所需的方法,您可以通过调用 require("net")来使用此模块。