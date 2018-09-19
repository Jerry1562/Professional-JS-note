## Cookie的安全性问题
### 1. 为什么要使用Cookie？
答：众所周知，http是一种无状态的协议，这会给传递用户登录状态等操作造成困扰，为了解决这个问题，人们引入了`Cookie`。`Cookie`是设置在http头部的一段信息，一般由服务器设置，保存在客户端中。当客户端发送请求时，就会在头部携带一段`Cookie`信息，向服务器表明自己的状态（身份）。
### 2. Cookie有哪些配置项?
+ 名称：name
+ 值：value
+ 域名：domain
+ 路径：path
+ 失效时间：expires
+ 安全标志：secure

注意，除了name和value，其他属性都是服务器给浏览器的指示，客户端发给服务器的`Cookie`信息中不会携带这些内容，只会携带name和value。
### 3. Cookie的生存时间
+ 在默认状态下，`Cookie`的生存时间是从创建`Cookie`开始到会话关闭结束。
+ 在由服务器返回的http响应头`set-Cookie`中，也可以通过设置属性`expires`（失效时间）的值来指定`Cookie`的生存时间。
+ 当`expires`被设置为过去的某个值时，`Cookie`就会被立刻删除。
### 4. 客户端如何访问Cookie？
在JS中可以使用`document.cookie`对`Cookie`进行访问，对`Cookie`的操作都是通过操纵字符串来实现的，要注意的一点是：传入的name和value的值都必须进行URL编码。
推荐使用encodeURIComponent()进行编码，使用decodeURIComponent()进行解码。
### 5. Cookie带来的安全问题
既然可以通过客户端JS读取Cookie的信息，那么当网页遭到`XSS`攻击的时候，它的`Cookie`就会泄露。攻击者可以在`Cookie`传送的过程中访问它。
针对这两个问题，有对应的两点解决方案：
+ 服务器和客户端都可以设置`HttpOnly`，保证`Cookie`只能被服务端读取,避免`XSS`攻击。
+ 服务端可以设置安全标志`secure`保证`Cookie`只有在使用`SSL`链接时才能传输。

即使是这样，也不要在`Cookie`中储存敏感信息。
### 6. 最后看看`Cookie`长什么样子：
```javascript
"name=value; domain=.word.com; path=/; secure; HttpOnly"
```
这是服务器发给客户端的`Cookie`,头部属性为`Set-Cookie`。
