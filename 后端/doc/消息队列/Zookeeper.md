# zookeeper

### ZooKeeper 是什么

是一个开放源码的分布式协调服务，它是集群的管理者，监视着集群中各个节点的状态根据节点提交的反馈进行下一步合理操作。最终，将简单易用的接口和性能高效、功能稳定的系统提供给用户。分布式应用程序可以基于 Zookeeper 实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理Master 选举、分布式锁和分布式队列等功能

### Zookeeper 文件系统

Zookeeper 提供一个多层级的节点命名空间（节点称为 znode）。与文件系统不同的是，这些节点都可以设置关联的数据，而文件系统中只有文件节点可以存放数据而目录节点不行

Zookeeper 为了保证高吞吐和低延迟，在内存中维护了这个树状的目录结构，这种特性使得 Zookeeper 不能用于存放大量的数据，每个节点的存放数据上限为1M

### znode

zookeeper集群自身维护了一套数据结构。这个存储结构是一个树形结构，其上的每一个节点，我们称之为“znode”。

![image-20210812151729674](https://gitee.com/Sean0516/image/raw/master/img/image-20210812151729674.png)

- 每一个znode默认能够存储1MB的数据（对于记录状态性质的数据来说，够了）
- 可以使用zkCli命令，登录到zookeeper上，并通过ls、create、delete、sync等命令操作这些znode节点
- znode除了名称、数据以外，还有一套属性：zxid。这套zid与时间戳对应，记录zid不同的状态（后续我们将用到)

### znode结构

- zxid  时间戳，每次修改znode 都会生成一个新的 zxid ，如果zxid1 小于zxid 2 那么zxid1在zxid2 之前发生
- version 对节点的每次修改将使得节点的版本号增加1
- data 每一个znode 默认能够存储1M 的数据，对data的修改都会引起两者的变化
- tick 租约协议的具体体现，如果当前节点是“临时节点” 在tick 时间周期内没有收到新的客户端租约，则视为失效

此外，znode还有操作权限。如果我们把以上几类属性细化，又可以得到以下属性的细节

- czxid：创建节点的事务的zxid
- mzxid：对znode最近修改的zxid
- ctime：以距离时间原点(epoch)的毫秒数表示的znode创建时间
- mtime：以距离时间原点(epoch)的毫秒数表示的znode最近修改时间
- version：znode数据的修改次数
- cversion：znode子节点修改次数
- aversion：znode的ACL修改次数
- ephemeralOwner：如果znode是临时节点，则指示节点所有者的会话ID；如果不是临时节点，则为零。
- dataLength：znode数据长度。
- numChildren：znode子节点个数

### 四种类型的数据节点 Znode

###### PERSISTENT-持久节点

除非手动删除，否则节点一直存在于 Zookeeper 上

###### EPHEMERAL-临时节点

临时节点的生命周期与客户端会话绑定，一旦客户端会话失效（客户端与zookeeper 连接断开不一定会话失效），那么这个客户端创建的所有临时节点都会被移除

###### PERSISTENT_SEQUENTIAL-持久顺序节点

基本特性同持久节点，只是增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字

###### EPHEMERAL_SEQUENTIAL-临时顺序节点

基本特性同临时节点，增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字

### ZAB 协议

ZAB 协议是为分布式协调服务 Zookeeper 专门设计的一种支持崩溃恢复的原子广播协议  ZAB 协议包括两种基本的模式：崩溃恢复和消息广播



当整个 zookeeper 集群刚刚启动或者 Leader 服务器宕机、重启或者网络故障导致不存在过半的服务器与 Leader 服务器保持正常通信时，所有进程（服务器）进入崩溃恢复模式，首先选举产生新的 Leader 服务器，然后集群中 Follower 服务器开始与新的 Leader 服务器进行数据同步

当集群中超过半数机器与该 Leader服务器完成数据同步之后，退出恢复模式进入消息广播模式，Leader 服务器开始接收客户端的事务请求生成事物提案来进行事务请求处理

### Zab 协议有两种模式 - 恢复模式（选主）、广播模式（同步 ）

Zab协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。当服务启动或者在领导者崩溃后，Zab 就进入了恢复模式，当领导者被选举出来，且大多数 Server 完成了和 leader 的状态同步以后，恢复模式就结束了。状态同步保证了 leader 和 Server 具有相同的系统状态

### ZAB 协议 4 阶段

