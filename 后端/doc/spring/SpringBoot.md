### new SpringApplication 执行的方法

```java
this.resourceLoader = resourceLoader; // 配置resource loader  
this.webApplicationType = WebApplicationType.deduceFromClasspath(); //  判断当前应用程序的类型
this.setInitializers(this.getSpringFactoriesInstances(ApplicationContextInitializer.class)); // 获取初始化容器的实例对象 从 ,ETA-INFO/spring.factories 读取到ApplicationContextInitializer 类的实例合计并去重 （一七共个）
this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class)); // 获取到监听器的实例对象  （10 个对象） 从 META-INFO/spring.factories 读取ApplicationListener 类的实例名称集合并去重
this.mainApplicationClass = this.deduceMainApplicationClass(); // 找到当前应用程序的主类
```

### run 方法

```java
public ConfigurableApplicationContext run(String... args) {
    StopWatch stopWatch = new StopWatch(); // 设置启动的时间
    stopWatch.start();
    ConfigurableApplicationContext context = null; //设置应用上下文
    this.configureHeadlessProperty(); //  设置java.awt.headless系统参数为true
    SpringApplicationRunListeners listeners = this.getRunListeners(args);// 创建监听器对象 SpringApplicationRunLinstener 从配置文件中读取到EventPublishingRunListener
    listeners.starting();

    try {
        ApplicationArguments applicationArguments = new DefaultApplicationArguments(args); // 装配命令行参数
        ConfigurableEnvironment environment = this.prepareEnvironment(listeners, applicationArguments); // 准备应用程序运行的环境变量
        this.configureIgnoreBeanInfo(environment); 
        Banner printedBanner = this.printBanner(environment);// 打印 Banner 
        context = this.createApplicationContext(); // 创建 AnnotationConfigServletWebServerApplicationContext 对象（根据webApplicationType 进行选择创建）
        this.prepareContext(context, environment, listeners, applicationArguments, printedBanner);
        this.refreshContext(context); // abstractApplicaitonContext ioc 的基本refresh 流程
        this.afterRefresh(context, applicationArguments);  // 用于完成后续的扩展 
        stopWatch.stop(); // 结束时间打印
        if (this.logStartupInfo) {
            (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), stopWatch);
        }

        listeners.started(context); // 监听器需要执行的方法 根据对应的SpringApplicationStartdEvent事件,获取 ApplicationListeners 并执行
        this.callRunners(context, applicationArguments); // 
    } catch (Throwable var9) {
        this.handleRunFailure(context, var9, listeners);
        throw new IllegalStateException(var9);
    }

    try {
        listeners.running(context); // 执行context ，并返回context 
        return context;
    } catch (Throwable var8) {
        this.handleRunFailure(context, var8, (SpringApplicationRunListeners)null);
        throw new IllegalStateException(var8);
    }
}
	
    private void callRunners(ApplicationContext context, ApplicationArguments args) {
        // ApplicaitonReadyEvent 
        List<Object> runners = new ArrayList();
        runners.addAll(context.getBeansOfType(ApplicationRunner.class).values());
        runners.addAll(context.getBeansOfType(CommandLineRunner.class).values());
        AnnotationAwareOrderComparator.sort(runners);
        Iterator var4 = (new LinkedHashSet(runners)).iterator();

        while(var4.hasNext()) {
            Object runner = var4.next();
            if (runner instanceof ApplicationRunner) {
                this.callRunner((ApplicationRunner)runner, args);
            }

            if (runner instanceof CommandLineRunner) {
                this.callRunner((CommandLineRunner)runner, args);
            }
        }

    }

```

### 准备应用程序运行的环境变量

