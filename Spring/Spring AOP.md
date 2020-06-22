### Spring AOP（Aspect Oriented Program 面向切面编程）



AOP的理念：就是将**分散在各个业务逻辑代码中相同的代码通过横向切割的方式**抽取到一个独立的模块中！

核心技术是动态代理

在Java中动态代理有**两种**方式：

- JDK动态代理
- CGLib动态代理

Spring提供了3种类型的AOP支持：

- 基于代理的经典SpringAOP

  - 需要实现接口，手动创建代理

- 纯POJO切面

  - 使用XML配置，aop命名空间

- ```
  @AspectJ
  ```

  注解驱动的切面

  - 使用注解的方式，这是最简洁和最方便的！



参考：

https://juejin.im/post/5b06bf2df265da0de2574ee1