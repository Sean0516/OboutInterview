## MySql 构成

### MySQL由哪些部分组成, 分别用来做什么

###### Server 

- 连接器: 管理连接, 权限验证.
- 分析器: 词法分析, 语法分析.
- 优化器: 执行计划生成, 索引的选择.
- 执行器: 操作存储引擎, 返回执行结果

###### 存储引擎

存储数据, 提供读写接口

### MySQL 的引擎

#### InnoDB（B+树）

InnoDB 底层存储结构为B+树， B树的每个节点对应innodb的一个page， page大小是固定的，一般设为 16k。其中非叶子节点只有键值，
叶子节点包含完成数据

适用场景：
1）经常更新的表，适合处理多重并发的更新请求。
2）支持事务。
3）可以从灾难中恢复（通过 bin-log 日志等）。
4）外键约束。只有他支持外键。
5）支持自动增加列属性 auto_increment

#### MyIASM

MyIASM是 MySQL默认的引擎，但是它没有提供对数据库事务的支持，也不支持行级锁和外键，因此当 NSERT(插入)或 UPDATE(更新)数据
时即写操作需要锁定整个表，效率便会低一些。

ISAM 执行读取操作的速度很快，而且不占用大量的内存和存储资源。在设计之初就预想数据组织成有固定长度的记录，按顺序存储的。 ---
ISAM 是一种静态索引结构。 缺点是它不 支持事务处理。

#### InnoDB与MyISAM的区别

1. InnoDB支持事务，MyISAM不支持，对于InnoDB每一条SQL语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条
   SQL语言放在begin和commit之间，组成一个事务；
2. InnoDB支持外键，而MyISAM不支持。对一个包含外键的InnoDB表转为MYISAM会失败；
3. InnoDB是聚集索引，数据文件是和索引绑在一起的，必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到
   主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。而MyISAM是非聚集索引，数据文
   件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
4. InnoDB不保存表的具体行数，执行select count(*) from table时需要全表扫描。而MyISAM用一个变量保存了整个表的行数，执行上
   述语句时只需要读出该变量即可，速度很快；
5. Innodb不支持全文索引，而MyISAM支持全文索引，查询效率上MyISAM要高

### Explain 执行计划

使用 EXPLAIN 关键字可以模拟优化器执行 SQL 查询语句，从而知道 MySQL 是如何处理SQL 语句的。分析查询语句或是表结构的性能瓶颈

#### 执行计划中包含的信息

**id**

select查询的序列号，包含一组数字，表示查询中执行select子句或者操作表的顺序

id号分为三种情况：

​	1、如果id相同，那么执行顺序从上到下

```
explain select * from emp e join dept d on e.deptno = d.deptno join salgrade sg on e.sal between sg.losal and sg.hisal;
```

​	2、如果id不同，如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行

```
explain select * from emp e where e.deptno in (select d.deptno from dept d where d.dname = 'SALES');
```

​	3、id相同和不同的，同时存在：相同的可以认为是一组，从上往下顺序执行，在所有组中，id值越大，优先级越高，越先执行

```
explain select * from emp e join dept d on e.deptno = d.deptno join salgrade sg on e.sal between sg.losal and sg.hisal where e.deptno in (select d.deptno from dept d where d.dname = 'SALES');
```

**select_type**

主要用来分辨查询的类型，是普通查询还是联合查询还是子查询

| `select_type` Value  |                           Meaning                            |
| :------------------: | :----------------------------------------------------------: |
|        SIMPLE        |        Simple SELECT (not using UNION or subqueries)         |
|       PRIMARY        |                       Outermost SELECT                       |
|        UNION         |         Second or later SELECT statement in a UNION          |
|   DEPENDENT UNION    | Second or later SELECT statement in a UNION, dependent on outer query |
|     UNION RESULT     |                      Result of a UNION.                      |
|       SUBQUERY       |                   First SELECT in subquery                   |
|  DEPENDENT SUBQUERY  |      First SELECT in subquery, dependent on outer query      |
|       DERIVED        |                        Derived table                         |
| UNCACHEABLE SUBQUERY | A subquery for which the result cannot be cached and must be re-evaluated for each row of the outer query |
|  UNCACHEABLE UNION   | The second or later select in a UNION that belongs to an uncacheable subquery (see UNCACHEABLE SUBQUERY) |

