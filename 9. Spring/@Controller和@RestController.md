# @Controller和@RestController
#9. Spring/注解#
- - - -
`@RestController`相当于`@Controller`和`@ResponseBody`共同作用。

* 若只使用@ResponseBody注解Controller，则Controller方法无法返回jsp、html页面，只能返回JASON、XML。
* 使用`@Controller`注解，对应的方法上视图解析器可以解析return的jsp、HTML页面，并且跳转到相应的页面。