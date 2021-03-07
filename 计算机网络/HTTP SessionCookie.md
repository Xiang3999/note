# 彻底了解Cookie和Session的区别（面试）

## 参考一

- `面试常考`
- ①Cookie可以存储在浏览器或者本地，Session只能存在服务器
- ②session 能够存储任意的 java 对象，cookie 只能存储 String 类型的对象
- ③Session比Cookie更具有安全性（Cookie有安全隐患，通过拦截或本地文件找得到你的cookie后可以进行攻击）
- ④Session占用服务器性能，Session过多，增加服务器压力
- ⑤单个Cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个Cookie，Session是没有大小限制和服务器的内存大小有关。

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/20200309140629545.png)

------

### 一.Cookie详解

> **（1）Cookie是什么 ？**

- Cookie，有时也用其复数形式Cookies。类型为“小型文本文件”，`是某些网站为了辨别用户身份`，进行Session跟踪而储存在用户本地终端上的数据（通常经过加密），`由用户客户端计算机暂时或永久保存的信息。`

> **（2）为什么要使用Cookie？解决了什么问题 ？**

- web程序是使用HTTP协议传输的，而HTTP协议是无状态的协议，对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。
- **cookie的出现就是为了解决这个问题。**
- `第一次登录后服务器返回一些数据（cookie）给浏览器，然后浏览器保存在本地，当该用户发送第二次请求的时候，就会自动的把上次请求存储的cookie数据自动的携带给服务器，服务器通过浏览器携带的数据就能判断当前用户是哪个了`。
- 特点：`cookie存储的数据量有限`，不同的浏览器有不同的存储大小，但一般不超过4KB。因此使用cookie只能存储一些小量的数据。
- 给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理。

> **（3）Cookie什么时候产生 ？**

- Cookie的使用一先要看需求。因为**浏览器可以禁用Cookie，同时服务端也可以不Set-Cookie。**
- `客户端向服务器端发送一个请求的时，服务端向客户端发送一个Cookie然后浏览器将Cookie保存`
- Cookie有两种保存方式，`一种是浏览器会将Cookie保存在内存中，还有一种是保存在客户端的硬盘中`，之后每次HTTP请求浏览器都会将Cookie发送给服务器端。
  ![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/20200309133040839.png)

> **（4）Cookie的生存周期？**

- Cookie在生成时就会被指定一个Expire值，这就是Cookie的生存周期，在这个周期内Cookie有效，超出周期Cookie就会被清除。有些页面将Cookie的生存周期设置为“0”或负值，这样在关闭浏览器时，就马上清除Cookie，不会记录用户信息，更加安全。

> **（5）Cookie有哪些缺陷 ？**

- ①`数量受到限制`。一个浏览器能创建的 Cookie 数量最多为 300 个，并且每个不能超过 4KB，每个 Web 站点能设置的
  Cookie 总数不能超过 20 个
- ②`安全性无法得到保障`。通常跨站点脚本攻击往往利用网站漏洞在网站页面中植入脚本代码或网站页面引用第三方法脚本代码，均存在跨站点脚本攻击的可能，在受到跨站点脚本攻击时，脚本指令将会读取当前站点的所有Cookie 内容（已不存在 Cookie 作用域限制），然后通过某种方式将 Cookie 内容提交到指定的服务器（如：AJAX）。一旦 Cookie 落入攻击者手中，它将会重现其价值。
- ③`浏览器可以禁用Cookie`，禁用Cookie后，也就无法享有Cookie带来的方便。

> **（6）cookie的应用场景**

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/20200309140918347.png)

------

### 二.Session详解

> **（1）web中什么是会话 ？**

- `用户开一个浏览器，点击多个超链接，访问服务器多个web资源，然后关闭浏览器，整个过程称之为一个会话`。

> **（2）什么是Session ？**

- Session:在计算机中，尤其是在网络应用中，称为“`会话控制`”。Session 对象存储特定用户会话所需的属性及配置信息。

