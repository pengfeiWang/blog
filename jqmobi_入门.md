###jqmobi 入门

在 [jqmobi官网](http://app-framework-software.intel.com/) 获取最新的版本, 或者 通过 `git clone git@github.com:01org/appframework.git` 获取.


从 `index.html` 文件会看到详细的 api, 不过在看 `index.html` 原文件的时候确实很是头疼, 写的一个乱啊, 简直就是惨不忍睹, 不管它了, 先从整理他的文件开始.

解压后的文件其中包含了框架的 压缩 合并 等策略文件, 很详细, 不过我在合并的过程当中发现 `jshint` 语法插件提示了个错误, 也许是规则没有写好吧, 这个可以不管, 注释掉就可以了,

######注释文件
打开 `Gruntfile.js` 文件, 在最下边找到

```
grunt.registerTask("default", [
        // "jshint",
        "test",
        "clean",
        "cssmin",
        "concat",
        "closure-compiler",
        "usebanner"
    ]);
```    
注释掉上边的 `jshint` 就可以了



`grunt` 依赖 `nodejs`, 首先确定安装了 `nodejs`, 通过终端进入解压后的目录

安装好nodejs后按照如下步骤操作就可以完成依赖打包了
进入目录 -> 安装包依赖 -> 执行打包合并, 运行如下命令
其中包文件包含了其他策略, 不过基本上用不到, 我们只要合并了得到 build 后的文件供我们使用就ok了

```
cd appframework 
npm install
grunt default

```

合并后的文件都在当前目录 `build` 文件夹下, 先从整理 `index.html` 文件开始

######css

```
 <link rel="stylesheet" type="text/css" href="build/css/icons.css" />    
 <link rel="stylesheet" type="text/css" href="build/css/af.ui.base.css"  />
 <link rel="stylesheet" type="text/css" href="build/css/af.ui.css"  />

```

######js


```
 <script src="build/appframework.js" ></script>
 <script src="build/ui/appframework.ui.js"></script>
 <script src="build/af.plugins.min.js"></script>

```

这样看起来 html 文件干净了不少 