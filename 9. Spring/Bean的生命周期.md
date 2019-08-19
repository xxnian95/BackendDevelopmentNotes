# Bean的生命周期
#9. Spring/Bean#
[怎么回答面试官：你对Spring的理解？ - 知乎](https://www.zhihu.com/question/48427693/answer/691483076)
- - - -
![](Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F/v2-55c5b61912d232437d49139c8b381b7a_hd.jpg)
1. 实例化一个 Bean－－也就是我们常说的 new；
2. 按照 Spring 上下文对实例化的 Bean 进行配置－－也就是 ::IOC 注入::；
3. 如果这个 Bean 已经实现了 `BeanNameAware` 接口，会调用它实现的 setBeanName(String)方法，此处传递的就是 Spring 配置文件中 Bean 的 id 值；
4. 如果这个 Bean 已经实现了 `BeanFactoryAware` 接口，会调用它实现的 setBeanFactory(setBeanFactory(BeanFactory)传递的是 Spring 工厂自身（可以用这个方式来获取其它 Bean，只需在 Spring 配置文件中配置一个普通的 Bean 就可以）；
5. 如果这个 Bean 已经实现了 `ApplicationContextAware` 接口，会调用 setApplicationContext(ApplicationContext)方法，传入 Spring 上下文（同样这个方式也可以实现步骤 4 的内容，但比 4 更好，因为 ApplicationContext 是 BeanFactory 的子接口，有更多的实现方法）；
6. 如果这个 Bean 关联了 `BeanPostProcessor` 接口，将会调用 postProcessBeforeInitialization(Object obj, String s)方法，BeanPostProcessor 经常被用作是 Bean
7. 内容的更改，并且由于这个是在 Bean 初始化结束时调用那个的方法，也可以被应用于内存或缓存技术；
8. 如果 Bean 在 Spring 配置文件中配置了 init-method 属性会自动调用其配置的初始化方法。
9. 如果这个 Bean 关联了 `BeanPostProcessor` 接口，将会调用postProcessAfterInitialization(Object obj, String s)方法。注：以上工作完成以后就可以应用这个 Bean 了，那这个 Bean 是一个 Singleton 的，所以一般情况下我们调用同一个 id 的 Bean 会是在内容地址相同的实例，当然在 Spring配置文件中也可以配置非 Singleton，这里我们不做赘述。
10. 当 Bean 不再需要时，会经过清理阶段，如果 Bean 实现了 `DisposableBean` 这个接口，会调用那个其实现的 destroy()方法；
11. 最后，如果这个Bean 的 Spring 配置中配置了 destroy-method 属性，会自动调用其配置的销毁方法。