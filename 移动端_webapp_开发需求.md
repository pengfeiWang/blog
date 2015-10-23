# 移动端 webapp 开发需求

##监听 请求初始页面返回状态
##检查 设备网络服务状态是否正常 以及 是否可以拨号


米ui 与 其他机型不同, 底部会有多余
老的浏览器 flex 支持非常差
用浏览器的默认滚动需要处理下滚动条的尺寸


高分屏媒体查询

```
@mixin retinize($file, $type, $width, $height) {
    background-image: url('../img/' + $file + '.' + $type);
    @media (-webkit-min-device-pixel-ratio: 1.5),
           (min--moz-device-pixel-ratio: 1.5),
           (-o-min-device-pixel-ratio: 3/2),
           (min-device-pixel-ratio: 1.5),
           (min-resolution: 1.5dppx) {
                & {
                    background-image: url('../img/' + $file + '-2x.' + $type);
                    -webkit-background-size: $width $height;
                    -moz-background-size: $width $height;
                    background-size: $width $height;
                }
            }
}
```

绑定 touchstart的button不能通过disabled来禁用事件

app 后台切换到前台需要 调用页面js 来初始 html以及相关程序

av 数据类型保持一致, 需要监听的数据需要初始预先声明

js 判断 UA 不准确, 由app调用js, 返回UA信息

nativeMethod=goBack
navtiveMethod=getUserAgent