#avalon

##插值


截取10个字符

```
{{ msg | truncate( 10 ) }} 
```

转成html "貌似不转译 < > .... "

```
{{ text | html }}
或者 
ms-html="text"
```

转成文本 "应该是把 < > 转译 后输出" , 默认应该是字符串

```
{{ text | text}}
或者
ms-text="text"
```