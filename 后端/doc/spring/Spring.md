# SpringBoot

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

### Spring Boot  中配置文件的加载顺序是怎么样的

优先级从高到低。

1. 命令行参数
2. 来自java comp env  的jndi 属性
3. Java 系统属性
4. 操作系统环境变量
5. jar 包外的 yml 配置文件
6. jar 包内部的yml 文件
7. @Configuration 注解上的@PropertySource 

# Spring Cloud 

### 什么是 Hystrix？它如何实现容错

Hystrix 是一个延迟和容错库，旨在隔离远程系统，服务和第三方库的访问点，当出现故障是不可避免的故障时，停止级联故障并在复杂的分布式系统中实现弹性。通常对于使用微服务架构开发的系统，涉及到许多微服务。这些微服务彼此协作

### 什么是 Netflix Feign？它的优点是什么



### 什么是 Spring Cloud Bus

spring cloud bus 将分布式的节点用轻量的消息代理连接起来，它可以用于广播配置文件的更改或者服务直接的通讯，也可用于监控。如果修改了配置文件，发送一次请求，所有的客户端便会重新读取配置文件

### 什么是服务雪崩，什么是服务限流i

1. 当服务A 调用服务B ，服务B 调用C ，此时大量请求突然请求服务A ，加入服务A 本身可以抗住这些请求，但是如果服务C 扛不住，导致服务C 请求堆积，从而服务B 请求堆积。 从而服务A 不可用，这就是服务雪崩。 解决方式是服务降级和服务熔断
2. 服务限流是指高并发请求下，为了保护系统，可以对访问服务的请求进行数量上的限制，从而防止系统不被大量请求压垮。



### 什么是服务熔断？什么是服务降级

熔断机制是应对雪崩效应的一种微服务链路保护机制。当某个微服务不可用或者响应时间太长时，会进行服务降级，进而熔断该节点微服务的调用，快速返回“错误”的响应信息。当检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通过Hystrix实现，Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内调用20次，如果失败，就会启动熔断机制

服务降级，一般是从整体负荷考虑。就是当某个服务熔断之后，服务器将不再被调用，此时客户端可以自己准备一个本地的fallback回调，返回一个缺省值。这样做，虽然水平下降，但好歹可用，比直接挂掉强

熔断是下游服务故障触发的，降级是为了降低系统负载





### Spring Cloud Gateway

Spring Cloud Gateway是Spring Cloud官方推出的第二代网关框架，取代Zuul网关。网关作为流量的，在微服务系统中有着非常作用，网关常见的功能有路由转发、权限校验、限流控制等作用

### spring cloud 的核心组件有哪些

- Eureka：服务注册于发现
- Feign：基于动态代理机制，根据注解和选择的机器，拼接请求 url 地址，发起请求
- Ribbon：实现负载均衡，从一个服务的多台机器中选择一台。
- Hystrix：提供线程池，不同的服务走不同的线程池，实现了不同服务调用的隔离，避免了服务雪崩的问题
- Zuul：网关管理，由 Zuul 网关转发请求给对应的服务

### SpringCloud 和 Dubbo 有哪些区别

首先，他们都是分布式管理框架。

1. dubbo 是二进制传输，占用带宽会少一点。SpringCloud是http 传输，带宽会多一点，同时使用http协议一般会使用JSON报文，消耗会更大
2. dubbo 开发难度较大，所依赖的 jar 包有很多问题大型工程无法解决。SpringCloud 对第三方的继承可以一键式生成，天然集成
3. SpringCloud 接口协议约定比较松散，需要强有力的行政措施来限制接口无序升级
4. 最大的区别: Spring Cloud抛弃了Dubbo 的RPC通信，采用的是基于HTTP的REST方式

### Ribbon和Feign的区别

1. Ribbon都是调用其他服务的，但方式不同
2. 启动类注解不同，Ribbon是@RibbonClient feign的是@EnableFeignClients
3. 服务指定的位置不同，Ribbon是在@RibbonClient注解上声明，Feign则是在定义抽象方法的接口中使用@FeignClient声明
4. 调用方式不同，Ribbon需要自己构建http请求，模拟http请求然后使用RestTemplate发送给其他服务，步骤相当繁琐。Feign需要将调用的方法定义成抽象方法即可

# Spring Cloud Alibaba