```java
private ConfigurableEnvironment prepareEnvironment(SpringApplicationRunListeners listeners, ApplicationArguments applicationArguments) {
    ConfigurableEnvironment environment = this.getOrCreateEnvironment(); // 根据系统类型来创建环境变量对象 StandardServletEnvironment
    this.configureEnvironment((ConfigurableEnvironment)environment, applicationArguments.getSourceArgs()); 
    ConfigurationPropertySources.attach((Environment)environment); // 创建configtionProperties
    listeners.environmentPrepared((ConfigurableEnvironment)environment); // 调用对应的listenser 加载对应的配置文件
    this.bindToSpringApplication((ConfigurableEnvironment)environment);
    if (!this.isCustomEnvironment) {
        environment = (new EnvironmentConverter(this.getClassLoader())).convertEnvironmentIfNecessary((ConfigurableEnvironment)environment, this.deduceEnvironmentClass());  // 根据环境对Environment 做转换 ，如果Environment类型不对，会做转换，正常则不做转换
    }

    ConfigurationPropertySources.attach((Environment)environment);
    return (ConfigurableEnvironment)environment;
}
	
	//  在MutablePropertySources 中添加  servletConfigInitParams  servletContextInitParams systemProperties systemEnvironment 属性值
    protected void customizePropertySources(MutablePropertySources propertySources) {
        propertySources.addLast(new StubPropertySource("servletConfigInitParams")); // servlet config 的配置值
        propertySources.addLast(new StubPropertySource("servletContextInitParams")); // servlet context 的配置值
        if (JndiLocatorDelegate.isDefaultJndiEnvironmentAvailable()) {
            propertySources.addLast(new JndiPropertySource("jndiProperties"));
        }

        super.customizePropertySources(propertySources);
    }

    protected void configureEnvironment(ConfigurableEnvironment environment, String[] args) {
        if (this.addConversionService) {
            ConversionService conversionService = ApplicationConversionService.getSharedInstance(); // 加载转换器和格式化器
            environment.setConversionService((ConfigurableConversionService)conversionService);
        }

        this.configurePropertySources(environment, args); // 配置PropertySources
        this.configureProfiles(environment, args); // 加载 spring.profile 
    }


```

### 准备Context信息

```java
    public AnnotationConfigServletWebServerApplicationContext() {
        this.annotatedClasses = new LinkedHashSet();
        this.reader = new AnnotatedBeanDefinitionReader(this); // 注解 bean对象读取器 
        this.scanner = new ClassPathBeanDefinitionScanner(this); // 扫描注解 bean 对象
    }
private void prepareContext(ConfigurableApplicationContext context, ConfigurableEnvironment environment, SpringApplicationRunListeners listeners, ApplicationArguments applicationArguments, Banner printedBanner) {
    context.setEnvironment(environment);
    this.postProcessApplicationContext(context); 
    this.applyInitializers(context); // 执行应用初始化器 ,添加BeanFactoryPostProcessor 和 ApplicationListener
    listeners.contextPrepared(context); // ApplicationContextInitializedEvent  事件， 遍历监听器执行相关逻辑
    if (this.logStartupInfo) { // 日志打印
        this.logStartupInfo(context.getParent() == null);
        this.logStartupProfileInfo(context);
    }

    ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
    beanFactory.registerSingleton("springApplicationArguments", applicationArguments);
    if (printedBanner != null) {
        beanFactory.registerSingleton("springBootBanner", printedBanner);
    }

    if (beanFactory instanceof DefaultListableBeanFactory) {
        ((DefaultListableBeanFactory)beanFactory).setAllowBeanDefinitionOverriding(this.allowBeanDefinitionOverriding);// 不允许bean 重写
    }

    if (this.lazyInitialization) {
        context.addBeanFactoryPostProcessor(new LazyInitializationBeanFactoryPostProcessor()); // 添加懒加载的BeanFactoryPostProcessor 
    }

    Set<Object> sources = this.getAllSources();
    Assert.notEmpty(sources, "Sources must not be empty");
    this.load(context, sources.toArray(new Object[0]));  //  注册当前springboot启动的主类到 BeanDefinition
    listeners.contextLoaded(context); // 通过ApplicationPreparedEvent  执行对应的Linstener  (ConfigFileApplicationLinstener ,LoggingApplicationListener BackGroundApplicationListener  )
}
// load  方法
protected void load(ApplicationContext context, Object[] sources) {
        if (logger.isDebugEnabled()) {
            logger.debug("Loading source " + StringUtils.arrayToCommaDelimitedString(sources));
        }

        BeanDefinitionLoader loader = this.createBeanDefinitionLoader(this.getBeanDefinitionRegistry(context), sources);
        if (this.beanNameGenerator != null) {
            loader.setBeanNameGenerator(this.beanNameGenerator);
        }

        if (this.resourceLoader != null) {
            loader.setResourceLoader(this.resourceLoader);
        }

        if (this.environment != null) {
            loader.setEnvironment(this.environment);
        }

        loader.load();
    }

 private int load(Object source) {
        Assert.notNull(source, "Source must not be null");
        if (source instanceof Class) {
            return this.load((Class)source);
        } else if (source instanceof Resource) {
            return this.load((Resource)source);
        } else if (source instanceof Package) {
            return this.load((Package)source);
        } else if (source instanceof CharSequence) {
            return this.load((CharSequence)source);
        } else {
            throw new IllegalArgumentException("Invalid source type " + source.getClass());
        }
    }
    private int load(Class<?> source) {
        if (this.isGroovyPresent() && BeanDefinitionLoader.GroovyBeanDefinitionSource.class.isAssignableFrom(source)) {
            BeanDefinitionLoader.GroovyBeanDefinitionSource loader = (BeanDefinitionLoader.GroovyBeanDefinitionSource)BeanUtils.instantiateClass(source, BeanDefinitionLoader.GroovyBeanDefinitionSource.class);
            this.load(loader);
        }

        if (this.isEligible(source)) {
            this.annotatedReader.register(new Class[]{source}); // 注册当前springboot启动的主类到 BeanDefinition
            return 1;
        } else {
            return 0;
        }
    }


// 应用初始化器 ,添加BeanFactoryPostProcessor 和 ApplicationListener
    protected void applyInitializers(ConfigurableApplicationContext context) {
        Iterator var2 = this.getInitializers().iterator();

        while(var2.hasNext()) {
            ApplicationContextInitializer initializer = (ApplicationContextInitializer)var2.next();
            Class<?> requiredType = GenericTypeResolver.resolveTypeArgument(initializer.getClass(), ApplicationContextInitializer.class);
            Assert.isInstanceOf(requiredType, context, "Unable to call initializer.");
            initializer.initialize(context);
        }

    }
```

