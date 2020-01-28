# 网络

## History历史记录管理

```js
window对象通过History对象提供对浏览器历史记录的访问能力,允许用户在历史记录中自由的前进和后退,而在HTML5中,还可以操作历史记录中的数据;
1.history.pushState(stateObj1,'','bar.html')添加一条历史记录,并不会刷新页面,也就是说页面还是原来的页面,只是可以后退,前进罢了;
2.history.replaceState(stateObj,'','bar.html')替换当前历史记录,所以现在是既不能后退也不能前进,而且页面依旧是展示老的内容,并没有被新设置的页面所替换;
window.onpopstate历史记录发生变化时触发,1,2不会触发;
window.onpopstate = function(e) { // 这里的e就是传过来的参数 stateObj
   console.log(e.state)
}
History属于Browser对象,Browser对象描述浏览器信息的,下有Window(全局对象,包含了所有的对象,也包括下面这些对象),Navigator(浏览器信息,浏览器版本号,浏览器品牌名称),Screen(客户端浏览器显示屏幕的信息),History(可以前进页面,可以后退页面),Location(操作URL,获取URL域名的部分,参数的部分); https://www.w3school.com.cn/jsref/dom_obj_history.asp
history.back();
history.forward();
history.go(); // -1表示后退1; 1表示往前进1
history.length; // 当前页面一共有多少条历史记录了,一共前进,后退操作了多少次
```

```
问题: 我们使用ajax获取数据的时候是局部获取数据,并不会进行页面的刷新,所以浏览器后退是不能够返回上一个操作的;
解决问题: 无刷新历史记录切换
```

## Cookie使用机制

```
个性化服务: HTTP是无状态的请求/相应连接,导致连接断开猴,再次连接服务器无法识别用户;
我们需要用一些技术来帮助服务器去识别用户:
1.跟踪客户端IP地址,动态IP;
2.借助http首部放置用户身份信息,referer,e-mail;
3.胖URL在URL中嵌入识别信息,丑陋,无法共享URL,非持久;
4.cookie在客户端存储用户标识信息,识别用户,持久化最好的方式;
```

## webStorage本地存储

```
1.storage本地存储
本地存储可以使用cookie,但是cookie本地存储不便:大小限制(4k),随http传输;
storage本地存储容量大(5M)仅供本地存储使用;
2.分类
sessionStorage 临时存储 浏览器关闭存储结束
localStorage 永久存储 除非用户手动删除
```

```js
localStorage.username = 'sxw'
var obj = {
  "name": "sxw"
}
localStorage.obj = JSON.stringify(obj)
var str = localStorage.obj
console.log(JSON.parse(str))
```

## CORS跨域资源共享

```
1.cross-origin resource sharing(CORS)跨域资源共享,是一种使用额外HTTP首部实现跨域获取资源权限的机制;
XMLHttprequest获取非同源资源会被浏览器拦截;
eg: localhost下的资源发送XMLSttprequest请求同源,非同源资源
2.CORS分类
a.简单请求
   使用下列方法之一: GET POST HEAD
   content-Type值为下列之一: text/plain multipart/form-data application/x-www-form-urlencoded
   除了正常发送请求之外,若想实现cors跨域,还需要服务器配置正确的相应首部,否则无法获取;
   access-contral-allow-origin: *(http://localhost)访问控制允许源
b.预检请求
```

## postMessage通信

```
为了安全考虑,浏览器会有同源策略限制,禁止跨域访问数据,但是具有src属性的标签不受限制,可以跨域访问资源。
```

```html
<img src="" alt="">
<script src=""></script>
<iframe src="" frameborder="0"></iframe>
```

```
该window.postMessage()方法安全的启用Window对象之间的跨源通信,提供了一种受控制的机制来安全地规避这种限制;使用targetWindow.postMessage()在其上发送一个MessageEvent,然后,接收窗口可根据需要自由处理此事件,传递给window.postMessage()的参数(即“message")通过事件对象暴露给接口窗口;otherWindow.postMessage(message,targetOrigion),发送数据;message事件: 相应postMessage发送的数据;
webSocket双向通信
```

## `HTTP`状态码

```
HTTP是hypertext transfer protocol（超文本传输协议）的简写,它是TCP/IP协议的一个应用层协议,用于定义WEB浏览器与WEB服务器之间交换数据的过程。客户端连上web服务器后,若想获得web服务器中的某个web资源,需遵守一定的通讯格式,HTTP协议用于定义客户端与web服务器通迅的格式;
```

```
a)200,请求成功,一切正常,数据成功返回;
b)301,永久性重定向,是指所请求的文档在别的地方,文档新的URL会在定位响应头信息中给出,浏览器会自动连接到新的URL;
c)302,临时重定向,该状态码表示请求的资源已被分配了新的URI,希望用户能使用新的URI访问;
d)303,该状态码表示由于请求对应的资源存在着另一个URI,应使用GET方法定向获取请求的资源;
e)403,Foribidden 服务器端理解本次请求,但是拒绝执行任务,没有权限访问;
f)404,NOT,found 请求的资源,网页无法找到,不存在;
g)503,服务器端无法响应,服务器由于在维护或已经超载而无法响应;
```

