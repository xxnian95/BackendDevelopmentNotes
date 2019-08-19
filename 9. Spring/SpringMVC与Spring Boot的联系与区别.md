# SpringMVC与Spring Boot的联系与区别
#9. Spring/Spring Boot#
- - - -
* Spring最初利用工厂模式（DI)和代理模式解耦应用组件，为了解耦开发了springmvc；而实际开发过程中，经常会使用到注解，程序的样板很多，于是开发了starter，这套就是springboot。
* springboot是约定大于配置，可以简化spring的配置流程；springmvc是基于servlet的mvc框架。
> springboot引入自动配置的概念，让项目配置变得更容易，Spring Boot本身并不提供Spring框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于Spring框架的应用程序。也就是说， _它并不是用来替代Spring的解决方案，而是和Spring框架紧密结合用于提升Spring开发者体验的工具。_ 同时它集成了大量常用的第三方库配置(例如Jackson, JDBC, Mongo, Redis, Mail等等)，Spring Boot应用中这些第三方库几乎可以零配置的开箱即用(out-of-the-box)，大部分的SpringBoot应用都只需要非常少量的配置代码，开发者能够更加专注于业务逻辑。Spring Boot只是承载者，辅助开发者简化项目搭建过程的。如果承载的是WEB项目，使用Spring MVC作为MVC框架，那么工作流程和SpringMVC的是完全一样的，因为这部分工作是Spring MVC做的而不是Spring Boot。  