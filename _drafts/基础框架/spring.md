spring
===
xmlBeanFactory 是怎么解析bean的

传统web 环境是怎么解析bean的，怎么解析注解bean的
- ContextLoaderListener
- DispatcherServlet

Spring boot 的容器是怎么解析bean 的，除了解析bean 还怎么初始化web 容器的

Spring 中的扩展点有哪些

spring 怎么get 到一个bean的

spring 是怎么解析@service@controller的
spring 是怎么解决依赖的
spring 有哪几种解析beanDefinition 的方式
- xml 的方式
- @service @controller@component
- @autoconfigation

spring 是怎么解决bean 的依赖关系的，在什么时机解决的？

bean的生命周期，初始化顺序是什么样的？


```
/**
 * 系统启动入口，Spring容器配置、启动入口
 */
@EnableScheduling
@EnableAsync
@ImportResource({"classpath:promo-web-spring.xml"})
public class BootStrap implements WebApplicationInitializer {
    private Log logger = LogFactory.getLog(getClass());

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        AnnotationConfigWebApplicationContext rootContext = new AnnotationConfigWebApplicationContext();
        rootContext.register(BootStrap.class);
        servletContext.addListener(new ContextLoaderListener(rootContext));

        servletContext.addListener(new ServletContextListener() {
            @Override
            public void contextInitialized(ServletContextEvent contextEvent) {
                logger.info("Context Initialized!");
            }

            @Override
            public void contextDestroyed(ServletContextEvent contextEvent) {
                logger.info("Context Destroyed!");
            }
        });

        servletContext.addServlet("dispatcher", new DispatcherServlet(rootContext)).addMapping("/");
    }
}
```


