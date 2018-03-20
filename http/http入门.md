#### 一、http简介
http是超文本传输协议 是客户端和服务端为了相互通信约定的一种数据格式。
这种数据格式分为客户端向服务端通信约定的**请求格式**和服务端向客户端通信约定的**响应格式**。
#### 二、请求格式
##### 1、结构
**请求行**：（请求方法/url/http版本号）、**请求头**（用来说明服务器要使用的附加信息）、**空行**、**请求体**
![2964446-fdfb1a8fce8de946](https://user-images.githubusercontent.com/17128499/36630215-cb161c7a-199d-11e8-95ca-9995d58afa2a.png)
##### 2、请求方法
##### 1）、创建的请求方法：
get: 请求服务发送某个资源
head: 和get类似，但是服务器在响应中只返回响应头， 不返回响应数据。
put:回向服务器写入文档，允许用户对对内容进行修改。
post: ;用来向服务器输入数据。
delete: 请求服务器删除url指定的资源
 post put head delete

#####2）、 get和post区别
a、传参位置 get方法传输数据，是将数据放在url后面，用？分隔url和传输数据，多个参数用&连接，如果数据是中文，则用base64加密；
    post方法是将数据放在请求体传输，在url中不可见。
b、传参大小 浏览器对url的长度有限制，因此get方法传参大小有限制，post传参理论上无限制。
##### 3、请求头
1）、Host: 服务器域名和端口号
2）、User-Agent: 它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等。
3）、Referer: 当前请求的来源地址
4）、accept:  声明客户端可以接受哪些类型的数据
5）、Content-Encoding: 声明客户端可以接受哪些压缩方式
eg:
- GET /562f25980001b1b106000338.jpg HTTP/1.1
- Host:    img.mukewang.com
- User-Agent:  Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
- Accept:  image/webp,image/*,*/*;q=0.8
- Referer: http://www.imooc.com/
- Accept-Encoding: gzip, deflate, sdch
- Accept-Language:  zh-CN,zh;q=0.8

#### 三、响应格式
##### 1、结构
**状态行**（HTTP协议版本号  状态码  状态消息）、 **响应头**（说明客户端要使用的附加信息）、 **空行**、 **请求体**
![2964446-1c4cab46f270d8ee](https://user-images.githubusercontent.com/17128499/36630697-81c35882-19a5-11e8-9775-7374bbaa48a5.jpg)

##### 2、状态码
1xx: 指示信息 表示请求已被接受
2xx: 成功信息 表示请求已被接受并成功处理
3xx: 重定向 表示要完成请求必须进一步处理
4xx: 客户端错误 请求有语法错误或无法实现
5xx: 服务端错误 服务器未能实现合法的请求

> 200 OK                        //客户端请求成功
400 Bad Request               //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
403 Forbidden                 //服务器收到请求，但是拒绝提供服务
404 Not Found                 //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error     //服务器发生不可预期的错误
503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常

>

##### 3、响应头
1）、Content-Type: 服务端传过来的数据格式
常见的Content-Type字段的值：
- text/plain
- text/html
- text/css
- image/jpeg
- image/png
- image/svg+xml
- audio/mp4
- video/mp4
- application/javascript
- application/pdf
- application/zip
- application/atom+xml
这些数据类型统称为MIME type， 每个值包括一级类型和二级类型，之间用/分隔
*厂商也可以自定义类型如： application/vnd.debain.binary-package
MIME type还可以在尾部使用分号 添加参数 如：
Content-Type: text/html; charset=utf-8
2）、Age: 从初创开始，响应持续的时间
3）、Public: 支持的请求方法列表
4）、Server: 服务器应用程序的名称和版本
5）、Title: html源端的标题

#### * 通用头信息
Connection: 请求或响应结束后是否断开TCP连接
Date: 报文创建时间
Cache-Control 缓存指示
Transfer-Encoding: 告知接收端，传输内容采用了何种编码方式
via: 显示报文经过的中间节点（代理、网关）

#### 四、http工作原理
##### 1、http客户端连接到web服务器
客户端与服务端http端口（默认80）建立一个TCP套接字连接。
##### 2、客户端发送http请求
通过TCP套接字，客户端向服务端发送http请求报文，有请求行 请求头 空行 请求数据 构成。
##### 3、服务端接受http请求并返回响应
服务器解析请求报文，定位请求资源，将资源复本写到TCP套接字，由客户端读取，响应报文由 状态行 响应头 空行 响应数据构成。
##### 4、释放TCP连接
http1.1协议默认TCP连接不关闭，当客户端和服务器发现对方一段时间没有活动就可以主动关闭连接，规范的做法是客户端在发送最后一次请求时，发送Connection: close，明确要求服务器关闭TCP连接。
##### 5、客户端解析返回的响应
客户端首先解析响应行，查看状态码是否为表示请求成功,然后解析每一个响应头，根据响应头解析响应数据。

#### 五、HTTP1.1新增特性
##### 1、持续连接
见本文 四、4
##### 2、管道机制
在同一个TCP连接里，可以客户端同时发送多个请求。以前的做法，在同一个TCP连接里，客户端先发送A请求，等服务器成功响应A请求，客户端在发送B请求。管道机制里，客户端可以同时发送AB请求，不过服务器仍然按顺序，先响应A请求，在响应B请求。
##### 3、Content-Length字段（Content-Length: <length>）
由于1.1版本里同一个TCP连接服务器可以响应多个请求，所以需要区分数据包属于哪个请求的响应，通过Content-Length声明本次响应的长度，就可以达到目的。
##### 4、分块传输编码
对于耗时较长的动态操作，如果等所有操作完成才能发送数据，性能很差，因此1.1版本有Transfer-Encoding字段，只要请求或响应的头信息里有Transfer-Encoding字段，表明回应将由数量未定的数据块组成。Transfer-Encoding: chunked。
在每个非空数据块之前，会有一个16进制的数值表示这个数据块的长度，当这个数值为0时，表明这个回应的数据全部发送完了。如：
```
HTTP1.1 200 ok
Content-Type: text/plain
Transfer-Encoding: chunked

35
this is the first chunk of this response

1c
this is the second chunk of this response

0
```
#### 六、HTTP1.1缺点
1.1版本里客户端虽然可以同时发送多个请求，但是服务器只能按顺序依次处理多个请求，如果前面的请求耗时较长，则后面就会有多个请求排队等候处理吗，这成为**队头都塞**。
为了避免这个问题，有两种解决办法，1、减少http请求数  2、同时多开持久连接。

#### 七、 HTTP2特性
#### 1、二进制协议
1.1版本里头信息是ASCLL编码的文本，数据体可以是文本也可以是二进制。在http2里，头信息和数据体都必须是二进制，统称为“帧”：头信息帧 数据帧
二进制的好处是可以自定义帧，http2定义了近10种帧，方便未来的高级应用，如果通过文本实现该功能，解析数据比较麻烦。个人理解好比是http的状态码是使用数字而不是使用文本。
#### 2、多工
在一个连接里，客户端和服务器可以同时发送多个请求或响应，不用按照顺序一一对应，这避免了“队头堵塞”。
如在一个TCP连接里。服务器同时收到了A请求和B请求，服务器先响应A请求，处理到一半发现该过程非常耗时，就先已经处理好的部分发送至客户端，接着响应B请求，完成后，在接着响应A请求。
#### 3、数据流
由于http2的数据包不是按顺序发送的，同一个连接的数据包，可能属于不同的响应，所以需要对数据包做出区分，指明它属于哪个响应。
每个请求或响应的所有数据包叫做数据流。一个数据流都有一个id，每个数据包被发送的时候都会被标记id，用来区它分属于哪个数据流。协议规定，客户端发送的数据流id为奇数，服务器端发送的数据流id为偶数。
当数据发送到一半时，客户端和服务端都可以发送一个信号（RST_STREAM帧）取消这个数据流，1,1版本取消数据流只能通过关闭TCP连接，而http2取消数据流不必关闭TCP连接，该连接还可以被其他请求使用。
#### 4、头信息压缩
由于http协议是无状态的，每次请求都要附上所有信息。但是请求的很多字段都是重复的，比如Cookie，和User Agent，这造成了带宽的浪费。
http2引入了头信息压缩机制。1、头信息使用gzip或compress压缩后发送；2、客户端和服务端共同维护一张头信息表，所有字段都会存入这个表，生成一个索引号，以后请求不需要发送字段，只需要发送对应的索引号就行了。
#### 5、服务器推送
http2允许服务器未经请求，主动向客户端发送资源。

#### 参考：
[1、阮一峰http入门]( http://www.ruanyifeng.com/blog/2016/08/http.html)

[2、关于http协议这一篇就够了](https://www.jianshu.com/p/80e25cb1d81a)

[3、http知识积累及相关实践](http://www.notedeep.com/note/36)
