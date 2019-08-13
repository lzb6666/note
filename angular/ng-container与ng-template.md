## ng-container
ng-container仅仅起到逻辑容器的效果，ng-container最终不会出现在dom树中，
但是其本身可以作用一些结构性的指令，来减少不需要的层级嵌套，使得我们不必
为了使用`ngFor`或`ngIf`等结构型指令，而专门添加外层dom容器；

## ng-template
ng-template顾明思议是做模版，单独添加并不会被渲染；除非使用结构型指令或者
其他方式调用；