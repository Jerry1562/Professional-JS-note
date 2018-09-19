## Storage---Cookie之外的储存会话数据的途径
### 1. 为什么要用Storage？
答：这个问题要从如何储存会话信息说起。众所周知，Cookie可以用来储存会话信息，它会跟随http请求从客户端返回到服务器中。但是如果有一些无需频繁发回服务器的会话信息，使用Cookie是不合理的。为了解决这个问题，Web Storage诞生了。Web Storage的目的是克服由cookie带来的一些限制。   
《JS高级程序设计》中表明Web Storage的两个主要目标是：
+ 提供一种Cookie之外的储存会话数据的途径。
+ 提供一种储存大量可以跨会话存在的数据机制。

简单来说，Storage是本地用来储存大量会话信息的结构，它储存的信息不会被持续地发回服务器。
### 2. Stroage的方法
+ clear() --- 删除所有值
+ getItem(name) --- 根据指定名字获取值
+ key(Index) --- 获取Index位置处的值的名字
+ removeItem(name) --- 删除name指定的名值对
+ setItem(name,value) --- 新增名值对
+ .length --- 返回储存名值对的数量

Storage中的数据是以名值对的方式储存，它只能储存字符串，非字符串的数据会被转成字符串保存。
### 3. Stroage的类型
+ sessionStorage   
特点：生存时间为会话开始到会话关闭，其中储存的数据可以跨越页面刷新而存在。它不能被跨页面访问，即使是两个完全相同的页面。
+ globalStorage  
特点：使用时需要指定域名，如`globalStorage["wrox.com"]`，这个Storage就可以被wrox.com以及它的所有子域访问到。同时，它还有同协议同端口的限制。
+ localStorage  
特点：相当于`globalStorage[loaction.host]`，它有有同域名（子域名无效）同协议同端口的限制。
### 4. 三种Storage对象的选用
SessionStorage适合用于储存仅针对会话的小段数据的储存，globalStorage和localStorage适合用于跨越会话的数据的储存。