```
--sample:简单的查询，不包含子查询和union
explain select * from emp;

--primary:查询中若包含任何复杂的子查询，最外层查询则被标记为Primary
explain select staname,ename supname from (select ename staname,mgr from emp) t join emp on t.mgr=emp.empno ;

--union:若第二个select出现在union之后，则被标记为union
explain select * from emp where deptno = 10 union select * from emp where sal >2000;

--dependent union:跟union类似，此处的depentent表示union或union all联合而成的结果会受外部表影响
explain select * from emp e where e.empno  in ( select empno from emp where deptno = 10 union select empno from emp where sal >2000)

--union result:从union表获取结果的select
explain select * from emp where deptno = 10 union select * from emp where sal >2000;

--subquery:在select或者where列表中包含子查询
explain select * from emp where sal > (select avg(sal) from emp) ;

--dependent subquery:subquery的子查询要受到外部表查询的影响
explain select * from emp e where e.deptno in (select distinct deptno from dept);

--DERIVED: from子句中出现的子查询，也叫做派生类，
explain select staname,ename supname from (select ename staname,mgr from emp) t join emp on t.mgr=emp.empno ;

--UNCACHEABLE SUBQUERY：表示使用子查询的结果不能被缓存
 explain select * from emp where empno = (select empno from emp where deptno=@@sort_buffer_size);
 
--uncacheable union:表示union的查询结果不能被缓存：sql语句未验证
```

**table**

对应行正在访问哪一个表，表名或者别名，可能是临时表或者union合并结果集 1、如果是具体的表名，则表明从实际的物理表中获取数据，当然也可以是表的别名

​	2、表名是derivedN的形式，表示使用了id为N的查询产生的衍生表

​	3、当有union result的时候，表名是union n1,n2等的形式，n1,n2表示参与union的id

**type**

type显示的是访问类型，访问类型表示我是以何种方式去访问我们的数据，最容易想的是全表扫描，直接暴力的遍历一张表去寻找需要的数据，效率非常低下，访问的类型有很多，效率从最好到最坏依次是：

system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL

一般情况下，得保证查询至少达到range级别，最好能达到ref

```
--all:全表扫描，一般情况下出现这样的sql语句而且数据量比较大的话那么就需要进行优化。
explain select * from emp;

--index：全索引扫描这个比all的效率要好，主要有两种情况，一种是当前的查询时覆盖索引，即我们需要的数据在索引中就可以索取，或者是使用了索引进行排序，这样就避免数据的重排序
explain  select empno from emp;

--range：表示利用索引查询的时候限制了范围，在指定范围内进行查询，这样避免了index的全索引扫描，适用的操作符： =, <>, >, >=, <, <=, IS NULL, BETWEEN, LIKE, or IN() 
explain select * from emp where empno between 7000 and 7500;

--index_subquery：利用索引来关联子查询，不再扫描全表
explain select * from emp where emp.job in (select job from t_job);

--unique_subquery:该连接类型类似与index_subquery,使用的是唯一索引
 explain select * from emp e where e.deptno in (select distinct deptno from dept);
 
--index_merge：在查询过程中需要多个索引组合使用，没有模拟出来

--ref_or_null：对于某个字段即需要关联条件，也需要null值的情况下，查询优化器会选择这种访问方式
explain select * from emp e where  e.mgr is null or e.mgr=7369;

--ref：使用了非唯一性索引进行数据的查找
 create index idx_3 on emp(deptno);
 explain select * from emp e,dept d where e.deptno =d.deptno;

--eq_ref ：使用唯一性索引进行数据查找
explain select * from emp,emp2 where emp.empno = emp2.empno;

--const：这个表至多有一个匹配行，
explain select * from emp where empno = 7369;
 
--system：表只有一行记录（等于系统表），这是const类型的特例，平时不会出现
```

**possible_keys**

 显示可能应用在这张表中的索引，一个或多个，查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询实际使用

```
explain select * from emp,dept where emp.deptno = dept.deptno and emp.deptno = 10;
```

**key**

​	实际使用的索引，如果为null，则没有使用索引，查询中若使用了覆盖索引，则该索引和查询的select字段重叠。

```
explain select * from emp,dept where emp.deptno = dept.deptno and emp.deptno = 10;
```

**key_len**

表示索引中使用的字节数，可以通过key_len计算查询中使用的索引长度，在不损失精度的情况下长度越短越好。

```
explain select * from emp,dept where emp.deptno = dept.deptno and emp.deptno = 10;
```

**ref**

显示索引的哪一列被使用了，如果可能的话，是一个常数

```
explain select * from emp,dept where emp.deptno = dept.deptno and emp.deptno = 10;
```

**rows**

根据表的统计信息及索引使用情况，大致估算出找出所需记录需要读取的行数，此参数很重要，直接反应的sql找了多少数据，在完成目的的情况下越少越好

```
explain select * from emp;
```

**extra**

包含额外的信息。

```
--using filesort:说明mysql无法利用索引进行排序，只能利用排序算法进行排序，会消耗额外的位置
explain select * from emp order by sal;

--using temporary:建立临时表来保存中间结果，查询完成之后把临时表删除
explain select ename,count(*) from emp where deptno = 10 group by ename;

--using index:这个表示当前的查询时覆盖索引的，直接从索引中读取数据，而不用访问数据表。如果同时出现using where 表名索引被用来执行索引键值的查找，如果没有，表面索引被用来读取数据，而不是真的查找
explain select deptno,count(*) from emp group by deptno limit 10;

--using where:使用where进行条件过滤
explain select * from t_user where id = 1;

--using join buffer:使用连接缓存，情况没有模拟出来

--impossible where：where语句的结果总是false
explain select * from emp where empno = 7469;
```