> **（3）Session什么时候产生 ？**

- 当用户请求来自应用程序的 Web 页时，如果该用户还没有会话，则 Web 服务器将自动创建一个 Session 对象。
- 这样，当用户在应用程序的 Web 页之间跳转时，存储在 Session 对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/2020030913325953.png)

- 服务器会向客户浏览器发送一个每个用户特有的会话编号sessionID，让他进入到cookie里。

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/20200309133309391.png)

- 服务器同时也把sessionID和对应的用户信息、用户操作记录在服务器上，这些记录就是session。再次访问时会带入会发送cookie给服务器，其中就包含sessionID。

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/20200309133334750.png)

![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/20200309133342968.png)

- 服务器从cookie里找到sessionID，再根据sessionID找到以前记录的用户信息就可以知道他之前操控些、访问过哪里。

> **（4）Session的生命周期 ？**

- `根据需求设定`，一般来说，半小时。举个例子，你登录一个服务器，服务器返回给你一个sessionID，登录成功之后的半小时之内没有对该服务器进行任何HTTP请求，半小时后你进行一次HTTP请求，会提示你重新登录。

------

- **小结**：Session是另一种记录客户状态的机制，`不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上`。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。这就是Session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。

------

### 三.cookie和session结合使用

> web开发发展至今，cookie和session的使用已经出现了一些非常成熟的方案。在如今的市场或者企业里，一般有两种存储方式：

- 1、`存储在服务端：`通过cookie存储一个session_id，然后具体的数据则是保存在session中。如果用户已经登录，则服务器会在cookie中保存一个session_id，下次再次请求的时候，会把该session_id携带上来，服务器根据session_id在session库中获取用户的session数据。就能知道该用户到底是谁，以及之前保存的一些状态信息。这种专业术语叫做server side session。
- 2、`将session数据加密，然后存储在cookie中。`这种专业术语叫做client side session。flask采用的就是这种方式，但是也可以替换成其他形式。







## 参考二

https://blog.csdn.net/chen13333336677/article/details/100939030

**一、共同之处：**
==cookie和session都是用来跟踪浏览器用户身份的会话方式。==

**二、工作原理：**
**1.Cookie的工作原理**
（1）浏览器端第一次发送请求到服务器端
（2）服务器端创建Cookie，该Cookie中包含用户的信息，然后将该Cookie发送到浏览器端
（3）浏览器端再次访问服务器端时会携带服务器端创建的Cookie
（4）服务器端通过Cookie中携带的数据区分不同的用户
![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/20190917204655188.png)
**2.Session的工作原理**
（1）浏览器端第一次发送请求到服务器端，服务器端创建一个Session，同时会创建一个特殊的Cookie（name为JSESSIONID的固定值，value为session对象的ID），然后将该Cookie发送至浏览器端
（2）浏览器端发送第N（N>1）次请求到服务器端,浏览器端访问服务器端时就会携带该name为JSESSIONID的Cookie对象
（3）服务器端根据name为JSESSIONID的Cookie的value(sessionId),去查询Session对象，从而区分不同用户。
name为JSESSIONID的Cookie不存在（**关闭或更换浏览器**），返回1中重新去创建Session与特殊的Cookie
name为JSESSIONID的Cookie存在，根据value中的SessionId去寻找session对象
value为SessionId不存在**（Session对象默认存活30分钟）**，返回1中重新去创建Session与特殊的Cookie
value为SessionId存在，返回session对象
**Session的工作原理图**
![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/2019091720521815.png)
![在这里插入图片描述](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/2019091720523621.png)
**三、区别：**

cookie数据保存在客户端，session数据保存在服务端。

session
简单的说，当你登陆一个网站的时候，如果web服务器端使用的是session，那么所有的数据都保存在服务器上，客户端每次请求服务器的时候会发送当前会话sessionid，服务器根据当前sessionid判断相应的用户数据标志，以确定用户是否登陆或具有某种权限。由于数据是存储在服务器上面，所以你不能伪造。

