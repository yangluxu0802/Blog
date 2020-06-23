### Spring MVC工作流程

1.Tomcat的工作线程将请求转交给Spring mvc框架的DispatcherServlet

2.DispatcherServlet查找@Controller注解的controller，我们一般会给controller加上你@RequestMapping的注解，标注说哪些controller用来处理哪些请求，此时根据请求的uri，去定位到哪个controller来进行处理

3.根据@RequestMapping去查找，使用这个controller内的哪个方法来进行请求的处理，对每个方法一般也会加@RequestMapping的注解

4.DispatcherServlet会直接调用我们的controller里面的某个方法来进行请求的处理

5.我们的controller的方法会有一个返回值，以前的时候，一般来说还是走jsp、模板技术，我们会把前端页面放在后端的工程里面，返回一个页面模板的名字，Spring mvc的框架使用模板技术，对html页面做一个渲染；返回一个json串，前后端分离，可能前端发送一个请求过来，我们只要返回json数据

6.再把渲染以后的html页面返回给浏览器去进行显示；前端负责把html页面渲染给浏览器就可以了

> 
>
> Spring MVC工作流程:
>
> 1. 用户发起请求。
> 2. DispatcherServlet接收到请求,并去调用HandlerMapping查找处理器。
> 3. HandlerMapping根据请求的URL查找对应的处理器,并返回给前端控制器DispatcherServlet。
> 4. DispatcherServlet调用HandlerAdapter执行处理器。
> 5. HandlerAdapter先判断处理器的类型进行适配,然后执行处理器。
> 6. 处理器进行数据和业务请求的处理,将ModelAndView对象返回给HandlerAdapter。
> 7. HandlerAdapter将ModelAndView对象返回到前端控制器DispatcherServlet。
> 8. DispatcherServlet调用ViewResolver解析逻辑视图ModelAndView。
> 9. ViewResolver通过逻辑视图的名称查找对应的视图对象,并返回给前端控制器。
> 10. DispatcherServlet调用View渲染视图到前端。
> 11. 响应处理的结果。
>
> Spring MVC主要由以下几个部分组成:
>
> - 前端控制器(DispatcherServlet):接收请求并响应到前端,并且负责各组件职责的分派。
> - 处理器(Handler):处理数据与业务请求,Handler需要符合适配器的规则。
> - 处理器映射器(HandlerMapping):根据每个请求的URL查找对应的处理器Handler。
> - 处理器适配器(HandlerAdapter):根据类型适配每个处理器Handler并执行。
> - 视图解析器(ViewResolver):根据逻辑视图的名称将逻辑视图解析为视图对象。
> - 视图(View):在Spring MVC中,View是一个接口,通过不同的实现类支持不同的类型。(jsp、freemarker等)

参考：

https://apppukyptrl1086.pc.xiaoe-tech.com/detail/v_5e101e9b7ded6_g2qrjY56/3?from=p_5dd3ccd673073_9LnpmMju&type=6

https://sylvanassun.github.io/2016/06/15/2016-06-15-springmvc-process/