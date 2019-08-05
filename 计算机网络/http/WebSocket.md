于是提出了WebSocket协议，一种浏览器与服务器进行全双工通讯的网络技术，属于
应用层协议。它基于TCP传输协议，并复用HTTP的握手通道。  

WebSocket虽然能实现全双工通信，但是只能由客户端发起WebSocket连接，可以防
止恶意连接；

WebSocket使用http进行协议升级，需要发送升级协议相关的请求头来与
服务器协商升级协议；
```
GET ws://localhost:3000/ws/chat HTTP/1.1
Host: localhost
Upgrade: websocket
Connection: Upgrade
Origin: http://localhost:3000
Sec-WebSocket-Key: client-random-string
Sec-WebSocket-Version: 13
```
1、url中的ws表示协议，wss则表示是基于SSL/TSL的安全链接；  
2、`Upgrade`和`Connection`首部表示与服务器协商升级；  
3、`Sec-WebSocket-Key`用于标记连接，提供基本的防护，防止无意连接，与响应
中的`Sec-WebSocket-Accept`搭配；  
4、`Sec-WebSocket-Version`用于表明希望使用的WebSocket的版本；  

服务器响应升级：
```
HTTP/1.1 101 Switching Protocols
Connection:Upgrade
Upgrade: websocket
Sec-WebSocket-Accept: Oy4NRAQ13jhfONC7bP8dTKb4PTU=
```
其中Sec-WebSocket-Accept的生成方式为;
```
toBase64( sha1( Sec-WebSocket-Key + 258EAFA5-E914-47DA-95CA-C5AB0DC85B11 )  )
```
