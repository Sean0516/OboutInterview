# Mybatis

### Mybatis 架构设计

![image-20211103165641735](https://gitee.com/Sean0516/image/raw/master/img/image-20211103165641735.png)



## mybatis 核心组件

### Configuration

用于描述mybatis  的主配置信息，其他组件需要获取配置信息时，直接通过configuration 对象获取，除此志位，mybatis  在应用启动时，将mapper 配置信息，类型别名，typeHandler 等注册到 configuration 组件中



### MappedStatement

MappedStatement 用于描述mapper 中的sql  配置信息，是对 mapper xml  配置文件中  <select delete update insert > 等标签或者 @Select 等注解配置信息的封装



### SqlSession 

Sql Session 是mybatis  提供的面向用户的API ，表示和数据库交互时的会话对象，用于完成数据库的增删改查功能， SqlSession 是Excecutor 组件的外观， 母的是对外提供易于理解和使用的数据库操作接口

### Excecutor 

Excecutor 是mybatis  的SQL 执行器。 mybatis 中对数据库所有的操作都是由Excecutor 组件完成的

mybatis 提供了三种不同的 Executor  

#### SimpleExecutor

最基础的Executor ,完成简单的增删改查方法

#### ResueExecutor （享元模式）

对jdbc 的statement 对象做缓存，当执行相同的SQL 语句时，直接从缓存中获取statement 对象进行复用，避免了频繁创建和销毁statement 对象。 从而提升系统性能

#### BatchExecutor

对调用同一个mapper 执行的 update  insert  delete 操作。 调用statement 对象的批量操作功能。

#### CachingExecutor  (装饰器模式)

当mybatis  开启二级缓存的时候，会使用 CachingExecutor 对 SimpleExecutor ResueExecutor BatchExecutor 进行装饰， 为查询操作增加二级缓存

### StatementHandler  

StatementHandler 封装了对JDBC statement 对象的操作。 比如对statement 对象设置参数，调用statement 接口提供方法与数据库交互。

SimpleStatementHandler继承至BaseStatementHandler，封装了对JDBCStatement对象的操作，PreparedStatementHandler封装了对JDBC PreparedStatement对象的操作，而CallableStatementHandler则封装了对JDBCCallableStatement对象的操作。RoutingStatementHandler会根据Mapper配置中的statementType属性（取值为STATEMENT、PREPARED或CALLABLE）创建对应的StatementHandler实现。

### ParameterHandler 

用于statement 对象参数占位符设置值

ParameterHandler的作用是在PreparedStatementHandler和CallableStatementHandler操作对应的Statement执行数据库交互之前为参数占位符设置值

MyBatis通过ParameterMapping描述参数映射的信息。在DefaultParameterHandler类的setParameters()方法中，首先获取Mapper配置中的参数映射，然后对所有参数映射信息进行遍历，接着根据参数名称获取对应的参数值，调用对应的TypeHandler对象的setParameter()方法为Statement对象中的参数占位符设置值

### ResultSetHandler 

ResultSetHandler 封装了对JDBC ResultSet 对象操作，当执行SQL 类型为select 语句时， ResultSetHandler 用于将查询结果转换为Java 对象



### TypeHandler 

TypeHandler 是mybatis  中的类型处理器 ，用于处理Java 类型与 jdbc 类型之间的映射。 他的主要作用体现在能根据Java 类型调用 preparestatement 或者 callable statement 对象对应的set 方法为statement 对象设置值， 而且能够根据 Java类型调用 result set 对象对应的get 方法获取sql  执行 结果

涉及Java类型和JDBC类型转换的两种情况如下：

1. PreparedStatement对象为参数占位符设置值时，需要调用PreparedStatement接口中提供的一系列的setXXX()方法，将Java类型转换为对应的JDBC类型并为参数占位符赋值
2. 执行SQL语句获取ResultSet对象后，需要调用ResultSet对象的getXXX()方法获取字段值，此时会将JDBC类型转换为Java类型



MyBatis通过TypeHandlerRegistry建立JDBC类型、Java类型与TypeHandler之间的映射关系

简单来说，SqlSession是Executor组件的外观，目的是为用户提供更友好的数据库操作接口，真正执行SQL操作的是Executor组件，Executor可以理解为SQL执行器，它会使用StatementHandler组件对JDBC的Statement对象进行操作。当Statement类型为CallableStatement和PreparedStatement时，会通过ParameterHandler组件为参数占位符赋值。ParameterHandler组件中会根据Java类型找到对应的TypeHandler对象，TypeHandler中会通过Statement对象提供的setXXX()方法（例如setString()方法）为Statement对象中的参数占位符设置值。StatementHandler组件使用JDBC中的Statement对象与数据库完成交互后，当SQL语句类型为SELECT时，MyBatis通过ResultSetHandler组件从Statement对象中获取ResultSet对象，然后将ResultSet对象转换为Java对象



### mybatis  的优缺点

###### 优点

1. 基于SQL语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL写在XML里，解除sql与程序代码的耦合，便于统一管理；提供XML标签，支持编写动态SQL语句，并可重用
2. 与JDBC相比，减少了50%以上的代码量，消除了JDBC大量冗余的代码，不需要手动开关连接
3. 很好的与各种数据库兼容（因为MyBatis使用JDBC来连接数据库，所以只要JDBC支持的数据库MyBatis都支持
4. 能够与Spring很好的集成
5. 提供映射标签，支持对象与数据库的ORM字段关系映射；提供对象关系映射标签，支持对象关系组件维护

###### 缺点

1. SQL语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写SQL语句的功底有一定要求
2. SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库

### mybatis  操作数据库流程

1. 获取配置文件数据流
2. 通过SqlSessionFactoryBuilder 的 build 方法创建一个SqlSessionFactory 实例。 
3. 调用openSession 方法创建SqlSession 实例
4. 通过SqlSession 实例获取Mapper 代理对象 （动态代理对象）
5. 执行Mapper 方法 ，获取执行结果
6. 调用session.close()关闭会话

### MyBatis的工作原理

    1. 读取 MyBatis 配置文件：mybatis-config.xml 为 MyBatis 的全局配置文件，配置了 MyBatis 的运行环境等信息，例如数据库连接信息
    2. 加载映射文件。映射文件即 SQL 映射文件，该文件中配置了操作数据库的 SQL 语句，需要在MyBatis 配置文件 mybatis-config.xml 中加载。mybatis-config.xml 文件可以加载多个映射文件，每个文件对应数据库中的一张表
    3. 构造会话工厂：通过 MyBatis 的环境等配置信息构建会话工厂 SqlSessionFactory
    4. 创建会话对象：由会话工厂创建 SqlSession 对象，该对象中包含了执行 SQL 语句的所有方法
    5. Executor 执行器：MyBatis 底层定义了一个 Executor 接口来操作数据库，它将根据 SqlSession 传递的参数动态地生成需要执行的 SQL 语句，同时负责查询缓存的维护
    6. MappedStatement 对象：在 Executor 接口的执行方法中有一个 MappedStatement 类型的参数，该参数是对映射信息的封装，用于存储要映射的 SQL 语句的 id、参数等信息
    7. 输入参数映射：输入参数类型可以是 Map、List 等集合类型，也可以是基本数据类型和 POJO 类型。输入参数映射过程类似于 JDBC 对 preparedStatement 对象设置参数的过程
    8. 输出结果映射：输出结果类型可以是 Map、 List 等集合类型，也可以是基本数据类型和 POJO 类型。输出结果映射过程类似于 JDBC 对结果集的解析过程





### Mybatis 的 Xml 映射文件中，不同的 Xml 映射文件，id 是否可以重复

不同的 Xml 映射文件，如果配置了 namespace，那么 id 可以重复；如果没有配置 namespace，那么 id 不能重复；原因就是 namespace+id 是作为 Map<String, MapperStatement>的 key使用的，如果没有 namespace，就剩下 id，那么，id 重复会导致数据互相覆盖。了 namespace，自然 id 就可以重复，namespace 不同，namespace+id 自然也就不同

### MyBatis 实现一对一有几种方式

有联合查询和嵌套查询,联合查询是几个表联合查询,只查询一次, 通过在resultMap 里面配置 association 节点配置一对一的类就可以完成

### MyBatis 实现一对多有几种方式

有联合查询和嵌套查询。联合查询是几个表联合查询,只查询一次,通过在resultMap 里面的 collection 节点配置一对多的类就可以完成

### Mybatis 是否支持延迟加载

Mybatis 仅支持 association 关联对象和 collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的就是一对多查询。在 Mybatis配置文件中，可以配置是否启用延迟加载 lazyLoadingEnabled=true|false

### #{}和${}的区别是什么

#{}是预编译处理，${}是字符串替换。
Mybatis 在处理#{}时，会将 sql 中的#{}替换为?号，调用 PreparedStatement 的 set方法来赋值；
Mybatis 在处理${}时，就是把${}替换成变量的值。使用#{}可以有效的防止 SQL 注入，提高系统安全性

### Mybatis 是如何进行分页的？分页插件的原理是什么

Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页，可以在 sql 内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。
分页插件的基本原理是使用 Mybatis 提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的 sql，然后重写 sql，根据 dialect 方言，添加对应的物理分页语句和物理分页参数



### 什么是 MyBatis 的接口绑定？有哪些实现方式

接口绑定，就是在 MyBatis 中任意定义接口,然后把接口里面的方法和 SQL 语句绑定, 我们直接调用接口方法就可以,这样比起原来了 SqlSession 提供的方法我们可以有更加灵活的选择和设置。

接口绑定有两种实现方式

一种是通过注解绑定，就是在接口的方法上面加上@Select、@Update 等注解，里面包含 Sql 语句来绑定；

另外一种就是通过 xml里面写 SQL 来绑定,在这种情况下,要指定 xml 映射文件里面的 namespace 必须为接口的全路径名。当 Sql 语句比较简单时候,用注解绑定, 当 SQL 语句比较复杂时候,用 xml 绑定,一般用 xml 绑定的比较多



### Mybatis都有哪些Executor执行器？它们之间的区别是什么

Mybatis有三种基本的Executor执行器，SimpleExecutor、ReuseExecutor、BatchExecutor、

- SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象
- ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map<String, Statement>内，供下一次使用。简言之，就是重复使用Statement对象
- BatchExecutor：执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同



### 在mapper中如何传递多个参数

1. 顺序传参法  where user_name = #{0} and dept_id = #{1}     #{}里面的数字代表传入参数的顺序  这种方法不建议使用，sql层表达不直观，且一旦顺序调整容易出错。
2. @Param注解传参法  @Param("userName") String name   where user_name = #{userName}  #{}里面的名称对应的是注解@Param括号里面修饰的名称  这种方法在参数不多的情况还是比较直观的，（推荐使用）
3. Map传参法  
4. Java Bean传参法  #{}里面的名称对应的是User类里面的成员属性



### Mybatis  缓存

Mybatis中有一级缓存和二级缓存，默认情况下一级缓存是开启的，而且是不能关闭的。一级缓存是指 SqlSession 级别的缓存，当在同一个 SqlSession 中进行相同的 SQL 语句查询时，第二次以后的查询不会从数据库查询，而是直接从缓存中获取，一级缓存最多缓存 1024 条 SQL。二级缓存是指可以跨SqlSession 的缓存。是 mapper 级别的缓存，对于 mapper 级别的缓存不同的sqlsession 是可以共享的

- ​	Mybatis 的一级缓存原理 的一级缓存原理 （ sqlsession 级别 ）

  第一次发出一个查询 sql，sql 查询结果写入 sqlsession 的一级缓存中，缓存使用的数据结构是一
  个 map。
  key：MapperID+offset+limit+Sql+所有的入参

  value：用户信息

  同一个 sqlsession 再次发出相同的 sql，就从缓存中取出数据。如果两次中间出现 commit 操作（修改、添加、删除），本 sqlsession 中的一级缓存区域全部清空，下次再去缓存中查询不到所以要从数据库查询，从数据库查询到再写入缓存

- 二级缓存原理 二级缓存原理 （ mapper 级别）

  二级缓存的范围是 mapper 级别（mapper同一个命名空间），mapper 以命名空间为单位创建缓存数据结构，结构是 map。mybatis 的二级缓存是通过 CacheExecutor 实现的。CacheExecutor   其实是 Executor 的代理对象。所有的查询操作，在 CacheExecutor 中都会先匹配缓存中是否存
  在，不存在则查询数据库

  key：MapperID+offset+limit+Sql+所有的入参

  具体使用需要配置

  1. Mybatis 全局配置中启用二级缓存配置
  2. 在对应的 Mapper.xml 中配置 cache 节点
  3. 在对应的 select 查询节点中添加 useCache=true

