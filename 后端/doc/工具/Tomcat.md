# Tomcat



### Tomcat  的缺省端口是多少，怎么修改

修改 conf/server.xml 下的 Connector  节点，修改 port

### Tomcat 为什么要使用自定义类加载器

​	一个tomcat 中可以部署多个应用，而每个应用中都存在很多类，并且各个应用中的类都是独立的。在tomcat中，会为每个部署的应用都生成一个类加载器，名字叫做WebAppClassLoader ，这样Tomcat 中每个应用就可以使用自己的类加载器去加载自己的类。 从而达到应用间的类隔离。不出现冲突

### tomcat  部署方式

1. 直接把web 项目放在webapps  下，tomcat 会自动将其部署
2. 在server.xml 文件上配置<Context> 节点。设置相关属性即可
3. 通过Catalina  进行配置，进入到conf/Catalina/localhost 文件下，创建一个xml 文件，该文件的名字就是站点的名字

### tomcat 容器是如何创建 servlet 类实例，用到什么原理

当容器启动时，会读取在webapps  目录下所有的web 应用中的web.xml 文件，然后对xml 进行解析。 并且读取servlet 注册信息，然后，将每个应用中注册的servlet  类都进行加载。 并通过反射的方式实例化

### tomcat如何优化

​	对于tomcat 优化，可以从两个方面进行跳转 ： 内存和线程

​	首先启动tomcat ，实际上就是启动一个JVM ，所以可以按照JVM 调优的方式来进行调整，从而达到Tomcat 优化的目的

​	另外Tomcat 设计了一些缓存区， 比如AppReadBufSize 等缓存区来提高吞吐量

​	还可以调整tomcat 的线程， 比如调整 minSpareThreads 参数来改变tomcat空闲时的线程数，调整maxThreads 参数来设置tomcat 处理连接的最大线程数。还可以调整IO 模型，比如使用NIO APR 这种相比BIO 更高效的IO 模型

### 内存调优

内存方式的设置是在 catalina.sh 中，调整一下 JAVA_OPTS 变量即可，因为后面的启动参数会把 JAVA_OPTS 作为 JVM 的启动参数来处理

具体设置如下：
JAVA_OPTS="$JAVA_OPTS  -Xmx3550m  -Xms3550m  -Xss128k  -XX:NewRatio=4 -XX:SurvivorRatio=4"

其各项参数如下：

-Xmx3550m：设置 JVM 最大可用内存为 3550M

-Xms3550m：设置 JVM 初始内存为 3550m。此值可以设置与-Xmx 相同，以避免每次垃圾回收完成后 JVM 重新分配内存

-Xmn2g：设置年轻代大小为 2G。整个堆大小=年轻代大小 + 年老代大小 +持久代大小。持久代一般固定大小为 64m，所以增大年轻代后，将会减小年老代大小。此值对系统性能影响较大，Sun 官方推荐配置为整个堆的 3/8

-Xss128k：设置每个线程的堆栈大小。JDK5.0 以后每个线程堆栈大小为 1M，以前每个线程堆栈大小为 256K。更具应用的线程所需内存大小进行调整。在相同物理内存下，减小这个值能生成更多的线程。但是操作系统对一个进程内的线程数还是有限制的，不能无限生成

-XX:NewRatio=4:设置年轻代（包括 Eden 和两个 Survivor 区）与年老代的比值（除去持久代）。设置为 4，则年轻代与年老代所占比值为 1：4，年轻代占整个堆栈的 1/5

-XX:MaxPermSize=16m:设置持久代大小为 16m。
-XX:MaxTenuringThreshold=0：设置垃圾最大年龄。如果设置为 0 的话，则年轻代对象不经过 Survivor 区，直接进入年老代。对于年老代比较多的应用，可以提高效率。如果将此值设置为一个较大值，则年轻代对象会在 Survivor 区进行多次复制，这样可以增加对象再年轻代的存活时间，增加在年轻代即被回收的概论

### tomcat 一个请求的完整过程

1. 请求被发送到本机端口 8080，被在那里侦听的 Coyote  HTTP/1.1Connector 获得
2. Connector 把该请求交给它所在的 Service 的 Engine 来处理，并等待来自Engine 的回应
3. Engine 获得请求路径localhost/demo/1，匹配它所拥有的所有虚拟主机 Host
4. Engine 匹配到名为 localhost 的 Host（即使匹配不到也把请求交给该 Host处理，因为该 Host 被定义为该 Engine 的默认主机）
5. localhost Host 获得请求/demo/1，匹配它所拥有的所有 Context
6. Host 匹配到路径为/demo 的 Context（如果匹配不到就把该请求交给路径名为”“的 Context 去处理
7. path=”/demo”的 Context 获得请求/1，在它的 mapping table 中寻找对应的 servlet
8. Context 匹配到 对应的 servlet 类
9. 构造 HttpServletRequest 对象和 HttpServletResponse 对象，作为参数调用Servlet 的 doGet 或 doPost 方法
10. Context 把执行完了之后的 HttpServletResponse 对象返回给 Host
11. Host 把 HttpServletResponse 对象返回给 Engine
12. Engine 把 HttpServletResponse 对象返回给 Connector
13. Connector 把 HttpServletResponse 对象返回给客户 browser