### http和https的区别

```
为了数据传输的安全,HTTPS在HTTP的基础上加入了SSL协议,SSL依靠证书来验证服务器的身份,并为浏览器和服务器之间的通信加密;
http是超文本传输协议,信息是明文传输,https则是具有安全性的ssl加密传输协议;
http的连接很简单,是无状态的;HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议,比http协议安全;
```

## 跨域

```
浏览器最核心,最基本的安全功能是同源策略。限制了一个源中加载文本或者脚本与其他源中资源的交互方式,当浏览器执行一个脚本时会检查是否同源,只有同源的脚本才会执行,如果不同源即为跨域;
	a.Jsonp: 原理就是利用了script标签不受同源策略的限制,在页面中动态插入script,script标签的src属性就是后端api接口的地址,并且以get的方式将前端回调处理函数名称告诉后端,后端在响应请求时会将回调返还,并且将数据以参数的形式传递回去;
	b.CORS: (跨域资源共享)是一种允许当前域的资源被其他域的脚本请求访问的机制;
	当使用XMLHttpRequest发送请求时,浏览器如果发现违反了同源策略就会自动加上一个请求头: origin;后端在接受到请求后,确定响应后,会在Response Headers中加入一个属性: Access-Control-Allow-Origin,值就是发起请求的源地址,浏览器得到响应会进行判断Access-Control-Allow-Origin的值是否和当前的地址相同,只有匹配成功后才进行响应处理;现代浏览器和移动端都支持CORS,IE下需要8+;
	c.服务器跨域,服务器中转代理;
	前端向本地服务器发送请求,本地服务器代替前端再向服务器接口发送请求进行服务器间通信,本地服务器是个中转站的角色,再将响应的数据返回给前端;
```

## 优化网站性能

```
a.减少页面体积,提升网络加载;
    静态资源压缩合并,(js/css代码压缩合并,雪碧图);
    静态资源缓存;
    使用CDN(内容分发网络)加载资源更快;
b.优化页面渲染;
    css放在前面,js放后面;
    懒加载;
    减少dom操作;
```

## 浏览器端存储

```
cookie / localStorage / sessionStorage 他们都是保存在浏览器端,且同源;
区别:
	a.cookie数据始终在同源的http请求中携带,即cookie在浏览器和服务器间来回传递;而sessionStorage和localStorage不会自动把数据发给服务器,仅在本地保存;
	b.存储大小限制也不同;cookie数据不能超过4k,同时因为每次http请求都会携带cookie,所以cookie只适合保存很小的数据,如会话标识;sessionStorage和localStorage 虽然也有存储大小的限制,但比cookie大得多,可以达到5M或更大;
	c.数据有效期不同,sessionStorage仅在当前浏览器窗口关闭前有效,自然也就不可能持久保持;localStorage 始终有效,窗口或浏览器关闭也一直保存,因此用作持久数据;cookie只在设置的cookie过期时间之前一直有效,即使窗口或浏览器关闭;
	d.作用域不同,sessionStorage不在不同的浏览器窗口中共享,即使是同一个页面;localStorage在所有同源窗口中都是共享的;cookie也是在所有同源窗口中都是共享的;
```

## get和post请求

```
a.如果不设定请求方式,默认是get方式;
b.GET请求只能进行url编码,而POST支持多种编码方式;
c.GET请求参数会被完整保留在浏览器历史记录里,而POST中的参数不会被保留;
d.GET请求在URL中传送的参数是有长度限制的,而POST没有;
e.对参数的数据类型: GET只接受ASCII字符,而POST没有限制;
f.安全性,GET比POST更不安全,因为参数直接暴露在URL上,所以不能用来传递敏感信息。GET参数通过URL传递,POST放在Request body中;
g.GET产生一个TCP数据包,POST产生两个TCP数据包;对于GET方式的请求,浏览器会把http header和data一并发送出去,服务器响应200(返回数据);而对于POST,浏览器先发送header,服务器响应100 continue,浏览器再发送data,服务器响应200 ok(返回数据);
```

## 一个页面从输入URL到页面加载完的过程中都发生了什么事情？

```
加载过程:
    浏览器根据DNS服务器解析得到域名的IP地址;
    向这个IP的机器发送HTTP请求;
    服务器收到,处理并返回HTTP请求;
    浏览器得到返回内容;
    渲染过程;
    根据HTML结构生成DOM树;
    根据CSS生成CSSOM;
    将DOM和CSSOM整合形成RenderTree;
    根据RenderTree开始渲染和展示;
```

## TCP三次握手与四次挥手

