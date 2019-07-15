## CAS1.0
CAS1.0定义了一些术语：  

- TGT：Ticket Grangting Ticket  
门票发放票，TGT 是 CAS 为用户签发的登录票据，拥有了 TGT，用户就可以证明
自己在 CAS 成功登录过。TGT 封装了 Cookie 值以及此 Cookie 值对应的用户信
息。当 HTTP 请求到来时，CAS 以此 Cookie 值（TGC）为 key 查询缓存中有无 
TGT ，如果有的话，则相信用户已登录过。
- TGC：Ticket Granting Cookie
CAS server与客户端之间维持登陆状态和身份凭证的cookie，一般形式为：
`CASTGC=TGT-XXX...XXX`的一组cookie；  
它其实起着和session相同的作用，只是
作为完整的协议，应该包含该部分，有session时，可以不需要此key-value对，或
者放进session里；除了CAS serverr它并不会被发送到其他服务器，纯粹是CAS 
server验证用，因此可以用session代替。
- ST：Service Ticket  
具体访问某个服务的凭证，由CAS server发放；当带着由CAS server发放的ST访问
对应的服务，可以不需要登陆；  

## 访问流程
假设当前有CAS server和A、B两个服务；
1、用户访问A服务，A服务检测到用户没有登陆A（具体表现为：没有A的cookie或
session或前两个过期或者session中缺少登陆信息，具体由所设计的登陆凭据为准
），A服务将用户重定向至CAS server，一般形式为：`casUrl?lastUrl=xxx`，其
中的lastUrl为刚才用户访问A的地址，用于登陆后回到该页面，可以使用其他手段
保存，那么这里就单纯重定向至CAS服务器；  
这里为了能重定向回来，所以发送至CAS服务器的请求中，应当包含回调地址；
2、用户重定向至CAS服务器后，CAS服务器发现，该用户未在CAS服务器登陆过，表
现为不持有TGT或者TGT失效（TGT可能被放在cookie或session里）；然后将用户
重定向至登陆界面，要求用户登陆；
3、用户输入用户名密码，成功登陆；CAS服务器将该用户标记为登陆，生成TST（
因为是首次登陆CAS服务器），同时重定向回原来的服务地址，但是会带上参数该
服务的ST；
4、用户带上该ST重新请求A服务，A服务向CAS服务器验证该ST的合法性，合法则为
该用户执行一系列登陆操作；至此成功通过CAS登陆A服务；
5、用户此时访问B服务，如果B服务将用户重定向至CAS服务器，CAS服务器发现该
用户已登陆CAS，并且其他操作合法后，就直接生成B服务的ST，并重定向回去；
6、用户带着颁发的ST访问B服务，B服务验证成功后，执行一系列操作。


CAS也可以使用基于Rest Ful的API交换上述信息，本质上是信息交互的问题。
>作者：丁香园F2E
>链接：https://juejin.im/post/5a002b536fb9a045132a1727
>来源：掘金
>著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。 