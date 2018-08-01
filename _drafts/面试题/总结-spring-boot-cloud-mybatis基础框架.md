面试-spring-boot-cloud
===

7、项目用 Spring 比较多，有没有了解 Spring 的原理？AOP 和 IOC 的原理

8、Spring Boot除了自动配置，相比传统的 Spring 有什么其他的区别？

9、Spring Cloud 有了解多少？

10、Spring Bean 的生命周期
### **3.1、SSM/Servlet**



*   BeanFactory 和 ApplicationContext 有什么区别

*   Spring Bean 的生命周期

*   Spring IOC 如何实现
**SSM相关**

*   Spring中@Autowired和@Resource注解的区别？ 

*   Spring声明一个 bean 如何对其进行个性化定制； 

*   MyBatis有什么优势； 

*   MyBatis如何做事务管理；
*   Spring中Bean的作用域，默认的是哪一个

*   说说 Spring AOP、Spring AOP 实现原理

*   动态代理（CGLib 与 JDK）、优缺点、性能对比、如何选择

*   Spring 事务实现方式、事务的传播机制、默认的事务类别

*   Spring 事务底层原理

*   一个Controller调用两个Service，这两Service又都分别调用两个Dao，问其中用到了几个数据库连接池的连接？

**框架相关**


*   Spring框架如何实现事务的；

*   如果一个接⼝有2个不同的实现, 那么怎么来Autowire一个指定的实现？(可以使用Qualifier注解限定要注入的Bean，也可以使用Qualifier和Autowire注解指定要获取的bean，也可以使用Resource注解的name属性指定要获取的Bean)

*   Spring框架中需要引用哪些jar包，以及这些jar包的用途；

*   Spring Boot没有放到web容器⾥为什么能跑HTTP服务？

*   Spring中循环注入是什么意思，可不可以解决，如何解决；

*   Spring的声明式事务 @Transaction注解⼀般写在什么位置? 抛出了异常会⾃动回滚吗？有没有办法控制不触发回滚?

*   MyBatis怎么防止SQL注入；


*   了解哪几种序列化协议？如何选择合适的序列化协议；


*   比如我有个电商平台，做每日订单的异常检测，服务端代码应该写；

