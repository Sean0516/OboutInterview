# Netty



### netty 的特点

一个高性能，异步事件驱动的NIO 框架，他提供了对TCP ，UDP 和文件传输的支持，使用更高效的socket 底层。对epoll 空轮询引起的cpu 占用飙升在内部进行了处理，避免了直接使用NIO 的陷阱，简化了NIO 的处理方式

采用多种decoder / encoder 支持，对TCP 粘包和分包进行自动化处理

可以使用接受/初始线程池，提高连接效率，对重连，心跳检测的简单支持

可配置IO 线程数，TCP 参数，TCP 接受和发送缓冲区使用直接内存代替堆内存，通过内存池的方式循环利用ByteBuf 

通过引用计数器及时申请释放不再引用的对象，降低GC 频率

使用单线程串行化的方式吗，高效的Reactor 线程模型

大量使用了volitale ，使用了CAS 和原子类，线程安全类的使用，读写锁的使用。

### 多路复用 通讯方式

Netty 架构按照 Reactor 模式设计和实现，它的服务端通信序列图如下

![image-20210806114444817](https://gitee.com/Sean0516/image/raw/master/img/image-20210806114444817.png)

客户端通信序列图如下

![image-20210806114548001](https://gitee.com/Sean0516/image/raw/master/img/image-20210806114548001.png)

Netty 的 IO 线程 NioEventLoop 由于聚合了多路复用器 Selector，可以同时并发处理成百上千个客户端 Channel，由于读写操作都是非阻塞的，这就可以充分提升 IO 线程的运行效率，避免由于频繁 IO 阻塞导致的线程挂起

1. 异步通讯 异步通讯 NIO

   由于 Netty 采用了异步通信模式，一个 IO 线程可以并发处理 N 个客户端连接和读写操作，这从根本上解决了传统同步阻塞 IO 一连接一线程模型，架构的性能、弹性伸缩能力和可靠性都得到了极大的提升

2. 零拷贝（ （DIRECT BUFFERS 使用堆外直接内存 使用堆外直接内存） ）

   1. Netty 的接收和发送 ByteBuffer 采用 DIRECT BUFFERS，使用堆外直接内存进行 Socket 读写，不需要进行字节缓冲区的二次拷贝。如果使用传统的堆内存（HEAP BUFFERS）进行 Socket 读写，JVM 会将堆内存 Buffer 拷贝一份到直接内存中，然后才写入 Socket 中。相比于堆外直接内存，消息在发送过程中多了一次缓冲区的内存拷贝
   2. Netty 提供了组合 Buffer 对象，可以聚合多个 ByteBuffer 对象，用户可以像操作一个 Buffer 那样方便的对组合 Buffer 进行操作，避免了传统通过内存拷贝的方式将几个小 Buffer 合并成一个大的Buffer
   3. Netty的文件传输采用了transferTo方法，它可以直接将文件缓冲区的数据发送到目标Channel，避免了传统通过循环 write 方式导致的内存拷贝问题

3. 内存池 （ 基于内存池的缓冲区重用机制）

   随着 JVM 虚拟机和 JIT 即时编译技术的发展，对象的分配和回收是个非常轻量级的工作。但是对于缓冲区 Buffer，情况却稍有不同，特别是对于堆外直接内存的分配和回收，是一件耗时的操作。为了尽量重用缓冲区，Netty 提供了基于内存池的缓冲区重用机制



### netty 的线程模型 

netty 通过 Reactor 模型，基于多路复用接收器接收并处理用户请求， 内部实现了两个线程池

boss 线程池和work 线程池。其中boss 线程池的线程负责处理请求的accept 事件，当接受到accept 事件的请求时，把对应的socket 封装到一个NioSocketChannel 中，并交给work 线程，其中work 线程池负责请求的read 和write 事件 ，由对应的handler 处理

######  Reactor单线程模型

所有的I/O操作都由一个线程完成。即多路复用，事件分发和处理都在一个Reactor 线程完成，既要接受客户端的连接请求，向服务端发起连接，又要发送/读取请求或应当/响应消息。 一个NIO 线程同时处理成百上千的链路，性能上无法支持，速度慢，若线程进入死循环，整个程序不可用，对高负载，大并发的应用场景不适合

###### Reactor多线程模型 

有一个NIO 线程只负责监听服务端，接收客户端的TCP连接请求。 NIO 线程池负责网络IO 的操作， 即数据的读取，解码，编码和发送。 一个NIO 线程可以同时处理N 条链路，但是一个链路只对应一个NIO 线程，这是为了防止发生并发操作问题。 但在并发百万客户端连接或需要安全认证时，一个Acceptor 线程可能会存在性能不足问题

###### Reactor主从多线程模型  

Acceptor 线程用于绑定监听端口，接收客户端连接，将SocketChannel 从主线程池的Reactor 线程的多路复用器上移除，重新注册到Sub 线程池的线程上，用于处理I/O 的读写操作，从而保证main Reactor 只负责认证，握手等操作

### TCP 粘包/ 拆包的原因及解决方法

TCP 是以流的方式来处理数据，一个完整的包可能会被TCP 拆分成多个包进行发送，也可能把小的封装成一个大的数据包发送。

###### TCP/ 粘包/分包的原因

应用程序写入的字节大小大于套接字发送缓冲区的大小，会发生拆包现象，而应用程序写入数据小于套接字缓冲区大小，网卡将应用多次写入的数据发送到网络上，这将发生粘包现象

###### 解决方法

消息定长： FixedLengthFrameDecoder 类

包尾增加特殊字符分割： 行分隔符类  LineBaseFrameDecoder 或者自定义分隔符类。 DelimiterBaseFrameDecoder 

将消息分割消息头和消息体  LengthFieldBaseFrameDecoder 类，分为有头部的拆包和粘包，长度字段在前且有头部的拆包和粘包，多扩展头部的拆包与粘包

### Netty 的零拷贝实现

Netty 的接受和发送ByteBuffer 采用DIRECT BUFFERS 。 使用堆外直接内存进行socket 读写，不需要进行字节缓冲区的二次拷贝，堆内存多了一个内存拷贝，JVM 会将堆内存 Buffer 拷贝一份到直接内存中，然后才写入Socket 中，ByteBuffer 由 ChannelConfig  分配， 而ChannelConfig 创建ByteBufAllocator 默认使用Direct Buffer 

CompositeByteBuf 类可以将多个 ByteBuf 合并为一个逻辑上的 ByteBuf, 避免了传统通过内存拷贝的方式将几个小 Buffer 合并成一个大的 Buffer。addComponents 方法将  header与 body 合并为一个逻辑上的 ByteBuf, 这两个 ByteBuf 在 CompositeByteBuf 内部都是单独存在的, CompositeByteBuf 只是逻辑上是一个整体

通过 FileRegion 包装的 FileChannel.tranferTo 方法 实现文件传输, 可以直接将文件缓冲区的数据发送到目标 Channel，避免了传统通过循环 write 方式导致的内存拷贝问题

通过  wrap 方法,  我们可以将  byte[]  数组、ByteBuf、ByteBuffer 等包装成一个  NettyByteBuf 对象, 进而避免了拷贝操作

### Netty 的高性能表现在哪些方面

1. 心跳  

   对服务端：会定时清除闲置会话 inactive(netty5)，对客户端:用来检测会话是否断开，是否重来，检测网络延迟，其中 idleStateHandler 类 用来检测会话状态

2. 串行无锁化设计

   即消息的处理尽可能在同一个线程内完成，期间不进行线程切换，这样就避免了多线程竞争和同步锁。表面上看，串行化设计似乎 CPU 利用率不高，并程度不够。但是，通过调整 NIO 线程池的线程参数，可以同时启动多个串行化的线程并行运行，
   这种局部无锁化的串行线程设计相比一个队列-多个工作线程模型性能更优

3. 可靠性，链路有效性检测

   链路空闲检测机制，读/写空闲超时机制；内存保护机制：通过内存池重用ByteBuf;ByteBuf 的解码保护；优雅停机：不再接收新消息、退出前的预处理操作、资源的释放操作

4. 安全性

   SSL V2 和 V3，TLS，SSL 单向认证、双向认证和第三方 CA认证

5. 高效并发编程的体现

   volatile 的大量、正确使用；CAS 和原子类的广泛使用；线程安全容器的使用；通过读写锁提升并发性能。IO 通信性能三原则：传输（AIO）、协议（Http）、线程（主从多线程）

6. 流量整型的作用

   防止由于上下游网元性能不均衡导致下游网元被压垮，业务流中断；防止由于通信模块接受消息过快，后端业务线程处理不及时导致撑死问题