- Leader election （选举阶段 - 选出准 Leader 

  Leader election（选举阶段）：节点在一开始都处于选举阶段，只要有一个节点得到超半数节点的票数，它就可以当选准 leader。只有到达 广播阶段（broadcast） 准 leader 才会成为真正的 leader。这一阶段的目的是就是为了选出一个准 leader，然后进入下一个阶段

- Discovery （发现阶段 - 接受提议、生成 epoch 、接受 epoch ）

  Discovery（发现阶段）：在这个阶段，followers 跟准 leader 进行通信，同步 followers最近接收的事务提议。这个一阶段的主要目的是发现当前大多数节点接收的最新提议，并且准 leader 生成新的 epoch，让 followers 接受，更新它们的 accepted Epoch

  一个 follower 只会连接一个 leader，如果有一个节点 f 认为另一个 follower p 是 leader，f在尝试连接 p 时会被拒绝，f 被拒绝之后，就会进入重新选举阶段。

- Synchronization （同步阶段 - 同步 follower 副本 ）

  Synchronization（同步阶段）：同步阶段主要是利用 leader 前一阶段获得的最新提议历史，同步集群中所有的副本。只有当 大多数节点都同步完成，准 leader 才会成为真正的 leader。follower 只会接收 zxid 比自己的 lastZxid 大的提议

- Broadcast （广播阶段 -leader 消息广播

  Broadcast（广播阶段）：到了这个阶段，Zookeeper 集群才能正式对外提供事务服务，并且 leader 可以进行消息广播。同时如果有新的节点加入，还需要对新节点进行同步

### Zookeeper Watcher 机制 （ 数据变更通知）

Zookeeper 允许客户端向服务端的某个 Znode 注册一个 Watcher 监听，当服务端的一些指定事件触发了这个 Watcher，服务端会向指定客户端发送一个事件通知来实现分布式的通知功能，然后客户端根据 Watcher 通知状态和事件类型做出业务上的改变

1. watcher 的工作机制

   - 客户端注册watcher
   - 服务端处理watcher
   - 客户端回调watcher

### Watcher 特性总结

1. 一次性

   无论是服务端还是客户端，一旦一个watcher 被触发，zookeeper 都会将其从相应的存储中移除，再次使用需要重新注册。 这样的设计有效的减轻了服务端的压力，不然对于更新非常频繁的节点，服务端会不断的向客户端发送事件通知，无论对于网络还是服务端的压力都非常大。 

2. 客户端串行执行

   客户端watcher 回调的过程是一个串行同步的过程，只有回调后客户端才能看到最新的数据状态。一个Watcher回调逻辑不应该太多，以免影响别的watcher执行

3. 轻量级

   WatchEvent是最小的通信单元，结构上只包含通知状态、事件类型和节点路径，并不会告诉数据节点变化前后的具体内容

4. 时效性

   Watcher只有在当前session彻底失效时才会无效，若在session有效期内快速重连成功，则watcher依然存在，仍可接收到通知

### 客户端注册 Watcher 实现

      1. 调用 getData()/getChildren()/exist()三个 API，传入 Watcher 对象
      2. 标记请求 request，封装 Watcher 到 WatchRegistration
      3. 封装成 Packet 对象，发服务端发送 request
      4. 收到服务端响应后，将 Watcher 注册到 ZKWatcherManager 中进行管理
      5. 请求返回，完成注册

### 服务端处理 Watcher 实现

1. 服务端接收 Watcher 并存储
   	接收到客户端请求，处理请求判断是否需要注册 Watcher，需要的话将数据节点的节点路径和 ServerCnxn（ServerCnxn 代表一个客户端和服务端的连接，实现了 Watcher 的process 接口，此时可以看成一个 Watcher 对象）存储在WatcherManager 的 WatchTable 和 watch2Paths 中去

2. Watcher 触发

3. 调用 process 方法来触发 Watcher

   这里 process 主要就是通过 ServerCnxn 对应的 TCP 连接发送 Watcher 事件通知

### 客户端回调 Watcher

客户端 SendThread 线程接收事件通知，交由 EventThread 线程回调 Watcher。客户端的 Watcher 机制同样是一次性的，一旦被触发后，该 Watcher 就失效了

### Zookeeper 服务器角色

###### Leader

1. 事务请求的唯一调度和处理者，保证集群事务处理的顺序性
2. 集群内部各服务的调度者

###### Follower

1. 处理客户端的非事务请求，转发事务请求给 Leader 服务器
2. 参与事务请求 Proposal 的投票
3. 参与 Leader 选举投票