## 索引

索引（Index）是帮助 MySQL 高效获取数据的数据结构。 常见的查询算法,顺序查找,二分查找,二叉排序树查找,哈希散列法,分块查找,平衡多路搜索树 B 树（B-tree） ，索引是对数据库表中一个或多个列的值进行排序的结构，建立索引有助于快速获取信息

mysql 有4种不同的索引：
主键索引（PRIMARY）
唯一索引（UNIQUE）
普通索引（INDEX）
全文索引（FULLTEXT）

1. 

### 为什么 B+Tree 做索引

1. B+Tree 的磁盘读写代价更低

   B+Tree 的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对 B-Tree 更小。如果把所有同一内部结点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的关键字也就越多。相对来说 IO 读写次数也就降低了

2. B+ 树的查询效率更加稳定 ，任何关键字的查找都必须走一条从根节点到叶子节点的录，所以关键字查询的路径长度相同，每个数据的查询效率相当

3.  B+ 树只需要遍历叶子节点就可以实现整颗树的遍历。  

   #### hash 表

   同时，如果考虑使用hash 表做索引的话， 哈希表是把索引字段映射成对应的哈希码然后再存放在对应的位置，这样的话，如果我们要进行模糊查找的话，显然哈希表这种结构是不支持的，只能遍历这个表。而B+树则可以通过最左前缀原则快速找到对应的数据 ，如果我们要进行范围查找，例如查找ID为100 ~ 400的人，哈希表同样不支持，只能遍历全表 ， 索引字段通过哈希映射成哈希码，如果很多字段都刚好映射到相同值的哈希码的话，那么形成的索引结构将会是一条很长的链表，这样的话，查找的时间就会大大增加

   #### B- Tree

   B-Tree 的关键字和记录是放在一起的，叶子节点可以看作外部节点，不包含任何信息； 

   B+Tree 的非叶子节点不存放实际的数据，这样每个节点可容纳的元素个数比 B-Tree 多，树高比 B-Tree 小，这样带来的好处是减少磁盘访问次数。尽管 B+Tree 找到一个记录所需的比较次数要比 B-Tree 多，但是一次磁盘访问的时间相当于成百上千次内存比较的时间，因此实际中 B+Tree 的性能可能还会好些，而且 B+Tree 的叶子节点使用指针连接在一起，方便顺序遍历（例如查看一个目录下的所有文件，一个表中的所有记录等），这也是很多数据库和文件系统使用 B+Tree 的缘故



### 联合索引是什么?为什么需要注意联合索引中的顺序

MySQL可以使用多个字段同时建立一个索引,叫做联合索引.在联合索引中,如果想要命中索引,需要按照建立索引时的字段顺序挨个使用,否则无法命中索引

具体原因为:

MySQL使用索引时需要索引有序,假设现在建立了"name,age,school"的联合索引,那么索引的排序为: 先按照name排序,如果name相同,则按照age排序,如果age的值也相等,则按照school进行排序

当进行查询时,此时索引仅仅按照name严格有序,因此必须首先使用name字段进行等值查询,之后对于匹配到的列而言,其按照age字段严格有序,此时可以使用age字段用做索引查找,,,以此类推.因此在建立联合索引的时候应该注意索引列的顺序,一般情况下,将查询需求频繁或者字段选择性高的列放在前面.此外可以根据特例的查询或者表结构进行单独的调整.

### 那么在哪些情况下会发生针对该列创建了索引但是在查询的时候并没有使用呢

1. 使用不等于查询
2. 列参与了数学运算或者函数
3. 在字符串like时左边是通配符.类似于'%aaa'
4. 当mysql分析全表扫描比使用索引快的时候不使用索引.
5. 当使用联合索引,前面一个条件为范围查询,后面的即使符合最左前缀原则,也无法使用索引
6. OR 语句前后没有同时使用索引
7. 数据类型出现隐式转化（如 varchar 不加单引号的话可能会自动转换为 int 型）

### 索引的目的是什么

快速访问数据表中的特定信息，提高检索速度
创建唯一性索引，保证数据库表中每一行数据的唯一性。
加速表和表之间的连接
使用分组和排序子句进行数据检索时，可以显著减少查询中分组和排序的时间

### 索引对数据库系统的负面影响是什么

创建索引和维护索引需要耗费时间，这个时间随着数据量的增加而增加；索引需要占用物理空间，不光是表需要占用数据空间，每个索引也
需要占用物理空间；当对表进行增、删、改、的时候索引也要动态维护，这样就降低了数据的维护速度

### 建立索引的原则

在最频繁使用的、用以缩小查询范围的字段上建立索引。
在频繁使用的、需要排序的字段上建立索引



