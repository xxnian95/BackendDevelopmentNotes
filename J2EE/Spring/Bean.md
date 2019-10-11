# Bean

## 作用域
1. singleton：整个应用当中只创建一个实例。
2. prototype：每次注入都创建一个新的实例。
3. session：对每个会话创建一个新的实例。
4. request：对每个请求创建一个新的实例。
5. Global Session：全局

## 生性周期
![][image-1]

实例化一个 Bean－－也就是我们常说的 new；
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

---- 

## BeanFactory
BeanFactory负责**生产和管理Bean**的一个工厂。在Spring中，BeanFactory是IOC容器的核心接口。
它的职责包括：
1. 实例化、定位、配置应用程序中的对象。
	2. 建立这些对象间的依赖。
BeanFactory只是个**接口**，并不是IOC容器的具体实现，但是Spring容器给出了很多种实现，如 DefaultListableBeanFactory、XmlBeanFactory、ApplicationContext等。
可以通过BeanFactory获取IOC管理的bean，BeanFactory提供的方法如下：
BeanFactory负责**生产和管理Bean**的一个工厂。在Spring中，BeanFactory是IOC容器的核心接口。
它的职责包括：
1. 实例化、定位、配置应用程序中的对象。
2. 建立这些对象间的依赖。
BeanFactory只是个**接口**，并不是IOC容器的具体实现，但是Spring容器给出了很多种实现，如 DefaultListableBeanFactory、XmlBeanFactory、ApplicationContext等。
可以通过BeanFactory获取IOC管理的bean，BeanFactory提供的方法如下：
![][image-2]
---
## ApplicationContext
-ApplicationContext*以一种更加**面向框架**的方式工作以及对上下文进行分层和实现继承，ApplicationContext包还提供了以下的功能：
- MessageSource, 提供全局的消息访问 -
- 资源访问，如URL和文件
- 事务传播 
- 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层

在不使用spring框架之前，我们的service层中要使用dao层的对象，不得不在service层中new一个对象。存在的问题：层与层之间的依赖。
service层要用dao层对象需要配置到xml配置文件中，至于对象是怎么创建的，关系是怎么组合的都交给了spring框架去实现。
---
## BeanFactory对比FactoryBean
一般情况下，Spring通过反射机制利用<bean>的class属性指定实现类实例化Bean，在某些情况下，实例化Bean过程比较复杂，如果按照传统的方式，则需要在<bean>中提供大量的配置信息。配置方式的灵活性是受限的，这时采用编码的方式可能会得到一个简单的方案。Spring为此提供了一个`org.springframework.bean.factory.FactoryBean`的工厂类接口，用户可以通过实现该接口定制实例化Bean的逻辑。FactoryBean接口对于Spring框架来说占用重要的地位，Spring自身就提供了70多个FactoryBean的实现。它们隐藏了实例化一些复杂Bean的细节，给上层应用带来了便利。从Spring3.0开始，FactoryBean开始支持泛型，即接口声明改为FactoryBean<T>的形式。
以Bean结尾，表示它是一个Bean，不同于普通Bean的是：它是实现了FactoryBean<T>接口的Bean，根据该Bean的ID从BeanFactory中获取的实际上是FactoryBean的getObject()返回的对象，而不是FactoryBean本身，如果要获取FactoryBean对象，请在id前面加一个&符号来获取。
FactoryBean是一个**接口**，当在IOC容器中的Bean实现了FactoryBean后，通过getBean(String BeanName)获取到的Bean对象并不是FactoryBean的实现类对象，而是这个实现类中的getObject()方法返回的对象。要想获取FactoryBean的实现类，就要getBean(&BeanName)，在BeanName之前加上&。

---- 

##  ApplicationContext
- **ApplicationContext**以一种更加**面向框架**的方式工作以及对上下文进行分层和实现继承，ApplicationContext包还提供了以下的功能：
- MessageSource, 提供全局的消息访问
- 资源访问，如URL和文件
- 事务传播
- 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层
在不使用spring框架之前，我们的service层中要使用dao层的对象，不得不在service层中new一个对象。存在的问题：层与层之间的依赖。
service层要用dao层对象需要配置到xml配置文件中，至于对象是怎么创建的，关系是怎么组合的都交给了spring框架去实现。

## BeanFactory对比FactoryBean
一般情况下，Spring通过反射机制利用\<bean\>的class属性指定实现类实例化Bean，在某些情况下，实例化Bean过程比较复杂，如果按照传统的方式，则需要在\<bean\>中提供大量的配置信息。配置方式的灵活性是受限的，这时采用编码的方式可能会得到一个简单的方案。Spring为此提供了一个`org.springframework.bean.factory.FactoryBean`的工厂类接口，用户可以通过实现该接口定制实例化Bean的逻辑。FactoryBean接口对于Spring框架来说占用重要的地位，Spring自身就提供了70多个FactoryBean的实现。它们隐藏了实例化一些复杂Bean的细节，给上层应用带来了便利。从Spring3.0开始，FactoryBean开始支持泛型，即接口声明改为FactoryBean\<T\>的形式。
以Bean结尾，表示它是一个Bean，不同于普通Bean的是：它是实现了FactoryBean\<T\>接口的Bean，根据该Bean的ID从BeanFactory中获取的实际上是FactoryBean的getObject()返回的对象，而不是FactoryBean本身，如果要获取FactoryBean对象，请在id前面加一个&符号来获取。
FactoryBean是一个**接口**，当在IOC容器中的Bean实现了FactoryBean后，通过getBean(String BeanName)获取到的Bean对象并不是FactoryBean的实现类对象，而是这个实现类中的getObject()方法返回的对象。要想获取FactoryBean的实现类，就要getBean(&BeanName)，在BeanName之前加上&。

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191010140005.jpg
[image-2]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191010133523.png