```
TCP是主机对主机层的传输控制协议,提供可靠的连接服务,采用三次握手确认建立一个连接:
位码即tcp标志位,有6种表示:
	SYN(synchronous建立连接)
	ACK(acknowledgement 表示响应、确认)
	PSH(push表示有DATA数据传输)
	FIN(finish关闭连接)
	RST(reset表示连接重置)
	URG(urgent紧急指针字段值有效)
```

```
三次握手:
第一次握手: 客户端发送syn包(syn=x)到服务器,并进入SYN_SEND状态,等待服务器确认;
第二次握手: 服务器收到syn包,必须确认客户的SYN（ack=x+1）,同时自己也发送一个SYN包（syn=y）,即SYN+ACK包,此时服务器进入SYN_RECV状态;
第三次握手: 客户端收到服务器的SYN＋ACK包,向服务器发送确认包ACK(ack=y+1),此包发送完毕,客户端和服务器进入ESTABLISHED状态,完成三次握手;
握手过程中传送的包里不包含数据,三次握手完毕后,客户端与服务器才正式开始传送数据。理想状态下,TCP连接一旦建立,在通信双方中的任何一方主动关闭连接之前,TCP 连接都将被一直保持下去;
确认号: 其数值等于发送方的发送序号+1(即接收方期望接收的下一个序列号);
```

```
四次挥手:
与建立连接的“三次握手”类似,断开一个TCP连接则需要“四次挥手”:
第一次挥手: 主动关闭方发送一个FIN,用来关闭主动方到被动关闭方的数据传送,也就是主动关闭方告诉被动关闭方: 我已经不会再给你发数据了(当然,在fin包之前发送出去的数据,如果没有收到对应的ack确认报文,主动关闭方依然会重发这些数据),但是,此时主动关闭方还可以接受数据;
第二次挥手: 被动关闭方收到FIN包后,发送一个ACK给对方,确认序号为收到序号+1(与SYN相同,一个FIN占用一个序号);
第三次挥手: 被动关闭方发送一个FIN,用来关闭被动关闭方到主动关闭方的数据传送,也就是告诉主动关闭方,我的数据也发送完了,不会再给你发数据了;
第四次挥手: 主动关闭方收到FIN后,发送一个ACK给被动关闭方,确认序号为收到序号+1,至此,完成四次挥手;
```

```
TCP的四次挥手过程（简言之）: 主动关闭方向被动关闭方发送不会再给你发数据了的信息;被动关闭方对收到的主动关闭方的报文段进行确认;被动关闭方向主动关闭方发送我也不会再给你发数据了的信息;主动关闭方再次对被动关闭方的确认进行确认;
```

## 重排(回流)和重绘

```
重绘: 浏览器会把HTML解析成DOM,把CSS解析成CSSOM,DOM和CSSOM合并就产生了Render Tree;有了RenderTree,我们就知道了所有节点的样式,然后计算他们在页面上的大小和位置,最后把节点绘制到页面上;
回流: 当Render Tree中部分或全部元素的尺寸、结构、或某些属性发生改变时,浏览器重新渲染部分或全部文档的过程称为回流;
回流触发的情况: 页面首次渲染;浏览器窗口大小发生改变;元素尺寸或位置发生改变;元素内容变化（文字数量或图片大小等等）;元素字体大小变化;添加或者删除可见的DOM元素;触发display: none;
当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等）,浏览器会将新样式赋予给元素并重新绘制它,这个过程称为重绘;
避免频繁的样式操作,最好一次性重写style,或者一次性更改class,避免频繁操作dom,对具有复杂动画的元素使用绝对定位,使它脱离文档流,否则会引起父元素及后续元素频繁回流;
```

## 浏览器执行时间线

```
根据js执行那一刻开始的执行顺序,浏览器加载的时间线:
1.创建document对象,开始解析web页面,这时document.readyState等于’loading’;
2.遇到link标签（外部引用css）创建线程加载,并继续解析文档,即异步加载;
3.遇到非异步的script标签,浏览器加载并阻塞,等待js加载完成;
4.遇到异步的script标签,浏览器创建线程加载,并继续解析文档;对于async属性的脚本,脚本加载完成后立即执行;对于defer属性的脚本,脚本等到页面加载完之后再执行（异步禁止使用document.write）;
5.遇到img等,先正常解析dom结构,然后浏览器异步加载src,并继续解析文档;
6.当文档解析完成之后（即renderTree构建完成之后， 此时还未下载完对吧）,document.readyState=‘interative’,活跃的,动态的;
7.文档解析完成后,所有设置有defer的脚本会按照顺序执行;
8.文档解析完成之后,页面会触发document上的一个DOMContentLoad事件;
9.当页面所有部分都执行完成之后 document.readyState == ‘complete’ 之后就可以执行window.onload事件;
```

## 异步加载js

```
javascript异步加载的三种方案:
1.defer 异步加载,但要等到dom文档全部解析完才会被执行,只有IE能用;
2.async 异步加载,加载完就执行,async只能加载外部脚本,不能把js写在script标签里
(1.2 执行时也不阻塞页面)
3.创建script,插入到DOM中,加载完毕后callBack;
```

