面试-tomcat
===
# io

##   BIO、NIO、AIO的概念


## nio了解吗？nio和普通的io有什么区别？怎么使用的？

## NIO和BIO区别
## Nio和aio的区别


## Java IO，NIO，Java中有没有实现异步IO
  Java IO实现的是同步阻塞，它是怎么实现同步阻塞的。我拿了read()方法举例来讲的。NIO实现的是同步非阻塞，我详细讲了一下Selector中的select()方法轮询说明它是如何实现多路复用IO的。然后对比了一下他们的效率。面试官可能看我对这一块比较了解，又继续问我Java中有没有实现异步IO，我感觉好像没有，但面试官说有，让我想想，其实这里我并不清楚啦，所以我就对面试官讲了一下我对Unix中异步IO模型的理解，然后说至于Java里面有没有我真的不太清楚。（他居然笑了！说你理解是对的，Java里面有没有不重要！哈哈） 



##  说说nio的架构，为什么变快了，说说select和**buffer**都是怎么用的？

##  nio中， 如果不显式的调用 `system.gc()` 那会出现什么问题？


## 说说同步／异步，阻塞／非阻塞的区别

## io多路复用的优势在哪里？
## 直接内存原理？请说明JavaNIO中直接内存的使用方法？你们项目中是怎么用的？

## 熟悉IO么？与NIO的区别，阻塞与非阻塞的区别**

## IO会阻塞吗？readLine是不是阻塞的



# Netty

##  为什么选择 Netty

##    说说业务中，Netty 的使用场景

##  原生的 NIO 在 JDK 1.7 版本存在 epoll bug

##   什么是TCP 粘包/拆包

##  TCP粘包/拆包的解决办法

##  Netty 线程模型

##  说说 Netty 的零拷贝

##   Netty 内部执行流程

##   Netty 重连实现
##   Servlet如何保证单例模式,可不可以编程多例的哪？
##   Tomcat本身的参数你⼀般会怎么调整？


## netty buffer 缓冲区清楚吗？
虽然netty 用的是nio，jdk 中有nio 的实现；单netty 自定义了buffer 接口
buffer 的实现有多种：堆缓存，直接缓存区
# Tomcat

##   Tomcat的基础架构（Server、Service、Connector、Container）

##   Tomcat如何加载Servlet的

##   Pipeline-Valve机制

*   可参考：《[四张图带你了解Tomcat系统架构](http://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247484905&idx=1&sn=6c8acd89476fadbc4cb9ccfda9c9c2e4&chksm=e9c5fc58deb2754e7519511bb0ed8dcbfa3fe29179663b53f3626643f8b9c82068d9b0464ee6&scene=21#wechat_redirect)！》

## Tomcat的集群会话了解吗？
1）使用tomcat自带的cluster方式，多个tomcat间自动实时复制session信息，配置起来很简单。但这个方案的效率比较低，在大并发下表现并不好。
2）利用nginx的基于访问ip的hash路由策略，保证访问的ip始终被路由到同一个tomcat上，这个配置更简单。但如果应用是某一个局域网大量用户同时登录，这样负载均衡就没什么作用了。
4）利用memcached实现（MSM工具）。memcached存储session，并把多个tomcat的session集中管理，前端在利用nginx负载均衡和动静态资源分离，在兼顾系统水平扩展的同时又能保证较高的性能。
5）利用redis实现。使用redis不仅仅可以将缓存的session持久化，还因为它支持的单个对象比较大，而且数据类型丰富，不只是缓存 session，还可以做其他用途，可以一举几得。
6）利用filter方法实现。这种方法比较推荐，因为它的服务器使用范围比较多，不仅限于tomcat ，而且实现的原理比较简单容易控制。
7）通过cookie 标示会话id，程序中获取到会话id，从分布式缓存中取session 信息
## 分布式Session（会话共享）怎么实现的？
同上
## tomcat的类加载顺序了解吗？



## Tomcat的原理了解吗？


## Tomcat优化了解吗？常用的配置参数都有哪些？



##  Tomcat，Apache，JBoss的区别
tomcat服务器


##  使用什么命令来确定是否有 Tomcat 实例运行在机器上

##  简单讲讲 Tomcat 结构，以及其类加载器流程

# Servlet

##   转发与重定向的区别
##  WEB容器主要有哪些功能? 并请列出一些常见的WEB容器名字。
## Forward和redirect的区别
## Cookie&Session的格式、传输的内容

## 服务器怎么记忆用户登录状态 

## session和cookie的区别和联系，session的生命周期，多个服务部署时session管理。
##  描述 Cookie 和 Session 的作用，区别和各自的应用范围，Session工作原理
## 谈谈网页登录模块里记住我这个功能？

从session一直到cookie，巴拉巴拉说了下各自的实现原理 以及缓存机制。。。

## servlet和filter的区别。filter你在哪些地方用到过。

servlet是一种运行服务器端的java应用程序，具有独立于平台和协议的特性，并且可以动态的生成web页面，它工作在客户端请求与服务器响应的中间层。

1) 客户端发送请求至服务器端；

2) 服务器将请求信息发送至 Servlet；

3) Servlet 生成响应内容并将其传给服务器。响应内容动态生成，通常取决于客户端的请求；

4) 服务器将响应返回给客户端。

在 Web 应用程序中，一个 Servlet 在一个时刻可能被多个用户同时访问。这时 Web 容器将为每个用户创建一个线程来执行 Servlet。

filter是一个可以复用的代码片段，可以用来转换HTTP请求、响应和头信息。Filter不像Servlet，它不能产生一个请求或者响应，它只是修改对某一资源的请求，或者修改从某一的响应。只要你在web.xml文件配置好要拦截的客户端请求，它都会帮你拦截到请求，此时你就可以对请求或响应(Request、Response)统一设置编码，简化操作；同时还可进行逻辑判断，如用户是否已经登陆、有没有权限访问该页面等等工作。

Filter对用户请求进行预处理，接着将请求交给Servlet进行处理并生成响应，最后Filter再对服务器响应进行后处理。

创建一个Filter只需两个步骤：

*   *   建Filter处理类；
    *   web.xml文件中配置Filter。

一定要实现javax.servlet包的Filter接口的三个方法init()、doFilter()、destroy().

listener：监听器，从字面上可以看出listener主要用来监听只用。通过listener可以监听web服务器中某一个执行动作，并根据其要求作出相应的响应。

interceptor：是在面向切面编程的，就是在你的service或者一个方法，前调用一个方法，或者在方法后调用一个方法，是基于JAVA的反射机制。比如动态代理就是拦截器的简单实现
## 说一说在浏览器中输入一个url后，直到浏览器显示页面的过程中发生了什么（我主要说了DNS，然后他有接着问了DNS的细节，然后就是ARP路由，然后服务器处理，返回，浏览器呈现，获取html中的依赖资源）