### 索引的分类

1. 普通索引
2. 唯一索引
3. 主键索引
4. 组合索引
5. 全文索引

### 主键、外键和索引的区别

###### 定义 ：

主键–唯一标识一条记录，不能有重复的，不允许为空
外键–表的外键是另一表的主键, 外键可以有重复的, 可以是空值
索引–该字段没有重复值，但可以有一个空值

###### 作用：

主键–用来保证数据完整性
外键–用来和其他表建立联系用的
索引–是提高查询排序的速度

###### 个数：

主键–主键只能有一个
外键–一个表可以有多个外键
索引–一个表可以有多个唯一索引

### 索引的优缺点

优点：

1. 索引加快数据库的检索速度
2. 唯一索引可以确保每一行数据的唯一性
3. 通过使用索引，可以在查询的过程中使用优化隐藏器，提高系统的性能

缺点：

1. 索引降低了插入、删除、修改等维护任务的速度
2. 增加了数据库的存储空间

### 常见索引原则

1. 选择唯一性索引，唯一性索引的值是唯一的，可以更快速的通过该索引来确定某条记录。
2. 为经常需要排序、分组和联合操作的字段建立索引。
3. 为常用作为查询条件的字段建立索引。
4. 限制索引的数目：越多的索引，会使更新表变得很浪费时间。尽量使用数据量少的索引
5. 如果索引的值很长，那么查询的速度会受到影响。尽量使用前缀来索引
6. 删除不再使用或者很少使用的索引
7. 最左前缀匹配原则，非常重要的原则。
8. 尽量选择区分度高的列作为索引区分度的公式是表示字段不重复的比例
9. 索引列不能参与计算，保持列“干净”：带函数的查询不参与索引。
10. 尽量的扩展索引，不要新建索引

### 聚簇索引和非聚簇索引

 聚簇索引表示数据和索引放一起 （主键ID ） 非聚簇索引表示数据和索引分开存储 （非主键ID 保存 ID 的key） 因此InnDb 有聚簇索引 和非聚簇索引  而MYSN 只有非聚簇索引

1. 一个表可以创建多少个索引（理论上无限制）
2. 每个索引一颗B+ 树
3. 如果有多个索引， 数据存储几份 一份
4. 如果存储一份 其他索引的叶子节点放什么 ？ inndb 存储引擎在进行数据插入的时候，数据必须要和某个索引列绑定存储。如果有主键选择主键，有唯一键选择唯一键。 也就是说，数据和其一个索引放一起。其他的索引树的叶子节点存储的是数据聚集存储索引列的key 值

### 什么是回表

当存在 id name age 三个列字段， id 为主键 Name 为普通索引 select * from table where name ='' 查找过程 先根据name 值去查找叶子节点上的数据，找到主键id 的值，在根据ID 的值，去ID B+ 树，查找叶子节点上的全量数据 。 这个过程叫做回表 效率低，不推荐

### 什么是索引覆盖索引覆盖

当存在 id name age 三个列字段，  id 为主键 Name 为普通索引 。 使用命令 select id , name from table where name ='' 查找过程 先根据name 值去查找叶子节点上的数据 ,叶子节点包含的数据满足我们查询列需要 （name 索引中存放这 id 主键的信息），此时不需要回表，这个过程叫做索引覆盖 效率高 。 索引的叶子节点包含要查询的全部列数据的时候会使用索引覆盖 



### 最左匹配原则：

 针对组合索引 在进行值匹配的时候，必须要先匹配左边的字段才能匹配到右边的字段。 需要注意的是，如果值包含所有的索引列，但是顺序一致，同样会匹配索引，因为SQL 优化器会将SQL 顺序进行优化

### SQL优化

1. 查询语句中不要使用select *
2. 尽量减少子查询，使用关联查询（left join,right join,inner join）替代
3. 减少使用IN或者NOT IN ,使用exists，not exists或者关联查询语句替代
4. or 的查询尽量用 union或者union all 代替(在确认没有重复数据或者不用剔除重复数据时，union all会更好)
5. 应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。
6. 应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如： select id from t where num is null 可以在num上设置默认值0，确保表中num列没有null值，然后这样查询： select id from t where num=0

### 索引优化案例：

通过查看执行计划后，发现使用了索引列之后，效率低， 通过判断SQL 语句，发现要查询的列基本都包含在条件之后，但是之前创建的索引都是单列索引。因此创建了联合索引，避免了回表的产生。 执行之后，效率提高



## MySQL 分布式集群

### MySQL的主从复制原理以及流程

基本原理流程，3个线程以及之间的关联

1. 主：binlog 线程——记录下所有改变了数据库数据的语句，放进master上的binlog中
2. 从：io thread 线程——在使用start slave 之后，负责从master上拉取 binlog 内容，放进 自己的relay log （中继日志）中
3. 从：sql thread 执行线程——读取relay log ，执行 relay log 中的语句

