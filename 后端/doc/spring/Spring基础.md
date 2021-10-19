# 

![image-20210830173211265](https://gitee.com/Sean0516/image/raw/master/img/image-20210830173211265.png)



Spring IOC 流程



### 什么是Spring IOC 容器

Spring 框架的核心是 Spring 容器。容器创建对象，将它们装配在一起，配置它们并管理它们的完整生命周期。Spring 容器使用依赖注入来管理组成应用程序的组件。容器通过读取提供的配置元数据来接收对象进行实例化，配置和组装的指令。该元数据可以通过 XML，Java 注解或 Java 代码提供

##### IoC 的一些好处是：

1. 它将最小化应用程序中的代码量。
2. 它将使您的应用程序易于测试，因为它不需要单元测试用例中的任何单例或 JNDI 查找机制。
3. 它以最小的影响和最少的侵入机制促进松耦合。
4. 它支持即时的实例化和延迟加载服务

### 什么是依赖注入

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

### Spring IoC 的实现机制

Spring 中的 IoC 的实现原理就是工厂模式加反射机制

### 什么是 spring bean

1. 它们是构成用户应用程序主干的对象。
2. Bean 由 Spring IoC 容器管理。
3. 它们由 Spring IoC 容器实例化，配置，装配和管理。
4. Bean 是基于用户提供给容器的配置元数据创建

### Spring Bean 是线程安全的吗

spring 本身并没有针对Bean 做线程安全处理 所以

1. 如果Bean 是无状态的，那么Bean 是线程安全的
2. 如果Bean 是有状态的，则不是线程安全的



### spring 支持的 bean scope

Singleton - 默认，每个容器中只有一个bean的实例，单例的模式由BeanFactory自身来维护

Prototype - 为每一个bean请求提供一个实例

Request - 为每一个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收

Session - 每一次 HTTP 请求都会产生一个新的 bean，同时该 bean 仅在当前HTTP session 内有效。在session过期后，bean会随之失效

Global-session -  全局作用域，global-session和Portlet应用相关。当你的应用部署在Portlet容器中工作时，它包含很多portlet。如果你想要声明让所有的portlet共用全局的存储变量的话，那么这全局变量需要存储在global-session中。全局作用域与Servlet中的session作用域效果相同

仅当用户使用支持 Web 的 Application Context 时，最后三个才可用

### spring 容器启动流程是怎么样的

1. 首先会进行扫描，扫描得到所有的BeanDefinition对象，并存放在一个Map 中
2. 然后筛选出非懒加载的单例BeanDefinition 进行创建bean ，对于多例bean不需要再启动过程中进行创建，对于多利bean 会在每次获取 bean 时利用beanDefinition去创建
3. 利用BeanDefinition 创建 Bean
4. 单例Bean 创建完之后，spring 会发布一个容器启动事件

![image-20210806111818713](https://gitee.com/Sean0516/image/raw/master/img/image-20210806111818713.png)

### spring bean 容器的生命周期

1. Spring 容器根据配置中的 bean 定义中实例化 bean。

   对于BeanFactory容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。对于ApplicationContext容器，当容器启动结束后，通过获取BeanDefinition对象中的信息，实例化所有的bean

2. Spring 使用依赖注入填充所有属性，如 bean 中所定义的配置。实例化后的对象被封装在BeanWrapper对象中，紧接着，Spring根据BeanDefinition的信息 以及 通过BeanWrapper提供的设置属性的接口完成依赖注入

3. 如果 bean 实现BeanNameAware 接口，则工厂通过传递 bean 的 ID 来调用setBeanName()。

4. 如果 bean 实现 BeanFactoryAware 接口，工厂通过传递自身的实例来调用 setBeanFactory()。

5. 如果存在与 bean 关联的任何BeanPostProcessors，则调用 preProcessBeforeInitialization() 方法。

6. 如果为 bean 指定了 init 方法（ 的 init-method 属性），那么将调用它。

7. 最后，如果存在与 bean 关联的任何 BeanPostProcessors，则将调用 postProcessAfterInitialization() 方法。

8. 如果 bean 实现DisposableBean 接口，当 spring 容器关闭时，会调用 destory()。

9. 如果为bean 指定了 destroy 方法（ 的 destroy-method 属性），那么将调用它

### @Autowired 注解有什么用

@Autowired 可以更准确地控制应该在何处以及如何进行自动装配。此注解用于在 setter 方法，构造函数，具有任意名称或多个参数的属性或方法上自动装配bean。默认情况下，它是类型驱动的注入

### @Qualifier 注解有什么用

当您创建多个相同类型的 bean 并希望仅使用属性装配其中一个 bean 时，您可以使用@Qualifier 注解和 @Autowired 通过指定应该装配哪个确切的 bean来消除歧义

### @RequestMapping 注解有什么用

@RequestMapping 注解用于将特定 HTTP 请求方法映射到将处理相应请求的控制器中的特定类/方法。 此注释可应用于两个级别：
类级别：映射请求的 URL 

方法级别：映射 URL 以及 HTTP 请求方法

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

![image-20210806112045164](C:\Users\Sean\AppData\Roaming\Typora\typora-user-images\image-20210806112045164.png)

### Spring MVC 组件

1. DispatcherServlet：作为前端控制器，整个流程控制的中心，控制其它组件执行，统一调度，降低组件之间的耦合性，提高每个组件的扩展
2. HandlerMapping：通过扩展处理器映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。
3. HandlAdapter：通过扩展处理器适配器，支持更多类型的处理器。
4. ViewResolver：通过扩展视图解析器，支持更多类型的视图解析，例如：jsp、freemarker、pdf、excel等

### DispatcherServlet 的工作流程

![image-20210630153343101](C:\Users\Sean\AppData\Roaming\Typora\typora-user-images\image-20210630153343101.png)

1. 向服务器发送 HTTP 请求，请求被前端控制器 DispatcherServlet 捕获。
2. DispatcherServlet 根据 -servlet.xml 中的配置对请求的 URL 进行解析，得到请求资源标识符（URI）。然后根据该 URI，调用HandlerMapping获得该 Handler 配置的所有相关的对象（包括 Handler 对象以及 Handler 对象对应的拦截器），最后以 HandlerExecutionChain 对象的形式返回。
3. DispatcherServlet 根据获得的 Handler，选择一个合适的HandlerAdapter。（附注：如果成功获得 HandlerAdapter 后，此时将开始执行拦截器的 preHandler(...)方法）。
4. 提取 Request 中的模型数据，填充 Handler 入参，开始执行 Handler（ Controller)。在填充 Handler 的入参过程中，根据你的配置，Spring 将帮你做一些额外的工作：
   -  HttpMessageConveter：将请求消息（如 Json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息
   -  数据转换：对请求消息进行数据转换。如 String 转换成 Integer、Double 等。
   -  数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
   -  数据验证：验证数据的有效性（长度、格式等），验证结果存储到BindingResult 或 Error 中。
5. Handler(Controller)执行完成后，向 DispatcherServlet 返回一个ModelAndView 对象；
6. 根据返回的 ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的 ViewResolver)返回给DispatcherServlet。
7. ViewResolver 结合 Model 和 View，来渲染视图。
8. 视图负责将渲染结果返回给客户端。

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