### SpringBoot  自动装配原理

#### 	springboot 自动装配是什么，解决了什么问题

​	把配置的bean（包括自己写的和第三方的sdk）自动加入到IOC容器中

#### 	自动装配的实现原理

	1. 当启动spring boot 程序时，会先创建springApplicaiton  对象， 在对象的构造方法中，会进行一些参数的初始化工作。 最主要的是判断当前应用程序的类型以及初始化器和监听器，在这个过程中会加载整个应用程序中的spring.factories 文件，将文件的内容放到缓存中，方便后续使用
 	2. springApplication 对象创建完成之后，开始执行run方法，来启动整个应用，在启动过程中， 最主要的方法有两个。 第一个是prepareConext 和 refreshContext 。这两个关键步骤中完成了自动装配的核心功能，前面的处理逻辑包含了上下文的创建，banner 的打印。 异常报告的准备等准备工作。
 	3. 在prepareContext 方法中，最主要的是完成了对上下文对象的初始化操作。 包括了属性值的设置。 在整个过程中，有一个非常重要的方法叫做load ，load  主要完成一件事情，将当前启动类作为一个BeanDefinition 注册到registry 中，方便后续在BeanFactoryPostProcessor 调用执行的时候，找到主类，完成 @SpringBootApplication @EnableAutoConfiguration 等注解的解析工作
 	4. 在refreshContext 方法中，会进行整个容器刷新过程，会调用spring  的refresh 方法。在自动装配中，会调用invokeBeanFactoryPostProcessor 方法来执行 BeanFactoryPostProcessor 对应的方法，这里主要是对 ConfigrationClassPostProcessor 类的处理。  在执行postProccessorBeanDefinitionRegistry 的时候，会解析处理各种注解 。包括 @PropertySource @ComponentScan @ComponentScans @Bean @Import 注解，这里最重要的是对@Import 注解的解析
 	5. 在解析Import 注解的时候，会有一个getImports 的方法，从主类开始递归解析注解，把所有的Import  的注解都解析到。然后再processImport 方法中，对Import 的类进行分类，此处主要识别的是AutoConfigurationImportSelect 归属于ImprotSelect 的子类，在后续过程中会调用deferredImportSelectorHandler  中的process 方法，来完成@EnableAutoConfiguration 的加载