###### Observer

1. 3.0 版本以后引入的一个服务器角色，在不影响集群事务处理能力的基础上提升集群的非事务处理能力
2. 处理客户端的非事务请求，转发事务请求给 Leader 服务器
3. 不参与任何形式的投票

### Zookeeper 下 Server 工作状态

服务器具有四种状态，分别是 LOOKING、FOLLOWING、LEADING、OBSERVING。

1. LOOKING：寻找 Leader 状态。当服务器处于该状态时，它会认为当前集群中没有 Leader，因此需要进入 Leader 选举状态。
2. FOLLOWING：跟随者状态。表明当前服务器角色是 Follower。
3. LEADING：领导者状态。表明当前服务器角色是 Leader。
4. OBSERVING：观察者状态。表明当前服务器角色是 Observer

### 数据同步

整个集群完成 Leader 选举之后，Learner（Follower 和 Observer 的统称）回向Leader 服务器进行注册。当 Learner 服务器想 Leader 服务器完成注册后，进入数据同步环节

##### 数据同步流程：（均以消息传递的方式进行）

1. Learner 向 Learder 注册
2. 数据同步
3. 同步确认

### Zookeeper 的数据同步种类

1. 直接差异化同步（DIFF 同步）
2. 先回滚再差异化同步（TRUNC+DIFF 同步）
3. 仅回滚同步（TRUNC 同步）
4. 全量同步（SNAP 同步

### zookeeper 是如何保证事务的顺序一致性的

zookeeper 采用了全局递增的事务 Id 来标识，所有的 proposal（提议）都在被提出的时候加上了 zxid，zxid 实际上是一个 64 位的数字，高 32 位是 epoch（时期; 纪元; 世; 新时代）用来标识 leader 周期，如果有新的 leader 产生出来，epoch会自增，低 32 位用来递增计数。当新产生 proposal 的时候，会依据数据库的两阶段过程，首先会向其他的server 发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行

### zk 节点宕机如何处理

Zookeeper 本身也是集群，推荐配置不少于 3 个服务器。Zookeeper 自身也要保证当一个节点宕机时，其他节点会继续提供服务。

如果是一个 Follower 宕机，还有 2 台服务器提供访问，因为 Zookeeper 上的数据是有多个副本的，数据并不会丢失；
如果是一个 Leader 宕机，Zookeeper 会选举出新的 Leader。

ZK 集群的机制是只要超过半数的节点正常，集群就能正常提供服务。只有在 ZK节点挂得太多，只剩一半或不到一半节点能工作，集群才失效。

所以3 个节点的 cluster 可以挂掉 1 个节点(leader 可以得到 2 票>1.5)
2 个节点的 cluster 就不能挂掉任何 1 个节点了(leader 可以得到 1 票<=1)

### 分布式集群中为什么会有 Master

在分布式环境中，有些业务逻辑只需要集群中的某一台机器进行执行，其他的机器可以共享这个结果，这样可以大大减少重复计算，提高性能，于是就需要进行leader 选举

### Zookeeper 分布式锁

有了 zookeeper 的一致性文件系统，锁的问题变得容易。锁服务可以分为两类，一个是保持独占，另一个是控制时序 对于第一类，我们将 zookeeper 上的一个 znode 看作是一把锁，通过 createznode的方式来实现。所有客户端都去创建 /distribute_lock 节点，最终成功创建的那个客户端也即拥有了这把锁。用完删除掉自己创建的 distribute_lock 节点就释放出锁。

对于第二类， /distribute_lock 已经预先存在，所有客户端在它下面创建临时顺序编号目录节点，和选 master 一样，编号最小的获得锁，用完删除，依次方便

###### zk 基本锁 原理

利用临时节点与 watch 机制。每个锁占用一个普通节点 /lock，当需要获取锁时在 /lock 目录下创建一个临时节点，创建成功则表示获取锁成功，失败则 watch/lock 节点，有删除操作后再去争锁。临时节点好处在于当进程挂掉后能自动上锁的节点自动删除即取消锁。 缺点：所有取锁失败的进程都监听父节点，很容易发生羊群效应，即当释放锁后所有等待进程一起来创建节点，并发量很大

###### zk 锁优化 原理

上锁改为创建临时有序节点，每个上锁的节点均能创建节点成功，只是其序号不同。只有序号最小的可以拥有锁，如果这个节点序号不是最小的则 watch 序号比本身小的前一个节点 (公平锁)

