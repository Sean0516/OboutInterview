# 

![image-20210830173211265](https://gitee.com/Sean0516/image/raw/master/img/image-20210830173211265.png)



## 基本概念

### spring  的优点

1. ioc  集中管理对象。 对象与对象之间耦合度降低，方便维护对象
2. aop  在步修改代码的情况下，可以对业务代码进行增强，减少重复
3. 声明事务的支持  提高开发效率，只需要一个简单注解就可以实现事务管理
4. 方便集成各种优秀的框架

### BeanFactory 和FactoryBean 有什么区别

都是用来创建Bean 对象的

BeanFactory 创建对象的时候是一套完整的标准化流程，如果想要创建Bean的对象，必须要严格遵循一定的步骤，否证无法创建对象

而FactoryBean 主要是为了适配创建Bean 的时候，不需要遵循固定的规则，同时想自己定义对象的创建过程。那么就可以使用FactoryBean接口了 ，因为在此接口中定义了三个方法

1. isSingleton 判断当前对象是否为单例
2. getObjectType 返回当前对象的类型
3. getObject ，此方法需要自己自定义实现，自己完全的控制Bean 的创建流程。

### 说说 BeanFactory 和 ApplicationContext 的区别？ 什么是延迟实例化，它的优缺点是什么

BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口

BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。

ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能

- 继承MessageSource，因此支持国际化。
- 统一的资源文件访问方式。
- 提供在监听器中注册bean的事件。
- 同时加载多个配置文件。
- 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层
- BeanFactroy采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。
- ApplicationContext，它是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 ApplicationContext启动后预载入所有的单实例Bean，通过预载入单实例bean ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。
- 相对于基本的BeanFactory，ApplicationContext 唯一的不足是占用内存空间。当应用程序配置Bean较多时，程序启动较慢。
- BeanFactory通常以编程的方式被创建，ApplicationContext还能以声明的方式创建，如使用ContextLoader。
- BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册

### Spring　中用到的设计模式

单例模式　：　bean 默认都是单例的

原型模式：　指定作用域　prototype

工程模式　　beanfactory

模板方法　　

策略模式　　ｘｍｌＢｅａｎＤｅｆｉｎｉｔｉｏｎＲｅａｄｅｒ

观察者模式　　ｌｉｓｔｅｎｅｒ　　ｅｖｅｎｔ

适配器模式　　　adapter

责任链模式　　使用ａｏｐ　的时候，会先生成一个拦截器

代理模式　　动态代理



### Spring框架中都用到了哪些设计模式

1. 工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
2. 单例模式：Bean默认为单例模式。
3. 代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；
4. 模板方法：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。
5. 观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现--ApplicationListener
6. 适配器模式 advisorAdapter 接口
7. 责任链模式  BeanPostProcessor

### spring 中的事件

1. 上下文更新事件（ContextRefreshedEvent）：该事件会在 ApplicationContext 被初始化或者更新时发布。也可以在调用 ConfigurableApplicationContext 接口中的 refresh()方法时被触发
2. 上下文开始事件（ContextStartedEvent）：当容器调用 ConfigurableApplicationContext 的Start()方法开始/重新开始容器时触发该事件
3. 上下文停止事件（ContextStoppedEvent）：当容器调用 ConfigurableApplicationContext 的Stop()方法停止容器时触发该事件
4. 上下文关闭事件（ContextClosedEvent）：当 ApplicationContext 被关闭时触发该事件。容器被关闭时，其管理的所有单例 Bean 都被销毁
5. 请求处理事件（RequestHandledEvent）：在 Web 应用中，当一个 http 请求（request）结束触发该事件。

## IOC

### IOC 的理解，原理和实现

#### 总

控制反转： 理论思想，原理的对象由使用者控制，有了spring 之后，可以把整个对象交给spring容器来帮我们进行管理

​	DI 依赖注入  把对于的属性的值注入到具体的对象中，@Autowired  完成装载

容器：  存储对象 ，使用Map 结构来存储。 一般存在三级缓存中，其中singletonObjects 存放完整的对象 

​		整个bean 的生命周期，从创建到使用到销毁的过程全部是由容器来管理（bean 的生命周期）

#### 分

1. 一般聊ioc 容器要涉及到容器的创建过程 （BeanFactory DefaultListableBeanFactory）x向Bean 工程中设置一些参数 。 BeanPostProcessor Aware 接口的子类
2. 加载解析bean 对象。 准备要创建的bean 对象的的BeanDefinition （xml 或者注解的解析过程）
3. BeanFactoyPostProcessor  的处理 
4. BeanPostProcessor 的注册  ，方便后续对Bean 对象完成具体的扩展功能
5. 通过反射等方式，将BeanDefinition 对象实例化为具体的对象
6. Bean 对象的初始化过程 （填充属性，调用 aware 子类的方法， 调用 beanPostProcesser 前置方法 -- 执行 init-method 方法 ，执行 后置方法）
7. 生成完整的bean 对象，通过gentBean 可以直接获取
8. 销毁过程

​	

使用工厂模式以及反射来实现的 。同时包含很多的扩展点， 最常使用的对 BeanFactory 的扩展，对Bean 的扩展 （对占位符的处理）

以上是我对ioc 的理解 ，您有什么想问的？

### IOC 的底层实现

 底层原理 ，过程，数据结构，流程 ，设计模式。设计实现

反射， 工厂模式  设计模式 ，关键的方法

1. 通过createBeanFactory 创建出一个Bean 工厂
2. 开始循环创建对象
3. 通过createBean doCreateBean 方法。 以反射的方法来创建对象
4. 进行对象的属性天长  populateBean
5. 进行其他的初始化操作





### IOC创建对象的过程图 （BeanFactory）

![image-20211020092633386](https://gitee.com/Sean0516/image/raw/master/img/image-20211020092633386.png)





### IOC 创建的流程

### ![6ca29c4f1cf4656f3c8b5e1b0f5f5a9a](https://gitee.com/Sean0516/image/raw/master/img/6ca29c4f1cf4656f3c8b5e1b0f5f5a9a.png)

1. 准备工作
2. 创建Bean 工厂 
3. 对xml 或注解进行读取或解析，并放入BeanDefinitionMap
4. 给容器对象进行某些初始化操作
5. 执行beanFactoryPostProcessor的扩展工作
6. 初始化对象前的准备工作
   1. 注册BeanPostPressor。 只是注册功能
   2. 初始化广播器
   3. 初始化message 源 , 国际化处理
   4. 注册监听器
7. 对象实例化操作 
   1. 实例化对象  使用createBeanInstance 方法 ，通过反射的方式生成对象
   2. 自定义属性   populateBean填充属性  
   3. 调用所有自定义了 Aware对象 的接口方法
      1. BeanNameAware
      2. BeanFactoryAware
      3. ApplicationContextAware
   4. 调用benPostProcessor before 前置处理方法进行扩展 （preProcessBeforeInitialization()）
   5. 调用init- method 进行初始化方法的调用
      1. 实现InitializingBean 接口 ，调用afterPropertiesSet
      2. 指定init-method 方法 
   6. 调用 benPostProcessor before 后置方法进行扩展 （postProcessAfterInitialization() ）
8.  销毁 （ destroy）

### Bean 的生命周期

1. 实例化对象  使用createBeanInstance 方法 ，通过反射的方式生成对象
2. 自定义属性   populateBean填充属性  
3. 调用所有自定义了 Aware对象 的接口方法
   1. BeanNameAware
   2. BeanFactoryAware
   3. ApplicationContextAware
4. 调用benPostProcessor before 前置处理方法进行扩展 （preProcessBeforeInitialization()）
5. 调用init- method 进行初始化方法的调用
   1. 实现InitializingBean 接口 ，调用afterPropertiesSet
   2. 指定init-method 方法 
6. 调用 benPostProcessor before 后置方法进行扩展 （postProcessAfterInitialization() ） spring  的aop就是在这里实现的。 AbstractAutoProxyCreator
7. 获取到完整的对象，可以通过getBean 的方式来进行对象的获取
8. 销毁流程 
   1. 判断是否实现了DispoableBean 接口 
   2. 调用destroyMethod 方法

### IOC 的扩展点及调用时机

1. BeanDefinitionRegistryPostProcessor  动态注册 BeanDefinition

   调用时机 ： IOC 加载时，注册BeanDefinition 的时候会调用

2. BeanFactoryPostProcessor  对BeanFactory 进行扩展

3. BeanPostProcessor 

4. Aware 接口

### Aware 接口存在的意义

主要是方便通过spring 中的bean 对象来获取对应容器中的相关属性



### refresh方法

```java
public void refresh() throws BeansException, IllegalStateException {
   synchronized (this.startupShutdownMonitor) {
      // Prepare this context for refreshing.
      /**
       * 做容器属性前的准备工作
       * 1. 设置容器的启动时间
       * 2. 设置活跃状态为true
       * 3. 设置关闭状态为false
       * 4. 获取Environment 对象，添加当前系统属性到 Environment 对象中
       * 5. 准备监听器和事件的集合对象，默认为空的集合
       */
      prepareRefresh();

      // Tell the subclass to refresh the internal bean factory.
      // 创建BeanFactory 加载xml 配置文件的属性值到当前工厂中。 最重要的就是 BeanDefinition
      ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

      // Prepare the bean factory for use in this context.
      //  BeanFactory的预准备工作（BeanFactory进行一些设置，比如context的类加载器，BeanPostProcessor和XXXAware自动装配等）
      prepareBeanFactory(beanFactory);

      try {
         // Allows post-processing of the bean factory in context subclasses.
         //BeanFactory准备工作完成后进行的后置处理工作
         postProcessBeanFactory(beanFactory);

         // Invoke factory processors registered as beans in the context.
         // 执行BeanFactoryPostProcessor的方法
         invokeBeanFactoryPostProcessors(beanFactory);

         // Register bean processors that intercept bean creation.
         // 注册BeanPostProcessor Bean 的前置后置处理器
         registerBeanPostProcessors(beanFactory);

         // Initialize message source for this context.
         // 初始化国际化消息
         initMessageSource();

         // Initialize event multicaster for this context.
         // 初始化事件处理器
         initApplicationEventMulticaster();

         // Initialize other special beans in specific context subclasses.
         // 子类重写这个方法，在容器刷新的时候可以自定义逻辑；如创建Tomcat，Jetty等WEB服务器
         onRefresh();

         // Check for listener beans and register them.
         // 注册应用的监听器 就是注册实现了ApplicationListener接口的监听器bean，这些监听器是注册到ApplicationEventMulticaster中的
         registerListeners();

         // Instantiate all remaining (non-lazy-init) singletons.
         //初始化所有剩下的非懒加载的单例bean
         finishBeanFactoryInitialization(beanFactory);

         // Last step: publish corresponding event.
         // 完成context的刷新。主要是调用LifecycleProcessor的onRefresh()方法，并且发布事件（ContextRefreshedEvent）
         finishRefresh();
      }

      catch (BeansException ex) {
         if (logger.isWarnEnabled()) {
            logger.warn("Exception encountered during context initialization - " +
                  "cancelling refresh attempt: " + ex);
         }

         // Destroy already created singletons to avoid dangling resources.
         destroyBeans();

         // Reset 'active' flag.
         cancelRefresh(ex);

         // Propagate exception to caller.
         throw ex;
      }

      finally {
         // Reset common introspection caches in Spring's core, since we
         // might not ever need metadata for singleton beans anymore...
         resetCommonCaches();
      }
   }
}
```

### spring 标签解析流程

1. 加载spring.handlers 配置文件
2. 将配置文件内容加载到map 集合中
3. 根据指定的key 去获取对应的处理器

### 自定义标签

1. 创建一个对应的解析器处理类 （在init方法中添加parser 类）
   1. 继承NameSpaceHandlerSupport
   2. 重写 init 方法 （注册 parser 类 和标签节点）
2. 创建一个普通的Spring.handlers 配置文件，让应用程序能够完成加载工作
   1. Spring.handlers  设置处理器路径
   2. Spring.schemas   指定 xsd 文件路径
   3. 创建 xsd 文件，并设置规则
3. 创建对应标签的parser 类 （对当前标签的其他属性值进行解析工作）
   1. extends  AbstractSingleBeanDefinitionParser
   2. 重写 gerBeanClass
   3. 重写doParse



### 哪些是重要的 bean 生命周期方法？

有两个重要的 bean 生命周期方法，第一个是 setup ， 它是在容器加载 bean的时候被调用。第二个方法是 teardown 它是在容器卸载类的时候被调用。The bean 标签有两个重要的属性（init-method 和 destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct 和@PreDestroy）

### 配置Bean 有那几种方法

1. xml
2. 注解  （@Component @Controller    前提  需要配置扫描包  component-scan  反射调用构造方法
3. java config @Bean  可以自己控制实例化的过程
4. @Import  有三种方式
   1. 

### 什么是Bean 装配，什么是Bean 的自动装配

​	

### 自动装配有哪几种

1. no 不进行自动装配  通过 @Autowired 来进行手动自定需要自动注入的属性
2. byName 通过bean 的名称进行自动装配，如果一个bean 的property 和另一个 bean 的name 相同，就进行自动装配
3. byType 通过参数的数据类型进行自动装配
4. constructor  利用构造函数进行装配 ，并且构造函数的参数通过byType 进行装配
5. auto detect  自动探测。 如果有构造方法，通过construct 的方式自动装配，否则使用byType 的方式自动装配 （已经弃用了）



### BeanDefinition 的加载流程

1. 读取配置  BeanDefinitionReader
2. 解析注解。 
3. 扫描 
4. 注册BeanDefinition  到 Map 中

### Bean 的创建顺序是什么样的

Bean 的创建顺序是由BeanDefinition 的注册顺序来决定的，当然依赖关系也会影响Bean 的创建顺序

### BeanDefinition 的注册顺序由什么来决定

主要由注解的解析顺序来决定

1. @Configuration
2. @Component
3. @Import --类
4. @Bean
5. @Import --  实现了ImportBeanDefinitionRegister  的类

### java config  是如何替换 spring xml 的

#### 应用：

1. 以前xml 
   1. spring  容器 ClassPathXmlApplicationContext (".xml")
   2. Spring.xml
   3. <bean />
   4. 扫描包   <component-sacn>
   5. 引入外部属性配置文件  <property-placeHodeler >
   6. 指定其他配置文件
2. java  config 
   1. spring 容器  AnnotationConfigApplicationContext(javaConfig.class)
   2. 配置类  @Configration
   3. @Bean  
   4. 扫描包  @ComponentScan
   5. 引入外部属性配置文件@PropertySource()
   6. @Import 

### 源码

 ![image-20211026202657617](https://gitee.com/Sean0516/image/raw/master/img/image-20211026202657617.png)



### 如何在自动注入没有找到 依赖Bean 的时候不报错

将 required 设置为false



## 循环依赖

### 什么是循环依赖

A B 对象相互依赖对方

![image-20211021113623954](https://gitee.com/Sean0516/image/raw/master/img/image-20211021113623954.png)

### 如何解决循环依赖问题

三级缓存 ， 提前暴露对象  aop

#### 总：

A依赖B B依赖A

#### 分

​	bean 的创建过程  ： 实例化和初始化

 	1. 先创建A 对象，实例化A 对象，此时A对象中的B





#### 初始化和实例化 

![image-20211020110251764](https://gitee.com/Sean0516/image/raw/master/img/image-20211020110251764.png)

​	使用三级缓存解决（实例化和初始化分开处理，在中间过程中，给其他对象赋值的时候，并不是一个完整对象，而是把半成品对象赋值给其他对象。 提前暴露对象）  ，  set 方式可以解决循环依赖（set 先创建对象，再进行赋值） ，而构造方法是没有办法解决循环依赖问题的



### 如何使用三级缓存解决循环依赖

### 三级缓存分别存储什么类型的对象

	1. 一级缓存 完整对象
	2. 二级缓存 半成品对象
	3. 三级缓存  ObjectFactory lamda 表达式



 ![image-20211020105730858](https://gitee.com/Sean0516/image/raw/master/img/image-20211020105730858.png)

```java
/** Cache of singleton objects: bean name to bean instance. */
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256); // 一级缓存

/** Cache of singleton factories: bean name to ObjectFactory. */
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16); // 三级缓存

/** Cache of early singleton objects: bean name to bean instance. */
private final Map<String, Object> earlySingletonObjects = new ConcurrentHashMap<>(16); // 二级缓存
```



### 如果只使用一级缓存能否解决循环依赖的问题

不能，因为在整个处理过程中，缓存中放的是初始化完成和未初始化的对象，如果只有一级缓存，那么初始化和未初始化的对象都会放在一级缓存中。有可能在获取过程中获取到未初始化的对象，而未初始化的对象是不能使用的。 不能直接进行相关处理。 因此需要分开来存储 

### 如果只有两个缓存能不能解决

二级缓存可以解决部分的循环依赖问题，但是如果循环依赖过程中包含了代理对象的创建，那么就必须要使用三级缓存了

###  为什么使用三级缓存能够解决循环依赖

三级缓存的value 类型是一个ObjectFactory  ，是一个函数接口，存在的意义是保证在整个容器的运行过程中同名的Bean 对象只能有一个

### 如果一个对象需要被代理，或者说需要生成代理对象， 那么要不要先生成一个普通对象

​	需要。 因为aop 代理需要在 实例化之后再执行。 因此需要先提前创建一个普通对象。 

​	普通对象和代理对象是不能同时出现再容器中的， 因此，当一个对象需要被代理的时候，需要使用代理对象来覆盖之前的普通对象。 在实际的调用过程中，是没有办法确定什么时候对象被使用，所以就要求当某个对象被调用的时候，优先判断此对象是否需要被代理，类似一种回调机制，因此，传入lambda 表达时，可以通过表达式进行对象覆盖。

因此， 所有的Bean 对象在创建的时候都要优先放到三级缓存中，在后续的使用过程中，如果需要被代理则返回代理对象，不需要被代理，则返回普通对象

 

### 什么是Spring IOC 容器

Spring 框架的核心是 Spring 容器。容器创建对象，将它们装配在一起，配置它们并管理它们的完整生命周期。Spring 容器使用依赖注入来管理组成应用程序的组件。容器通过读取提供的配置元数据来接收对象进行实例化，配置和组装的指令。该元数据可以通过 XML，Java 注解或 Java 代码提供

##### IoC 的一些好处是：

1. 它将最小化应用程序中的代码量。
2. 它将使您的应用程序易于测试，因为它不需要单元测试用例中的任何单例或 JNDI 查找机制。
3. 它以最小的影响和最少的侵入机制促进松耦合。
4. 它支持即时的实例化和延迟加载服务

### 什么是依赖注入 DI

在依赖注入中，您不必创建对象，但必须描述如何创建它们。您不是直接在代码中将组件和服务连接在一起，而是描述配置文件中哪些组件需要哪些服务。由 IoC容器将它们装配在一起

### spring 中有多少种 IOC 容器

BeanFactory - BeanFactory 就像一个包含 bean 集合的工厂类。它会在客户端要求时实例化 bean。
ApplicationContext - ApplicationContext 接口扩展了 BeanFactory 接口。它在 BeanFactory 基础上提供了一些额外的功能

| BeanFactory              | ApplicationContext     |
| ------------------------ | ---------------------- |
| 使用懒加载               | 使用即时加载           |
| 使用语法显式提高资源对象 | 自己创建和管理资源对象 |
| 不支持国际化             | 支持国际化             |
| 不支持基于依赖的注解     | 支持基于依赖的注解     |



### Spring Bean 是线程安全的吗

spring 本身并没有针对Bean 做线程安全处理 所以

1. 如果Bean 是无状态的，那么Bean 是线程安全的
2. 如果Bean 是有状态的 ,包含属性，则不是线程安全的



### spring 支持的 bean scope

Singleton - 默认，每个容器中只有一个bean的实例，单例的模式由BeanFactory自身来维护

Prototype - 为每一个bean请求提供一个实例

Request - 为每一个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收

Session - 每一次 HTTP 请求都会产生一个新的 bean，同时该 bean 仅在当前HTTP session 内有效。在session过期后，bean会随之失效

Global-session -  全局作用域，global-session和Portlet应用相关。当你的应用部署在Portlet容器中工作时，它包含很多portlet。如果你想要声明让所有的portlet共用全局的存储变量的话，那么这全局变量需要存储在global-session中。全局作用域与Servlet中的session作用域效果相同

仅当用户使用支持 Web 的 Application Context 时，最后三个才可用



## 注解

### @Component @Controller  @Repository @Service 有什么区别

实际无区别，主要是用于代码分层 ，让代码更清晰

### @Import 注解可以有几种用法

1. 直接指定类 （如果是配置类，会按照配置类正常解析，如果是普通类，则会解析为普通Bean ）
2. 实现了 ImportSelector 的类 ，通过数组的方式，返回一个完整的类路径 ，可以一次性注册多个
3. 通过实现ImportBeanDefinitionRegistrar 来注册bean ，一次性可以注册多个。 可以通过BeanDefinitionRegistry 来动态的注册BeanDefinition
4. 

### @Autowired 注解有什么用

@Autowired 可以更准确地控制应该在何处以及如何进行自动装配。此注解用于在 setter 方法，构造函数，具有任意名称或多个参数的属性或方法上自动装配bean。默认情况下，它是类型驱动的注入

### @Autowired 和 @Resource 的区别

都可以自动注入 

1. Autowired  由spring 提供。Resource 由jdk 提供
2. Autowired   根据类型去注入 ，而 Resource 根据name 注入，当 name 找不到，才会使用类型去查找

### Spring中Autowired和Resource关键字的区别

@Resource和@Autowired都是做bean的注入时使用，其实@Resource并不是Spring的注解，它的包是javax.annotation.Resource，需要导入，但是Spring支持该注解的注入。

##### 共同点

两者都可以写在字段和setter方法上。两者如果都写在字段上，那么就不需要再写setter方法

##### 不同点

1. @Autowired为Spring提供的注解，需要导入包org.springframework.beans.factory.annotation.Autowired;只按照byType注入
2. @Autowired注解是按照类型（byType）装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它的required属性为false。如果我们想使用按照名称（byName）来装配，可以结合@Qualifier注解一起使用
3. @Resource默认按照ByName自动注入，由J2EE提供，需要导入包javax.annotation.Resource。@Resource有两个重要的属性：name和type，而Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以，如果使用name属性，则使用byName的自动注入策略，而使用type属性时则使用byType自动注入策略。如果既不制定name也不制定type属性，这时将通过反射机制使用byName自动注入策略
4. @Resource装配顺序：
   如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异
   常。
   如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常。
   如果指定了type，则从上下文中找到类似匹配的唯一bean进行装配，找不到或是找到多个，都会抛出异常
   如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹
   配则自动装配。@Resource的作用相当于@Autowired，只不过@Autowired按照byType自动注入
5. 

### @Autowired 自动装配的过程是什么样的

Autowired  通过bean 的后置处理器进行解析的。 

1. 在创建一个spring 容器的时候，在构造函数中进行注册AutowiredAnnotationBeanPostProcessor
2. 在Bean 的创建过程中进行解析
   1. 在实例化后预解析
   2. 在属性注入真正解析（拿到上一步缓存的元数据，从ioc 容器中进行查找，并且返回） 



![image-20211028195054453](https://gitee.com/Sean0516/image/raw/master/img/image-20211028195054453.png)

### @Qualifier 注解有什么用

当您创建多个相同类型的 bean 并希望仅使用属性装配其中一个 bean 时，您可以使用@Qualifier 注解和 @Autowired 通过指定应该装配哪个确切的 bean来消除歧义

### @RequestMapping 注解有什么用

@RequestMapping 注解用于将特定 HTTP 请求方法映射到将处理相应请求的控制器中的特定类/方法。 此注释可应用于两个级别：
类级别：映射请求的 URL 

方法级别：映射 URL 以及 HTTP 请求方法

### @Configuration 注解的作用及解析原理

1. 加的注解会为配置类创建cglib 动态代理（保证配置类Bean 方法调用Bean 的单例）@Bean 方法的调用就会通过容器getBean 进行获取

### 如何将第三方类配置为Bean

1. @Bean 注解 
2. 通过@Import 注解导入  这种方法，无法控制实例化过程
3. 通过重写 BeanDefinitionRegistryPostProcessor 来进行注册

### 为什么@ComponentScan 不设置basePackage 也会扫描

因为 spring 在解析 CompinentScan的时候拿到 basepackage ，如果没拿到，会将类所在的包的地址，作为扫描包的地址



## AOP

### Spring AOP里面的几个名词

1. 切面（Aspect）：被抽取的公共模块，可能会横切多个对象。 在Spring AOP中，切面可以使用通用类（基于模式的风格） 或者在普通类中以 @AspectJ 注解来实现
2. 连接点（Join point）：指方法，在Spring AOP中，一个连接点 总是 代表一个方法的执行。
3. 通知（Advice）：在切面的某个特定的连接点（Join point）上执行的动作。通知有各种类型，其中包括“around”、“before”和“after”等通知。许多AOP框架，包括Spring，都是以拦截器做通知模型， 并维护一个以连接点为中心的拦截器链
4. 切入点（Pointcut）：切入点是指 我们要对哪些Join point进行拦截的定义。通过切入点表达式，指定拦截的方法，比如指定拦截add、search
5. 引入（Introduction）：（也被称为内部类型声明（inter-type declaration））。声明额外的方法或者某个类型的字段。Spring允许引入新的接口（以及一个对应的实现）到任何被代理的对象。例如，你可以使用一个引入来使bean实现 IsModified 接口，以便简化缓存机制
6. 目标对象（Target Object）： 被一个或者多个切面（aspect）所通知（advise）的对象。也有人把它叫做 被通知（adviced） 对象。 既然Spring AOP是通过运行时代理实现的，这个对象永远是一个 被代理（proxied） 对象
7. 织入（Weaving）：指把增强应用到目标对象来创建新的代理对象的过程。Spring是在运行时完成织入

### 有哪些类型的AOP通知（Advice）

1. Before - 这些类型的 Advice 在 joinpoint 方法之前执行，并使用@Before 注解标记进行配置。
2. After Returning - 这些类型的 Advice 在连接点方法正常执行后执行，并使用@AfterReturning 注解标记进行配置。
3. After Throwing - 这些类型的 Advice 仅在 joinpoint 方法通过抛出异常退出并使用 @AfterThrowing 注解标记配置时执行。
4. After (finally) - 这些类型的 Advice 在连接点方法之后执行，无论方法退出是正常还是异常返回，并使用 @After 注解标记进行配置。
5. Around - 这些类型的 Advice 在连接点之前和之后执行，并使用@Around 注解标记进行配置

### AOP 面向切面编程

ａｏｐ　是ｉｏｃ　的一个扩展功能，现有IOC　，再有ａｏｐ　，只是ｉｏｃ　整个流程中的一个扩展点。　　ｂｅａｎｐｏｓｔＰｒｏｃｅｓｓｏｒ

总：	ａｏｐ　概念，动态代理

分：　ａｏｐ　　本身是一个扩展功能，在BｅａｎＰｏｓｔＰｒｏｃｅｓｓｏｒ　的后置处理方法中来进行实现





1.  代理对象的创建过程（ａｄｖｉｃｅ　　切面，切点）
2. 通过ｊｄｋ　或者ｃｇｌｉｂ　的方式来生成代理对象
3. 在执行方法调用时，会调用到生成的字节码文件中，直接返回DｙｎａｍｉｃＡｄｖｉｓｏｒｅｄＩｎｔｅｒｃｅｐｔｏｒ　类的ｉｎｔｅｒｃｅｐｔ　方法，从此方法开始执行
4. 根据之前定义好的通知来生成拦截器链
5. 从拦截器中一次获取每一个通知开始进行执行，在执行过程中，为了方便找到下一个通知是那个，会有一个CｇｌｉＭｅｔｈｏｄＩｎｖｏｃａｔｉｏｎ　对象，找的时候从-1的位置依次开始查找并执行



### JDK 动态代理和CGLIB 动态代理的区别

1. JDK 动态代理只支持接口的代理，不支持类的代理
2. JDK 在运行时为目标生成了一个动态代理类
3. 该代理类是实现目标类接口，并且代理类会实现接口中的所有方法，进行增强
4. 调用时，会通过代理类先去调用处理类进行增强，在通过反射的方法进行调用目标方法。 

如果代理类没有实现接口，会使用CGLIB 来动态代理目标类

1. CGLIB 的底层通过ASM 在运行时，动态的生成目标类的一个子类。 
2. 并且会重写父类所有的方法增强
3. 调用时先通过代理类进行增强，再直接调用父类对象的方法进行调用目标方法，从而实现AOP



### AOP内部调用失效的原因，怎么解决

1. 内部调用不会触发内部方法的AOP ，必须走代理
2. 重新拿到代理对象再次执行方法，才能进行增强
   1. 在本类中自动注入当前的bean
   2. 设置暴露当前代理对象到本地线程，可以通过AopContext.currentProxy（） 拿到当前正在调用的动态代理对象

### 自己通过aop 的方式来实现某个统一的功能，我们应该怎么做。 （声明式事务）

​	进行通知类型进行扩展  。 

### Spring AOP 是在哪里创建的动态代理 

1. 正常会在Bean  生命周期初始化后，有BeanPostProcessor 的后置处理器来创建aop
2. 还有一种特殊情况  ： 在bean 属性注入时，存在循环依赖的情况，也会为循环依赖的Bean 通过 MergedBeanDefinitionPostProcessor.postProcessMergedBeanDefinition 创建aop 

### AOP 的完整实现流程

1. **解析切面** ： 在Bean 创建之前的第一个Bean 后置处理器会去解析切面  （解析切面中的通知 ，切点 ，一个通知就会解析成一个advisor （通知，切点））
2. **创建动态代理**， 正常Bean  会在初始化后，调用BeanPostProcessor 拿到之前缓存的advisor  ，再通过advisor中pointcut 判断当前Bean 是否被切点表达式命中，如果匹配，就会为bean 创建动态代理（jdk 动态代理 ，cglib ）
3. **调用**  拿到动态代理对象，调用方法就会判断当前方法是否增强方法，就会通过调用链的方式一次执行通知





## MVC

![image-20210806112045164](C:\Users\Sean\AppData\Roaming\Typora\typora-user-images\image-20210806112045164.png)

### Spring MVC 组件

1. DispatcherServlet：（前端控制器） 由框架提供，作用： 接收请求，进行请求分发，处理响应结果
2. HandlerMapping：(处理器映射器) 根据请求URL 找到对应的Handler 
3. HandlAdapter：处理器适配器   调用处理器（Handler| Controller ）的方法
4. Handler  后端处理器   接收用户请求数据，调用业务方法处理请求
5. ViewResolver： 试图解析器  把逻辑视图解析为真正的物理视图   （支持多种视图技术  ）

### DispatcherServlet 的工作流程

![image-20210630153343101](C:\Users\Sean\AppData\Roaming\Typora\typora-user-images\image-20210630153343101.png)

![image-20211029154054847](https://gitee.com/Sean0516/image/raw/master/img/image-20211029154054847.png)

1. 向服务器发送 HTTP 请求，请求被前端控制器 DispatcherServlet 捕获。

2. DispatcherServlet  调用HandlerMapping获得该 Handler 配置的所有相关的对象（包括 Handler 对象以及 Handler 对象对应的拦截器），最后以 HandlerExecutionChain （处理器执行链）形式返回 

3. DispatcherServlet 根据获得的 Handler，选择一个合适的HandlerAdapter。（附注：如果成功获得 HandlerAdapter 后，此时将开始执行拦截器的 preHandler(...)方法）。

4. 通过处理器适配器，调用处理器中相关方法

   1. 提取 Request 中的模型数据，填充 Handler 入参，开始执行 Handler（ Controller)。在填充 Handler 的入参过程中，根据你的配置，Spring 将帮你做一些额外的工作：

   -  HttpMessageConveter：将请求消息（如 Json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息
   -  数据转换：对请求消息进行数据转换。如 String 转换成 Integer、Double 等。
   -  数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
   -  数据验证：验证数据的有效性（长度、格式等），验证结果存储到BindingResult 或 Error 中。

5. Handler(Controller)执行完成后，向 DispatcherServlet 返回一个ModelAndView 对象；

6. 根据返回的 ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的 ViewResolver)返回给DispatcherServlet。

7. ViewResolver 结合 Model 和 View，来渲染视图。

8. 视图负责将渲染结果返回给客户端。



### SpringMVC 参数注入 

通过参数解析器进行参数解析 

### Spring 和Spring MVC 容器之间的关系

Spring Spring MVC 父子容器  Spring容器为父容器，SpringMVC为子容器，子容器可以引用父容器中的Bean，而父容器不可以引用子容器中的Bean。Spring 可以排除 Controller  的Bean  

### Springmvc controller方法中为什么不能定义局部变量

因为controller是默认单例模式，高并发下全局变量会出现线程安全问题
现这种问题如何解决呢

1. 既然是全局变量惹的祸，那就将全局变量都编程局部变量，通过方法参数来传递
2. jdk提供了java.lang.ThreadLocal,它为多线程并发提供了新思路。
3. 使用@Scope("session")，会话级别
4. 将控制器的作用域从单例改为原型，即在spring配置文件Controller中声明 scope="prototype"，每次都创建新的controller

### SpringMVC中的拦截器和Servlet中的filter有什么区别

首先最核心的一点他们的拦截侧重点是不同的，SpringMVC中的拦截器是依赖JDK的反射实现的，SpringMVC的拦截器主要是进行拦截请求，通过对Handler进行处理的时候进行拦截，先声明的拦截器中的preHandle方法会先执行，然而它的postHandle方法（他是介于处理完业务之后和返回结果之前）和afterCompletion方法却会后执行。并且Spring的拦截器是按照配置的先后顺序进行拦截的

而Servlet的filter是基于函数回调实现的过滤器，Filter主要是针对URL地址做一个编码的事情、过滤掉没用的参数、安全校验（比较泛的，比如登录不登录之类）

## 事务

### Spring 支持的事务管理类型和实现方式

1. 编程式事务管理。 通过编码的方式管理事务 ，但是难以维护
2. 声明式事务管理  将业务和事务管理分离。 只需要注解和XML 配置来管理事务

#### 声明式事务的实现方式

1. 基于接口
   1. 基于TransactionInterceptor 
   2. 基于TransactionProcxyFactoryBean 
2. 基于xml
3. 基于@Transactional 注解



### Spring 中的事务是如何实现的

#### 	总：

​	spring  事务底层是基于数据库事务和AOP机制，首先要生成具体的代理对象，然后按照ａｏｐ　的整套流程来执行具体的操作逻辑，正常情况下要通过通知来完成核心功能，但是事务不是通过通知来实现，而是通过一个transactioninterceptor　来实现的。　然后调用invoke　来实现具体的逻辑

#### 分

1. 先做准备工作，解析各个方法上事务相关的属性，根据具体的属性来判断是否开启新的事务
2. 当需要开启时，获取数据库连接。　关闭自动提交，开启事务
3. 执行具体的ｓｑｌ　逻辑操作
4. 在操作过程中，如果执行失败了，则完成事务回滚操作
5. 如果没有发生意外，则进行事务提交
6. 当事务执行完毕后，需要清除相关的事务对象

### Spring 中的事务是如何实现的

1. 
2. 首先对使用@Transactional 注解的Bean ,spring 会创建一个代理对象作为Bean
3. 当调用代理对象的方法时，会先判断该方法是否加上了@Transactional 注解
4. 如果加了，则利用事务管理器创建一个数据库连接
5. 并且修改数据库连接的auto commit属性 为false ，禁止此连接的自动提交。
6. 然后执行当前方法，方法中会执行sql
7. 执行完当前方法后，如果没有出现异常，则直接提交事务，。
8. 如果出现事务，并且这个异常是需要回滚的，就会提交事务，否则任然提交事务



### Spring 中什么时候@Transactional 会失效

因为spring 事务是基于代理来实现的，所以某个加了@Transactional 的方法只有是被代理对象调用时，那么注解才会生效

同时，如果某个方法是private 的，那么@Transactional 也会失效，因为底层cglib 是基于父子类来实现的，子类是不能重载父类的private方法的。 所以无法很好的利用代理。 也会导致注解失效

### spring的事务传播行为

spring事务的传播行为说的是，当多个事务同时存在的时候，spring如何处理这些事务的行为　，spring　有其中事务传播行为

1. PROPAGATION_REQUIRED：如果当前没有事务，就创建一个新事务，如果当前存在事务，就加入该事务，该设置是最常用的设置
2. PROPAGATION_SUPPORTS：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就以非事务执行
3. PROPAGATION_MANDATORY：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就抛出异常
4. PROPAGATION_REQUIRES_NEW：创建新事务，无论当前存不存在事务，都创建新事务
5. PROPAGATION_NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
6. PROPAGATION_NEVER：以非事务方式执行，如果当前存在事务，则抛出异常
7. PROPAGATION_NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则按REQUIRED属性执行

![image-20211029145447204](https://gitee.com/Sean0516/image/raw/master/img/image-20211029145447204.png)

### Spring中的隔离级别

1.  ISOLATION_DEFAULT：这是个 PlatfromTransactionManager 默认的隔离级别，使用数据库默认的事务隔离级别
2.  ISOLATION_READ_UNCOMMITTED：读未提交，允许另外一个事务可以看到这个事务未提交的数据
3.  ISOLATION_READ_COMMITTED：读已提交，保证一个事务修改的数据提交后才能被另一事务读取，而且能看到该事务对已有记录的更新
4.  ISOLATION_REPEATABLE_READ：可重复读，保证一个事务修改的数据提交后才能被另一事务读取，但是不能看到该事务对已有记录的更新
5.  ISOLATION_SERIALIZABLE：一个事务在执行的过程中完全看不到其他事务对数据库所做的更新。



### Spring如何管理事务的

Spring事务管理主要包括3个接口，Spring事务主要由以下三个共同完成的

1. PlatformTransactionManager：事务管理器，主要用于平台相关事务的管理。主要包括三个方法：①、commit：事务提交。②、rollback：事务回滚。③、getTransaction：获取事务状态
2. TransacitonDefinition：事务定义信息，用来定义事务相关属性，给事务管理器PlatformTransactionManager使用这个接口有下面四个主要方法：①、getIsolationLevel：获取隔离级别。②、getPropagationBehavior：获取传播行为。③、getTimeout获取超时时间。④、isReadOnly：是否只读（保存、更新、删除时属性变为false--可读写，查询时为true--只读）事务管理器能够根据这个返回值进行优化，这些事务的配置信息，都可以通过配置文件进行配置
3. TransationStatus：事务具体运行状态，事务管理过程中，每个时间点事务的状态信息。例如：①、hasSavepoint()：返回这个事务内部是否包含一个保存点。②、isCompleted()：返回该事务是否已完成，也就是说，是否已经提交或回滚。③、isNewTransaction()：判断当前事务是否是一个新事务

## Spring Security 