cookie
sessionid是服务器和客户端连接时候随机分配的，如果浏览器使用的是cookie，那么所有数据都保存在浏览器端，比如你登陆以后，服务器设置了cookie用户名，那么当你再次请求服务器的时候，浏览器会将用户名一块发送给服务器，这些变量有一定的特殊标记。服务器会解释为cookie变量，所以只要不关闭浏览器，那么cookie变量一直是有效的，所以能够保证长时间不掉线。

如果你能够截获某个用户的cookie变量，然后伪造一个数据包发送过去，那么服务器还是 认为你是合法的。所以，使用cookie被攻击的可能性比较大。

如果cookie设置了有效值，那么cookie会保存到客户端的硬盘上，下次在访问网站的时候，浏览器先检查有没有cookie，如果有的话，读取cookie，然后发送给服务器。

所以你在机器上面保存了某个论坛cookie，有效期是一年，如果有人入侵你的机器，将你的cookie拷走，放在他机器下面，那么他登陆该网站的时候就是用你的身份登陆的。当然，伪造的时候需要注意，直接copy cookie文件到 cookie目录，浏览器是不认的，他有一个index.dat文件，存储了 cookie文件的建立时间，以及是否有修改，所以你必须先要有该网站的 cookie文件，并且要从保证时间上骗过浏览器

两个都可以用来存私密的东西，session过期与否，取决于服务器的设定。cookie过期与否，可以在cookie生成的时候设置进去。

**四、区别对比：**
(1)cookie数据存放在客户的浏览器上，session数据放在服务器上
(2)cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,如果主要考虑到安全应当使用session
(3)session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，如果主要考虑到减轻服务器性能方面，应当使用COOKIE
(4)单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能3K。
(5)所以：将登陆信息等重要信息存放为SESSION;其他信息如果需要保留，可以放在COOKIE中





--------------------

## 参考三

cookie 大家应该都熟悉，比如说登录某些网站一段时间后，就要求你重新登录；再比如有的同学很喜欢玩爬虫技术，有时候网站就是可以拦截住你的爬虫，这些都和 cookie 有关。如果你明白了服务器后端对于 cookie 和 session 的处理逻辑，就可以解释这些现象，甚至钻一些空子无限白嫖，待我慢慢道来。

### 一、session 和 cookie 简介

cookie 的出现是因为 HTTP 是无状态的一种协议，换句话说，服务器记不住你，可能你每刷新一次网页，就要重新输入一次账号密码进行登录。这显然是让人无法接受的，cookie 的作用就好比服务器给你贴个标签，然后你每次向服务器再发请求时，服务器就能够 cookie 认出你。

抽象地概括一下：**一个 cookie 可以认为是一个「变量」，形如 `name=value`，存储在浏览器；一个 session 可以理解为一种数据结构，多数情况是「映射」（键值对），存储在服务器上**。

注意，我说的是「一个」cookie 可以认为是一个变量，但是服务器可以一次设置多个 cookie，所以有时候说 cookie 是「一组」键值对儿，这也可以说得通。

cookie 可以在服务器端通过 HTTP 的 SetCookie 字段设置 cookie，比如我用 Go 语言写的一个简单服务：

```go
func cookie(w http.ResponseWriter, r *http.Request) {
    // 设置了两个 cookie 
    http.SetCookie(w, &http.Cookie{
        Name:       "name1",
        Value:      "value1",
    })

    http.SetCookie(w, &http.Cookie{
        Name:  "name2",
        Value: "value2",
    })
    // 将字符串写入网页
    fmt.Fprintln(w, "页面内容")
}
```

当浏览器访问对应网址时，通过浏览器的开发者工具查看此次 HTTP 通信的细节，可以看见服务器的回应发出了两次 `SetCookie` 命令：

