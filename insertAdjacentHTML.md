###insertAdjacentHTML

```
element.insertAdjacentHTML(position, html);
position是相对于element元素的位置，并且只能是以下的字符串之一：
beforebegin 在 element 元素的前面。
afterbegin 在 element 元素的第一个子元素前面。
beforeend在 element 元素的最后一个子元素后面。
afterend在 element 元素的后面。
html是字符串被解析成HTML或XML插入到DOM树中。
其中beforeend参数就是我们需要的appendHTML效果。
element.insertAdjacentHTML(beforeend, html);
```

