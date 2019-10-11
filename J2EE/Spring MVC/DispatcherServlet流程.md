# DispatcherServlet流程

![][image-1]

1. 用户发送请求至前端控制器 *DispatcherServlet*
2. DispatcherServlet 收到请求调用 HandlerMapping 处理器映射器。
3. 处理器映射器根据请求 url 找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给 DispatcherServlet。
4. DispatcherServlet 通过 HandlerAdapter 处理器适配器调用处理器
5. 执行处理器(Controller，也叫后端控制器)。
6. *Controller*执行完成返回 ModelAndView
7. HandlerAdapter 将Controller执行结果 ModelAndView 返回给 DispatcherServlet
8. DispatcherServlet 将 ModelAndView 传给 ViewReslover 视图解析器
9. ViwReslover 解析后返回具体 View
10. DispatcherServlet 对 View 进行渲染视图（即*将模型数据填充至视图*中）。
11. DispatcherServlet 响应用

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191010140212.png