### 为什么springboot的jar 可以直接运行

1. spring boot 提供了一个插件 spring-boot-maven-plugin 用于把程序打包成一个可以执行的jar 包
2. spring boot  应用打包后，生成一个Fat jar 包含了应用依赖的jar 包和spring boot  loader 相关的类
3. java -jar 会去找jar 中的 manifest 文件，在里面找到真正的启动类
4. Fat jar 的启动 Main 函数是 JarLauncher ，他负责创建 LaunchedURLClassLoader 来加载 boot-lib 下面的jar ，并以一个新线程启动应用程序的main 函数

### Spring 内置Tomcat 启动原理





### Spring Boot  自定义start 

1. 在resources 下创建文件夹 META-INF   并在目录下创建spring.factories 文件
2. 在文件中添加我们需要自动装配的 class 
3. 

### Spring Aplication run 方法





### Spring Boot 有哪些优点

1. 减少开发，测试时间和努力。

2. 使用 JavaConfig 有助于避免使用 XML。

3. 避免大量的 Maven 导入和各种版本冲突。

4. 提供意见发展方法。

5. 通过提供默认值快速开始开发。

6. 没有单独的 Web 服务器需要。这意味着你不再需要启动 Tomcat，Glassfish或其他任何东西。

7. 需要更少的配置 因为没有 web.xml 文件。只需添加用@ Configuration 注释的类，然后添加用@Bean 注释的方法，Spring 将自动加载对象并像以前一样对其进行管理。您甚至可以将@Autowired 添加到 bean 方法中，以使 Spring 自动装入需要的依赖关系中。

8. 基于环境的配置 使用这些属性，您可以将您正在使用的环境传递到应用程序：-Dspring.profiles.active = {enviornment}。在加载主应用程序属性文件后，Spring 将在（application{environment} .properties）中加载后续的应用程序属性文件

   

### Spring Boot 的核心注解 @SpringBootApplication

@SpringBootApplication，主要组合包含了以下3 个注解：

@SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。
@EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能：
@ComponentScan：标识扫描路径，因为默认是没有实际扫描路径 ，所以spring boot 扫描的路径是启动类所在的当前目录

### Spring Boot启动的时候运行一些特定的代码

如果你想在Spring Boot启动的时候运行一些特定的代码，你可以实现接口ApplicationRunner或者CommandLineRunner，这两个接口实现方式一样，它们都只提供了一个run方法。CommandLineRunner：启动获取命令行参数

### Spring Boot 中静态资源直接映射的优先级是怎样的

优先级顺序为：META-INF/resources > resources > static > public

### Spring Boot扫描流程

1. 调用run方法中的 refreshContext 方法
2. 用AbstractApplicationContext中的 refresh 方法
3. 委托给 invokeBeanFactoryPostProcessors 去处理调用链
4. 其中一个方法 postProcessBeanDefinitionRegistry会 去调用 processConfigBeanDefinitions解析 beandefinitions
5. 在 processConfigBeanDefinitions 中有一个 parse 方法，其中有 componentScanParser.parse的方法，这个方法会扫描当前路径下所有 Component 组件

### Spring boot 是如何启动tomcat

1. 首先springboot 在启动时，会先创建一个spring 容器‘
2. 在创建spring容器过程中，会利用@ConditionalOnClass 技术来判断当前classpath 中是否存在tomcat 依赖，如果存在，则生成一个启动tomcat的bean
3. spring 容器创建完之后，会获取启动tomcat 的bean ，并创建tomcat 对象，并绑定端口等。 然后启动tomcat

### Spring  boot  加载配置文件的原理

通过事件监听的方式读取配置文件

1. 会加载一个ApplicationEnviromentPreparedEvent 事件 
2. 执行ConfigFileApplicationListener 来加载配置文件

### Spring Boot  中配置文件的加载顺序是怎么样的

优先级从高到低。

1. 命令行参数
2. 来自java comp env  的jndi 属性
3. Java 系统属性
4. 操作系统环境变量
5. jar 包外的 yml 配置文件
6. jar 包内部的yml 文件
7. @Configuration 注解上的@PropertySource 

```java
classpath:/,classpath:/config/,file:./,file:./config/*/,file:./config/
```