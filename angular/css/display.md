display属性决定了元素以何种方式展示在dom中，  
常用属性：
```
none	此元素不会被显示。
block	此元素将显示为块级元素，此元素前后会带有换行符。
inline	默认。此元素会被显示为内联元素，元素前后没有换行符。
inline-block	行内块元素。（CSS2.1 新增的值）
```
`display:none`相比于`visibility:hidden`区别在于，`display:none`根本不会
渲染该元素，dom中是看不到的，而`visibility:hidden`则存在与dom中，只是不
显示。  
不同的元素默认的display属性不同，最常见的是块级元素和行内元素之分；  

display还有其他许多更高级的特性，控制列表、表格等显示：
> [链接](http://www.w3school.com.cn/css/pr_class_display.asp) ：http://www.w3school.com.cn/css/pr_class_display.asp
