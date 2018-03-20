#### 一、使用缓存方式
1、HTML的<meta>  如： <meta http-equiv="Pragma" content="no-store">

2、http的头信息
#### 二、缓存机制
先讲一下http缓存的机制，下文的内容都是对这个机制进行的解释说明。
当客户端接受服务器发送的响应时，会选择是否缓存，如果允许缓存，那下次发送相同请求时，会首先判断改缓存是否过期，如果没有过期则直接使用缓存，如果过期，则发送请求，服务器根据请求头的某些字段来判断该该过期缓存是否与服务器资源一致，如果一致则返回304，客户端使用缓存，如果该过期缓存与服务器资源不一致，则服务器处理该请求，如果成功处理返回200，及相应最新资源。
![61](https://user-images.githubusercontent.com/17128499/36640392-844900f8-1a58-11e8-8405-41bac97208a8.png)

#### 三、http缓存的三大策略
#### 1、缓存存储策略（决定客户端是否进行缓存）
字段Cache-Control: public private no-cache max-age no-store都是用来声明客户端是否可以缓存响应内容。
#### 2、缓存过期策略（决定客户端缓存是否过期）
字段Expires指明了缓存数据是否有效的绝对时间，客户端通过将其与本地当前时间对比判断该缓存是否过期。如果没有过期，则使用该缓存，这种情况叫**强缓存**；如果过期了执行对比策略。
     注：1、no-cache 和 max-age是一种复合策略的字段值，既能执行缓存存储策略也能执行缓存过期策略。
     Cache-Control: max-age相当于 Cache-Controll: public/private  Expires: 当前时间 + max-age
     Cache-Control: no-cache 相当于 Cache-Control: max-age=0
    2、Cache-Control的优先级高于Expires
#### 3、缓存对比策略（当客户端缓存过期后，用来判断该缓存与服务器上对应资源是否一致）
如果缓存已经过期，客户端会向服务器发送请求，服务器判断缓存和当前资源是否一致，如果一致，则返回304，告诉服务端继续使用本地缓存。这种情况叫**协商缓存**。
判断缓存和当前资源是否一致有两种方法：
 1、Last-Modified/If-Modified-Since
 服务器首次响应时要传给客户端一个Last-Modified响应头，客户端再次请求时，将Last-Modified赋给If-Modified-Since作为请求头传给服务器，服务器将该值和当前资源最近修改时间进行对比判断两者是否一致。
![v2-04ce0e01ac686d025f12c23f8a25b940_r](https://user-images.githubusercontent.com/17128499/36641330-d1e362d0-1a68-11e8-9c5d-d74e8f4c6b5f.jpg)

 2、ETag/If-None-Match
 服务器首次响应 时生成一个hash字符串，作为ETag的值传给客户端，客户端再次请求时，在请求头上带上If-None-Match:ETag，服务器检查If-None-Match，返回304或200.
![v2-0f8aa8248633f7684ad9403fae7702f2_r](https://user-images.githubusercontent.com/17128499/36641313-a88779ee-1a68-11e8-93ae-cdda2a9e19e2.jpg)

注：
1、如果响应头没有设置Expires或Cache-Control: max-age=data 或 Cache-Control: no-cache，每个浏览器都会有自己的一套算法来计算缓存有效时间。
2、Cache-Control各指令适用情况如下：
![51](https://user-images.githubusercontent.com/17128499/36641390-e54e25e8-1a69-11e8-9269-1d45f78cc414.png)

#### 参考：
[1、饿了么知乎专栏](https://zhuanlan.zhihu.com/p/29750583?group_id=901822939576557568)

[2、腾讯Bugly知乎专栏](https://zhuanlan.zhihu.com/p/24467558)

[3、AlloyTeam博客 浅谈http缓存](http://www.alloyteam.com/2016/03/discussion-on-web-caching/#prettyPhoto)
