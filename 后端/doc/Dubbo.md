# Dubbo

### Dubbo 的整体架构设计有哪些分层

###### 接口服务层（Service）：

该层与业务逻辑相关，根据 provider 和 consumer 的业务设计对应的接口和实现

###### 配置层（Config）：

对外配置接口，以 ServiceConfig 和 ReferenceConfig 为中心
服务代理层（Proxy）：服务接口透明代理，生成服务的客户端 Stub 和 服务端的 Skeleton，以 ServiceProxy 为中心，扩展接口为ProxyFactory

###### 服务注册层（Registry）：

封装服务地址的注册和发现，以服务 URL 为中心，扩展接口为 RegistryFactory、Registry、RegistryService

###### 路由层（Cluster）：

封装多个提供者的路由和负载均衡，并桥接注册中心，以Invoker 为中心，扩展接口为 Cluster、Directory、Router和 LoadBlancce

###### 监控层（Monitor）：

RPC 调用次数和调用时间监控，以 Statistics 为中心，扩展接口为 MonitorFactory、Monitor 和 MonitorService

###### 远程调用层（Protocal）：

封装 RPC 调用，以 Invocation 和 Result 为中心，扩展接口为 Protocal、Invoker 和 Exporter

###### 信息交换层（Exchange）：

封装请求响应模式，同步转异步。以 Request 和Response 为中心，扩展接口为 Exchanger、ExchangeChannel、ExchangeClient 和 ExchangeServer

###### 网络传输层（Transport）：

抽象 mina 和 netty 为统一接口，以 Message 为中心，扩展接口为 Channel、Transporter、Client、Server和 Codec

###### 数据序列化层（Serialize）：

可复用的一些工具，扩展接口为 Serialization、ObjectInput、ObjectOutput 和 ThreadPool

### 服务调用是阻塞的吗

Dubbo 是基于 NIO 的非阻塞实现并行调用，客户端不需要启动多线程即可完成并行调用多个远程服务，相对多线程开销较小，异步调用会返回一个 Future 对象

### 默认使用什么序列化框架，你知道的还有哪些

推荐使用 Hessian 序列化，还有 Duddo、FastJson、Java 自带序列化

### 服务提供者能实现失效踢出是什么原理

服务失效踢出基于 zookeeper 的临时节点原理

### Dubbo 集群容错有几种方案

| 集群容错方案      | 说明                                       |
| ----------------- | ------------------------------------------ |
| Failover Cluster  | 失败自动切换，自动重试其他服务器（默认）   |
| Failfast Cluster  | 快速失败，立即报错，只发起一次调用         |
| Failsafe Cluster  | 失败安全，出现异常时，直接忽略             |
| Failback Cluster  | 失败自动恢复，记录失败请求，定时重发       |
| Forking Cluster   | 并行调用多个服务器，只要一个成功即返回     |
| Broadcast Cluster | 广播逐个调用所有提供者，任意一个报错则报错 |



### 画一画服务注册与发现的流程图



### Dubbo 服务降级，失败重试怎么做

可以通过 dubbo:reference 中设置 mock="return null"。mock 的值也可以修改为 true，然后再跟接口同一个路径下实现一个 Mock 类，命名规则是 “接口名称+Mock” 后缀。然后在 Mock 类里实现自己的降级逻辑

### Dubbo Monitor 实现原理

Consumer 端在发起调用之前会先走 filter 链；provider 端在接收到请求时也是先走 filter 链，然后才进行真正的业务逻辑处理。
默认情况下，在 consumer 和 provider 的 filter 链中都会有 Monitorfilter。

1. MonitorFilter 向 DubboMonitor 发送数据
2. DubboMonitor 将数据进行聚合后（默认聚合 1min 中的统计数据）暂存到ConcurrentMap<Statistics, AtomicReference>statisticsMap，然后使用一个含有 3 个线程（线程名字：DubboMonitorSendTimer）的线程池每隔 1min 钟，调用
   SimpleMonitorService 遍历发送 statisticsMap 中的统计数据，每发送完毕一个，就重置当前的 Statistics 的 AtomicReference
3. SimpleMonitorService 将这些聚合数据塞入 BlockingQueue queue 中（队列大写为 100000）
4. SimpleMonitorService 使用一个后台线程（线程名为：DubboMonitorAsyncWriteLogThread）将 queue 中的数据写入文件（该线程
   以死循环的形式来写）
5. SimpleMonitorService 还会使用一个含有 1 个线程（线程名字：DubboMonitorTimer）的线程池每隔 5min 钟，将文件中的统计数据
   画成图表

### Dubbo 如何优雅停机

Dubbo 是通过 JDK 的 ShutdownHook 来完成优雅停机的，所以如果使用kill -9 PID 等强制关闭指令，是不会执行优雅停机的，只有通过kill PID 时，才会执行

### Dubbo 用到哪些设计模式



### Dubbo有哪几种负载均衡策略，默认是哪种

| 负载均衡策略    | 说明                                           |
| --------------- | ---------------------------------------------- |
| Random          | 随机，按权重设置随机概率（默认）               |
| RoundRobin      | 轮询，按公约后的权重                           |
| Least Active    | 最少活跃调用数，相同活跃数的随机               |
| Consistent Hash | 一致hash  ，相同参数的请求总是发到同一个提供者 |

### 服务读写推荐的容错策略是怎样的

读操作建议使用 Failover 失败自动切换，默认重试两次其他服务器。
写操作建议使用 Failfast 快速失败，发一次调用失败就立即报错。

