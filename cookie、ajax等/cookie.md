## cookie
cookie为了辨别用户身份，储存会话信息而储存在本地的文本数据，通常经过加密；
cookie通常由服务器在响应时通过Set-Cookie请求头设置，通过js也可以设置
cookie，不过这不是cookie的主要作用。  

### cookie属性
一个cookie通常包含一个自定义的key=value键值对，以及该cookie在时间、作用
域等属性，具体的包括：
```
private String comment; // ;Comment=VALUE ... describes cookie's use
private String domain; // ;Domain=VALUE ... domain that sees cookie
private int maxAge = -1; // ;Max-Age=VALUE ... cookies auto-expire
private String path; // ;Path=VALUE ... URLs that see the cookie
private boolean secure; // ;Secure ... e.g. use SSL
private boolean httpOnly; // Not in cookie specs, but supported by browsers
//摘自tomcat8.5中Cookie类定义中
```
- comment解释该cookie的作用
- domain表示该cookie作用的域范围
- maxAge表示该cookie的最大存活时间，超出该事件后该cookie会过期失效
- path表示对cookie作用路径的过滤，只对path下的路径有效
- secure设置为true时，只在https请求时才会带上该cookie
- httpOnly
### 服务器设置cookie
服务器通过如下形式的http响应头来通知浏览器设置cookie，如果cookie符合浏览
器限制，那么浏览器就会保存该cookie，并且在发送同源请求时自动带上该cookie
（跨域除外）。
```
Set-Cookie: PSTM=1561946271; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
```
### 浏览器对cookie的限制
一般浏览器都会对cookie的大小和同一域名下的cookie数量进行限制，在大多数高
级浏览器中，对cookie的大小限制为4Kb左右，同一域名下的数量限制在30到50个；

当cookie的大小超过限制时，浏览器会拒绝保存该cookie；当cookie的数量超过限
制时，浏览器会淘汰部分cookie，最典型的算法有LRU和随机淘汰。
### cookie跨域问题
默认情况下，在跨域请求中浏览器是不会携带cookie的，为了让跨域请求能够携带
cookie，在浏览器和服务器需要修改两个方面：
```
//原生ajax
var xhr = new XMLHttpRequest();
xhr.open('GET', url);
xhr.withCredentials = true;
xhr.send();
//jquery中的ajax
$.ajax({
   url: a_cross_domain_url,
   xhrFields: {
      withCredentials: true
   }
});
```
withCredentials设置为true时，可携带cookie；但同时要经过服务器的同意，服
务器的响应中，需要有下面两个响应头：
```
Access-Control-Allow-Origin: http://index.com:4001//允许携带cookies的域名，不能使用*统配；
Access-Control-Allow-Credentials: true//允许携带cookies
```