1. 在 /lock 节点下创建一个有序临时节点 (EPHEMERAL_SEQUENTIAL)。
2. 判断创建的节点序号是否最小，如果是最小则获取锁成功。不是则取锁失败，然后 watch 序号比本身小的前一个节点。（避免很多线程watch同一个node，导致羊群效应）
3. 当取锁失败，设置 watch 后则等待 watch 事件到来后，再次判断是否序号最小。
4. 取锁成功则执行代码，最后释放锁（删除该节点）

### epoch

epoch：可以理解为当前集群所处的年代或者周期，每个 leader 就像皇帝，都有自己的年号，所以每次改朝换代， leader 变更之后，都会在前一个年代的基础上加 1。这样就算旧的 leader 崩溃恢复之后，也没有人听他的了，因为 follower 只听从当前年代的 leader 的命令

### 投票机制

​	每个 sever 首先给自己投票， 然后用自己的选票和其他 sever 选票对比， 权重大的胜出，使用权重较大的更新自身选票箱。 具体选举过程如下

1. 每个 Server 启动以后都询问其它的 Server 它要投票给谁。对于其他 server 的询问，server 每次根据自己的状态都回复自己推荐的 leader 的 id 和上一次处理事务的zxid（系统启动时每个 server 都会推荐自己）
2. 收到所有 Server 回复以后，就计算出 zxid 最大的哪个 Server，并将这个 Server 相关信息设置成下一次要投票的 Server。
3. 计算这过程中获得票数最多的的 sever 为获胜者，如果获胜者的票数超过半数，则改server 被选为 leader。否则，继续这个过程，直到 leader 被选举出来
4. leader 就会开始等待 server 连接
5. Follower 连接 leader，将最大的 zxid 发送给 leader
6. Leader 根据 follower 的 zxid 确定同步点，至此选举阶段完成。
7. 选举阶段完成 Leader 同步后通知 follower 已经成为 uptodate 状态
8. Follower 收到 uptodate 消息后，又可以重新接受 client 的请求进行服务

### Zookeeper 工作原理（原子广播）

1. Zookeeper 的核心是原子广播，这个机制保证了各个 server 之间的同步。实现这个机制的协议叫做 Zab 协议。 Zab 协议有两种模式，它们分别是恢复模式和广播模式。
2. 当服务启动或者在领导者崩溃后， Zab 就进入了恢复模式，当领导者被选举出来，且大多数 server 的完成了和 leader 的状态同步以后，恢复模式就结束了。
3. 状态同步保证了 leader 和 server 具有相同的系统状态
4. 一旦 leader 已经和多数的 follower 进行了状态同步后，他就可以开始广播消息了，即进入广播状态。这时候当一个 server 加入 zookeeper 服务中，它会在恢复模式下启
   动，发现 leader，并和 leader 进行状态同步。待到同步结束，它也参与消息广播。 Zookeeper服务一直维持在 Broadcast 状态，直到 leader 崩溃了或者 leader 失去了大
   部分的followers 支持。
5. 广播模式需要保证 proposal 被按顺序处理，因此 zk 采用了递增的事务 id 号(zxid)来保证。所有的提议(proposal)都在被提出的时候加上了 zxid。
6. 实现中 zxid 是一个 64 为的数字，它高 32 位是 epoch 用来标识 leader 关系是否改变，每次一个 leader 被选出来，它都会有一个新的 epoch。低 32 位是个递增计数。
7. 当 leader 崩溃或者 leader 失去大多数的 follower，这时候 zk 进入恢复模式，恢复模式需要重新选举出一个新的 leader，让所有的 server 都恢复到一个正确的状态

### Zookeeper 都有哪些功能

1. 集群管理：监控节点存活状态、运行请求等
2. 主节点选举：主节点挂掉了之后可以从备用的节点开始新一轮选主，主节点选举说的就是这个选举的过程，使用 Zookeeper 可以协助完成这个过程
3. 分布式锁：Zookeeper 提供两种锁：独占锁、共享锁。独占锁即一次只能有一个线程使用资源，共享锁是读锁共享，读写互斥，即可以有多线线程同时读同一个资源，如果要使用写锁也只能有一个线程使用。Zookeeper 可以对分布式锁进行控制
4. 命名服务：在分布式系统中，通过使用命名服务，客户端应用能够根据指定名字来获取资源或服务的地址，提供者等信息

### zookeeper 写数据流程

![image-20210812152705779](https://gitee.com/Sean0516/image/raw/master/img/image-20210812152705779.png)

# 