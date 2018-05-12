springboot-异步
===

## 背景
在平常应用中，绝大多数情况下都是通过同步的方式来实现交互处理的；但是在处理与第三方系统交互的时候，容易造成响应迟缓的情况，之前大部分都是使用多线程来完成此类任务，其实，在spring 3.x之后，就已经内置了@Async来完美解决这个问题，本文将完成介绍@Async的用法

## 使用场景案例
 例如，我们需要在一个方法中完成方法内的逻辑后需要通知其他接口，但是不需要等其他接口返回结果就直接在当前方法中返回（比如用户支付完订单后需要及时给用户返回支付结果，同时需要通知商户：用户已完成支付），这时就需要异步调用（不用等待通知完商户才给用户返回支付结果）。
## 实现
1.在启动类上添加@EnableAsync注解，表示项目支持异步方法调用；

```
@SpringBootApplication
@EnableAsync
public class SpringbootAsyncMethodsApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootAsyncMethodsApplication.class, args);
    }


}
```

2.在需要异步调用的方法上添加@Async注解，表示该方法为异步方法，即该方法和调用者不在一个线程中进行。

示例代码：

```
 @Async
    public Future<String> findByUrl(String url) throws InterruptedException {
        log.info("Looking up " + url);
        String results = netPageDownService.findByUrl(url);
        // Artificial delay of 1s for demonstration purposes
        Thread.sleep(1000L);
        log.info("当前线程{}", Thread.currentThread().getName());
        return new AsyncResult<>(results);
    }
```


## 配置详解
### 配置并开启@Async扫描支持

让我们开始使用JAVA的注解配置开启异步处理机制，只需要简单的加上@EnableAsync注解到配置类上即可。
*   **annotation** - 默认情况下, @EnableAsync 会扫描使用了Spring @Async与EJB 3.1 javax.ejb.Asynchronous的方法；此选项也可以用来扫描其他的，如用户自定义的注解类型；
*   **mode** - 指定应该使用哪种AOP进行切面处理 - JAVA代理或AspectJ；
*   **proxyTargetClass** - 指定应该使用哪种代理类 - CGLIB或JDK;此属性只有当**mode**设置成**AdviceMode.PROXY**才会产生效果。
*   **order** - 设置AsyncAnnotationBeanPostProcessor执行顺序(生命周期有关);默认情况下会最后一个执行，所以这样就能顾及到所有已存在的代理。

### 异步方法加@Async

首先 - 让我们来了解一些规则 - @Async有两点局限性（无法正常工作）。
*   方法名必须是public进行修饰的
*   必须不能在同一个类中调用异步方法
原因很简单 - 方法名必须用public修饰才能被进行代理;而同一个类中调用方法的话会略过代理进行直接调用。

spring通过@Async定义异步任务

异步的方法有2种 
1. 最简单的异步调用，返回值为void 

```
@Async
public void asyncMethodWithVoidReturnType() {
    System.out.println("Execute method asynchronously. "
      + Thread.currentThread().getName());
}
```

2. 异常调用返回Future

```
@Async
public Future<String> asyncMethodWithReturnType() {
    System.out.println("Execute method asynchronously - "
      + Thread.currentThread().getName());
    try {
        Thread.sleep(5000);
        return new AsyncResult<String>("hello world !!!!");
    } catch (InterruptedException e) {
        //
    }

    return null;
}
```
Spring 同样也提供了一个Future的实现类叫AsyncResult，此类可以用来跟踪异步方法调用结果。 
现在,让我们来调用上面的方法并通过Future进行获取到异步处理的结果。

```
public void testAsyncAnnotationForMethodsWithReturnType()
  throws InterruptedException, ExecutionException {
    System.out.println("Invoking an asynchronous method. "
      + Thread.currentThread().getName());
    Future<String> future = asyncAnnotationExample.asyncMethodWithReturnType();

    while (true) {
        if (future.isDone()) {
            System.out.println("Result from asynchronous process - " + future.get());
            break;
        }
        System.out.println("Continue doing something else. ");
        Thread.sleep(1000);
    }
}
```


## 自定义配置

Spring异步线程池的接口类，其实质是java.util.concurrent.Executor

