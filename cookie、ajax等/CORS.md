CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源
使用的限制。

CORS将跨域请求分为简单请求和非简单请求，对于简单请求，像WebSocket一样，
在请求头中加上origin字段，表示协议、端口及域；  

#### 简单请求的判定
1、请求方法是get、post、head三个其中之一，  

2、请求头仅包含以下项：
```
Accept
Accept-Language
Content-Language
Last-Event-ID
Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
```
#### 简单请求处理
同时满足以上两个条件的，判定为简单请求，直接加origin进行跨域；  

如果服务器响应头带有：`Access-Control-Allow-Origin`，且值为origin中的值，则表示服务
器同意了该域的ajax跨域请求，没有该头部则任务服务器禁止该源进行ajax跨域。  

如果响应头带有`Access-Control-Allow-Credentials`，则表示服务器允许该ajax
请求带上cookies信息（有上面那个字段时才有效），  

如果响应头带有`Access-Control-Expose-Headers`，则表示服务器允许从响应头
中获取该字段指定的响应头的值；   
该字段可选。CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能
拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、
Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers
里面指定。上面的例子指定，getResponseHeader('FooBar')可以返回FooBar字段的值。
#### 非简单请求的处理
非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者
Content-Type字段的类型是application/json。
##### 预检请求发送
非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"
请求（preflight）；  
预检请求的请求方法为OPTIONS，通常由两个请求头：
```
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
origin
```
`Access-Control-Request-Method`字段，表示当前是在为一个put方法，发送预检
请求；即当前要进行跨域的是一个put方法；
`Access-Control-Request-Headers`字段，表示询问`X-Custom-Header`请求头是否
被允许；  
即这两个字段，一个询问方法，一个询问请求头；
origin标识源。
##### 预检请求响应
```
Access-Control-Allow-Methods: GET, POST, PUT//必需
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 1728000
```
`Access-Control-Allow-Methods`，标识所有支持的跨域方法；
`Access-Control-Allow-Headers`,该字段是因为请求时发送了询问请求头的询问，
这里表示同意该请求头；
`Access-Control-Max-Age`，标识该预检请求的有效时间，即该时间段内，满足该
返回的ajax跨域不需要再询问。
`Access-Control-Allow-Credentials`，用来标识cookies；
当服务器同意了请求之后，此后每次浏览器正常的CORS请求，就都跟简单请求一样
，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin
头信息字段

如果服务器不同意该跨域请求，则不会在响应头中包含这些字段；