![img](https://labuladong.github.io/algo/pictures/session/1.png)

在这之后，浏览器的请求中的 `Cookie` 字段就带上了这两个 cookie：

![img](https://labuladong.github.io/algo/pictures/session/2.png)

**cookie 的作用其实就是这么简单，无非就是服务器给每个客户端（浏览器）打的标签**，方便服务器辨认而已。当然，HTTP 还有很多参数可以设置 cookie，比如过期时间，或者让某个 cookie 只有某个特定路径才能使用等等。

但问题是，我们也知道现在的很多网站功能很复杂，而且涉及很多的数据交互，比如说电商网站的购物车功能，信息量大，而且结构也比较复杂，无法通过简单的 cookie 机制传递这么多信息，而且要知道 cookie 字段是存储在 HTTP header 中的，就算能够承载这些信息，也会消耗很多的带宽，比较消耗网络资源。

session 就可以配合 cookie 解决这一问题，比如说一个 cookie 存储这样一个变量 `sessionID=xxxx`，仅仅把这一个 cookie 传给服务器，然后服务器通过这个 ID 找到对应的 session，这个 session 是一个数据结构，里面存储着该用户的购物车等详细信息，服务器可以通过这些信息返回该用户的定制化网页，有效解决了追踪用户的问题。

**session 是一个数据结构，由网站的开发者设计，所以可以承载各种数据**，只要客户端的 cookie 传来一个唯一的 session ID，服务器就可以找到对应的 session，认出这个客户。

当然，由于 session 存储在服务器中，肯定会消耗服务器的资源，所以 session 一般都会有一个过期时间，服务器一般会定期检查并删除过期的 session，如果后来该用户再次访问服务器，可能就会面临重新登录等等措施，然后服务器新建一个 session，将 session ID 通过 cookie 的形式传送给客户端。

那么，我们知道 cookie 和 session 的原理，有什么切实的好处呢？**除了应对面试，我给你说一个鸡贼的用处，就是可以白嫖某些服务**。

有些网站，你第一次使用它的服务，它直接免费让你试用，但是用一次之后，就让你登录然后付费继续使用该服务。而且你发现网站似乎通过某些手段记住了你的电脑，除非你换个电脑或者换个浏览器才能再白嫖一次。

那么问题来了，你试用的时候没有登录，网站服务器是怎么记住你的呢？这就很显然了，服务器一定是给你的浏览器打了 cookie，后台建立了对应的 session 记录你的状态。你的浏览器在每次访问该网站的时候都会听话地带着 cookie，服务器一查 session 就知道这个浏览器已经免费使用过了，得让它登录付费，不能让它继续白嫖了。

那如果我不让浏览器发送 cookie，每次都伪装成一个第一次来试用的小萌新，不就可以不断白嫖了么？浏览器会把网站的 cookie 以文件的形式存在某些地方（不同的浏览器配置不同），你把他们找到然后删除就行了。但是对于 Firefox 和 Chrome 浏览器，有很多插件可以直接编辑 cookie，比如我的 Chrome 浏览器就用的一款叫做 EditThisCookie 的插件，这是他们官网：

![http://www.editthiscookie.com/](https://labuladong.github.io/algo/pictures/session/3.png)

这类插件可以读取浏览器在当前网页的 cookie，点开插件可以任意编辑和删除 cookie。**当然，偶尔白嫖一两次还行，不鼓励高频率白嫖，想常用还是掏钱吧，否则网站赚不到钱，就只能取消免费试用这个机制了**。

以上就是关于 cookie 和 session 的简单介绍，cookie 是 HTTP 协议的一部分，不算复杂，而 session 是可以定制的，所以下面详细看一下实现 session 管理的代码架构吧。

### 二、session 的实现

session 的原理不难，但是具体实现它可是很有技巧的，一般需要三个组件配合完成，它们分别是 `Manager`、`Provider` 和 `Session` 三个类（接口）。

![img](https://labuladong.github.io/algo/pictures/session/4.jpg)

1、浏览器通过 HTTP 协议向服务器请求路径 `/content` 的网页资源，对应路径上有一个 Handler 函数接收请求，解析 HTTP header 中的 cookie，得到其中存储的 sessionID，然后把这个 ID 发给 `Manager`。

2、`Manager` 充当一个 session 管理器的角色，主要存储一些配置信息，比如 session 的存活时间，cookie 的名字等等。而所有的 session 存在 `Manager` 内部的一个 `Provider` 中。所以 `Manager` 会把 `sid`（sessionID）传递给 `Provider`，让它去找这个 ID 对应的具体是哪个 session。

3、`Provider` 就是一个容器，最常见的应该就是一个散列表，将每个 `sid` 和对应的 session 一一映射起来。收到 `Manager` 传递的 `sid` 之后，它就找到 `sid` 对应的 session 结构，也就是 `Session` 结构，然后返回它。

4、`Session` 中存储着用户的具体信息，由 Handler 函数中的逻辑拿出这些信息，生成该用户的 HTML 网页，返回给客户端。

那么你也许会问，为什么搞这么麻烦，直接在 Handler 函数中搞一个哈希表，然后存储 `sid` 和 `Session` 结构的映射不就完事儿了？

**这就是设计层面的技巧了**，下面就来说说，为什么分成 `Manager`、`Provider` 和 `Session`。

先从最底层的 `Session` 说。既然 session 就是键值对，为啥不直接用哈希表，而是要抽象出这么一个数据结构呢？

第一，因为 `Session` 结构可能不止存储了一个哈希表，还可以存储一些辅助数据，比如 `sid`，访问次数，过期时间或者最后一次的访问时间，这样便于实现想 LRU、LFU 这样的算法。

第二，因为 session 可以有不同的存储方式。如果用编程语言内置的哈希表，那么 session 数据就是存储在内存中，如果数据量大，很容易造成程序崩溃，而且一旦程序结束，所有 session 数据都会丢失。所以可以有很多种 session 的存储方式，比如存入缓存数据库 Redis，或者存入 MySQL 等等。

因此，`Session` 结构提供一层抽象，屏蔽不同存储方式的差异，只要提供一组通用接口操纵键值对：

```go
type Session interface {
    // 设置键值对
    Set(key, val interface{})
    // 获取 key 对应的值
    Get(key interface{}) interface{}
    // 删除键 key
    Delete(key interface{})
}
```

再说 `Provider` 为啥要抽象出来。我们上面那个图的 `Provider` 就是一个散列表，保存 `sid` 到 `Session` 的映射，但是实际中肯定会更加复杂。我们不是要时不时删除一些 session 吗，除了设置存活时间之外，还可以采用一些其他策略，比如 LRU 缓存淘汰算法，这样就需要 `Provider` 内部使用哈希链表这种数据结构来存储 session。

PS：关于 LRU 算法的奥妙，参见前文「LRU 算法详解」。

因此，`Provider` 作为一个容器，就是要屏蔽算法细节，以合理的数据结构和算法组织 `sid` 和 `Session` 的映射关系，只需要实现下面这几个方法实现对 session 的增删查改：

```go
type Provider interface {
    // 新增并返回一个 session
    SessionCreate(sid string) (Session, error)
    // 删除一个 session
    SessionDestroy(sid string)
    // 查找一个 session
    SessionRead(sid string) (Session, error)
    // 修改一个session
    SessionUpdate(sid string)
    // 通过类似 LRU 的算法回收过期的 session
    SessionGC(maxLifeTime int64)
}
```

最后说 `Manager`，大部分具体工作都委托给 `Session` 和 `Provider` 承担了，`Manager` 主要就是一个参数集合，比如 session 的存活时间，清理过期 session 的策略，以及 session 的可用存储方式。`Manager` 屏蔽了操作的具体细节，我们可以通过 `Manager` 灵活地配置 session 机制。

综上，session 机制分成几部分的最主要原因就是解耦，实现定制化。我在 Github 上看过几个 Go 语言实现的 session 服务，源码都很简单，有兴趣的朋友可以学习学习：

https://github.com/alexedwards/scs

https://github.com/astaxie/build-web-application-with-golang





# [详解Cookie、Session和缓存](https://www.cnblogs.com/laoluoits/p/10855117.html)

引用：https://www.cnblogs.com/laoluoits/p/10855117.html

## **1 Cookie和Session**

   Cookie和Session都为了用来保存状态信息，都是保存客户端状态的机制，它们都是==为了解决HTTP无状态的问题==而所做的努力。

   Session可以用Cookie来实现，也可以用URL回写的机制来实现。用Cookie来实现的Session可以认为是对Cookie更高级的应用。

### 1.1两者比较

Cookie和Session有以下明显的不同点：

   1）Cookie将状态保存在客户端，Session将状态保存在服务器端；

   2）Cookies是服务器在本地机器上存储的小段文本并随每一个请求发送至同一个服务器。Cookie最早在RFC2109中实现，后续RFC2965做了增强。网络服务器用HTTP头向客户端发送cookies，在客户终端，浏览器解析这些cookies并将它们保存为一个本地文件，它会自动将同一服务器的任何请求缚上这些cookies。Session并没有在HTTP的协议中定义；

   3）Session是针对每一个用户的，变量的值保存在服务器上，用一个sessionID来区分是哪个用户session变量,这个值是通过用户的浏览器在访问的时候返回给服务器，当客户禁用cookie时，这个值也可能设置为由get来返回给服务器；

   4）就安全性来说：当你访问一个使用session 的站点，同时在自己机子上建立一个cookie，建议在服务器端的SESSION机制更安全些.因为它不会任意读取客户存储的信息。

### 1.2 Session机制

   Session机制是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。

当程序需要为某个客户端的请求创建一个session的时候，服务器首先检查这个客户端的请求里是否已包含了一个session标识 - 称为 session id，如果已包含一个session id则说明以前已经为此客户端创建过session，服务器就按照session id把这个 session检索出来使用（如果检索不到，可能会新建一个），如果客户端请求不包含session id，则为此客户端创建一个session并且生成一个与此session相关联的session id，session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个 session id将被在本次响应中返回给客户端保存。

#### 1.2.1 Session的实现方式

**1** **使用Cookie来实现**

   服务器给每个Session分配一个唯一的JSESSIONID，并通过Cookie发送给客户端。

当客户端发起新的请求的时候，将在Cookie头中携带这个JSESSIONID。这样服务器能够找到这个客户端对应的Session。

流程如下图所示：



[![clip_image002](D:\OneDrive_PAO\OneDrive - std.uestc.edu.cn\note\img\465934-20190513100826000-862722438.jpg)](https://img2018.cnblogs.com/blog/465934/201905/465934-20190513100825605-2060889336.jpg)



**2** **使用URL回显来实现**

   URL回写是指服务器在发送给浏览器页面的所有链接中都携带JSESSIONID的参数，这样客户端点击任何一个链接都会把JSESSIONID带会服务器。

   如果直接在浏览器输入服务端资源的url来请求该资源，那么Session是匹配不到的。

   Tomcat对Session的实现，是一开始同时使用Cookie和URL回写机制，如果发现客户端支持Cookie，就继续使用Cookie，停止使用URL回写。如果发现Cookie被禁用，就一直使用URL回写。jsp开发处理到Session的时候，对页面中的链接记得使用response.encodeURL() 。

#### 1.2.2 与Cookie相关的HTTP扩展头

   1）**Cookie**：客户端将服务器设置的Cookie返回到服务器；

   2）**Set-Cookie**：服务器向客户端设置Cookie；

   3）**Cookie2** (RFC2965)）：客户端指示服务器支持Cookie的版本；

   4）**Set-Cookie2** (RFC2965)：服务器向客户端设置Cookie。

#### 1.2.3 Cookie的流程

   服务器在响应消息中用Set-Cookie头将Cookie的内容回送给客户端，客户端在新的请求中将相同的内容携带在Cookie头中发送给服务器。从而实现会话的保持。

流程如下图所示：

[![clip_image004](D:\OneDrive_PAO\OneDrive - std.uestc.edu.cn\note\img\465934-20190513100826599-664996174.jpg)](https://img2018.cnblogs.com/blog/465934/201905/465934-20190513100826291-548763715.jpg)



## 2 缓存的实现原理

### 2.1 什么是Web缓存

WEB缓存(cache)位于Web服务器和客户端之间。

缓存会根据请求保存输出内容的副本，例如html页面，图片，文件，当下一个请求来到的时候：如果是相同的URL，缓存直接使用副本响应访问请求，而不是向源服务器再次发送请求。

HTTP协议定义了相关的消息头来使WEB缓存尽可能好的工作。

### 2.2缓存的优点

   **减少相应延迟**：因为请求从缓存服务器（离客户端更近）而不是源服务器被相应，这个过程耗时更少，让web服务器看上去相应更快。

   **减少网络带宽消耗**：当副本被重用时会减低客户端的带宽消耗；客户可以节省带宽费用，控制带宽的需求的增长并更易于管理。

### 2.3与缓存相关的HTTP扩展消息头

   **Expires**：指示响应内容过期的时间，格林威治时间GMT

   **Cache-Control**：更细致的控制缓存的内容

   **Last-Modified**：响应中资源最后一次修改的时间

   **ETag**：响应中资源的校验值，在服务器上某个时段是唯一标识的。

   **Date**：服务器的时间

   **If-Modified-Since**：客户端存取的该资源最后一次修改的时间，同Last-Modified。

   **If-None-Match**：客户端存取的该资源的检验值，同ETag。

### 2.4客户端缓存生效的常见流程

   服务器收到请求时，会在200OK中回送该资源的Last-Modified和ETag头，客户端将该资源保存在cache中，并记录这两个属性。当客户端需要发送相同的请求时，会在请求中携带If-Modified-Since和If-None-Match两个头。两个头的值分别是响应中Last-Modified和ETag头的值。服务器通过这两个头判断本地资源未发生变化，客户端不需要重新下载，返回304响应。常见流程如下图所示：

[![clip_image006](D:\OneDrive_PAO\OneDrive - std.uestc.edu.cn\note\img\465934-20190513100827230-1960987982.jpg)](https://img2018.cnblogs.com/blog/465934/201905/465934-20190513100826909-1243729344.jpg)



### 2.5 Web缓存机制

   HTTP/1.1中缓存的目的是为了在很多情况下减少发送请求，同时在许多情况下可以不需要发送完整响应。前者减少了网络回路的数量；HTTP利用一个“过期（expiration）”机制来为此目的。后者减少了网络应用的带宽；HTTP用“验证（validation）”机制来为此目的。

HTTP定义了3种缓存机制：

   1）**Freshness**：允许一个回应消息可以在源服务器不被重新检查，并且可以由服务器和客户端来控制。例如，Expires回应头给了一个文档不可用的时间。Cache-Control中的max-age标识指明了缓存的最长时间；

   2）**Validation**：用来检查以一个缓存的回应是否仍然可用。例如，如果一个回应有一个Last-Modified回应头，缓存能够使用If-Modified-Since来判断是否已改变，以便判断根据情况发送请求；

   3）**Invalidation：** 在另一个请求通过缓存的时候，常常有一个副作用。例如，如果一个URL关联到一个缓存回应，但是其后跟着POST、PUT和DELETE的请求的话，缓存就会过期。