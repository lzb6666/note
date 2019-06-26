Subject是Observable的子类,是RxJS对可观察对象系列操作的封装。Subject自身
即使可观察对象也是观察者；
### subject对象创建发布，作为可观察对象
```
//创建
subject: Subject<string> = new Subject<string>();
//订阅，这里使用lamdba表达式定义的匿名观察者，subcribe函数返回一个订阅事
//件，该事件可以被取消
 const subscriptionA: Subscription = subject.subscribe(
      (val: string) => {
        console.log(`observerA: ${val}`);
      }
    );
    //取消订阅
subscriptionA.unsubscribe();
```
作为可观察对象时，使用next函数先subject中发送新值，所有订阅了的观察者都
会收到该值。
### subject作为观察者
```
const subject: Subject<string> = new Subject<string>();
    const subscriptionA: Subscription = subject.subscribe(
      (val: string) => {
        console.log(`observerA: ${val}`);
      }
    );
    const subscriptionB: Subscription = subject.subscribe(
      (val: string) => {
        console.log(`observerB: ${val}`);
      }
    );

    const observable: Observable<string> = from(['Raph', 'Don']);
    observable.subscribe(subject);
```
在这里例子中，有两次值的传递，首先由observable对象传递给subject对象，
lamdba定义的匿名观察者，subject接受到的值自动的又被发布出去了。