### 用于解决组从复制问题 MTS （组提交）



### MySQL  读写分离 

Sharding Sphere  





### 优化数据库的方法

1. 选取最适用的字段属性，尽可能减少定义字段宽度，尽量把字段设置 NOTNULL，例如’省份’、’性别’最好适用 ENUM
2. 使用连接(JOIN)来代替子查询
3. 适用联合(UNION)来代替手动创建的临时表
4. 事务处理
5. 锁定表、优化事务处理
6. 适用外键，优化锁定表
7. 建立索引
8. 优化查询语句

## 事务

### Innodb 是如何实现事务的

Innodb 通过Buffer Pool LogBuffer  Redo Log Undo Log 来实现事务

1. innodb  在收到一个update 语句后，会先根据条件找到数据所在的页，并将该页缓存当buffer pool 中
2. 执行update 语句，修改 buffer pool  中的数据，也就是内存中的数据
3. 执行update语句生成一个RedoLog 对象，并存放在LogBuffer 中
4. 针对update 语句生成的undolog 日志 ，用于事务回滚
5. 如果事务提交，那么则把redo log 对象进行持久化，后续还有其他机制将buffer  pool 中所修改的数据页持久化到磁盘中
6. 如果事务回滚，则使用undo log 日志进行回滚



### ACID是什么?可以详细说一下吗

A=Atomicity   原子性：就是上面说的,要么全部成功,要么全部失败.不可能只执行一部分操作 

#### 原子性的实现原理 (undo log 回滚日志)

undo log 是为了实现事务的原子性， 在操作任何数据之前，首先将数据备份到一个地方（这个存储数据备份的地方成为 undo log ） 然后进行数据的修改，如果出现了错误或者用户执行了 roll back 语句 ，系统可以利用 undo log 的备份 将数据恢复到事务开始之前的状态

undo log 是逻辑日志，可以用以下理解

1. 当 delete 一条记录的时候， undo log 中会记录一条对应的insert 记录
2. 当insert 一条记录时， undo log  会记录一条对应的delete 记录
3. 当update 一条记录时，它记录一条对应相反的update 记录

undo log 会形成一个链表，链首存储的是最新的旧数据，链尾存放的是最旧的旧记录， undolog 不会无线膨胀下去，会存在一个后台线程，purge 线程，当发现当前记录不需要回滚且不需要参与MVCC 的时候，就会把数据清理掉



C=Consistency  一致性：系统(数据库)总是从一个一致性的状态转移到另一个一致性的状态,不会存在中间状态

### 一致性

一致性 由原子性 隔离性和持久性来实现





Isolation   隔离性: 通常来说:一个事务在完全提交之前,对其他事务是不可见的.注意前面的通常来说加了红色,意味着有例外情况.

#### 隔离性的实现原理（MVCC）



D=Durability   持久性：一旦事务提交,那么就永远是这样子了,哪怕系统崩溃也不会影响到这个事务的结果

#### 持久性的实现原理 （Re do log  WAL （预写日志））

实际的数据没写成功，但是只要日志存在，就可以根据日志来恢复数据 



### MVCC

多版本并发控制，解决数据并发读写问题

1. 快照读  -----------读取的是mysql 对应数据库的历史版本 （select）
2. 当前读------------ 读取的最新的数据结果          
   1. select lock in share  mode  
   2.  select for  update
   3. insert delete update 

MVCC 多版本并发控制指的是一个数据的多个版本， 使得读写操作没有冲突，快照读是mysql 为了实现MVCC 的一个非阻塞读功能