Executor 的实现非常多，Spring 已经实现的异常线程池： 
1. SimpleAsyncTaskExecutor：不是真的线程池，这个类不重用线程，每次调用都会创建一个新的线程。  
2. SyncTaskExecutor：这个类没有实现异步调用，只是一个同步操作。只适用于不需要多线程的地方 
3. ConcurrentTaskExecutor：Executor的适配类，不推荐使用。如果ThreadPoolTaskExecutor不满足要求时，才用考虑使用这个类 
4. SimpleThreadPoolTaskExecutor：是Quartz的SimpleThreadPool的类。线程池同时被quartz和非quartz使用，才需要使用此类  
5. ThreadPoolTaskExecutor ：最常使用，推荐。 其实质是对java.util.concurrent.ThreadPoolExecutor的包装

默认情况下,Spring使用SimpleAsyncTaskExecutor来运行这些异步方法,默认的设置方式可以在两个层级上面进行覆盖 - 在应用全局配置上或在单独的方法上。
在配置类中配置所需的executor:

```
@Configuration
@EnableAsync
public class SpringAsyncConfig {

    @Bean(name = "threadPoolTaskExecutor")
    public Executor threadPoolTaskExecutor() {
        return new ThreadPoolTaskExecutor();
    }
}
```
然后,在@Async注解属性中使用executor的名称:

```
@Async("threadPoolTaskExecutor")
public void asyncMethodWithConfiguredExecutor() {
    System.out.println("Execute method with configured executor - "
      + Thread.currentThread().getName());
}
```

### 应用全局配置上覆盖Executor

自定义配置可以实现AsyncConfigurer 接口或继承AsyncConfigurerSupport，覆盖默认配置。

实现AsyncConfigurer接口 - 意思是getAsyncExecutor方法需要我们自己来进行实现，会返回Executor给整个应用实例使用 - 意味着现在充当默认的Executor去运行加了@Async注解的方法。

```
@Configuration
@EnableAsync
public class SpringAsyncConfig implements AsyncConfigurer {

    @Override
    public Executor getAsyncExecutor() {
        return new ThreadPoolTaskExecutor();
    }

}
```




# 异常处理

由于返回值类型是Future，异常处理就简单了 - Future.get()会抛出异常。 
但是,如果返回类型是void,异常将无法正常传送到调用的线程. 因此，我们需要添加一些额外的配置来处理异常。 
我们创建一个实现了AsyncUncaughtExceptionHandler接口的自定义异步异常处理类.一旦任意未捕获的异常产生后都会调用handleUncaughtException()方法。

```
public class CustomAsyncExceptionHandler
  implements AsyncUncaughtExceptionHandler {

    @Override
    public void handleUncaughtException(
      Throwable throwable, Method method, Object... obj) {

        System.out.println("Exception message - " + throwable.getMessage());
        System.out.println("Method name - " + method.getName());
        for (Object param : obj) {
            System.out.println("Parameter value - " + param);
        }
    }

}
```
在上一个代码段中,我们看到配置类实现了AsyncConfigurer接口.根据其中的部分，我们同样也需要实现getAsyncUncaughtExceptionHandler()方法来自定义我们的异步异常处理类:

```
@Override
public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
    return new CustomAsyncExceptionHandler();
}
```
## 多线程池

```
 @Bean(name = "threadPoolTaskExecutor1")
    public Executor threadPoolTaskExecutor1() {
        log.info("加载{}配置;MultThreadPoolConfig.threadPoolTaskExecutor1");
        ThreadPoolTaskExecutor threadPoolTaskExecutor = new ThreadPoolTaskExecutor();
        threadPoolTaskExecutor.setThreadNamePrefix("threadPoolTaskExecutor1");
        return threadPoolTaskExecutor;
    }

    @Bean(name = "threadPoolTaskExecutor2")
    public Executor threadPoolTaskExecutor2() {
        log.info("加载{}配置;MultThreadPoolConfig.threadPoolTaskExecutor2");
        ThreadPoolTaskExecutor threadPoolTaskExecutor = new ThreadPoolTaskExecutor();
        threadPoolTaskExecutor.setThreadNamePrefix("threadPoolTaskExecutor2");
        return threadPoolTaskExecutor;
    }
```
使用的时候表明使用哪个线程池

```
@Async("threadPoolTaskExecutor1")
    public Future<String> findByUrl(String url) throws InterruptedException {
        log.info("Looking up " + url);
        String results = netPageDownService.findByUrl(url);
        // Artificial delay of 1s for demonstration purposes
        Thread.sleep(1000L);
        log.info("当前线程{}", Thread.currentThread().getName());
        return new AsyncResult<>(results);
    }
```

代码地址参见：https://github.com/mongoding/spring-boot-side