### Spring框架中都用到了哪些设计模式

1. 工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
2. 单例模式：Bean默认为单例模式。
3. 代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；
4. 模板方法：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。
5. 观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现--ApplicationListener
6. 适配器模式 advisorAdapter 接口
7. 责任链模式  BeanPostProcessor



### 哪些是重要的 bean 生命周期方法？

有两个重要的 bean 生命周期方法，第一个是 setup ， 它是在容器加载 bean的时候被调用。第二个方法是 teardown 它是在容器卸载类的时候被调用。The bean 标签有两个重要的属性（init-method 和 destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct 和@PreDestroy）

### spring 中的事件

1. 上下文更新事件（ContextRefreshedEvent）：该事件会在 ApplicationContext 被初始化或者更新时发布。也可以在调用 ConfigurableApplicationContext 接口中的 refresh()方法时被触发
2. 上下文开始事件（ContextStartedEvent）：当容器调用 ConfigurableApplicationContext 的Start()方法开始/重新开始容器时触发该事件
3. 上下文停止事件（ContextStoppedEvent）：当容器调用 ConfigurableApplicationContext 的Stop()方法停止容器时触发该事件
4. 上下文关闭事件（ContextClosedEvent）：当 ApplicationContext 被关闭时触发该事件。容器被关闭时，其管理的所有单例 Bean 都被销毁
5. 请求处理事件（RequestHandledEvent）：在 Web 应用中，当一个 http 请求（request）结束触发该事件。

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

### 列举 IoC 的一些好处

1. 它将最小化应用程序中的代码量
2. 它将使您的应用程序易于测试，因为它不需要单元测试用例中的任何单例或 JNDI 查找机制。
3. 它以最小的影响和最少的侵入机制促进松耦合。
4. 它支持即时的实例化和延迟加载服务

### Spring 中的事务是如何实现的

1. spring  事务底层是基于数据库事务和AOP机制
2. 首先对使用@Transactional 注解的Bean ,spring 会创建一个代理对象作为Bean
3. 当调用代理对象的方法时，会先判断该方法是否加上了@Transactional 注解
4. 如果加了，则利用事务管理器创建一个数据库连接
5. 并且修改数据库连接的auto commit属性 为false ，禁止此连接的自动提交。
6. 然后执行当前方法，方法中会执行sql
7. 执行完当前方法后，如果没有出现异常，则直接提交事务，。
8. 如果出现事务，并且这个异常是需要会管的，就会提交事务，否则任然提交事务



### Spring 中什么时候@Transactional 会失效

因为spring 事务是基于代理来实现的，所以某个加了@Transactional 的方法只有是被代理对象调用时，那么注解才会生效

同时，如果某个方法是private 的，那么@Transactional 也会失效，因为底层cglib 是基于父子类来实现的，子类是不能重载父类的private方法的。 所以无法很好的利用代理。 也会导致注解失效

### spring的事务传播行为

spring事务的传播行为说的是，当多个事务同时存在的时候，spring如何处理这些事务的行为

1. PROPAGATION_REQUIRED：如果当前没有事务，就创建一个新事务，如果当前存在事务，就加入该事务，该设置是最常用的设置
2. PROPAGATION_SUPPORTS：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就以非事务执行
3. PROPAGATION_MANDATORY：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就抛出异常
4. PROPAGATION_REQUIRES_NEW：创建新事务，无论当前存不存在事务，都创建新事务
5. PROPAGATION_NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
6. PROPAGATION_NEVER：以非事务方式执行，如果当前存在事务，则抛出异常
7. PROPAGATION_NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则按REQUIRED属性执行

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

### Java中依赖注入有哪些方式

1. 构造器注入
2. Setter方法注入
3. 接口注入

### Spring Aplication run 方法

### Spring  start  自定义开发