###css3 弹性盒子模型 | 伸缩盒模型

Flexbox的布局是一个用于页面布局的全新CSS3模块功能。它可以把列表放在同一个方向（从左到右或从上到下排列），并且让这些列表能延伸到占用可用的空间。较为复杂的布局可以通过嵌套一个伸缩容器（flex container）来辅助实现。
Flexbox可以简单快速的创建一个具有弹性功能的布局，当在一个小屏幕上显示的时候，Flexbox可以让元素在容器（伸缩容器）中进行自由扩展和收缩，从而容易调整整个布局。它的目的是使用常见的布局模式，比如说三列布局，可以非常简单的实现

[About](http://)
####伸缩布局

弹性盒模型, 我们也可以叫他伸缩盒模型 
一个设有 `display:flex` 或 `display:inline-flex` 的元素是一个伸缩容器，伸缩容器的子元素被称为为伸缩项目，这些子元素使用伸缩布局模型来排版。

####伸缩流方向

伸缩流方向由伸缩容器的 `flex-flow` 属性决定, 根据为w3c官方给出的流方向标准术语解释, 分为 `主轴` `侧轴`, 个人白话点就理解为 x y, `flex-flow` 属性值为 `row` , 那么主轴为水平, 侧轴 垂直, 属性值为 `column` 那么主轴 侧轴 则互换, `reverse` 则为轴反方向

在之后的属性中会有针对 `主轴` `侧轴` 的属性设置, 同时也比较常用的属性

```
flex-flow: row;
flex-flow: row-reverse;
flex-flow: column;
flex-flow: column-reverse;
```

####flex-flow

`flex-flow` 属性是同时设定 `flex-direction` 与 `flex-wrap` 属性的缩写，两个属性决定了伸缩容器的主轴与侧轴。

chrome 支持 `flex-direction` , 火狐并不支持, 不过不需要单独设置 `flex-direction` , 只需`flex-flow` 即可

####flex-direction

看起来貌似和 `flex-flow` 一样, 只不过 `flex-flow` 是集合了这几个属性

```
flex-direction: row;
flex-direction: row-reverse;
flex-direction: column;
flex-direction: column-reverse;
```
####flex-wrap

分别为不换行, 换行, 以及相反方向

```
flex-wrap: nowrap;
flex-wrap: wrap;
flex-wrap: wrap-reverse;
```

####显示顺序 order

初始值是 `order: 0;`  如果设置某一伸缩项目 `order:-1;` 这个伸缩项目将排列最前;

#### 主轴对齐 justify-content

初始值 `flex-start` 适用于伸缩容器
以下值分别为 起点对齐 终点 居中 两端 以及 两端留空(伸缩项目之间距离的1/2)

```
justify-content: flex-srart;
justify-content: flex-end;
justify-content: center;
justify-content: space-between;
justify-content: space-around;
```

####侧轴对齐 align-items

初始值 `stretch` 适用于伸缩容器
以下值分别 起点 终点 居中, 最后两个属性貌似没发现有什么作用, 有了解的指正一下

```
align-items: flex-start ;
align-items: flex-end;
align-items: center;
align-items: baseline;
align-items: stretch;
```
以上内容纯个人理解, 如有错误欢迎指正



