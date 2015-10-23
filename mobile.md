###沉寂了几个月后终于能写点东西出来了, 最近一直在移动方面探索, 查找了很多移动开发方面的博文 资料, 自认为这两个月收益良多, 整理一下, 分享也好, 记录成长历程也好, 感慨下吧

前段事件一直很纠结, 最后还是决定在移动web开发方面在深入一些, 一直以来都觉得移动设备上的浏览器对于css js等方面的支持是非常好的, 感觉很easy, 后来发现, 虽然不用在考虑过多的兼容性问题, 但 随之而来的是如何提升性能, 以及js方面新的方法, 以及css3的动画还是有很大的挑战.


#####ios

添加主屏后启动浏览器可以隐藏地址栏 状态栏 当从HOME打开webapp时，navigator.standalone为true

`<meta name="apple-mobile-web-app-capable" content="yes"  />`

网上说是控制 ios 状态栏的 测试下没效果

`<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />`

禁止识别电话号码

`<meta name="format-detection" content="telephone=no"  />`

添加主屏图标

`<link rel="apple-touch-icon" href="icon.png" />`

启动动画图标 测试没生效, 有测试通过的童鞋指点下如何实现

`<link rel="apple-touch-startup-image" href="startup.png" />`


##### css3 首先看看css3比较常用的新特性


`box-sizing:border-box;` 可以不用在计算 `padding` 以及 `border`

`tap-highlight-color:rgba(0, 0, 0, 0);` ios 消除有绑定事件的元素 在触发事件的时候会有个般透明背景, 以及可以点击的元素, 以前曾经遇到过这个问题, 当时是做了个事件代理, 在父级元素上绑定了事件, 父级高度很大, 当时就发现, 父级元素触发事件会有灰色半透明背景, 不过好像现在木有这个问题了

`user-select: none;` 禁止选中文字

`touch-callout:none;` ios 防止复制 和 保存 图片

`nth-child()` 非常好用的选择器 可以传参  odd even 或者数字等, 用她来实现隔行换色简单的不得了

`[name="n"]` 属性选择器, 可以是任意属性, 非常方便, 如果想, 可以不用class id去选择元素了

`transition transform animation keyframes` css3动画大大减轻了js的负担啊, 在我开发过程中, 动画部分只需要js辅助一些就可以了, 比如切换个class等, 简单的令人发指, 可能是js动画相对来说难的缘故吧


`display:box;` 弹性盒子模型, 父级元素`display:box;`, 子集元素 `box-flex:1;` 作用是划分多少份, 占多少

比如: 下边代码是每个子集占一份, 父级平分3份

`div1 css box-flex:1;`

`div2 css box-flex:1;`

`div3 css box-flex:1;`

下边代码是第一个子集占3份, 第二个占2份, 第三个占1份, 父级平分6份

`div1 css box-flex:3;`

`div2 css box-flex:2;`

`div3 css box-flex:1;`


其他属性 `box-orient | box-direction | box-align | box-pack | box-lines`

1、box-orient: 排列方式

`[ horizontal | vertical | inline-axis | block-axis | inherit ]`

只用到了下边两个属性值

`box-orient: horizontal;`水平排列
`box-orient: vertical;`垂直排列

2、box-direction 用来确定父容器里的子容器排列顺序

`[ normal | reverse | inherit ]` 默认 倒序 继承

3、box-align 表示父容器里面子容器的垂直对齐方式

`[ start | end | center | baseline | stretch ]`

`start` 居顶对齐

`end` 居底对齐

`center` 居中对齐

`stretch` 拉伸到与父容器等高

4、box-pack: 表示子容器的水平对齐方式

`[ start | end | center | justify ]`

`start` 左对齐

`end` 右对齐

`center` 居中

`justify` 均分父级宽度


##### transform transition animation keyframes

transform: `[ rotate | scale | skew | translate | matrix; ]`


`rotate(10deg);` 旋转

`translate(x-px, y-px); translateX(x); translateY(y); translate3D(x, y, z);` 移动

`scale(1,1);` 缩放 参数数字, 当是负数的时候会旋转180度

`skew(10deg)` 扭曲

