`@HostListener`用来在代码中绑定事件的监听，本身作用于方法上，当监听的事件
发生时，会回调该方法；  

默认情况下，`@HostListener`监听的是宿主元素（当前组件或者当前指令作用的
组件）上的事件，也可以通过制定一些全局对象，使得能够监听其他的一些全局事
件，比如window、body、document等全局对象的事件；

```
//监听全局事件，并且将事件作为参数传入函数
@HostListener('document:keyup', ['$event'])
handleKeyboardEvent(kbdEvent: KeyboardEvent) { ... }

@HostListener('mouseleave') 
mouseleave() {
    this.backgroundColor = 'white';
}
```