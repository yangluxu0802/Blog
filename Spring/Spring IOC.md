## Spring IOC(Inversion of Control)



Ioc：全文是Inversion of Control。翻译过来就是控制反转，意思是对象之间的关系不再由传统的程序来控制，而是由Spring容器来统一控制这些对象创建、协调、销毁，而对象只需要完成业务逻辑即可。

通过反射机制实现的



#### 什么是 IoC 容器

所谓的 IoC 容器就是指的 Spring 中 Bean 工厂里面的 Map 存储结构（存储了 Bean 的实例）。



#### 如何创建 IoC 容器

ClassPathXmlApplicationContext：它是从类的**根路径**下加载配置文件，推荐使用这种。

FileSystemXmlApplicationContext：它是从**磁盘路径**上加载配置文件，配置文件可以在磁盘的任意位置。

AnnotationConfigApplicationContext：当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来**读取注解**。



##### Spring 提供了以下两种不同类型的容器。

1.Spring BeanFactory 容器

它是最简单的容器，给 DI 提供了基本的支持，它用 *org.springframework.beans.factory.BeanFactory* 接口来定义。BeanFactory 或者相关的接口，如 BeanFactoryAware，InitializingBean，DisposableBean，在 Spring 中仍然存在具有大量的与 Spring 整合的第三方框架的反向兼容性的目的。

2.Spring ApplicationContext 容器

该容器添加了更多的企业特定的功能，例如从一个属性文件中解析文本信息的能力，发布应用程序事件给感兴趣的事件监听器的能力。该容器是由 *org.springframework.context.ApplicationContext* 接口定义。



参考：

https://wiki.jikexueyuan.com/project/spring/ioc-containers.html

https://www.jianshu.com/p/9fe5a3c25ab6

https://blog.csdn.net/GoGleTech/article/details/79557416

