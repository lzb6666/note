oAuth2.0是目前普遍使用的授权第三方应用的方式，主要目的是能够严格控制第三方
的访问；最常用的场景是，不希望告诉第三方密码，又希望能让第三方访问，同时
能够随时终止第三方的访问； 

OAuth 2.0 的标准是 RFC 6749 文件

 OAuth 引入了一个授权层，用来分离两种不同的角色：客户端（申请使用资源者）
 和资源所有者。......资源所有者同意以后，资源服务器可以向客户端颁发令牌。
 客户端通过令牌，去请求数据。这个和CAS单点登录的对比，单点登录中申请使用
 资源者和资源所有者是同一个方，而oAuth是不同的两方；

oAuth2.0主要有四种模式：
- 授权码（authorization-code）
- 隐藏式（implicit）
- 密码式（password）：
- 客户端凭证（client credentials）
其中最常用的，也是最安全的是授权码模式（参见第三方QQ登陆）；

oAuth2.0中常用的几个字段：
- response_type：
表明要求返回的类型，同时也表明了使用的模式；可能值为token、code；  
token：直接返回access_token，用于隐藏式；  
code：返回授权码，用于授权码模式；
- client_id：
表示当前的第三方应用，用于授权服务器校验；
- redirect_uri：
授权服务器处理完毕后的回调地址；
- scope：请求的权限

## 授权码模式
以QQ登录为例  
1、当用户需要在第三方网站登录，第三方网站将用户重定向至QQ第三方登录界面；
```
https://graph.qq.com/oauth2.0/show?which=Login&display=pc&client_id=101003590
&response_type=code&redirect_uri=https://www.nowcoder.com/oauth2/qqconfig?
callBack=https%3A%2F%2Fwww.nowcoder.com%2Fprofile%2F819484865&state=web&scope=get_user_info
```
可以看到，上面的url包含我们提到的主要的几点：client_id，response_type、
redirect_uri、scope，含义也都容易理解，这里有三个https，因为在回调地址中
还包含了用户当前停留的页面，这么做是希望用户在完成第三方授权后，能够跳转
到刚才点击登录浏览的页面，而不是主页；
2、用户在授权页面，以二维码或者手机确认或密码等方式登陆QQ进行授权，授权后
，QQ的授权服务器请求回调地址，并且带上code（授权码）；
```
https://a.com/callback?code=AUTHORIZATION_CODE
```
3、第三方服务器通过授权码和自己在申请接入QQ登陆时获得的client_id参数和
client_secret参数，再去申请access_token（访问令牌）；
```
https://b.com/oauth/token?
 client_id=CLIENT_ID&
 client_secret=CLIENT_SECRET&
 grant_type=authorization_code&
 code=AUTHORIZATION_CODE&
 redirect_uri=CALLBACK_URL
```
4、QQ的授权服务器，校验client_id参数和client_secret参数以及授权码，校验
通过则向回调地址发送access_token；  

除了access_token，往往还会带上其他的信息，refresh_token，用来当token过期
时，请求新的token，而不必用户重新授权；
```

{    
  "access_token":"ACCESS_TOKEN",
  "token_type":"bearer",
  "expires_in":2592000,
  "refresh_token":"REFRESH_TOKEN",
  "scope":"read",
  "uid":100101,
  "info":{...}
}
```
5、第三方服务器通过access_token来访问QQ资源服务器提供得资源；通常是在首部
加上`Authorization`字段，来放置access_token;

为什么授权服务器不直接返回access_token而是返回授权码，需要再次请求获得access_token？  
因为在登陆授权时，唯一标识第三方应用的是client_id，这时一个公开值，那么对于任何网站
都可以将该url或该client_id获取，从而伪装成该网站，诱导用户授权等，有很大的安全风险，
因此隐藏模式同样有这个问题；  
client_secret是通过后台发送的，有效保证了只有公开的client_id，不能够获取资源；

## 隐藏模式
隐藏模式指授权码隐藏，也称位简单模式；用在纯前端应用，没有后台的场景，这
种场景下，安全性也较低，并不常用；不过它的交互过程值得了解一下；

1、用户需要在第三方页面授权时，将用户重定向至授权服务器的登陆界面；其中
和授权码模式的不同在于，response_type为token，标识直接返回access_token，
redirect_uri中的uri为通常为当前正在浏览的第三方页面的uri（为的是等会重定向
还能跳转至当前页面）；
2、用户在授权页面完成授权后，授权服务器将用户重定向，其中redirect_uri为：
```
https://a.com/callback#token=ACCESS_TOKEN
```
这里有两个问题：1、前端如何使用该token？2、这里用的是锚点而不是请求参数
来传递值；  

对于第一个问题：授权页面将浏览器重定向至上述地址，此时地址栏就是上述地址，
js可通过window对象，获取到该地址（参数、锚点等都可以），进而拿到token进行后续操作；  

对于第二个问题：浏览器重定向时，不会发送锚点，也就是说如果放在url参数里，
那么请求第三方网页服务器时会带上这个token，而这时不必要的（因为它只在前端
使用），同时也由于oAuth允许http，所以会有中间人的风险；  

## 密码式
如果你高度信任某个应用，RFC 6749 也允许用户把用户名和密码，直接告诉该应用
。该应用就使用你的密码，申请令牌，这种方式称为"密码式"（password）。

## 凭证式
最后一种方式是凭证式（client credentials），适用于没有前端的命令行应用，
即在命令行下请求令牌。
```
https://oauth.b.com/token?
  grant_type=client_credentials&
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET
```
这种方式给出的令牌，是针对第三方应用的，而不是针对用户的，即有可能多个用
户共享同一个令牌。
> http://www.ruanyifeng.com/blog/2019/04/oauth_design.html