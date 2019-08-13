Angular 提供了一种叫做 DOM Query 的技术，用于从组件或者指令中获取View相关
的引用，主要来源于 @ViewChild 和 @ViewChildren 注解（或叫做装饰器，decorators）  

通过@ViewChild和@ViewChildren，获得的引用，能够对视图做进一步的操作；
```
@Component({
    selector: 'sample',
    template: `
        <span #tref>I am span</span>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("tref", {read: ElementRef}) tref: ElementRef;

    ngAfterViewInit(): void {
        console.log(this.tref.nativeElement.textContent);
    }
}
```
如果使用antDesign，那么ViewChild还能获取到对组件的引用，不过若非必须，应
当避免通过这种方式对组件过度操作；  

第二个参数 read 是可选的，因为 Angular 会根据 DOM 元素的类型推断出该引用
类型。例如，如果它（#tref）挂载的是类似 span 的简单 html 元素，Angular 返
回 ElementRef；如果它挂载的是 template 元素，Angular 返回 TemplateRef。一
些引用类型如 ViewContainerRef 就不可以被 Angular 推断出来，所以必须在 
read 参数中显式申明，antDesign组件将会返回Component引用；