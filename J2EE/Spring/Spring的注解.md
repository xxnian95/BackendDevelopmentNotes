# Spring的注解

### @Conponent
@Component是所有受Spring管理组件的通用形式，@Component注解可以放在类的头上，@Component不推荐使用。

### @Controller
表现层的Bean，也就是Action。
[@Controller和@RestController]()(bear://x-callback-url/open-note?id=1DF88982-BB40-4B57-8ABA-38FF00DCEC53-875-0000019CD5A2C9CD)

### @Service
业务层的Bean。

### @Repository
DAO层的Bean。

---- 
## @Component和@Bean

- @Component注解在类上使用表明这个类是个组件类，需要Spring为这个类创建bean。
- @Bean注解使用在方法上，告诉Spring这个方法将会返回一个Bean对象，需要把返回的对象注册到Spring的应用上下文中。


## RequestMapping

RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
RequestMapping注解有六个属性，下面我们把她分成三类进行说明。
### value， method：
* value：指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）；
	* method：指定请求的method类型， GET、POST、PUT、DELETE等；
### consumes，produces
* consumes：指定处理请求的提交内容类型（Content-Type），例如application/json, text/html；
	* produces：指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；
### params，headers
* params： 指定request中必须包含某些参数值是，才让该方法处理。
	* headers：指定request中必须包含某些指定的header值，才能让该方法处理请求。

---- 
## Controller和@RestController

`@RestController`相当于`@Controller`和`@ResponseBody`共同作用。

* 若只使用@ResponseBody注解Controller，则Controller方法无法返回jsp、html页面，只能返回JSON、XML。
* 使用`@Controller`注解，对应的方法上视图解析器可以解析return的jsp、HTML页面，并且跳转到相应的页面。

