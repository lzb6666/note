java中assert是一个关键字，与C/C++中一样，意味断言，默认不启用；需要在启动
时指定JVM参数，`-enableassertions`或`-en`来开启；多用于在开发时调试；  

assert关键字后需要跟一个boolean表达式，当该boolean表达式为flase时，将抛出
`AssertionError`，如果true则继续执行；

同样Junit中也有Assert封装类，和assert关键字有着几乎一样的意思；