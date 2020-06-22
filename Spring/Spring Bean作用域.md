### Spring Bean作用域



在`Spring`框架中，我们可以在六个内置的`spring bean`作用域中创建`bean`，还可以定义`bean`范围。在这六个范围中，只有在使用支持`Web`的`applicationContext`时，其中四个可用。`singleton`和`prototype`作用域可用于任何类型的`ioc`容器。



|   **SCOPE**   |                           **描述**                           |
| :-----------: | :----------------------------------------------------------: |
|   singleton   |           `Spring IoC`容器存在一个`bean`对象实例。           |
|   prototype   |     与单例相反，每次请求`bean`时，它都会创建一个新实例。     |
|    request    |        表示每个request作用域内的请求只创建一个实例。         |
|    session    | 在`HTTP`会话(`Session`) 的完整生命周期中，将创建并使用单个实例。 只适用于`web`环境中`Spring` `ApplicationContext`中有效。 |
| globalSession | 这个只在porlet的web应用程序中才有意义，它映射到porlet的global范围的session，如果普通的web应用使用了这个scope，容器会把它作为普通的session作用域的scope创建。 |

**singleton是默认的，它是线程不安全的**



参考：

https://juejin.im/post/5dad1455f265da5b6006fa6a

https://www.jianshu.com/p/502c40cc1c41