![image-20211111181754474](https://gitee.com/Sean0516/image/raw/master/img/image-20211111181754474.png)

#### 实现原理

##### 隐藏字段

	1. DB_TRX_ID 最近修改事务ID ,记录创建当前记录或者最后一个修改的事务id
	2. DB_ROLL_PTR   回滚指针  ，指向这条记录的上一个版本
	3. DB_ROW_ID    隐藏主键

### readview 

当事务进行快照读的时候，会生成一个视图来进行可见性判断，可见性判断由可见性算法来确定的 

read view 会有以下几个字段

1.  trx_list  当前系统活跃的事务id
2. Up_limit_id  活跃事务列表中最小的id 值
3. low_limit_id  当前系统尚未分配的下一个事务id 

#### 可见性算法

1. 首先比较DB_TRX_ID < up_limit_id  ，如果小于，则当前事务可以看到DB_TRX_ID  所在的记录，如果大于等于，则进入下一个判断
2. 接下来 判断DB_TRX_ID >= low_limit_id  ，如果大于等于则代表DB_TRX_ID  所在记录在 Read_view 生成后才出现，那么对于当前事务肯定是不可见，如果小于，则进入下一步判断
3. 判断 DB_TRX_ID 是否在活跃事务中，如果在，则代表 在Read_View 生成时刻，这个事务还是活跃状态，还没有commit  ，修改的数据，当前事务也是看不到的。 如果不在，则说明这个事务在 Read View 生成之前就已经开始commit ，那么修改的结果是能够看到的。

能否看到修改的数据，取决于可见性算法，可见性算法比较的时候，取决于read view 中的结果值，

因为在不同的隔离级别时， 生成read view 的时机是不同的。 

RC 每次执行快照读，都会生成新的read view

RR 只有在当前事务第一次进行快照读的时候生成read view 之后的快照读操作都会用当前的 read view 

### 幻读问题

如果当前的所有操作都是当前读，那么是不会产生幻读问题，只有当前读和快照读一起使用的时候才会产生幻读问题 

对于幻读的解决，可以采用加锁的方式解决



### MySQL的事务隔离级别

###### 未提交读(READ UNCOMMITTED)

这个隔离级别下,其他事务可以看到本事务没有提交的部分修改.因此会造成脏读的问题(读取到了其他事务未提交的部分,而之后该事务进行了回滚).

这个级别的性能没有足够大的优势,但是又有很多的问题,因此很少使用

###### 已提交读(READ COMMITTED) RC 

其他事务只能读取到本事务已经提交的部分.这个隔离级别有 不可重复读的问题,在同一个事务内的两次读取,拿到的结果竟然不一样,因为另外一个事务对数据进行了修改

###### REPEATABLE READ(可重复读) RR 

可重复读隔离级别解决了上面不可重复读的问题(看名字也知道),但是仍然有一个新问题,就是 幻读,当你读取id> 10 的数据行时,对涉及到的所有行加上了读锁,此时例外一个事务新插入了一条id=11的数据,因为是新插入的,所以不会触发上面的锁的排斥,那么进行本事务进行下一次的查询时会发现有一条id=11的数据,而上次的查询操作并没有获取到,再进行插入就会有主键冲突的问题.

###### SERIALIZABLE(可串行化)

这是最高的隔离级别,可以解决上面提到的所有问题,因为他强制将所以的操作串行执行,这会导致并发性能极速下降,因此也不是很常用

InnoDB默认使用的是可重复读隔离级别 RR Oracle 使用RC

隔离性越高，效率越低





### 并发事务带来哪些问题

###### 脏读（Dirty read）:

 当一个事务正在访问数据并且对数据进行了修改，而这种修改还没有提交到数据库中，这时另外一个事务也访问了这
个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是“脏数据”，依据“脏数据”所做的操
作可能是不正确的。

###### 丢失修改（Lost to modify）: 

指在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，第
二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修。 例如：事务1读取某表中的数据A=20，事务2也读
取A=20，事务1修改A=A-1，事务2也修改A=A-1，最终结果A=19，事务1的修改被丢失。

###### 不可重复读（Unrepeatableread）: 

指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第
一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的
数据是不一样的情况，因此称为不可重复读。

###### 幻读（Phantom read）: 

幻读与不可重复读类似。它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）插入了一些数据
时。在随后的查询中，第一个事务（T1）就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

###### 不可重复读和幻读区别：

不可重复读的重点是修改比如多次读取一条记录发现其中某些列的值被修改，幻读的重点在于新增或者删除比如多次读取一条记录发现记录
增多或减少了



### 数据更新的流程

1. 执行器先从引擎中找到数据，如果在内存中直接返回，如果不在内存中，查询后返回
2. 执行器拿到数据之后，会先修改数据，然后调用引擎接口重新吸入数据
3. 引擎将数据更新到内存，同时写数据到 re do log 中， 此时处于 prepare 阶段，并通过执行器执行完成。 随时可以操作
4. 执行器生成这个操作的 bin log
5. 执行器调用引擎的事务提交接口，引擎把刚刚写完的redo log 改为 commit 状态。 更新完成



### 什么是存储过程？有哪些优缺点

存储过程是一些预编译的SQL语句

1. 更加直白的理解：存储过程可以说是一个记录集，它是由一些T-SQL语句组成的代码块，这些T-SQL语句代码像一个方法一样实现一些功能（对单表或多表的增删改查），然后再给这个代码块取一个名字，在用到这个功能的时候调用他就行了
2. 存储过程是一个预编译的代码块，执行效率比较高,一个存储过程替代大量T_SQL语句 ，可以降低网络通信量，提高通信速率,可以一定程度上确保数据安全

### 说一说三个范式

###### 第一范式

每个列都不可以再拆分

###### 第二范式

非主键列完全依赖于主键,而不能是依赖于主键的一部分

###### 第三范式

非主键列只依赖于主键,不依赖于其他非主键.



### 什么是视图

视图是一种虚拟的表，具有和物理表相同的功能。可以对视图进行增，改，查，操作，视图通常是有一个表或者多个表的行或列的子集。对
视图的修改不影响基本表。它使得我们获取数据更容易，相比多表查

### 什么是内联接、左外联接、右外联接

内联接（Inner Join）：匹配2张表中相关联的记录。
左外联接（Left Outer Join）：除了匹配2张表中相关联的记录外，还会匹配左表中剩余的记录，右表中未匹配到的字段用NULL表示。
右外联接（Right Outer Join）：除了匹配2张表中相关联的记录外，还会匹配右表中剩余的记录，左表中未匹配到的字段用NULL表示。在
判定左表和右表时，要根据表名出现在Outer Join的左右位置关系



### 大表如何优化

###### 限定数据的范围

务必禁止不带任何限制数据范围条件的查询语句。比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内；

###### 读/写分离

经典的数据库拆分方案，主库负责写，从库负责读；

###### 垂直分区

根据数据库里面数据表的相关性进行拆分。 例如，用户表中既有用户的登录信息又有用户的基本信息，可以将用户表拆分成两个单独的
表，甚至放到单独的库做分库。

### 存储过程优化思路

1. 尽量利用一些 sql 语句来替代一些小循环，例如聚合函数，求平均函数等。
2. 中间结果存放于临时表，加索引。
3. 少使用游标。 sql 是个集合语言，对于集合运算具有较高性能。而 cursors 是过程运算。比如对一个 100 万行的数据进行查询。游标
   需要读表 100 万次，而不使用游标则只需要少量几次读取。
4. 事务越短越好。 sqlserver 支持并发操作。如果事务过多过长，或者隔离级别过高，都会造成并发操作的阻塞，死锁。导致查询极慢，
   cpu 占用率极地。
5. 使用 try-catch 处理错误异常。
6. 查找语句尽量不要放在循环内



### LIKE 声明中的％和_是什么意思

％对应于 0 个或更多字符，_只是 LIKE 语句中的一个字符

### BLOB 和 TEXT 有什么区别

BLOB 是一个二进制对象，可以容纳可变数量的数据。TEXT 是一个不区分大小写的 BLOB  

 BLOB 和 TEXT 类型之间的唯一区别在于对 BLOB 值进行排序和比较时区分大小写，对 TEXT 值不区分大小写





### MySQL 中有哪几种锁

1. 表级锁：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。
2. 行级锁：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。
3. 页面锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般



### 什么是行级锁

行级锁是一种排他锁，防止其他事务修改此行；在使用以下语句时， Oracle 会自动应用行级锁：

1. INSERT、 UPDATE、 DELETE、 SELECT … FOR UPDATE [OF columns] [WAIT n | NOWAIT];
2. SELECT … FOR UPDATE 语句允许用户一次锁定多条记录进行更新
3. 使用 COMMIT 或 ROLLBACK 语句释放锁

### 什么是表级锁

表示对当前操作的整张表加锁，它实现简单，资源消耗较少，被大部分 MySQL 引擎支持。最常使用的 MYISAM 与 INNODB 都支持表级锁
定。表级锁定分为表共享读锁（共享锁）与表独占写锁（排他锁）

### 什么是页级锁

页级锁是 MySQL 中锁定粒度介于行级锁和表级锁中间的一种锁。表级锁速度快，但冲突多，行级冲突少，但速度慢。所以取了折衷的页
级，一次锁定相邻的一组记录。 BDB 支持页级锁



### 一张表，里面有 ID 自增主键，当 insert 了 17 条记录之后，删除了第 15,16,17 条记录，再把 Mysql 重启，再 insert 一条记录，这条记录的 ID 是 18 还是 15

1. 如果表的类型是 MyISAM，那么是 18因为 MyISAM 表会把自增主键的最大 ID 记录到数据文件里，重启 MySQL 自增主键的最大ID 也不会丢失
2. 如果表的类型是 InnoDB，那么是 15  InnoDB 表只是把自增主键的最大 ID 记录到内存中，所以重启数据库或者是对表进行OPTIMIZE 操作，都会导致最大 ID 丢失

### drop,delete 与 truncate 的区别

1. DELETE 语句执行删除的过程是每次从表中删除一行，并且同时将该行的删除操作作为事务记录在日志中保存以便进行进行回滚操作。TRUNCATE TABLE则一次性地从表中删除所有的数据并不把单独的删除操作记录记入日志保存，删除行是不能恢复的。并且在删除的过程中不会激活与表有关的删除触发器。执行速度快

2. 表和索引所占空间。当表被 TRUNCATE 后，这个表和索引所占用的空间会恢复到初始大小，而 DELETE 操作不会减少表或索引所占用的空间。drop 语句将表所占用的空间全释放掉

3. 一般而言，drop > truncate > delete

4. TRUNCATE 只能对 TABLE；DELETE 可以是 table 和 view

5. TRUNCATE 和 DELETE 只删除数据，而 DROP 则删除整个表（结构和数据）

6. truncate 与不带 where 的 delete ：只删除数据，而不删除表的结构（定义）drop 语句将删除表的结构被依赖的约束（constrain),触发器（trigger)索引（index);依赖于该表的存储过程/函数将被保留，但其状态会变为：invalid

