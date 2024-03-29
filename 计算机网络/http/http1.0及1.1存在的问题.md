## HTTP：1.0存在的问题
默认每个http请求都要建立一个TCP连接，频繁的开启释放连接，影响效率并且浪费
资源；  

以及HTTP1.1中的问题

## HTTP1.1存在的问题
- 报文首部数据冗余
- 数据未压缩，传输量大
- 只能由客户端向服务器发送单向请求
- 请求并发问题

## 一些基于1.1的解决方案
上述问题中，大致可归为三类：数据传输量问题、单向请求问题以及请求并发问题；
#### 对于单向请求问题-WebSocket
传统的解决方案就是轮询，这样做的坏处显而易见；  

于是提出了WebSocket协议，一种浏览器与服务器进行全双工通讯的网络技术，属于
应用层协议。它基于TCP传输协议，并复用HTTP的握手通道。  

WebSocket虽然能实现全双工通信，但是只能由客户端发起WebSocket连接，可以防
止恶意连接；  

详见WebSocket.md
#### 对于请求并发问题
从http1.0升级至1.1的过程中，首先默认支持了**持久化TCP连接**，避免了多次建立
和释放TCP连接的开销；  

tips: http的请求和响应之间并无关系，在http1.1及1.0当中，保证请求和响应对
应的方式为，单个线程开启的TCP连接上，一定要等请求响应后才能继续发送下一
个请求，为的就是不打乱顺序；  

于是进而有了**请求管线化**，请求管线化可以一起请求多个http，但是服务器必须按
照，请求的顺序来响应；这样虽然减少了等待的时间，但是仍然受到HOL问题（队头
阻塞问题，队列中前面的阻塞了，后面的即使准备好了也只能等待）；因此管线化
技术并不被主流浏览器默认支持；  

并不是所有类型的 HTTP 请求都能用到流水线：只有幂等的方法，比如 GET
、HEAD、PUT 和 DELETE 能够被安全的重试：如果有故障发生时，流水线的内容要
能被轻易的重试。  

**域名分片**，主流浏览器对于网页的加速通常是通过多个线程来访问网络资源，这些
线程分别建立不同的TCP连接，浏览器往往会限制同一域名下的线程数量（太多
的线程并不一定有更好的结果），因此如果把资源放在不同的域名下就可以从一定
程度上绕开这个限制；不过显然这个技术并不友好，不建议使用；

**HTTP2.0**，为了更好的解决这些问题，HTTP2.0诞生了，HTTP2.0中，通过多路复
用，二进制传输，首部压缩等方法，进一步的解决了HTTP的瓶颈；