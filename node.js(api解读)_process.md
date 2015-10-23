#node.js(API解读) - process  


nodejs的process是一个全局对象，他提供了一些方法和属性，node.js官方的API说的很简单，并没有把一些详细的应用方法和作用写出来，下面结合我自己的学习，做一下小结吧。

process.uptime()  是进程运行了多久，是统计时间的，不是进程数量

1、Event: 'exit' 
这是process的退出事件，官方示例很清楚，当process退出时触发。即我们按“ctrl+c”时触发。

2、Event: 'uncaughtException' 
这是process的异常事件，uncaughtException译为：未捕获的异常，可以利用这个函数来捕获整个进程运行时的异常，这里可以理解为“使本node.js进程中断的异常”，因为它的官方示例在异常函数下面的代码将不会执行。

3、Signal Events 
这是process的自定义事件，可以为process对象自定义一些事件。这里官方示例如下：
process.stdin.resume(); //这句话是为了不让控制台推出
process.on('SIGINT', function () { //SIGINT这个信号是系统默认信号，代表信号中断，就是ctrl+c
  console.log('Got SIGINT.  Press Control-D to exit.');
});
官方API还说可以自定义名字来监听process事件，我们试一下，代码如下：
 process.on('SIGUSR1', function (d) { //这里监听 SIGUSR1 事件
      console.log('Bye-'+d); //这里将输出Bye-Bye,然后推出进程
      process.exit(0);
    }); 
  process.emit('SIGUSR1', 'Bye'); //利用emit触发SIGUSR1，然后传参数为Bye

4、process.stdout 
官方的示例和代码很清楚，控制台输出流。
console.log = function (d) {
  process.stdout.write(d + '\n');
};

5、process.stdin 
进程控制台输入流，官方代码看了不解，于是copy到linux系统里运行一下，就明白了
process.stdin.resume();
process.stdin.setEncoding('utf8');
process.stdin.on('data', function (chunk) {
  process.stdout.write('data: ' + chunk);
});
process.stdin.on('end', function () {
  process.stdout.write('end');
});
输入的命令和输出:
$ node test.js 
ffff
data: ffff

6、process.argv
这是一个数组，数组里存放着启动这个node.js进程各个参数和命令代码，官方代码一目了然。

7、process.execPath  
返回当前node.js进程的启动命令路径，也就是node.js安装目录下的node命令路径，是一个绝对路径
$ node test.js 
url: /usr/local/bin/node

8、process.chdir(directory) 和 process.cwd() 
process.cwd() 返回当前进程的工作目录，process.chdir(directory)则改变进程的工作目录，如果改变失败将抛出一个异常。代码见官方API。
举个例子：
如果一个进程工作在 /red-hat/，现在创建在 "./"目录下创建一个foo.txt，则会在/red-hat/foo.txt创建，如果改变进程工作目录为/red/，则创建foo.txt会在/red/foo.txt创建。注意：node.js的require()不受这个限制,他是以当前文件的。在执行child_process.exec()方法时需要考虑这一点。


9、process.env
返回当前linux系统的信息，我可以输入一下代码来看系统信息
console.log(JSON.stringify(process.env));

10、process.exit(code=0)
kill当前进程，退出本进程。

11、process.getgid()、process.setgid(id)、process.getuid()、process.setuid(id)
获取和设置进程的groupid和userid。

12、process.version、process.versions
node.js的版本和node.js的版本对象

13、process.installPrefix
返回nodejs的安装前缀

14、process.kill(pid, signal='SIGTERM')
发出一个kill信号给指定pid，如果signal不指定，则默认为“SIGTERM”，官方API特别提醒，这个kill信号只是一个信号，并不是linux下的kill命令，并不会真的将那个pid杀死，要想通过kill杀死指定pid，则需要在指定pid监听“SIGTERM”信号，然后执行process.exit(0)；即可。

15、process.pid、process.title、process.arch、process.platform 
进程id，进程名字，进程架构（如：X64），进程平台（如：linux）

16、process.memoryUsage()
进程的内存使用情况，API很清楚。

17、process.nextTick(callback)
异步执行callback函数，注意，这比 "setTimeout(fn, 0) " 要高效很多。当有一些比较耗时的操作可以用在process.nextTick(callback) 中，这样可以不阻塞整个函数执行。

18、process.umask([mask]) 
设置进程的user mask值，什么是umask值呢？
在linux系统有一个系统命令：$ umask，主要作用是修改系统默认的创建文件和文件夹的权限。
注意这句话：Returns the old mask if mask argument is given, otherwise returns the current mask.
当对process.umask()方法传递参数时，则返回旧的umask值，否则返回当前的umask值。
这里如果我们设置：
var oldmask, newmask = 0644;
oldmask = process.umask(newmask);
console.log('Changed umask from: ' + oldmask.toString(8) + ' to ' + newmask.toString(8));
输出022和0644，当umask = 022时，新建的目录权限是755，新建文件的权限是 644。具体可以参考linux umask手册。这里只是修改由本node.js进程创建的文件和目录的权限。

19、process.uptime() 
官方给出结果是node进程运行的秒数

20、process.reallyExit(status)
真实推出本进程，不触发‘exit’事件

21、process._kill(pid,sig)
用于给指定pid的进程发送指定信号（类似linux下的kill命令）
var pid=process.pid;
process._kill(pid,9);

22、process.binding(name)
这个方法用于返回指定名称的内置模块。例如下面的代码将打印node_net模块所有的可以调用的方法或变量。
var binding=process.binding('net');
console.dir(binding);