7. delete 语句为 DML （data maintain Language),这个操作会被放到 rollbacksegment 中,事务提交后才生效。如果有相应的 tigger,执行的时候将被触发

8. truncate、drop 是 DLL（data define language),操作立即生效，原数据不放到 rollback segment 中，不能回滚

### mysql 中  varchar 和char  的区别以及 varchar (50) 中，50 代表什么

1. varchar 和 char  的区别在于 char 是一种固定长度的类型，varchar  则是一种可变长度的类型
2. varchar （50） 中的50 含义最多存放50 个字符， varchar  （50） varchar （200） 存储hello 所占空间一样，但后者在排序时会消耗更多的内存，因为 order by col 采用 fixed_length 计算col 长度

### MySQL的binlog有有几种录入格式?分别有什么区别

有三种格式  statement  ，row  和mixed 

- statement 模式下，记录单元为语句，即每一个sql造成的影响会记录.由于sql的执行是有上下文的,因此在保存的时候需要保存相关的信息,同时还有一些使用了函数之类的语句无法被记录复制
- row级别下,记录单元为每一行的改动,基本是可以全部记下来但是由于很多操作,会导致大量行的改动(比如alter table),因此这种模式的文件保存的信息太多,日志量太大
  - mixed. 一种折中的方案,普通操作使用statement记录,当无法使用statement的时候使用row