*   Spring事务失效（事务嵌套），JDK动态代理给Spring事务埋下的坑，可参考《[JDK动态代理给Spring事务埋下的坑！](http://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247484940&idx=1&sn=0a0a7198e96f57d610d3421b19573002&chksm=e9c5ffbddeb276ab64ff3b3efde003193902c69acda797fdc04124f6c2a786255d58817b5a5c&scene=21#wechat_redirect)》

*   如何自定义注解实现功能

*   Spring MVC 运行流程

*   Spring MVC 启动流程

*   Spring 的单例实现原理

*   Spring 框架中用到了哪些设计模式

*   Spring 其他产品（Srping Boot、Spring Cloud、Spring Secuirity、Spring Data、Spring AMQP 等）

*   有没有用到Spring Boot，Spring Boot的认识、原理

*   MyBatis的原理

*   可参考《[为什么会有Spring](http://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247484822&idx=1&sn=6fbee2a12b31b6102a18d3725671d41b&chksm=e9c5fc27deb275319641c3f30d168b85c7c196fd276d47efa35046b5dc54f5b77174c5bf8808&scene=21#wechat_redirect)》

*   可参考《[为什么会有Spring AOP](http://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247484827&idx=1&sn=b9d82f3fced6a875f8dfc22e5849b28e&chksm=e9c5fc2adeb2753c516ef8fc959c0c9dd84ccacaa40473b64bc58b5137c30562a0b45803ba8e&scene=21#wechat_redirect)》
*   请简述 Servlet 的生命周期及其相关的方法
**框架相关**

*   聊下Spring源码，知道多少，都聊一下；

*   聊下Spring注解，@Autowire，@Resource，以及他们的解析过程；

*   聊一下架构，接入层架构，服务层架构。聊下技术栈，Spring Boot，Spring Cloud、Docker；

*   Spring ioc的具体优势，和直接New一个对象有什么区别;

*   Servlet生命周期，是否单例，为什么是单例;

*   Spring Mvc初始化过程；
spring boot：
1，spring boot 的原理
spring boot自动配置的核心在conditional ，其提供了多种conditional 的实现，spring boot 的各种 autoconfig 类正式通过这些conditional 实现的自动配置
2，spring boot 怎么实现多数据源
创建多个datasource 不通的名字
3，spring boot2 与之前的区别是什么

4，spring boot 是以main 方法启动的，怎么保持spring boot一直运行着，bu会结束掉？

spring cloud 
1，熔断器你们是怎么使用的

11.Spring的优点？Spring AOP的原理？Spring如何实现解耦合？
15.说说mybatis配置了xml过后是如何完成数据库操作的？
spring：
1，aop 是怎么实现的？
2，事务的传播性？


Spring的事务实现原理
Spring衍生的相关其他组件整理
Spring动态加载数据源
Spring boot应用
Spring中的设计模式
SpringMVC 有几个IOC容器？由什么决定？有点懵，特意反复跟面试官确认这个问题，在IOC概念饶了一
10，spring 是怎么创建一个bean的？
spring mvc原理了解吗？
# Spring AOP和IOC的实现具体原理是什么？
# Spring AOP中Service内部方法调用时会触发切面逻辑吗？

# spring boot使用过吗？
# Spring Bean的创建过程了解吗？
# Spring 读写分离你们怎么实现的？如果写操作内部调用了读操作如何保证读写分离？
# Mybatis插件编写过吗？$和#有什么区别？
**Spring自带依赖注入注解**

1 @Required：依赖检查；

2 @Autowired：自动装配2 自动装配，用于替代基于XML配置的自动装配 基于@Autowired的自动装配，默认是根据类型注入，可以用于构造器、字段、方法注入

3 @Value：注入SpEL表达式 用于注入SpEL表达式，可以放置在字段方法或参数上
@Value(value = "SpEL表达式")  
@Value(value = "#{message}")  
4 @Qualifier：限定描述符，用于细粒度选择候选者
@Qualifier限定描述符除了能根据名字进行注入，但能进行更细粒度的控制如何选择候选者
@Qualifier(value = "限定标识符") 

**JSR-250注解**
1 @Resource：自动装配，默认根据类型装配，如果指定name属性将根据名字装配，可以使用如下方式来指定
@Resource(name = "标识符")  
字段或setter方法 

2 @PostConstruct和PreDestroy：通过注解指定初始化和销毁方法定义

**JSR-330注解**
1 @Inject：等价于默认的@Autowired，只是没有required属性
2 @Named：指定Bean名字，对应于Spring自带@Qualifier中的缺省的根据Bean名字注入情况
3 @Qualifier：只对应于Spring自带@Qualifier中的扩展@Qualifier限定描述符注解，即只能扩展使用，没有value属性
**JPA注解**
@PersistenceContext
@PersistenceUnit用于注入EntityManagerFactory和EntityManager

具体这里不详细展开，大家可以参考一些专门针对注解的文章！

好了，接下来就提到spring mvc的分层了，总结如下：

使用Java spring进行MVC模式开发时，往往将数据模型分为两部分，即DAO(Data Access Object，数据访问对象)和Service(业务逻辑模型)。在第2步中，控制器向模型请求数据时，并不是直接向DAO请求数据，而是通过Service向DAO请求数据。这样做的好处是，可以将业务逻辑与数据库访问独立开，为将来系统更换数据保存介质（如目前系统使用文件系统存储数据，将来可以更换为使用数据库存储，又或者是现在使用了MSSQL存储数据，将来更换为MySQL等）提供了很大的灵活性。控制器只需要调用Service接口中的方法获取或是处理数据，Service层对控制器传入的数据进行业务逻辑处理封装后，传给DAO层，由DAO层负责将处理后的数据写入数据库中。在Service层使用了抽象工厂模式来实现Service层与DAO层的低耦合，Service层并不知道DAO层是如何实现的，实际上也不需要知道系统使用了哪种数据库或是文件系统。

action（控制器）、Dao（数据访问层）、Service（业务逻辑）！view层（视图 jsp之类的）！


**核心容器** 核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用控制反转 （IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。

**Spring 上下文** Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。

**Spring AOP** 通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了Spring 框架中。所以，可以很容易地使 Spring 框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。

**Spring DAO** JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。

**Spring ORM** Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。

**Spring Web 模块** Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。

**Spring MVC 框架** MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

Spring 框架的功能可以用在任何 J2EE 服务器中，大多数功能也适用于不受管理的环境。Spring 的核心要点是：支持不绑定到特定 J2EE 服务的可重用业务和数据访问对象。毫无疑问，这样的对象可以在不同 J2EE 环境 （Web 或 EJB）、独立应用程序、测试环境之间重用。

# PS：题外要知道的：SpringMVC工作流程描述

1.  向服务器发送HTTP请求，请求被前端控制器 DispatcherServlet 捕获。

2.  DispatcherServlet 根据 <servlet-name>-servlet.xml 中的配置对请求的URL进行解析，得到请求资源标识符（URI）。 然后根据该URI，调用 HandlerMapping 获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以 HandlerExecutionChain 对象的形式返回。

3.  DispatcherServlet 根据获得的Handler，选择一个合适的 HandlerAdapter。（附注：如果成功获得HandlerAdapter后，此时将开始执行拦截器的preHandler(…​)方法）。

4.  提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。 在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：

    HttpMessageConveter： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息。
    数据转换：对请求消息进行数据转换。如String转换成Integer、Double等。
    数据根式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等。
    数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中。

5.  Handler(Controller)执行完成后，向 DispatcherServlet 返回一个 ModelAndView 对象；

6.  根据返回的ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给DispatcherServlet。

7.  ViewResolver 结合Model和View，来渲染视图。

8.  视图负责将渲染结果返回给客户端。
8、Spring AOP原理
6.Java中有哪些注解?在SpringMVC中，requestmapping是自定义注解，问：如何实现自定义注解？
4.  如何理解MVC
6.  Spring bean的加载过程(推荐看Spring的源码)

7.  Spring如何实现AOP和IOC

8.  Spring bean注入方式

9.  Spring的事务管理(推荐看Spring的源码)

10.  Spring事务的传播特性

11.  springmvc原理

12.  springmvc用过哪些注解

13、Spring的aop怎么实现

14、Spring的aop有哪些实现方式

如何排查spring nobean defnation错误
**1、Spring：有没有用过Spring，Spring IOC、AOP机制与实现，Spring MVC** 
  其实我挺不想被问到Spring的细节的，框架这些我都没有复习不太记得了。所以我对面试官说Spring里面的一些比较重要的机制我理解的还不错，然后我用一个实际的例子把我对IOC、AOP理解讲了一下，他听了说对，理解的不错（难得遇到一个边面试边能给反馈的面试官，好开心） 
  Spring MVC其实我用过，我就对面试官讲了我的项目中用到的Servlet，jsp和javabean实现的MVC，以及MVC各个模块职责以及每个模块是怎么联系到一起的，最后我补充了一句我想SpringMVC的思想其实跟这个是一样的（他说对的，嘿嘿有反馈真好） 
Spring，mybatis, redis. 其中Spring不是单纯的理解AOP和IOC，还要知道是怎么实现的。

spring的AOP原理
Spring IoC



9、mybatis缓存 以及谈谈你们项目为什么用spring mvc+mybatis 而不用Hibernate？

主要是说说一级缓存 二级缓存 然后 讲讲mybatis相对于Hibernate来说的优点好处之类


3.mybatis只提供接口，那么在使用时，接口的实现在哪里？

通过代理实现。

*   你如何理解AOP中的连接点（Joinpoint）、切点（Pointcut）、增强（Advice）、引介（Introduction）、织入（Weaving）、切面（Aspect）这些概念

4.SpringMVC在处理前端页面请求时，各模块是如何工作的？
spring aop实现原理？动态代理的两种方式及特点？
10\. 自动事务如何实现



3.spring框架中需要引用哪些jar包，以及这些jar包的用途

4.srpingMVC的原理

5.springMVC注解的意思

6.spring中beanFactory和ApplicationContext的联系和区别

7.spring注入的几种方式

8.spring如何实现事物管理的

9.springIOC和AOP的原理

10.hibernate中的1级和2级缓存的使用方式以及区别原理

11.spring中循环注入的方式
*   什么是基于注解的切面实现
*   IOC的优点是什么
*   解释一下什么叫AOP（面向切面编程）
*   什么是控制反转（Inversion of Control）与依赖注入（Dependency Injection）

*   什么是懒加载（Lazy Loading）
*   MVC的各个部分都有那些技术来实现?如何实现?

11、Mybaits中#和$区别

1）${}是Properties文件中的变量占位符，它可以用于标签属性值和sql内部，属于静态文本替换，比如${driver}会被静态替换为com.mysql.jdbc.Driver。

2）#{}是sql的参数占位符，Mybatis会将sql中的#{}替换为?号，在sql执行前会使用PreparedStatement的参数设置方法，按序给sql的?号占位符设置参数值，比如ps.setInt(0, parameterValue)，#{item.name}的取值方式为使用反射从参数对象中获取item对象的name属性值，相当于param.getItem().getName()。

9、Spring的特性

1.方便解耦，简化开发

通过Spring提供的IoC容器，我们可以将对象之间的依赖关系交由Spring进行控制，避免硬编码所造成的过度程序耦合。

2.AOP编程的支持

通过Spring提供的AOP功能，方便进行面向切面的编程。

3.声明事物的支持

在Spring中，我们可以从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活地进行事务的管理，提高开发效率和质量。

4.方便程序的测试

可以用非容器依赖的编程方式进行几乎所有的测试工作。例如：Spring对Junit4支持，可以通过注解方便的测试Spring程序。

5.方便集成各种优秀框架

Spring不排斥各种优秀的开源框架，相反，Spring可以降低各种框架的使用难度，Spring提供了对各种优秀框架（如Struts,Hibernate、Hessian、Quartz）等的直接支持。

6.降低Java EE API的使用难度

Spring对很多难用的Java EE API（如JDBC，JavaMail，远程调用等）提供了一个薄薄的封装层，通过Spring的简易封装，这些Java EE API的使用难度大为降低。

10、spring aop的应用场景：

AOP用来封装横切关注点，具体可以在下面的场景中使用

Authentication 权限

Caching 缓存

Context passing 内容传递

Error handling 错误处理

Lazy loading 懒加载

Debugging 调试

logging, tracing, profiling and monitoring 记录跟踪 优化 校准

Performance optimization 性能优化

Persistence 持久化

Resource pooling 资源池

Synchronization 同步

Transactions 事务

5、执行某操作，前50次成功，第51次失败a全部回滚b前50次提交第51次抛异常，ab场景分别如何设置Spring（传播性）


9、MyBatis如何分页；如何设置缓存；MySQL分页

6、然后问了我springmvc和mybatis的工作原理，有没有看过底层源码？


然后问了我springmvc和mybatis的工作原理，有没有看过底层源码？

spring的IOC与AOP


5、执行某操作，前50次成功，第51次失败a全部回滚b前50次提交第51次抛异常，ab场景分别如何设置Spring（传播性）
7、SpringIOC , AOP 是怎么实现的？ 有什么应用场景
一、Spring

1、AOP 怎么实现的

2、代理采用的是那种

3、动态代理和静态代理的区别

4、JDK动态代理，和Cglib 动态代理的区别

5、Cglib动态代理是怎么实现的

1、Mybatis 中为什么写个Dao接口就行
2) Spring的Ioc容器管理的bean默认是单实例的.
7、怎么配置bean
3、Spring的工作原理，控制控制反转是怎么实现的，自己写过滤器过滤编码怎么实现
4、框架的源码有没有看过

**5、spring mvc框架结构 spring过滤器、登录过滤 spring mvc分层  spring注解**

答：首先问到的是spring mvc框架，让我讲下其组成及功能，由于当时思路有点凌乱，我主动提出这个问题留到最后讲！接下来面试官问了，spring过滤器，就简单问了下我一般用过滤器做什么，我回答一般处理字符以及编码，显然这不是面试官要的结果，于是面试官提示，加入你在做一个登陆注册系统，怎么用过滤器来验证是否匹配，我直接脱口而出查数据库啦，说完后自己都笑了，至于最后要问的是什么这里我好想也忘了，好想提到什么controller 我说是的，然后应该没答错了，接下来就问到了spring的注解问题，说自己常用的注解有哪些，这里我大致讲了一些，总结摘录如下：
7、Spring中autowire和resourse关键字的区别
Spring AOP与IOC的实现


6.都有哪些代理的实现框架，至少说出5种？spring底层是怎么实现代理的？

答：源代码层面：jdk动态代理

字节码层面：cglib、javassit、asm

编译器层面：AspectJ 
*   MVC设计思想
**6、然后问了我springmvc和mybatis的工作原理，有没有看过底层源码？**
**9、****MyBatis如何分页；如何设置缓存；MySQL分页**

### Spring

*   BeanFactory 和 ApplicationContext 有什么区别

*   Spring Bean 的生命周期

*   Spring IOC 如何实现

*   说说 Spring AOP

*   Spring AOP 实现原理

*   动态代理（cglib 与 JDK）

*   Spring 事务实现方式

*   Spring 事务底层原理

*   如何自定义注解实现功能

*   Spring MVC 运行流程

*   Spring MVC 启动流程

*   Spring 的单例实现原理

*   Spring 框架中用到了哪些设计模式

*   Spring 其他产品（Srping Boot、Spring Cloud、Spring Secuirity、Spring Data、Spring AMQP 等）
1\. junit用法，before,beforeClass,after, afterClass的执行顺序
11\. aop的底层实现，动态代理是如何动态，假如有100个对象，如何动态的为这100个对象代理
controller怎么处理的请求：路由

4.Mybatis和hibernate有什么区别

Hibernate和Mybatis都是orm对象关系映射框架，都是用于将数据持久化的框架技术。

Hiberante较深度的封装了jdbc，对开发者写sql的能力要求的不是那么的高，我们只要通过hql语句操作对象即可完成对数据持久化的操作了。

另外hibernate可移植性好，如一个项目开始使用的是mysql数据库，但是随着业务的发展，现mysql数据库已经无法满足当前的绣球了，现在决定使用Oracle数据库，虽然sql标准定义的数据库间的sql语句差距不大，但是不同的数据库sql标准还是有差距的，那么我们手动修改起来会存在很大的困难，使用hibernate只需改变一下数据库方言即可搞定。用hibernate框架，数据库的移植变的非常方便。

但是hibernate也存在着诸多的不足，比如在实际开发过程中会生成很多不必要的sql语句耗费程序资源，优化起来也不是很方便，且对存储过程支持的也不够强大。但是针对于hibernate它也提供了一些优化策略，比如说懒加载、缓存、策略模式等都是针对于它的优化方案。

Mybatis 也是对jdbc的封装，但是封装的没有hibernate那么深，我们可以再配置文件中写sql语句，可以根据需求定制sql语句，数据优化起来较hibernate容易很多。

Mybatis要求程序员写sql的能力要相对使用hibernate的开发人员要高的多，且可移植性也不是很好。

涉及到大数据的系统使用Mybatis比较好，因为优化较方便。涉及的数据量不是很大且对优化没有那么高，可以使用hibernate。

两者相同点

*   Hibernate与MyBatis都可以是通过SessionFactoryBuider由XML配置文件生成SessionFactory，然后由SessionFactory 生成Session，最后由Session来开启执行事务和SQL语句。其中SessionFactoryBuider，SessionFactory，Session的生命周期都是差不多的。
*   Hibernate和MyBatis都支持JDBC和JTA事务处理。

Mybatis优势

*   MyBatis可以进行更为细致的SQL优化，可以减少查询字段。
*   MyBatis容易掌握，而Hibernate门槛较高。

Hibernate优势

*   Hibernate的DAO层开发比MyBatis简单，Mybatis需要维护SQL和结果映射。
*   Hibernate对对象的维护和缓存要比MyBatis好，对增删改查的对象的维护要方便。
*   Hibernate数据库移植性很好，MyBatis的数据库移植性不好，不同的数据库需要写不同SQL。
*   Hibernate有更好的二级缓存机制，可以使用第三方缓存。MyBatis本身提供的缓存机制不佳。

他人总结

*   Hibernate功能强大，数据库无关性好，O/R映射能力强，如果你对Hibernate相当精通，而且对Hibernate进行了适当的封装，那么你的项目整个持久层代码会相当简单，需要写的代码很少，开发速度很快，非常爽。 
*   Hibernate的缺点就是学习门槛不低，要精通门槛更高，而且怎么设计O/R映射，在性能和对象模型之间如何权衡取得平衡，以及怎样用好Hibernate方面需要你的经验和能力都很强才行。 
*   iBATIS入门简单，即学即用，提供了数据库查询的自动对象绑定功能，而且延续了很好的SQL使用经验，对于没有那么高的对象模型要求的项目来说，相当完美。 
*   iBATIS的缺点就是框架还是比较简陋，功能尚有缺失，虽然简化了数据绑定代码，但是整个底层数据库查询实际还是要自己写的，工作量也比较大，而且不太容易适应快速数据库修改。

针对高级查询，Mybatis需要手动编写SQL语句，以及ResultMap。而Hibernate有良好的映射机制，开发者无需关心SQL的生成与结果映射，可以更专注于业务流程。