`matrix` 矩阵, 介个有点麻烦, 可以看下这篇文章 [理解CSS3 transform中的Matrix(矩阵)](http://www.zhangxinxu.com/wordpress/?p=2427)



 
transition ： `[ transition-property | transition-duration | transition-timing-function | transition-delay ]`

`transition-property` 执行变换的属性

`transition-duration` 变换延续的时间

`transition-timing-function` 在延续时间段，变换的速率变化 动作函数吧

`transition-delay` 变换延迟时间

[这个地址很详细](http://css3.bradshawenterprises.com/transitions/)


animation 和 transition 基本类似

`animation-name: name;` 动画名称, 名称是由 keyframes 声明而来

`animation-duration: 2s;` 变换延续的时间

`animation-iteration-count: 1;` 循环次数 `infinite` 无限循环

`animation-timing-function: linear;`动作类型

`animation-fill-mode: none | forwards | backwards | both` 依次 默认不设置, 动画结束时状态, 动画开始时状态, (向前和向后填充模式都被应用。) 类似于 toggle (个人理解), 常用的应该还是 forwards 吧, 因为动画结束默认是初始状态


`keyframes name {}` 像函数里边是动作帧

```
keyframes name {
	from { 
		-webkit-transform:translateY(-200%); 
	}
	to {
		-webkit-transform:translateY(0);
	}
}
```

```
keyframes name {
	0% { 
		-webkit-transform:translateY(-200%); 
	}
	10% {
		-webkit-transform:translateY(0);
	}
	100% {
		-webkit-transform:translateY(0);
	}
}
```


###js

ecma5 多啦很多新的API, 用起来真的方便很多, 除了`document.querySelector`之外最让我兴奋的是 `classList`

在读司徒正美的`avalon`时候 看到的有这个方法, 他用的两次replace来封装 addClass removeClass, 有兴趣的童鞋可以看下 `avalon` 源码

`classList` 提供了非常实用的几个方法

add: `elem.classList.add('class')` 

remove: `elem.classList.remove('class')`

contains: `elem.classList.contains('class')`

toggle: `elem.classList.toggle('class')`

看到这几个方法我第一想法就是 添加 时候, 如果存在会不会添加重复的 class, 实践证明我的想法是多余的, 系统提供的API处理的很好


获取 上 下 首 尾 元素, 我的封装方法

```
下边的 $.fn = prototype, $.each 是封装好的循环, 不过循环没有利用 forEach

var getNodeMap = {
	prev: function ( elem ) {
		return elem['previousElementSibling'];
	},
	next: function ( elem ) {
		return elem['nextElementSibling'];
	},
	first: function ( elem ) {
		return elem['firstElementChild'];
	},
	last: function ( elem ) {
		return elem['lastElementChild'];
	}
};

$.each( getNodeMap, function ( name, val ) {
	$.fn[ name ] = function () {
		if ( this.length === 0 ) {
			return this;
		}
		var v = val( this[ 0 ] );
		return v ? $( v ) : this;
	}
});

```

数组去重, 利用 `indexOf` 方便很多, 也可以用她来验证数组是否包含某个值

去重, 摘自jqmobi

```
for ( var i = 0, len = arr.length; i < len; i++ ) {
	if ( arr.indexOf(arr[ i ]) !== i ) {
		arr.splice(i, 1);
		i--;
	}
}
return arr;
```

判断是否 array, `isArray` 

另外还有 一些 `touch`  `css3Prefix` 等, 看了几个框架, 大致上实现思路基本差不多, 下边是我的处理方法

```
/* ==============================
	touch css3Prefix
================================ */
(function($, window, undefined){
	"use strict";
	var doc = document,
		style = doc.documentElement.style;
	$.cssPrefix = (function () {
		var items=[ 'webkit', 'Moz', 'ms', 'O' ];
		
		return {
			hasTouch         : 'ontouchstart' in window,
			cssTransformStart: '3d(',
			cssTransformEnd  : ', 0)',
			prefix           : (function (){
				var prefix;
				for( var j = 0; j < items.length; j++ ) {
					if(style[ items[ j ] + 'Transform' ] === '' ) {
						prefix = items[ j ];
					}
				}
				return prefix;
			})()
		};
	})();
	$.transEnd_EV = (function () {
		if ( $.isEmptyObject( $.cssPrefix ) ) return false;

		var transitionEnd = {
				''		: 'transitionend',
				'webkit': 'webkitTransitionEnd',
				'Moz'	: 'transitionend',
				'O'		: 'otransitionend',
				'ms'	: 'MSTransitionEnd'
			};

		return transitionEnd[ $.cssPrefix.prefix ];
	})();
	$.animateEnd_EV = (function () {
		if ( $.isEmptyObject( $.cssPrefix ) ) return false;

		var animateEnd = {
				''		: 'animationEnd',
				'webkit': 'webkitAnimationEnd',
				'Moz'	: 'animationend',
				'O'		: 'oAnimationEnd',
				'ms'	: 'MSAnimationEnd'
			};

		return animateEnd[ $.cssPrefix.prefix ];
	})();
	$.touch_EV = (function () {
		return {
			resize_EV : 'onorientationchange' in window ? 'orientationchange' : 'resize',
			start_EV  : $.cssPrefix.hasTouch ? 'touchstart' : 'mousedown',
			move_EV   : $.cssPrefix.hasTouch ? 'touchmove' : 'mousemove',
			end_EV    : $.cssPrefix.hasTouch ? 'touchend' : 'mouseup',
			cancel_EV : $.cssPrefix.hasTouch ? 'touchcancel' : 'mouseup'
		}
	})();
})( $$$ , window );
```

	 















