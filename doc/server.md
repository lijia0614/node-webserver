/*
	负载均衡
	同一时间多任务分发到每一个进程，如何平均
	
	windows下 cluster集群，多进程分配不均匀
	linux cluster集群, 多进程分配均匀
	
	容灾
	进程卡死，如何重启进程，

	cluster 单机多核cpu集群模块，共享同一端口，处理任务分发

	cpu主频 单位HZ
	同一时间内处理大量任务的速度
	
	cpu利用率/占用率
	性能最大化与同一时间处理任务个数的比值

*/
/*
	1、cluster集群
	master进程不会自动管理worker进程的生死，如果worker被外界杀掉了，
	不会自动重启，只会给master进程发送‘exit’消息，开发者需要自己做好管理。

	2、负载均衡问题
	一个web请求过来，是给worker进程A处理，还是worker进程B处理呢？
	怎么保证大家均等的干活呢？ 这就是负载均衡的问题。

	早期的负载均衡解决
	像Linux操作系统总是唤醒某几个进程，因为对于系统来说，上下文切换是很昂贵的操作，
	唤醒最近被唤醒的进程是比较好的选择。早期的这种方式负载是很不均衡的。

	round-robin模式（目前默认的处理方式）
	从0.11.2版本开始cluster开始增加了round-robin模式做负载均衡：
	master进程负责监听，收到请求后转发给worker进程，多个worker进程轮流干活。

	windows下的进程分发
	window采用的是最近唤醒进程优先分发任务。

*/
/*
    缓存原理
    客户端第一次请求，服务会发送一些字段如下
    Last-Modified 文件最后修改时间
    ETag token值

    缓存分类
    1.强缓存（本地缓存）不与服务器通信
		发生请求时，浏览器会先获取header信息，判断到期日、缓存时效等
		强缓存服务端设置以下两个字段即可，浏览器会自行判断处理
		Cache-Control 缓存时效
		Expires 到期日

    2.协商缓存（弱缓存）需与服务器通信
        客户端reqHeader携带If-Modified-Since（最后修改时间）、If-None-Match（token）
        服务器判断两个字段是否与本地文件Last-Modified（最后修改时间）、ETag token（token）
        协商后返回304还是200ok
        
    3.chrome浏览器请求状态分析
    	from memory cache-> 从内存缓存
        直接从内存中拿到，不会请求服务器，一般已经加载过该资源且缓存在了内存当中，当关闭该页面时，
        此资源就被内存释放掉了，再次重新打开相同页面时不会出现from memory cache的情况
        
    	from disk cache-> 从磁盘缓存
        直接从磁盘中拿到，不会请求服务器，也是在已经在之前的某个时间加载过该资源，
        此资源不会随着该页面的关闭而释放掉，因为是存在硬盘当中，下次打开仍会from disk cache
    
    4.chrome浏览器缓存存放位置
        磁盘-> 非脚本\如css等
        内存-> 脚本、字体、图片

*/
/*
	断点续传工具

	curl 字节命令
	curl --header "Range:0-20" -i http://localhost:8000/index.html

*/
/*
	登陆判断
	！提前预留一个登陆页面接口，和登陆验证接口，两个接口
	
	验证是否登陆->
	判断此请求，是否带有sessionId,并且此id是否存在于session库里

	已登陆->
	没登陆->Login->发送登陆请求->服务器验证成功->标记为已登陆
*/
/*
	重定向
	301谨慎使用

	而301重定向是永久的重定向，
	搜索引擎在抓取新的内容的同时也将旧的网址替换为了重定向之后的网址。

	302重定向只是暂时的重定向，搜索引擎会抓取新的内容而保留旧的地址，
	因为服务器返回302，所以，搜索搜索引擎认为新的网址是暂时的。
	
*/
/*
	Http Header 设置

	是否允许跨域ajax发送cookie
	res.setHeader('Access-Control-Allow-Credentials','true')

	ajax 发送cookie
	xhr.withCredentials = true;

	跨域响应头
	res.setHeader('Access-Control-Allow-Headers','x-requested-with,content-type')
	不支持mime内的类型，直接打开在浏览器
	res.setHeader('X-Content-Type-Options','nosniff')
*/