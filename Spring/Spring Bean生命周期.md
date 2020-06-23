### Spring Bean生命周期

首先是创建容器

1.实例化Bean

2.Bean所需要的属性（构造函数注入 setter方法注入）

3.处理xxxAware接口

4.BeanPostProcessor接口 

> ​	当经过上述几个步骤后，bean对象已经被正确构造，但如果你想要对象被使用前再进行一些自定义的处理，就可以通过BeanPostProcessor接口实现。
>
> ​    postProcessBeforeInitialzation( Object bean, String beanName ) 
> ​	当前正在初始化的bean对象会被传递进来，我们就可以对这个bean作任何处理。 
> ​	这个函数会先于InitialzationBean执行，因此称为前置处理。 
> ​	所有Aware接口的注入就是在这一步完成的。
>
> ​	postProcessAfterInitialzation( Object bean, String beanName ) 
> ​	当前正在初始化的bean对象会被传递进来，我们就可以对这个bean作任何处理。 
> ​	这个函数会在InitialzationBean完成后执行，因此称为后置处理。

5.InitializingBean与init-method

> 当BeanPostProcessor的前置处理完成后就会进入本阶段。 
> InitializingBean接口只有一个函数：
>
> - afterPropertiesSet()
>
> 这一阶段也可以在bean正式构造完成前增加我们自定义的逻辑，但它与前置处理不同，由于该函数并不会把当前bean对象传进来，因此在这一步没办法处理对象本身，只能增加一些额外的逻辑。 
> 若要使用它，我们需要让bean实现该接口，并把要增加的逻辑写在该函数中。然后Spring会在前置处理完成后检测当前bean是否实现了该接口，并执行afterPropertiesSet函数。
>
> 当然，Spring为了降低对客户代码的侵入性，给bean的配置提供了init-method属性，该属性指定了在这一阶段需要执行的函数名。Spring便会在初始化阶段执行我们设置的函数。init-method本质上仍然使用了InitializingBean接口。

6.DisposableBean和destroy-method

和init-method一样，通过给destroy-method指定函数，就可以在bean销毁前执行指定的逻辑。

参考：

https://www.zhihu.com/question/38597960