### 横向分表和纵向分表

横向分表是按行分表.假设我们有一张用户表,主键是自增ID且同时是用户的ID.数据量较大,有1亿多条,那么此时放在一张表里的查询效果就不太理想.我们可以根据主键ID进行分表,无论是按尾号分,或者按ID的区间分都是可以的. 假设按照尾号0-99分为100个表,那么每张表中的数据就仅有100w.这时的查询效率无疑是可以满足要求的



纵向分表是按列分表.假设我们现在有一张文章表.包含字段 id-摘要-内容 .而系统中的展示形式是刷新出一个列表,列表中仅包含标题和摘要,当用户点击某篇文章进入详情时才需要正文内容.此时,如果数据量大,将内容这个很大且不经常使用的列放在一起会拖慢原表的查询速度



### MySQL 数据库作发布系统的存储，一天五万条以上的增量， 预计运维三年,怎么优化

1. 设计良好的数据库结构， 允许部分数据冗余， 尽量避免 join 查询， 提高效率
2. 选择合适的表字段数据类型和存储引擎， 适当的添加索引
3. MySQL 库主从读写分离
4. 找规律分表， 减少单表中的数据量提高查询速度。
5. 添加缓存机制， 比如 memcached， apc等
6. 书写高效率的 SQL。比如 SELECT * FROM TABEL 改为 SELECT field_1, field_2, field_3 FROM TABLE.

### MySQL 外连接、内连接与自连接的区别

###### 内连接

只有条件的交叉连接，根据某个条件筛选出符合条件的记录，不符合条件的记录不会出现在结果集中， 即内连接只连接匹配的行

###### 外连接

其结果集中不仅包含符合连接条件的行，而且还会包括左表、右表或两个表中的所有数据行

### 主键使用自增ID还是UUID

推荐使用自增ID,不要使用UUID

因为在InnoDB存储引擎中,主键索引是作为聚簇索引存在的,也就是说,主键索引的B+树叶子节点上存储了主键索引以及全部的数据(按照顺序),如果主键索引是自增ID,那么只需要不断向后排列即可,如果是UUID,由于到来的ID与原来的大小不确定,会造成非常多的数据插入,数据移动,然后导致产生很多的内存碎片,进而造成插入性能的下降.

总之,在数据量大一些的情况下,用自增主键性能会好一些

### 如果要存储用户的密码散列,应该使用什么字段进行存储

密码散列,盐,用户身份证号等固定长度的字符串应该使用char而不是varchar来存储,这样可以节省空间且提高检索效率



### SQL 性能优化策略

1. 对查询进行优化，应尽量避免全表扫描，首先应考虑在where及order by涉及的列上建立索引

3. 分区分表

   分库分表有垂直切分和水平切分两种

   - 垂直切分 ( 按照功能模块）

     将表按照功能模块、关系密切程度划分出来，部署到不同的库上。例如，我们会建立定义数据库 workDB、商品数据库 payDB、用户数据库 userDB、日志数据库 logDB 等，分别用于存储项目数据定义表、商品定义表、用户数据表、日志数据表等

   - 水平切分  （按照规则划分存储 )

     当一个表中的数据量过大时，我们可以把该表的数据按照某种规则，例如 userID 散列，进行划分，然后存储到多个结构相同的表，和不同的库上

### 优化相关

1. 监控SQL
2. 数据库设计
3. 索引
4. SQL 语句
5. 设置 mysql 的参数
6. 分布式集群如何设计