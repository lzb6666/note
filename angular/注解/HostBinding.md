`@HostBinding`指令，用于绑定宿主元素的一些属性到组件或指令ts文件中的属性；
这些属性包括类、样式、普通属性等；

这样的绑定是一个双向绑定，可以通过`@HostBinding`获取宿主元素的属性值，也
可以通过该指令去设置这些值；

```
@HostBinding('style.color') color: string;
@HostBinding('style.borderColor') borderColor: string;  
```