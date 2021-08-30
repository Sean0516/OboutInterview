# OOP 面向对象

### 类和对象的关系

类是对象的抽象，对象是类的具体，类是对象的模板，对象是类的实例

### Java中的数据类型

整形：byte,short,int,long

浮点型：float,double
字符型：char
布尔型：boolean

| 类型    | 字节,位数   | 最小值           |               最大值 | 默认值 | 描述                                                         |
| :------ | ----------- | ---------------- | -------------------: | ------ | ------------------------------------------------------------ |
| byte    | 1，8        | -2^7             |                2^7-1 | 0      | byte 类型用在大型数组中节约空间，主要代替整数，因为byte 变量占用的空间只有int 类型的四分之一 |
| short   | 2，16       | -2^15            |               2^15-1 | 0      | short 也可以像byte那样节省空间，一个short变量是int型变量所占空间的二分之一 |
| Int     | 4，32       | -2^31            |               2^31-1 | 0      |                                                              |
| long    | 8，64       | -2^63            |               2^63-1 | 0L     | Java 里使用 long 类型的数据一定要在数值后面加上 L，否则将作为整型解析 |
| float   | 4，32       |                  |                      | 0.0f   | 单精度数据类型。 不能用来表示精确的值。如货币                |
| double  | 8，64       |                  |                      | 0.0d   | 双精度 ，不能表示精确的值                                    |
| char    | 2，16       | \u0000（即为 0） | \uffff（即为 65535） |        | char 数据类型可以存储任何字符                                |
| boolean | true，false |                  |                      | false  | 这种类型只作为一种标志来记录 true/false 情况                 |



### instanceof 关键字的作用

instanceof 严格来说是Java中的一个双目运算符，用来测试一个对象是否为一个类的实例，用法为

```java
boolean result = obj instanceof Class
```

### 什么是隐式转换，什么是显式转换

显示转换就是类型强转，把一个大类型的数据强制赋值给小类型的数据；隐式转换就是大范围的变量能够接受小范围的数据；隐式转换和显
式转换其实就是自动类型转换和强制类型转换

### 什么是自动拆箱装箱

装箱就是自动将基本数据类型转换为包装器类型 （int-->Integer）   new Integer(6); ，底层调用: Integer.valueOf(6)

拆箱 将包装类型转换为基本类型 （Integer-> int ）int i = new Integer(6); ，底层调用 i.intValue(); 方法实现。

​	区别

1. 声明方式不同  基本类型不使用new关键字，而包装类型需要使用new关键字来**在堆中分配存储空间**
2. 存储方式及位置不同：基本类型是直接将变量值存储在栈中，而包装类型是将对象放在堆中，然后通过引用来使用
3. 初始值不同：基本类型的初始值如int为0，boolean为false，而包装类型的初始值为null
4. 使用方式不同：基本类型直接赋值直接使用就好，而包装类型在集合如Collection、Map时会使用到



```
public class Main {
public static void main(String[] args) {
    Integer i1 = 100;
    Integer i2 = 100;
    Integer i3 = 200;
    Integer i4 = 200;
    System.out.println(i1==i2); true
    System.out.println(i3==i4); false  |  equals  为 true
}
}
```

### 为什么要有包装类型

为了让基本类型也具有对象的特征，也就出现了包装类型。



### 一个java 类中包含那些内容

属性、方法、内部类、构造方法、代码块

### String 是最基本的数据类型吗？

不是。Java 中的基本数据类型只有 8 个：byte、short、int、long、float、double、 char、boolean；除了基本类型（primitive type），剩下的都是引用类型（reference type），Java 5 以后引入的枚举类型也算是一种比较特殊的引用类型

### float f=3.4;是否正确 ？

不正确。3.4 是双精度数，将双精度型（double）赋值给浮点型（float）属于 下转型（down-casting，也称为窄化）会造成精度损失，因此需要强制类型转换 float f =(float)3.4; 或者写成 float f =3.4F

### short s1 = 1; s1 = s1 + 1;有错吗?short s1 = 1; s1 += 1; 有错吗？

   对于 short s1 = 1; s1 = s1 + 1;由于 1 是 int 类型，因此 s1+1 运算结果也是 int 型，需要强制转换类型才能赋值给 short 型。而 short s1 =1; s1 += 1;可以正确 编译，因为 s1+= 1;相当于 s1 = (short)(s1 + 1);其中有隐含的强制类型转换

### 重载和重写的区别

重写：

在子类中把父类本身有的方法重新写一遍。子类继承了父类原有的方法，但有时子类并不想原封不动的继承父类中的某个方法，所以在方法名，参数列表，返回类型(除过子类中方法的返回值是父类中方法返回值的子类时)都相同的情况下， 对方法体进行修改或重写，这就是重写。但要注意子类函数的访问修饰权限不能少于父类的

重载：

在一个类中，同名的方法如果有不同的参数列表（参数类型不同、参数个数不同甚至是参数顺序不同）则视为重载。同时，重载对返回类型没有要求，可以相同也可以不同，但不能通过返回类型是否相同来判断重载

### equals与==的区别

- ==：

  == 比较的是变量(栈)内存中存放的对象的(堆)内存地址，用来判断两个对象的地址是否相同，即是否是指相同一个对象。比较的是真正意义上的指针操作。

  1. 比较的是操作符两端的操作数是否是同一个对象
  2. 两边的操作数必须是同一类型的（可以是父子类之间）才能编译通过
  3. 比较的是地址，如果是具体的阿拉伯数字的比较，值相等则为true，如：int a=10 与 long b=10L 与 double c=10.0都是相同的（为true），因为他们都指向地址为10的堆

- equals

  equals用来比较的是两个对象的内容是否相等，由于所有的类都是继承自java.lang.Object类的，所以适用于所有对象，如果没有对该方法进行覆盖的话，调用的仍然是Object类中的方法，而Object中的equals方法返回的却是==的判断

- 总结

  所有比较是否相等时，都是用equals 并且在对常量相比较时，把常量写在前面，因为使用object的equals object可能为null 则空指针



### ++i  和 i++ 的区别

i++ ： 先赋值，后计算

++i : 先计算，后赋值

### 数组实例化有几种方式

- 静态实例化。 在创建数组的时候，指定数组中的元素
- 动态实例化， 实例会数组的时候，只指定数组的长度，数组中所有的元素都是数组类型的默认值

### Java 中各种数据默认值

- Byte,short,int,long默认是都是0
- Boolean默认值是false
- Char类型的默认值是’’
- Float与double类型的默认是0.0
- 对象类型的默认值是null

### java中有没有指针？

有指针，但是隐藏了，开发人员无法直接操作指针，由jvm来操作指针

### java中是值传递引用传递

理论上说，java都是引用传递，对于基本数据类型，传递是值的副本，而不是值本身。对于对象类型，传递是对象的引用，当在一个方法操作操作参数的时候，其实操作的是引用所指向的对象

### 形参和实参的区别

### 构造方法能不能显式调用

不能，构造方法当成普通方法调用，只有在创建对象的时候他才会被系统调用

### 内部类

###### 定义：

将一个类定义在另一个类里面或者一个方法里面，这样的类称为内部类

###### 内部类的作用

1. 成员内部类 

   成员内部类可以无条件访问外部类的所有成员属性和成员方法（包括private成员和静态成员）。 当成员内部类拥有和外部类同名的成员变量或者方法时，会发生隐藏现象，即默认情况下访问的是成员内部类的成员

2. 局部内部类

   局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内

3. 匿名内部类

   匿名内部类就是没有名字的内部类

4. 静态内部类

   指被声明为static的内部类，他可以不依赖内部类而实例，而通常的内部类需要实例化外部类，从而实例化。静态内部类不可以有与外部类有相同的类名。不能访问外部类的普通成员变量，但是可以访问静态成员变量和静态方法（包括私有类型） 一个 静态内部类去掉static 就是成员内部类，他可以自由的引用外部类的属性和方法，无论是静态还是非静态。但是不可以有静态属性和方法

   

### 内部类和静态内部类的区别

- 静态内部类

  1. 静态内部类相对与外部类是独立存在的，在静态内部类中无法直接访问外部类中变量，方法。 如果要访问的化，需要new 一个外部类的对象，使用new 出来的对象来访问。 但是可以直接访问静态的变量，调用静态的方法
  2. 如果其他类要访问静态内部类的属性或者调用静态内部类的方法，直接创建一个静态内部类对象即可

- 普通内部类

  1. 普通内部类作为外部类一个成员而存在，在普通内部类中可以直接访问外部类的属性，调用外部类的方法
  2. 如果外部类要访问内部类的属性或者调用内部类的方法，必须要创建一个内部类的对象，使用该对象访问属性或方法
  3. 如果其他的类要访问普通内部类的属性或者调用普通内部类的方法，必须要在外部类中创建一个普通内部类的对象作为一个属性，外同类可以通过该属性调用普通内部类的方法或者访问普通内部类的属性

  

### static 关键字的作用

1. static 可以修改内部类，方法，变量，代码块
2. static 修饰的类，是静态内部类
3. static 修饰的方法是静态方法，表示该方法属于当前类。 而不属于某个对象，静态方法也不能被重写。 可以直接用类名来掉哦那个。 
4. static 修饰变量是静态变量或者类变量。 静态变量被所有实例所共享。 不会依赖于对象， 静态变量在内存中只有一份拷贝，在JVM 加载类的时候，只为静态分配一次内存
5. static 修饰的代码块叫做静态代码块。通常用来做程序优化。静态代码块中的代码在整个类加载的时候只会执行一次

### final 在Java中的作用。

final作为Java中的关键字可以用于三个地方。用于修饰类、类属性和类方法。特征：凡是引用final关键字的地方皆不可修改

1. 被final 修饰的类不可以被继承
2. final 修饰的方法不能被重写
3. final 修饰的变量不可以被改变。 如果修饰是的引用，则引用不可变，引用指向的内容可变
4. final 修饰的方法,JVM 会尝试将其内联，以提高运行效率
5. final修饰的常量，在编译阶段会存入常量池中

### String str=”aaa”,与String str=new String(“aaa”)一样吗？

```java
String x = "张三";
String y = "张三";
String z = new String("张三");
System.out.println(x == y); // true
System.out.println(x == z); // false
```

String x = "张三" 的方式，Java 虚拟机会将其分配到常量池中，而常量池中没有重复的元素，比如当执行“张三”时，java虚拟机会先在常量池中检索是否已经有“张三”,如果有那么就将“张三”的地址赋给变量，如果没有就创建一个，然后在赋给变量；而 String z = new String(“张三”) 则会被分到堆内存中，即使内容一样还是会创建新的对象

### 强引用，弱引用，软引用和虚引用

- 强引用

  强引用是平常中使用最多的引用，强引用在程序内存不足（OOM）的时候也不会被回收，使用方式

  ```java
  String str = new String("str");
  ```

  

- 软引用

  软引用在程序内存不足时，会被回收。 

  可用场景： 创建缓存的时候，创建的对象放进缓存中，当内存不足时，JVM就会回收早先创建的对象

  ```java
  // 注意：wrf这个引用也是强引用，它是指向SoftReference这个对象的，
  // 这里的软引用指的是指向new String("str")的引用，也就是SoftReference类中T
  SoftReference<String> wrf = new SoftReference<String>(new String("str"));
  ```

- 弱引用

  弱引用就是只要JVN 来及收集器发现它，就会将之回收 . 一旦我不需要某个引用，JVM会自动帮我处理它，这样我就不需要做其它操作

  ```java
  WeakReference<String> weakReference=new WeakReference<>("sss");
  ```

- 虚引用

  虚引用的回收机制跟弱引用差不多，但是它被回收之前，会被放入ReferenceQueue中。注意哦，其它引用是被JVM回收后才被传入ReferenceQueue中的。由于这个机制，所以虚引用大多被用于引用销毁前的处理工作。还有就是，虚引用创建的时候，必须带有ReferenceQueue，使用

  ```java
  PhantomReference<String> phantomReference=new PhantomReference<>("demo",new ReferenceQueue<>());
  ```

### java 创建对象的几种方式

1. new 创建新对象
2. 通过反射机制
3. 采用clone机制
4. 通过序列化机制

### 有没有可能两个不相等的对象有相同的hashcode

在产生hash 冲突时，两个不相等的对象，就会有相同的hashcode 。 当hash 冲突产生时，一般用一下方法来处理

1. 拉链法 。 每个hash 表节点都要有一个next 指针，多个哈希表节点可以用next 指针构成一个单向链表。 被分配到同一个索引上的多个节点可以用这个单向链表进行存储
2. 开发定址法  一旦发生了冲突，就去寻找下一个空的散列地址，只要散列表足够大，空的散列表地址总能找到
3. 再哈希:又叫双哈希法,有多个不同的Hash函数.当发生冲突时,使用第二个,第三个….等哈希函数计算地址,直到无冲突

### a=a+b与a+=b有什么区别吗

+= 操作符会进行隐式自动类型转换,此处a+=b隐式的将加操作的结果类型强制转换为持有结果的类型, 而a=a+b则不会自动进行类型转换.

### final、finalize()、finally

1. final为关键字  final为用于标识常量的关键字，final标识的关键字存储在常量池中
2. finalize 为方法  finalize()方法在Object中进行了定义，用于在对象“消失”时，由JVM进行调用用于对对象进行垃圾回收，类似于C++中的析构函数；用户自定义时，用于释放对象占用的资源（比如进行I/0操作）
3. finally 为区块标志。 用于try catch 语句中  finally{}用于标识代码块，与try{}进行配合，不论try中的代码执行完或没有执行完（这里指有异常），该代码块之中的程序必定会进行

### 两个对象值相同(x.equals(y) == true)，但却可有不同的hash code，这句话对不对

不对，如果两个对象 x 和 y 满足 x.equals(y) == true，它们的哈希码（hash code）应当相同。Java 对于 eqauls 方法和 hashCode 方法是
这样规定的：

1. 如果两个 对象相同（equals 方法返回 true），那么它们的 hashCode 值一定要相同
2. 如果两个对象的 hashCode 相同，它们并不一定相同。当然，你未必要按照要求去做，但是如果你违背了上述原则就会发现在使用容器时，相同的对象可以出现在 Set 集合中，同时增加新元素的效率会大大下降（对于使用哈希存储的系统，如果哈希码频繁的冲突将会造成存取性能急剧下降）

### char 型变量中能不能存贮一个中文汉字，为什么？

char 类型可以存储一个中文汉字，因为 Java 中使用的编码是 Unicode（不选择任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一个 char 类型占 2 个字节（16 比特），所以放一个中文是没问题的

### 抽象的（abstract）方法是否可同时是静态的（static）,是否可同时是本地方法（native），是否可同时被 synchronized修饰

都不能。抽象方法需要子类重写，而静态的方法是无法被重写的，因此二者是矛盾的。本地方法是由本地代码（如 C 代码）实现的方法，而抽象方法是没有实现的，也是矛盾的。synchronized 和方法的实现细节有关，抽象方法不涉及实现细节，因此也是相互矛盾的

### 当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递

是值传递。Java 语言的方法调用只支持参数的值传递。当一个对象实例作为一个参数被传递到方法中时，参数的值就是对该对象的引用。对象的属性可以在被调用过程中被改变，但对对象引用的改变是不会影响到调用者的

### 阐述静态变量和实例变量的区别

静态变量是被 static 修饰符修饰的变量，也称为类变量，它属于类，不属于类的任何一个对象，一个类不管创建多少个对象，静态变量在内存中有且仅有一个拷贝；实例变量必须依存于某一实例，需要先创建对象然后通过对象才能访问到它。静态变量可以实现让多个对象共享内存

### String s = new String(“xyz”);创建了几个字符串对象

两个， 一个是常量区的xyz  一个是用new 创建在堆上的对象

### try{}里有一个 return 语句，那么紧跟在这个 try 后的finally{}里的代码会不会被执行，什么时候被执行，在 return前还是后

会执行，在方法返回调用者前执行

注意

在 finally 中改变返回值的做法是不好的，因为如果存在 finally 代码块，try中的 return 语句不会立马返回调用者，而是记录下返回值待 finally 代码块执行完毕之后再向调用者返回其值，然后如果在 finally 中修改了返回值，就会返回修改后的值。显然，在 finally 中返回或者修改返回值会对程序造成很大的困扰

### Collection 和 Collections 的区别

Collection 是一个接口，它是 Set、List 等容器的父接口；Collections 是个一个工具类，提供了一系列的静态方法来辅助容器操作，这些方法包括对容器的搜索、排序、线程安全化等等

### 当一个线程进入一个对象的 synchronized 方法 A 之后，其它线程是否可进入此对象的 synchronized 方法 B

不能。其它线程只能访问该对象的非同步方法，同步方法则不能进入。因为非静态方法上的 synchronized 修饰符要求执行方法时要获得对象的锁，如果已经进入A 方法说明对象锁已经被取走，那么试图进入 B 方法的线程就只能在等锁池（注意不是等待池哦）中等待对象的锁

### 阐述 JDBC 操作数据库的步骤

1. 加载驱动
2. 创建连接
3. 创建语句
4. 执行语句
5. 处理结果
6. 关闭连接

### volatile 类型变量提供什么保证

volatile 变量提供顺序和可见性保证，例如，JVM 或者 JIT 为了获得更好的性能会对语句重排序，但是 volatile 类型变量即使在没有同步块的情况下赋值也不会与其他语句重排序。 volatile 提供 happens-before 的保证，确保一个线程的修改能对其他线程是可见的

### 我们能将 int 强制转换为 byte 类型的变量吗？如果该值大于 byte 类型的范围，将会出现什么现象

是的，我们可以做强制转换，但是 Java 中 int 是 32 位的，而 byte 是 8 位的，所以，如果强制转化是，int 类型的高 24 位将会被丢弃，byte 类型的范围是从 -128 到 128

### int 和 Integer 哪个会占用更多的内存

Integer 对象会占用更多的内存。Integer 是一个对象，需要存储对象的元数据。但是 int 是一个原始类型的数据，所以占用的空间更少

### a==b”和”a.equals(b)”有什么区别

如果 a 和 b 都是对象，则 a==b 是比较两个对象的引用，只有当 a 和 b 指向的是堆中的同一个对象才会返回 true，而 a.equals(b) 是进行逻辑比较，所以通常需要重写该方法来提供逻辑一致性的比较。例如，String 类重写 equals() 方法，所以可以用于两个不同对象，但是包含的字母相同的比较

### ArrayList 和 HashMap 的默认大小是多数

在 Java 7 中，ArrayList 的默认大小是 10 个元素，HashMap 的默认大小是16 个元素（必须是 2 的幂）

### Java中引用数据类型有哪些，它们与基本数据类型有什么区别

简单来说 ，只要不是基本类型，其他都是引用类型 ，他们有以下不同

###### 从概念来说

1. 基本数据类型： 变量名指向具体的数值
2. 引用数据类型： 变量名不是指向具体的数值，而是指向内存数据的内存地址。也就是hash 值

###### 从内存的构建方面

1. 基本数据类型，被创建时，在栈内存中会被划分出一定的内存，并将数值存储在该内存中
2. 引用数据类型 ， 被创建时，首先会在栈内存中分配一块空间，然后在堆内存中也会分配一块具体空间来存储数据的具体信息，即 hash 值，然后由栈中引用指向堆中的对象地址

###### 从使用方面来说

1. 基本数据类型，判断数据是否想到，用 == != 判断
2. 引用数据类型， 判断数据是否相等， 用 equals 方法 。 比较内存地址

### 数据类型选择的原则

- 如果要表示整数就使用int，表示小数就使用double；
- 如果要描述日期时间数字或者表示文件（或内存）大小用long；
- 如果要实现内容传递或者编码转换使用byte；
- 如果要实现逻辑的控制，可以使用booleam；
- 如果要使用中文，使用char避免中文乱码；
- 如果按照保存范围：byte < int < long < double



# 集合

![image-20210805173935482](https://gitee.com/Sean0516/image/raw/master/img/image-20210805173935482.png)

![image-20210805173917414](https://gitee.com/Sean0516/image/raw/master/img/image-20210805173917414.png)

### List Map Set 之间的区别

- List 存储一组不唯一 ，有序的对象
- Set 不允许重复的集合，不会有多个元素引用相同的对象
- Map  使用键值对存储，Map 会维护与Key 有关联的值，两个key 可以引用相同的对象。 但是key 不能重复。

### ArrayList 

ArrayList 是最常用的 List 实现类，内部是通过数组实现的，它允许对元素进行快速随机访问。数组的缺点是每个元素之间不能有间隔， 当数组大小不满足时需要增加存储能力，就要将已经有数组的数据复制到新的存储空间中。 当从 ArrayList 的中间位置插入或者删除元素时，需要对数组进行复制、移动、代价比较高。因此，它适合随机查找和遍历，不适合插入和删除

### Vector（ 数组实现、 线程同步）

Vector 与 ArrayList 一样，也是通过数组实现的，不同的是它支持线程的同步，即某一时刻只有一个线程能够写 Vector，避免多线程同时写而引起的不一致性，但实现同步需要很高的花费，因此，访问它比访问 ArrayList 慢

### LinkList

LinkedList 是用链表结构存储数据的，很适合数据的动态插入和删除，随机访问和遍历速度比较慢。另外，他还提供了 List 接口中没有定义的方法，专门用于操作表头和表尾元素，可以当作堆栈、队列和双向队列使用

### Set 

Set 注重独一无二的性质,该体系集合用于存储无序(存入和取出的顺序不一定相同)元素， 值不能重复。对象的相等性本质是对象 hashCode 值（java 是依据对象的内存地址计算出的此序号） 判断的， 如果想要让两个不同的对象视为相等的，就必须覆盖 Object 的 hashCode 方法和 equals 方法

### HashSet

哈希表边存放的是哈希值。 HashSet 存储元素的顺序并不是按照存入时的顺序（和 List 显然不同） 而是按照哈希值来存的所以取数据也是按照哈希值取得。元素的哈希值是通过元素的hashcode 方法来获取的, HashSet 首先判断两个元素的哈希值，如果哈希值一样，接着会比较equals 方法 如果 equls 结果为 true ， HashSet 就视为同一个元素。如果 equals 为 false 就不是同一个元素

### Tree Set

1. TreeSet()是使用二叉树的原理对新 add()的对象按照指定的顺序排序（升序、降序），每增加一个对象都会进行排序，将对象插入的二叉树指定的位置。
2. Integer 和 String 对象都可以进行默认的 TreeSet 排序，而自定义类的对象是不可以的， 自己定义的类必须实现 Comparable 接口，并且覆写相应的 compareTo()函数，才可以正常使用。
3. 在覆写 compare()函数时，要返回相应的值才能使 TreeSet 按照一定的规则来排序
4. 比较此对象与指定对象的顺序。如果该对象小于、等于或大于指定对象，则分别返回负整数、零或正整数



### HashMap (数组+ 链表+ 红黑树)

HashMap 根据键的 hashCode 值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。 HashMap 最多只允许一条记录的键为 null，允许多条记录的值为 null。 HashMap 非线程安全，即任一时刻可以有多个线程同时写 HashMap，可能会导致数据的不一致。

在 Java8 中， 当链表中的元素超过了 8 个以后，会将链表转换为红黑树，在这些位置进行查找的时候可以降低时间复杂度为 O(logN)



大方向上，HashMap 里面是一个数组，然后数组中每个元素是一个单向链表。上图中，每个绿色的实体是嵌套类 Entry 的实例，Entry 包含四个属性：key, value, hash 值和用于单向链表的 next。

1.  capacity：当前数组容量，始终保持 2^n，可以扩容，扩容后数组大小为当前的 2 倍。
2.  loadFactor：负载因子，默认为 0.75
3.  threshold：扩容的阈值，等于 capacity * loadFactor

![](http://qvi33264o.hn-bkt.clouddn.com/img/hashmap.png)

### HashMap 的实现原理

HashMap基于Hash算法，我们通过put(key,value)存储，get(key)来获取。当传入key时，HashMap会根据key.hashCode()计算出hash值，根据hash值将value保存在bucket里。当计算出的hash值相同时怎么办呢，我们称之为Hash冲突，HashMap的做法是用链表和红黑树存储相同hash值的value。当Hash冲突的个数比较少时，使用链表，否则使用红黑树

#### 常用方法

###### 确认哈希桶数组索引位置

不管增加、删除、查找键值对，定位到哈希桶数组的位置都是很关键的第一步。前面说过HashMap的数据结构是数组和链表的结合，所以我们当然希望这个HashMap里面的元素位置尽量分布均匀些，尽量使得每个位置上的元素数量只有一个，那么当我们用hash算法求得这个位置的时候，马上就可以知道对应位置的元素就是我们要的，不用遍历链表，大大优化了查询的效率。HashMap定位数组索引位置，直接决定了hash方法的离散性能。先看看源码的实现(方法一+方法二):

```java
方法一：
static final int hash(Object key) { //jdk1.8 & jdk1.7
int h;
// h = key.hashCode() 为第一步 取hashCode值
// h ^ (h >>> 16) 为第二步 高位参与运算
return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
方法二：
static int indexFor(int h, int length) { //jdk1.7的源码，jdk1.8没有这个方法，但是实现原理一样的
return h & (length-1); //第三步 取模运算
}
```

这里的Hash算法本质上就是三步：取key的hashCode值、高位运算、取模运算

对于任意给定的对象，只要它的hashCode()返回值相同，那么程序调用方法一所计算得到的Hash码值总是相同的。我们首先想到的就是把hash值对数组长度取模运算，这样一来，元素的分布相对来说是比较均匀的。但是，模运算的消耗还是比较大的，在HashMap中是这样做的：调用方法二来计算该对象应该保存在table数组的哪个索引处

这个方法非常巧妙，它通过h & (table.length -1)来得到该对象的保存位，而HashMap底层数组的长度总是2的n次方，这是HashMap在速度上的优化。当length总是2的n次方时，h& (length-1)运算等价于对length取模，也就是h%length，但是&比%具有更高的效率

![image-20210812160333690](https://gitee.com/Sean0516/image/raw/master/img/image-20210812160333690.png)

###### put 操作的大致思路为

1. 对key的hashCode()做hash，然后再计算index;
2. 如果没碰撞直接放到bucket里；
3. 如果碰撞了，以链表的形式存在buckets后；
4. 如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD)，就把链表转换成红黑树；
5. 如果节点已经存在就替换old value(保证key的唯一性)
6. 如果bucket满了(超过load factor*current capacity)，就要resize （调整table 数组的大小）

在Java 8以前，每次产生hash冲突，就将记录追加到链表后面，然后通过遍历链表来查找。如果某个链表中记录过大，每次遍历的数据就越多，效率也就很低，复杂度为O(n)；
在Java 8中，加入了一个常量TREEIFY_THRESHOLD=8，如果某个链表中的记录大于这个常量的话，HashMap会动态的使用一个专门的treemap实现来替换掉它。这样复杂度是O(logn)，比链表的O(n)会好很多。

###### get 操作

1. bucket里的第一个节点，直接命中；
2. 如果有冲突，则通过key.equals(k)去查找对应的entry
3. 若为树，则在树中通过key.equals(k)查找，O(logn)；
4. 若为链表，则在链表中通过key.equals(k)查找，O(n)。

###### 扩容resize

扩容(resize)就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。当然Java里的数组是无法自动扩容的，方法是使用一个新的数组代替已有的容量小的数组，就像我们用一个小桶装水，如果想装更多的水，就得换大水桶

```java
1 final Node<K,V>[] resize() {
2 Node<K,V>[] oldTab = table;
3 int oldCap = (oldTab == null) ? 0 : oldTab.length;
4 int oldThr = threshold;
5 int newCap, newThr = 0;
6 if (oldCap > 0) {
7 // 超过最大值就不再扩充了，就只好随你碰撞去吧
8 if (oldCap >= MAXIMUM_CAPACITY) {
9 threshold = Integer.MAX_VALUE;
10 return oldTab;
11 }
12 // 没超过最大值，就扩充为原来的2倍
13 else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
14 oldCap >= DEFAULT_INITIAL_CAPACITY)
15 newThr = oldThr << 1; // double threshold
16 }
17 else if (oldThr > 0) // initial capacity was placed in threshold
18 newCap = oldThr;
19 else { // zero initial threshold signifies using defaults
20 newCap = DEFAULT_INITIAL_CAPACITY;
21 newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
22 }
23 // 计算新的resize上限
24 if (newThr == 0) {
25
26 float ft = (float)newCap * loadFactor;
27 newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
28 (int)ft : Integer.MAX_VALUE);
29 }
30 threshold = newThr;
31 @SuppressWarnings({"rawtypes"，"unchecked"})
32 Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
33 table = newTab;
34 if (oldTab != null) {
35 // 把每个bucket都移动到新的buckets中
36 for (int j = 0; j < oldCap; ++j) {
37 Node<K,V> e;
38 if ((e = oldTab[j]) != null) {
39 oldTab[j] = null;
40 if (e.next == null)
41 newTab[e.hash & (newCap - 1)] = e;
42 else if (e instanceof TreeNode)
43 ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
44 else { // 链表优化重hash的代码块
45 Node<K,V> loHead = null, loTail = null;
46 Node<K,V> hiHead = null, hiTail = null;
47 Node<K,V> next;
48 do {
49 next = e.next;
50 // 原索引
51 if ((e.hash & oldCap) == 0) {
52 if (loTail == null)
53 loHead = e;
54 else
55 loTail.next = e;
56 loTail = e;
57 }
58 // 原索引+oldCap
59 else {
60 if (hiTail == null)
61 hiHead = e;
62 else
63 hiTail.next = e;
64 hiTail = e;
65 }
66 } while ((e = next) != null);
67 // 原索引放到bucket里
68 if (loTail != null) {
69 loTail.next = null;
70 newTab[j] = loHead;
71 }
72 // 原索引+oldCap放到bucket里
73 if (hiTail != null) {
74 hiTail.next = null;
75 newTab[j + oldCap] = hiHead;
76 }
77 }
78 }
79 }
80 }
81 return newTab;
82 }
```





### ConcurrentHashMap

   结构和上面一致 。都使用了数组+ 链表+ 红黑树



### HashTable（线程安全）

Hashtable 是遗留类，很多映射的常用功能与 HashMap 类似，不同的是它承自 Dictionary 类，并且是线程安全的，任一时间只有一个线程能写 Hashtable，并发性不如 ConcurrentHashMap， Hashtable 不建议在新代码中使用，不需要线程安全的场合可以用 HashMap 替换，需要线程安全的场合可以用 ConcurrentHashMap 替换

### TreeMap（可排序）

TreeMap 实现 SortedMap 接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用 Iterator 遍历 TreeMap 时，得到的记录是排过序的。如果使用排序的映射，建议使用 TreeMap。在使用 TreeMap 时， key 必须实现 Comparable 接口或者在构造 TreeMap 传入自定义的Comparator，否则会在运行时抛出 java.lang.ClassCastException 类型的异常

### TreeMap 的实现原理

TreeMap是一个通过红黑树实现有序的key-value集合。
TreeMap继承AbstractMap，也即实现了Map，它是一个Map集合
TreeMap实现了NavigableMap接口，它支持一系列的导航方法，
TreeMap实现了Cloneable接口，它可以被克隆
TreeMap本质是Red-Black Tree，它包含几个重要的成员变量：root、size、comparator。其中root是红黑树的根节点。它是Entry类型，Entry是红黑树的节点，它包含了红黑树的6个基本组成：key、value、left、right、parent和color。Entry节点根据根据Key排序，包含的内容是value。Entry中key比较大小是根据比较器comparator来进行判断的。size是红黑树的节点个数

### 快速失败 (fail-fast) 和安全失败 (fail-safe) 的区别是什么

Iterator 的安全失败是基于对底层集合做拷贝，因此，它不受源集合上修改的影响

java.util 包下面的所有的集合类都是快速失败的，而 java.util.concurrent 包下面的所有的类都是安全失败的。快速失败的迭代器会抛出ConcurrentModificationException 异常，而安全失败的迭代器永远不会抛出这样的异常

### ArrayList,Vector, LinkedList 的存储性能和特性

ArrayList 和 Vector 都是使用数组方式存储数据，此数组元素数大于实际存储的数
据以便增加和插入元素，它们都允许直接按序号索引元素，但是插入元素要涉及数组
元素移动等内存操作，所以索引数据快而插入数据慢，Vector 由于使用了synchronized 方法（线程安全）。通常性能上较 ArrayList 差

LinkedList 使用双向链表实现存储，按序号索引数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快

ArrayList 在查找时速度快，LinkedList 在插入与删除时更具优势

### Hashmap 什么时候进行扩容呢

当 hashmap 中的元素个数超过数组大小 loadFactor 时，就会进行数组扩容loadFactor 的默认值为 0.75，也就是说，默认情况下，数组大小为 16，那么当hashmap 中元素个数超过 16 0.75=12 的时候，就把数组的大小扩展为 2 16=32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知 hashmap 中元素的个数，那么预设元素的个数能够有效的提高 hashmap 的性能。比如说，我们有 1000 个元素 new HashMap(1000),但是理论上来newHashMap(1024) 更合适，不过上面已经说过，即使是 1000，hashmap 也自动会将其设置为 1024。 但是 new HashMap(1024) 还不是更合适的，因为 0.75*1000 < 1000, 也就是说为了让 0.75 * size > 1000, 我们必须这样 new HashMap(2048) 才最合适，既考虑了 & 的问题，也避免了 resize的问题

### LinkedHashMap 的实现原理

LinkedHashMap 也是基于 HashMap 实现的，不同的是它定义了一个 Entry header，这个 header 不是放在 Table 里，它是额外独立出来的。LinkedHashMap 通过继承 hashMap 中的 Entry, 并添加两个属性 Entrybefore,after, 和 header 结合起来组成一个双向链表，来实现按插入顺序或访问顺序排序。LinkedHashMap 定义了排序模式 accessOrder，该属性为 boolean 型变量，对于访问顺序，为 true；对于插入顺序，则为 false。一般情况下，不必指定排序模式，其迭代顺序即为默认为插入顺序

### Iterator 和 ListIterator 的区别是什么

Iterator 可用来遍历 Set 和 List 集合，但是 ListIterator 只能用来遍历 List。

Iterator 对集合只能是前向遍历，ListIterator 既可以前向也可以后向。

ListIterator 实现了 Iterator 接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等

### poll()方法和remove()方法区别?

poll() 和 remove() 都是从队列中取出一个元素，但是 poll() 在获取元素失败的时候会返回空，但是remove() 失败的时候会抛出异常

### 集合框架相关的有哪些最好的实践

1. 根据需要选择正确的集合类型。比如，如果指定了大小，我们会选用Array而非ArrayList。如果我们想根据插入顺序遍历一个Map，我们需要使用TreeMap。如果我们不想重复，我们应该使用Set
2. 一些集合类允许指定初始容量，所以如果我们能够估计到存储元素的数量，我们可以使用它，就避免了重新哈希或大小调整
3. 基于接口编程，而非基于实现编程，它允许我们后来轻易地改变实现
4. 总是使用类型安全的泛型，避免在运行时出现ClassCastException
5. 使用JDK提供的不可变类作为Map的key，可以避免自己实现hashCode()和equals()
6. 尽可能使用Collections工具类，或者获取只读、同步或空的集合，而非编写自己的实现。它将会提供代码重用性，它有着更好的稳定性和可维护性

# 泛型

### 泛型类

泛型类的声明和非泛型类的声明类似，除了在类名后面添加了类型参数声明部分。和泛型方法一样，泛型类的类型参数声明部分也包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符。因为他们接受一个或多个参数，这些类被称为参数化的类或参数化的类型

```java
public class Box<T> {
private T t;
public void add(T t) {
this.t = t;
}
public T get() {
return t;
}
}
```

### 泛型方法

该方法在调用时可以接收不同类型的参数。根据传递给泛型方法的参数类型，编译器适当地处理每一个方法调用

```java
public static < E > void printArray( E[] inputArray )
	{
		for ( E element : inputArray ){
		System.out.printf( "%s ", element );
	}
}
// <? extends T>表示该通配符所代表的类型是 T 类型的子类
//<? super T>表示该通配符所代表的类型是 T 类型的父类
```

### 类型通配符 ?  

类 型 通 配 符 一 般 是 使 用 ? 代 替 具 体 的 类 型 参 数 。 例 如 List<?> 在 逻 辑 上 是List,List 等所有 List<具体类型实参>的父类

### 类型擦除

java 中的泛型基本上都是在编译器这个层次来实现的。在生成的 Java 字节代码中是不包含泛型中的类型信息的。使用泛型的时候加上的类
型参数，会被编译器在编译的时候去掉。这个过程就称为类型擦除

### 使用泛型的好处

以集合来举例，使用泛型的好处是我们不必因为添加元素类型的不同而定义不同类型的集合，如整型集合类，浮点型集合类，字符串集合类，我们可以定义一个集合来存放整型、浮点型，字符串型数据，而这并不是最重要的，因为我们只要把底层存储设置了Object即可，添加的数据全部都可向上转型为Object。 更重要的是我们可以通过规则按照自己的想法控制存储的数据类型

# 异常

### try catch fifinally，try里有return，finally还执行么

执行， 并且 finally 的执行早于 try 里面的return

1. 不管有没有出现异常，finally块中代码都会执行；
2. 当try和catch中有return时，finally仍然会执行；
3. finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，不管finally中的代码怎么样，返回的值都不会改变，任然是之前保存的值），所以函数返回值是在finally执行前确定的；
4. finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值。

### thow与thorws区别

- 位置不同
  1. throws 作用在函数上，后面跟异常类，而throw 用在函数内，后面跟异常对象
- 功能不同
  1. throws 用来声明异常，让调用者只知道该功能可能出现的问题，可以给出预先的处理方式；throw 抛出具体的问题对象，执行到 throw，功能就已经结束了，跳转到调用者，并将具体的问题对象抛给调用者。也就是说 throw 语句独立存在时，下面不要定义其他语句，因为执行不到
  2. throws 表示出现异常的一种可能性，并不一定会发生这些异常；throw 则是抛出了异常，执行 throw 则一定抛出了某种异常对象
  3. 两者都是消极处理异常的方式，只是抛出或者可能抛出异常，但是不会由函数去处理异常，真正的处理异常由函数的上层调用处理

# IO

### 字符流和字节流的区别

以字节为单位输入输出数据，字节流按照8位传输
以字符为单位输入输出数据，字符流按照16位传输

### 阻塞IO 模型 非阻塞IO模型 多路复用IO模型 信号驱动IO 模型 异步IO 模型

### BIO、NIO、AIO 有什么区别

- BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
- NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
- AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制

# NIO

### Java NIO

NIO 主要有三大核心部分： Channel(通道)， Buffer(缓冲区), Selector。传统 IO 基于字节流和字符流进行操作， 而 NIO 基于 Channel 和Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。 Selector(选择区)用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个线程可以监听多个数据通道。 NIO 和传统 IO 之间第一个最大的区别是， IO 是面向流的， NIO 是面向缓冲区的

### NIO 的缓冲区

java IO 面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据， 需要先将它缓存到一个缓冲区。 NIO 的缓冲导向方法不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程中的灵活性。但是，还需要检查是否该缓冲区中包含所有您需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的数据

### NIO 的非阻塞

NIO 的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取。而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。 非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。 线程通常将非阻塞 IO 的空闲时间用于在其它通道上执行 IO 操作，所以一个单独的线程现在可以管理多个输入和输出通道（channel）

### Channel

Channel 是双向的，既可以用来进行读操作，又可以用来进行写操作。NIO 中的 Channel 的主要实现有

- FileChannel 文件IO
- DatagramChannel UDP
- SocketChannel TCP
- ServerSocketChannel TCP Server 

### Buffer

缓冲区，实际上是一个容器，是一个连续数组。Channel 提供从文件、网络读取数据的渠道，但是读取或写入的数据都必须经由 Buffer

从一个客户端向服务端发送数据，然后服务端接收数据的过程。客户端发送数据时，必须先将数据存入 Buffer 中，然后将 Buffer 中的内容写入通道。服务端这边接收数据必须通过 Channel 将数据读入到 Buffer 中，然后再从 Buffer 中取出数据来处理

在 NIO 中，Buffer 是一个顶层父类，它是一个抽象类，常用的 Buffer 的子类有：
ByteBuffer、IntBuffer、 CharBuffer、 LongBuffer、 DoubleBuffer、FloatBuffer、ShortBuffer

### Selector

Selector 类是 NIO 的核心类， Selector 能够检测多个注册的通道上是否有事件发生，如果有事件发生，便获取事件然后针对每个事件进行相应的响应处理。这样一来，只是用一个单线程就可以管理多个通道，也就是管理多个连接。这样使得只有在连接真正有读写事件发生时，才会调用函数来进行读写，就大大地减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程，并且避免了多线程之间的上下文切换导致的开销

# 反射

### 反射的作用

反射机制是在运行时，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意个对象，都能够调用它的任意一个方法。在java中，只要给定类的名字，就可以通过反射机制来获得类的所有信息 。 这种动态获取信息以及动态调用对象方法的功能成为 Java 语言的反射机制

### 反射的实现

1. Class.forName("类的路径")
2. 类名.class
3. 对象名.getClass()
4. 基本类型的包装类，可以调用包装类的Type属性来获得该包装类的Class对象

### 反射类的组成

- class  表示正在运行的Java应用程序中的类和接口
- field 提供有关类和接口的属性信息，以及对他的动态访问权限
- constructor 提供关于类的单个构造方法的信息以及它的访问权限
- method  提供类或者接口中某个方法的信息

### 反射使用步骤

1. 获取想要操作的类的 Class 对象，他是反射的核心，通过 Class 对象我们可以任意调用类的方法
2. 调用 Class 类中的方法，既就是反射的使用阶段
3. 使用反射 API 来操作这些信息

### 创建对象的两种方法

1. 使用 Class 对象的 newInstance()方法来创建该 Class 对象对应类的实例，但是这种方法要求该 Class 对象对应的类有默认的空构造器

   ```java
   Person p=(Person) clazz.newInstance();
   ```

   

2. 先使用 Class 对象获取指定的 Constructor 对象，再调用 Constructor 对象的 newInstance()方法来创建 Class 对象对应类的实例,通过这种方法可以选定构造方法创建实例

   ```java
   Constructor c=clazz.getDeclaredConstructor(String.class,String.class,int.class);
   Person p1=(Person) c.newInstance("李四","男",20);
   ```

   

### 反射的优缺点

###### 优点

1. 能够动态获取类的实例，提高灵活性
2. 与动态编译结合

###### 缺点

1. 使用反射性能较低，需要解析字节码，将内存中的对象进行解析

   ###### 解决方案

   1. 通过setAccessible(true)关闭JDK的安全检查来提升反射速度；
   2. 多次创建一个类的实例时，有缓存会快很多
   3. ReflectASM工具类，通过字节码生成的方式加快反射速度

2. 相对不安全，破坏了封装性（因为通过反射可以获得私有方法和属性）

# 序列化

### 什么是Java序列化，如何实现Java序列化

序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决在对对象流进行读写操作时所引发的问题。序列化的实现：将需要被序列化的类实现Serializable接口，该接口没有需要实现的方法，implements Serializable只是为了标注该对象是可被序列化的，然后使用一个输出流(如：FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)，要恢复的话则用输入流

### 序列化 ID

虚拟机是否允许反序列化，不仅取决于类路径和功能代码是否一致，一个非常重要的一点是两个类的序列化 ID 是否一致（就是 private static final long serialVersionUID

### 序列化并不保存静态变量

需要注意地是， 对象序列化保存的是对象的”状态”，即它的成员变量。由此可知，对象序列化不会关注类中的静态变量 。 要想将父类对象也序列化，就需要让父类也实现 Serializable 接口

### Transient 关键字阻止该变量被序列化到文件中

在变量声明前加上 Transient 关键字，可以阻止该变量被序列化到文件中，在被反序列化后， transient 变量的值被设为初始值，如 int 型的是 0，对象型的是 null

# 注解

### 四种标准元注解

###### @Target 修饰对象范围

@Target说明了Annotation所修饰的对象范围： Annotation可被用于 packages、types（类、接口、枚举、Annotation 类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch 参数）。在 Annotation 类型的声明中使用了 target可更加明晰其修饰的目标

###### @Retention 定义被保留的时间长短

Retention 定义了该 Annotation 被保留的时间长短：表示需要在什么级别保存注解信息，用于描述注解的生命周期（即：被描述的注解在
什么范围内有效），取值（RetentionPoicy）由：

1. SOURCE:在源文件中有效（即源文件保留）
2. CLASS:在 class 文件中有效（即 class 保留）
3. RUNTIME:在运行时有效（即运行时保留）

###### @Documented 

用于描述其他类型的 annotation 应该被作为被标注的程序成员的公共 API，因此可以被例如 javadoc 此类的工具文档化

###### @Inherited 阐述了某个被标注的类型是被继承的

@Inherited 元注解是一个标记注解，@Inherited 阐述了某个被标注的类型是被继承的。如果一
个使用了@Inherited 修饰的 annotation 类型被用于一个 class，则这个 annotation 将被用于该
class 的子类。

### 注解是什么

Annotation（注解）是 Java 提供的一种对元程序中元素关联信息和元数据（metadata）的途径和方法。 Annatation(注解)是一个接口，程序可以通过反射来获取指定程序中元素的 Annotation对象，然后通过该 Annotation 对象来获取注解中的元数据信息

### 注解处理器 处理器

如果没有用来读取注解的方法和工作，那么注解也就不会比注释更有用处了。使用注解的过程中，很重要的一部分就是创建于使用注解处理器。Java SE5扩展了反射机制的API，以帮助程序员快速的构造自定义注解处理器



# 多线程

![image-20210805174400442](https://gitee.com/Sean0516/image/raw/master/img/image-20210805174400442.png)

### 如何停止一个正在运行的线程

- 使用退出标志，使得线程正常退出，也就是当run 方法完成后线程终止
- 使用stop方法 强行终止
- 使用interrupt 中断方法中断线程

### notify 和notifyAll 有什么区别

1. notify可能会导致死锁，而notifyAll则不会 
2. 任何时候只有一个线程可以获得锁，也就是说只有一个线程可以运行synchronized 中的代码使用notifyall,可以唤醒所有处于wait状态的线程，使其重新进入锁的争夺队列中，而notify只能唤醒一个。
3. wait() 应配合while循环使用，不应使用if，务必在wait()调用前后都检查条件，如果不满足，必须调用notify()唤醒另外的线程来处理，自己继续wait()直至条件满足再往下执行。
4. notify() 是对notifyAll()的一个优化，但它有很精确的应用场景，并且要求正确使用。不然可能导致死锁。正确的场景应该是 WaitSet中等待的是相同的条件，唤醒任一个都能正确处理接下来的事项，如果唤醒的线程无法正确处理，务必确保继续notify()下一个线程，并且自身需要重新回到WaitSet中.

### 四种线程池

- newCachedThreadPool   创建一个可缓存线程池 ，是 无 界 线 程 池 ， 如 果 线 程 池 的 大 小 超 过 了 处 理 任 务所 需 要 的 线 程 ， 那 么 就 会 回 收 部 分 空 闲 （ 60 秒 不 执 行 任 务 ） 线 程 ， 当任 务 数 增 加 时 ， 此 线 程 池 又 可 以 智 能 的 添 加 新 线 程 来 处 理 任 务 。线 程 池 大 小 完 全 依 赖 于 操 作 系 统 （ 或 者 说 JVM） 能 够 创 建 的 最 大 线 程大 小 
- newFixedThreadPool  创建一个定长线程池， 只 有 核 心 线 程 。 每 次 提 交 一 个任 务 就 创 建 一 个 线 程 ， 直 到 线 程 达 到 线 程 池 的 最 大 大 小 。 线 程 池 的 大 小一 旦 达 到 最 大 值 就 会 保 持 不 变 ， 如 果 某 个 线 程 因 为 执 行 异 常 而 结 束 ， 那么 线 程 池 会 补 充 一 个 新 线 程 
- newScheduledThreadPool  创建一个定长线程池，支持定时及周期性任务执行
- newSingleThreadExecutor  创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务  如 果 这 个 唯 一 的 线 程 因 为 异 常 结 束 ， 那 么 会 有 一 个 新 的 线 程 来替 代 它 。 此 线 程 池 保 证 所 有 任 务 的 执 行 顺 序 按 照 任 务 的 提 交 顺 序 执 行

![image-20210805174920035](https://gitee.com/Sean0516/image/raw/master/img/image-20210805174920035.png)

### 线程生命周期 

1. 新建状态（ 新建状态（NEW ）

   当程序使用 new 关键字创建了一个线程之后，该线程就处于新建状态，此时仅由 JVM 为其分配内存，并初始化其成员变量的值

2. 就绪状态（RUNNABLE ）

   当线程对象调用了 start()方法之后，该线程处于就绪状态。Java 虚拟机会为其创建方法调用栈和程序计数器，等待调度运行

3. 运行状 态（RUNNING ）

   如果处于就绪状态的线程获得了 CPU，开始执行 run()方法的线程执行体，则该线程处于运行状态

4. 阻塞状态（BLOCKED ）

   阻塞状态是指线程因为某种原因放弃了 cpu 使用权，也即让出了 cpu timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有机会再次获得 cpu timeslice 转到运行(running)状态。阻塞的情况分三种

   - 等待阻塞 （ o.wait-> 等待对列 ）  运行(running)的线程执行 o.wait()方法，JVM 会把该线程放入等待队列(waitting queue)中
   - 同步阻塞 (lock-> 锁池 )  运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则 JVM 会把该线程放入锁池(lock pool)中
   - 其他阻塞 (sleep/join)  运行(running)的线程执行 Thread.sleep(long ms)或 t.join()方法，或者发出了 I/O 请求时，JVM 会把该线程置为阻塞状态。当 sleep()状态超时、join()等待线程终止或者超时、或者 I/O处理完毕时，线程重新转入可运行(runnable)状态

5. 线程死亡（DEAD

   线程会以下面三种方式结束，结束后就是死亡状态

   1. run()或 call()方法执行完成，线程正常结束
   2. 线程抛出一个未捕获的 Exception 或 Error
   3. 直接调用该线程的 stop()方法来结束该线程—该方法通常容易导致死锁，不推荐使用

### sleep()和wait() 有什么区别

1. 对于 sleep()方法，我们首先要知道该方法是属于 Thread 类中的。而 wait()方法，则是属于Object 类中的
2. sleep()方法导致了程序暂停执行指定的时间，让出 cpu 给其他线程，但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态 （不释放锁）
3. 在调用 sleep()方法的过程中， 线程不会释放对象锁
4. 而当调用 wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用 notify()方法后本线程才进入对象锁定池准备获取对象锁进入运行状态

### Thread.sleep(0)的作用是什么

由于Java采用抢占式的线程调度算法，因此可能会出现某条线程常常获取到CPU控制权的情况，为了让某些优先级比较低的线程也能获取到CPU控制权，可以使用Thread.sleep(0)手动触发一次操作系统分配时间片的操作，这也是平衡CPU控制权的一种操作

### interrupted 和 isInterruptedd方法的区别

1. interrupted() 和 isInterrupted()的主要区别是前者会将中断状态清除而后者不会。Java多线程的中断机制是用内部标识来实现的，调用Thread.interrupt()来中断一个线程就会设置中断标识为true
2. 当中断线程调用静态方法Thread.interrupted()来检查中断状态时，中断状态会被清零
3. 而非静态方法isInterrupted()用来查询其它线程的中断状态且不会改变中断状态标识。简单的说就是任何抛出InterruptedException异常的方法都会将中断状态清零。无论如何，一个线程的中断状态有有可能被其它线程调用中断来改变

### synchronized 和 ReentrantLock 有什么不同

1. 最大区别就是对于Synchronized来说，它是java语言的关键字，是原生语法层面的互斥，需要jvm实现。而ReentrantLock它是JDK 1.5之后提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成
2. Synchronized进过编译，会在同步块的前后分别形成monitorenter和monitorexit这个两个字节码指令。在执行monitorenter指令时，首先要尝试获取对象锁。如果这个对象没被锁定，或者当前线程已经拥有了那个对象锁，把锁的计算器加1，相应的，在执行monitorexit指令时会将锁计算器就减1，当计算器为0时，锁就被释放了。如果获取对象锁失败，那当前线程就要阻塞，直到对象锁被另一个线程释放为止 
3. 相比Synchronized，ReentrantLock类提供了一些高级功能，主要有以下3项
   - 等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized来说可以避免出现死锁的情况
   - 公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized锁非公平锁，ReentrantLock默认的构造函数是创建的非公平锁，可以通过参数true设为公平锁，但公平锁表现的性能不是很好
   - 锁绑定多个条件，一个ReentrantLock对象可以同时绑定对个对象

### 如何保证多个线程顺序执行

在多线程中有多种方法让线程按特定顺序执行，你可以用线程类的join()方法在一个线程中启动另一个线程，另外一个线程完成该线程继续执行。

### SynchronizedMap和ConcurrentHashMap有什么区别

   SynchronizedMap()和Hashtable一样，实现上在调用map所有方法时，都对整个map进行同步。而ConcurrentHashMap的实现却更加精细，它对map中的所有桶加了锁。所以，只要有一个线程访问map，其他线程就无法进入map，而如果一个线程在访问ConcurrentHashMap某个桶时，其他线程，仍然可以对map执行某些操作。

   所以，ConcurrentHashMap在性能以及安全性方面，明显比Collections.synchronizedMap()更加有优势。同时，同步操作精确控制到桶，这样，即使在遍历map时，如果其他线程试图对map进行数据修改，也不会抛出ConcurrentModificationException

### Thread类中的yield方法有什么作用

   Yield方法可以暂停当前正在执行的线程对象，让其它有相同优先级的线程执行。它是一个静态方法而且只保证当前线程放弃CPU占用而不能保证使其它线程一定能占用CPU，执行yield()的线程有可能在进入到暂停状态后马上又被执行

### 说说自己是怎么使用 synchronized 关键字

修饰实例方法: 作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁

修饰静态方法: 也就是给当前类加锁，会作用于类的所有对象实例，因为静态成员不属于任何一个实例对象，是类成员（ static 表明这是该类的一个静态资源，不管new了多少个对象，只有一份）。所以如果一个线程A调用一个实例对象的非静态 synchronized 方法，而线程B需要调用这个实例对象所属类的静态 synchronized 方法，是允许的，不会发生互斥现象，因为访问静态 synchronized 方法占用的锁是当前类的锁，而访问非静态synchronized 方法占用的锁是当前实例对象锁。

修饰代码块: 指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。

### volatile关键字的作用

一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：

1. 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。
2. 禁止进行指令重排序

- volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住
- volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的
- volatile仅能实现变量的修改可见性，并不能保证原子性；synchronized则可以保证变量的修改可见性和原子性
- volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞
- volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化

### volatile如何保 证 变 量 对 所 有 线程 的 可 见 性 

Java 的 内 存 模 型 定 义 了 8 种 内 存 间 操 作

###### lock 和 unlock

- 把 一 个 变 量 标 识 为 一 条 线 程 独 占 的 状 态
- 把 一 个 处 于 锁 定 状 态 的 变 量 释 放 出 来 ， 释 放 之 后 的 变 量 才 能 被 其 他 线 程锁 定 

###### read 和 write

- 把 read 操 作 从 主 内 存 得 到 的 变 量 值 放 入 工 作 内 存 的 变 量 副 本 中 
- 把 工 作 内 存 的 变 量 值 传 送 到 主 内 存 ， 以 便 write

###### load 和 store

- 把 一 个 变 量 值 从 主 内 存 传 输 到 线 程 的 工 作 内 存 ， 以 便 load
- 把 store 操 作 从 工 作 内 存 得 到 的 变 量 的 值 ， 放 入 主 内 存 的 变 量 中

###### use 和 assgin

- 把 工 作 内 存 变 量 值 传 递 给 执 行 引 擎
- 将 执 行 引 擎 值 传 递 给 工 作 内 存 变 量 值

volatile 的 实 现 基 于 这 8 种 内 存 间 操 作 ， 保 证 了 一 个 线 程 对 某 个volatile 变 量 的 修 改 ， 一 定 会 被 另 一 个 线 程 看 见 ， 即 保 证 了 可 见 性



### Synchronized 同步锁

synchronized 它可以把任意一个非 NULL 的对象当作锁。他属于独占式的悲观锁，同时属于可重入锁

Synchronized 是 由 JVM 实 现 的 一 种 实 现 互 斥 同 步 的 一 种 方 式 ， 如 果你 查 看 被  Synchronized  修 饰 过 的 程 序 块 编 译 后 的 字 节 码 ， 会 发 现 ，被  Synchronized  修 饰 过 的 程 序 块 ， 在 编 译 前 后 被 编 译 器 生 成 了monitorenter 和 monitorexit 两 个 字 节 码 指 令

在 虚 拟 机 执 行 到 monitorenter 指 令 时 ， 首 先 要 尝 试 获 取 对 象 的 锁 ：如 果 这 个 对 象 没 有 锁 定 ， 或 者 当 前 线 程 已 经 拥 有 了 这 个 对 象 的 锁 ， 把 锁的 计 数 器  +1； 当 执 行 monitorexit 指 令 时 将 锁 计 数 器  -1； 当 计 数 器为 0 时 ， 锁 就 被 释 放 了 。如 果 获 取 对 象 失 败 了 ， 那 当 前 线 程 就 要 阻 塞 等 待 ， 直 到 对 象 锁 被 另 外 一个 线 程 释 放 为 止

Java  中  Synchronize  通 过 在 对 象 头 设 置 标 记 ， 达 到 了 获 取 锁 和 释 放锁 的 目 的

### Synchronized 作用范围

1. 作用于方法时，锁住的是对象的实例(this)
2. 当作用于静态方法时，锁住的是Class实例，又因为Class的相关数据存储在永久带PermGen（jdk1.8 则是 metaspace），永久带是全局共享的，因此静态方法锁相当于类的一个全局锁，会锁所有调用该方法的线程
3. synchronized 作用于一个对象实例时，锁住的是所有以该对象为锁的代码块。它有多个队列，当多个线程一起访问某个对象监视器的时候，对象监视器会将这些线程存储在不同的容器中

### Synchronized 核心 组件

1. Wait Set：哪些调用 wait 方法被阻塞的线程被放置在这里
2. Contention List：竞争队列，所有请求锁的线程首先被放在这个竞争队列中
3. Entry List：Contention List 中那些有资格成为候选资源的线程被移动到 Entry List 中
4. OnDeck：任意时刻，最多只有一个线程正在竞争锁资源，该线程被成为 OnDeck
5. Owner：当前已经获取到所资源的线程被称为 Owner
6. !Owner：当前释放锁的线程

### Synchronized 实现

![image-20210805175548757](https://gitee.com/Sean0516/image/raw/master/img/image-20210805175548757.png)

1. JVM 每次从队列的尾部取出一个数据用于锁竞争候选者（OnDeck），但是并发情况下，ContentionList 会被大量的并发线程进行 CAS 访问，为了降低对尾部元素的竞争，JVM 会将一部分线程移动到 EntryList 中作为候选竞争线程
2. Owner 线程会在 unlock 时，将 ContentionList 中的部分线程迁移到 EntryList 中，并指定EntryList 中的某个线程为 OnDeck 线程（一般是最先进去的那个线程）
3. Owner 线程并不直接把锁传递给 OnDeck 线程，而是把锁竞争的权利交给 OnDeck，OnDeck需要重新竞争锁。这样虽然牺牲了一些公平性，但是能极大的提升系统的吞吐量，在JVM 中，也把这种选择行为称之为“竞争切换”
4. OnDeck 线程获取到锁资源后会变为 Owner 线程，而没有得到锁资源的仍然停留在 EntryList中。如果Owner线程被wait方法阻塞，则转移到WaitSet队列中，直到某个时刻通过notify或者 notifyAll 唤醒，会重新进去 EntryList 中
5. 处于 ContentionList、EntryList、WaitSet 中的线程都处于阻塞状态，该阻塞是由操作系统来完成的（Linux 内核下采用 pthread_mutex_lock 内核函数实现的）
6. Synchronized 是非公平锁。 Synchronized 在线程进入 ContentionList 时，等待的线程会先尝试自旋获取锁，如果获取不到就进入 ContentionList，这明显对于已经进入队列的线程是不公平的，还有一个不公平的事情就是自旋获取锁的线程还可能直接抢占 OnDeck 线程的锁资源
7. 每个对象都有个 monitor 对象，加锁就是在竞争 monitor 对象，代码块加锁是在前后分别加上 monitorenter 和 monitorexit 指令来实现的，方法加锁是通过一个标记位来判断的
8. synchronized 是一个重量级操作，需要调用操作系统相关接口，性能是低效的，有可能给线程加锁消耗的时间比有用操作消耗的时间更多
9. Java1.6，synchronized 进行了很多的优化，有适应自旋、锁消除、锁粗化、轻量级锁及偏向锁等，效率有了本质上的提高。在之后推出的 Java1.7 与 1.8 中，均对该关键字的实现机理做了优化。引入了偏向锁和轻量级锁。都是在对象头中有标记位，不需要经过操作系统加锁
10. 锁可以从偏向锁升级到轻量级锁，再升级到重量级锁。这种升级过程叫做锁膨胀
11. JDK 1.6 中默认是开启偏向锁和轻量级锁，可以通过-XX:-UseBiasedLocking 来禁用偏向锁

### ThreadLocal是什么

即线程本地变量。如果你创建了一个ThreadLocal变量，那么访问这个变量的每个线程都会有这个变量的一个本地拷贝，多个线程操作这个变量的时候，实际是操作自己本地内存里面的变量，从而起到线程隔离的作用，避免了线程安全问题

### ThreadLocal原理

1. Thread类有一个类型为ThreadLocal.ThreadLocalMap的实例变量threadLocals，即每个线程都有一个属于自己的ThreadLocalMap
2. ThreadLocalMap内部维护着Entry数组，每个Entry代表一个完整的对象，key是ThreadLocal本身，value是ThreadLocal的泛型值
3. 每个线程在往ThreadLocal里设置值的时候，都是往自己的ThreadLocalMap里存，读也是以某个ThreadLocal作为引用，在自己的map里找对应的key，从而实现了线程隔离

### ThreadLocal 是 怎 么 解 决 并 发 安 全 的

ThreadLocal 这 是 Java 提 供 的 一 种 保 存 线 程 私 有 信 息 的 机 制 ， 因 为其 在 整 个 线 程 生 命 周 期 内 有 效 ， 所 以 可 以 方 便 地 在 一 个 线 程 关 联 的 不 同业 务 模 块 之 间 传 递 信 息 ， 比 如 事 务 ID、 Cookie 等 上 下 文 相 关 信 息 ThreadLocal 为 每 一 个 线 程 维 护 变 量 的 副 本 ， 把 共 享 数 据 的 可 见 范 围 限制 在 同 一 个 线 程 之 内 ， 其 实 现 原 理 是 ， 在 ThreadLocal 类 中 有 一 个Map， 用 于 存 储 每 一 个 线 程 的 变 量 的 副 本 。

### 很 多 人 都 说 要 慎 用 ThreadLocal， 谈 谈 你 的 理 解 ， 使 用ThreadLocal 需 要 注 意 些 什 么

ThreadLocal 的 实 现 是 基 于 一 个 所 谓 的 ThreadLocalMap， 在ThreadLocalMap 中 ， 它 的 key 是 一 个 弱 引 用 。通 常 弱 引 用 都 会 和 引 用 队 列 配 合 清 理 机 制 使 用 ， 但 是 ThreadLocal 是个 例 外 ， 它 并 没 有 这 么 做 。
这 意 味 着 ， 废 弃 项 目 的 回 收 依 赖 于 显 式 地 触 发 ， 否 则 就 要 等 待 线 程 结束 ， 进 而 回 收 相 应 ThreadLocalMap！ 这 就 是 很 多 OOM 的 来 源 ， 所以 通 常 都 会 建 议 ， 应 用 一 定 要 自 己 负 责 remove， 并 且 不 要 和 线 程 池 配合 ， 因 为 worker 线 程 往 往 是 不 会 退 出 的



### JVM 对 Java 的 原 生 锁 做 了 哪 些 优 化

1. 使 用 自 旋 锁 ， 即 在 把 线 程 进 行 阻 塞 操 作 之 前 先 让 线 程 自 旋 等待 一 段 时 间 ， 可 能 在 等 待 期 间 其 他 线 程 已 经 解 锁 ， 这 时 就 无 需 再 让 线 程执 行 阻 塞 操 作 ， 避 免 了 用 户 态 到 内 核 态 的 切 换

2. 现 代 JDK 中 还 提 供 了 三 种 不 同 的 Monitor 实 现 ， 也 就 是 三 种 不 同 的锁

   - 偏 向 锁  当没有竞争出现时，会默认使用偏向锁 
   - 轻量级锁
   - 重量级锁

   这 三 种 锁 使 得 JDK 得 以 优 化 Synchronized 的 运 行 ， 当 JVM 检 测到 不 同 的 竞 争 状 况 时 ， 会 自 动 切 换 到 适 合 的 锁 实 现 ， 这 就 是 锁 的 升 级 、降 级

   JVM 会 利 用 CAS 操 作 ， 在 对 象 头 上 的 Mark Word 部 分 设 置 线 程ID， 以 表 示 这 个 对 象 偏 向 于 当 前 线 程 ， 所 以 并 不 涉 及 真 正 的 互 斥 锁 ， 因为 在 很 多 应 用 场 景 中 ， 大 部 分 对 象 生 命 周 期 中 最 多 会 被 一 个 线 程 锁 定 ，使 用 偏向锁 可 以 降 低 无 竞 争 开 销 

   

   如 果 有 另 一 线 程 试 图 锁 定 某 个 被 偏 斜 过 的 对 象 ， JVM  就 撤 销 偏向锁 ，切 换 到 轻 量 级 锁 实 现

   

   轻 量 级 锁 依 赖 CAS 操 作 Mark Word 来 试 图 获 取 锁 ， 如 果 重 试 成 功 ，就 使 用 普 通 的 轻 量 级 锁 ； 否 则 ， 进 一 步 升 级 为 重 量 级 锁 

### 为什么说 synchronized 是非公平锁

非 公 平 主 要 表 现 在 获 取 锁 的 行 为 上 ， 并 非 是 按 照 申 请 锁 的 时 间 前 后 给 等待 线 程 分 配 锁 的 ， 每 当 锁 被 释 放 后 ， 任 何 一 个 线 程 都 有 机 会 竞 争 到 锁 ，这 样 做 的 目 的 是 为 了 提 高 执 行 性 能 ， 缺 点 是 可 能 会 产 生 线 程 饥 饿 现 象

### 什 么 是 锁 消 除 和 锁 粗 化

###### 锁 消 除

 指 虚 拟 机 即 时 编 译 器 在 运 行 时 ， 对 一 些 代 码 上 要 求 同 步 ， 但 被检 测 到 不 可 能 存 在 共 享 数 据 竞 争 的 锁 进 行 消 除 。 主 要 根 据 逃 逸 分 析 。程 序 员 怎 么 会 在 明 知 道 不 存 在 数 据 竞 争 的 情 况 下 使 用 同 步 呢 ？ 很 多 不 是程 序 员 自 己 加 入 的 

###### 锁 粗 化

 原 则 上 ， 同 步 块 的 作 用 范 围 要 尽 量 小 。 但 是 如 果 一 系 列 的 连 续操 作 都 对 同 一 个 对 象 反 复 加 锁 和 解 锁 ， 甚 至 加 锁 操 作 在 循 环 体 内 ， 频 繁地 进 行 互 斥 同 步 操 作 也 会 导 致 不 必 要 的 性 能 损 耗 。锁 粗 化 就 是 增 大 锁 的 作 用 域



### ReentrantLock

ReentrantLock 类实现了Lock ，它拥有与synchronized 相同的并发性和内存语义，但是添加了类似锁投票、定时锁等候和可中断锁等候的一些特性。此外，它还提供了在激烈争用情况下更佳的性能。（换句话说，当许多线程都想访问共享资源时，JVM可以花更少的时候来调度线程，把更多时间用在执行线程上。

他是一种可重入锁，除了能完成 synchronized 所能完成的所有工作外，还提供了诸如可响应中断锁、可轮询锁请求、定时锁等避免多线程死锁的方法

### Semaphore  与 ReentrantLock

Semaphore 基本能完成 ReentrantLock 的所有工作，使用方法也与之类似，通过 acquire()与release()方法来获得和释放临界资源。经实测，Semaphone.acquire()方法默认为可响应中断锁，与 ReentrantLock.lockInterruptibly()作用效果一致，也就是说在等待临界资源的过程中可以被
Thread.interrupt()方法中断

此外，Semaphore 也实现了可轮询的锁请求与定时锁的功能，除了方法名 tryAcquire 与 tryLock不同，其使用方法与 ReentrantLock几乎一致。Semaphore也提供了公平与非公平锁的机制，也可在构造函数中进行设定

Semaphore的锁释放操作也由手动进行，因此与 ReentrantLock 一样，为避免线程因抛出异常而无法正常释放锁的情况发生，释放锁的操作也必须在 finally 代码块中完成



### ReentrantLock 是 如 何 实 现 可 重 入 性 的 

ReentrantLock 内 部 自 定 义 了 同 步 器 Sync（ Sync 既 实 现 了 AQS，又 实 现 了 AOS， 而 AOS 提 供 了 一 种 互 斥 锁 持 有 的 方 式 ） ， 其 实 就 是加 锁 的 时 候 通 过 CAS 算 法 ， 将 线 程 对 象 放 到 一 个 双 向 链 表 中 ， 每 次 获取 锁 的 时 候 ， 看 下 当 前 维 护 的 那 个 线 程 ID 和 当 前 请 求 的 线 程 ID 是 否一 样 ， 一 样 就 可 重 入 了

### synchronized和ReentrantLock的区别

synchronized是和if、else、for、while一样的关键字，ReentrantLock是类，这是二者的本质区别。既然ReentrantLock是类，那么它就提供了比synchronized更多更灵活的特性，可以被继承、可以有方法、可以有各种各样的类变量，ReentrantLock比synchronized的扩展性体现在几点上

1. ReentrantLock可以对获取锁的等待时间进行设置，这样就避免了死锁
2. ReentrantLock可以获取各种锁的信息
3. ReentrantLock可以灵活地实现多路通知

另外，二者的锁机制其实也是不一样的。ReentrantLock底层调用的是Unsafe的park方法加锁，synchronized操作的应该是对象头中mark word，这点我不能确定

### ReadWriteLock 和 StampedLock

读 写 锁 基 于 的 原 理 是 多 个 读 操 作 不 需 要 互 斥 ， 如 果 读 锁 试 图 锁 定 时 ， 写锁 是 被 某 个 线 程 持 有 ， 读 锁 将 无 法 获 得 ， 而 只 好 等 待 对 方 操 作 结 束 ， 这样 就 可 以 自 动 保 证 不 会 读 取 到 有 争 议 的 数 据 。



ReadWriteLock  代 表 了 一 对 锁 ， 下 面 是 一 个 基 于 读 写 锁 实 现 的 数 据 结构 ， 当 数 据 量 较 大 ， 并 发 读 多 、 并 发 写 少 的 时 候 ， 能 够 比 纯 同 步 版 本 凸显 出 优 势 

StampedLock， 在 提 供 类 似 读 写 锁 的 同 时 ，还 支 持 优 化 读 模 式 。 优 化 读 基 于 假 设 ， 大 多 数 情 况 下 读 操 作 并 不 会 和 写操 作 冲 突 ， 其 逻 辑 是 先 试 着 修 改 ， 然 后 通 过 validate 方 法 确 认 是 否 进入 了 写 模 式 ， 如 果 没 有 进 入 ， 就 成 功 避 免 了 开 销 ； 如 果 进 入 ， 则 尝 试 获取 读 锁 

### Condition 类和 Object 类锁方法区别区别

1. Condition 类的 awiat 方法和 Object 类的 wait 方法等效
2. Condition 类的 signal 方法和 Object 类的 notify 方法等效
3. Condition 类的 signalAll 方法和 Object 类的 notifyAll 方法等效
4. ReentrantLock 类可以唤醒指定条件的线程，而 object 的唤醒是随机的

### tryLock 和 lock 和 lockInterruptibly 的区别

1. tryLock 能获得锁就返回 true，不能就立即返回 false， tryLock(long timeout,TimeUnit
   unit)，可以增加时间限制，如果超过该时间段还没获得锁，返回 false
2. lock 能获得锁就返回 true，不能的话一直等待获得锁
3. lock 和 lockInterruptibly，如果两个线程分别执行这两个方法，但此时中断这两个线程，
   lock 不会抛出异常，而 lockInterruptibly 会抛出异常

### ReadWriteLock 读写锁

为了提高性能， Java 提供了读写锁，在读的地方使用读锁，在写的地方使用写锁，灵活控制，如果没有写锁的情况下，读是无阻塞的,在一定程度上提高了程序的执行效率。 读写锁分为读锁和写锁，多个读锁不互斥，读锁与写锁互斥，这是由 jvm 自己控制的，你只要上好相应的锁即可

###### 读锁

如果你的代码只读数据，可以很多人同时读，但不能同时写，那就上读锁

###### 写锁

如果你的代码修改数据，只能有一个人在写，且不能同时读取，那就上写锁。总之，读的时候上读锁，写的时候上写锁

### 共享锁和独占锁  （java 并发包提供的加锁模式分为独占锁和共享锁）

###### 独占锁

独占锁模式下，每次只能有一个线程能持有锁， ReentrantLock 就是以独占方式实现的互斥锁。独占锁是一种悲观保守的加锁策略，它避免了读/读冲突，如果某个只读线程获取锁，则其他读线程都只能等待，这种情况下就限制了不必要的并发性，因为读操作并不会影响数据的一致性

###### 共享锁

共享锁则允许多个线程同时获取锁，并发访问 共享资源，如： ReadWriteLock。 共享锁则是一种乐观锁，它放宽了加锁策略，允许多个执行读操作的线程同时访问共享资源

### 重量级锁（Mutex Lock）

Synchronized 是通过对象内部的一个叫做监视器锁（monitor）来实现的。但是监视器锁本质又是依赖于底层的操作系统的 Mutex Lock 来实现的

操作系统实现线程之间的切换这就需要从用户态转换到核心态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么
Synchronized 效率低的原因。因此， 这种依赖于操作系统 Mutex Lock 所实现的锁我们称之为“重量级锁” 。 JDK 中对 Synchronized 做的种种优化，其核心都是为了减少这种重量级锁的使用

JDK1.6 以后，为了减少获得锁和释放锁所带来的性能消耗，提高性能，引入了“轻量级锁”和“偏向锁”

### 轻量级锁

### Java 中 的 线 程 池 是 如 何 实 现 的 

1. 在 Java 中 ， 所 谓 的 线 程 池 中 的 “ 线 程 ” ， 其 实 是 被 抽 象 为 了 一 个 静 态内 部 类  Worker， 它 基 于  AQS  实 现 ， 存 放 在 线 程 池 的HashSet<Worker> workers 成 员 变 量 中 
2. 而 需 要 执 行 的 任 务 则 存 放 在 成 员 变 量  workQueue（ BlockingQueue<Runnable> workQueue） 中 。这 样 ， 整 个 线 程 池 实 现 的 基 本 思 想 就 是 ： 从  workQueue  中 不 断 取 出需 要 执 行 的 任 务 ， 放 在 Workers 中 进 行 处 理

### ThreadPoolExecutor 的构造方法

1. corePoolSize：指定了线程池中的线程数量。
2. maximumPoolSize：指定了线程池中的最大线程数量。
3. keepAliveTime：当前线程池数量超过 corePoolSize 时，多余的空闲线程的存活时间，即多次时间内会被销毁。
4. unit： keepAliveTime 的单位。
5. workQueue：任务队列，被提交但尚未被执行的任务。
6. threadFactory：线程工厂，用于创建线程，一般用默认的即可。
7. handler：拒绝策略，当任务太多来不及处理，如何拒绝任务。

### 拒绝策略

当线程池中的线程已经用完了，无法继续为新任务服务，同时，等待队列也已经排满了，再也塞不下新任务了。这时候我们就需要拒绝策略机制合理的处理这个问题。JDK 内置的拒绝策略如下

1. AbortPolicy ： 直接抛出异常，阻止系统正常运行。

2. CallerRunsPolicy ： 只要线程池未关闭，该策略直接在调用者线程中，运行当前被丢弃的任务。显然这样做不会真的丢弃任务，但是，任务提交线程的性能极有可能会急剧下降。

3. DiscardOldestPolicy ： 丢弃最老的一个请求，也就是即将被执行的一个任务，并尝试再次提交当前任务。

4. DiscardPolicy ： 该策略默默地丢弃无法处理的任务，不予任何处理。如果允许任务丢失，这是最好的一种方案。

  以上内置拒绝策略均实现了 RejectedExecutionHandler 接口，若以上策略仍无法满足实际需要，完全可以自己扩展 RejectedExecutionHandler 接口。

![image-20210806094243946](https://gitee.com/Sean0516/image/raw/master/img/image-20210806094243946.png)

### Java 线程池工作过程

1. 线程池刚创建时，里面没有一个线程。任务队列是作为参数传进来的。不过，就算队列里面
   有任务，线程池也不会马上执行它们。
2. 当调用 execute() 方法添加一个任务时，线程池会做如下判断：
   a) 如果正在运行的线程数量小于 corePoolSize，那么马上创建线程运行这个任务；
   b) 如果正在运行的线程数量大于或等于 corePoolSize，那么将这个任务放入队列；
   c) 如果这时候队列满了，而且正在运行的线程数量小于 maximumPoolSize，那么还是要
   创建非核心线程立刻运行这个任务；
   d) 如果队列满了，而且正在运行的线程数量大于或等于 maximumPoolSize，那么线程池
   会抛出异常 RejectExecutionException。
3. 当一个线程完成任务时，它会从队列中取下一个任务来执行。
4. 当一个线程无事可做，超过一定的时间（keepAliveTime）时，线程池会判断，如果当前运
   行的线程数大于 corePoolSize，那么这个线程就被停掉。所以线程池的所有任务完成后，它
   最终会收缩到 corePoolSize 的大小。

### JAVA 阻塞队列原理

阻塞队列，关键字是阻塞，先理解阻塞的含义，在阻塞队列中，线程阻塞有这样的两种情况

1. 当队列中没有数据的情况下，消费者端的所有线程都会被自动阻塞（挂起），直到有数据放入队列
2. 当队列中填满数据的情况下，生产者端的所有线程都会被自动阻塞（挂起），直到队列中有空的位置，线程被自动唤醒

### Java 中的阻塞队列

1. ArrayBlockingQueue ：由数组结构组成的有界阻塞队列。 （公平、非公平）

   用数组实现的有界阻塞队列。此队列按照先进先出（FIFO）的原则对元素进行排序。 默认情况下不保证访问者公平的访问队列，所谓公平访问队列是指阻塞的所有生产者线程或消费者线程，当队列可用时，可以按照阻塞的先后顺序访问队列，即先阻塞的生产者线程，可以先往队列里插入元素，先阻塞的消费者线程，可以先从队列里获取元素。通常情况下为了保证公平性会降低吞吐量。我们可以使用以下代码创建一个公平的阻塞队列

2. LinkedBlockingQueue ：由链表结构组成的有界阻塞队列。 （两个独立锁提高并发）

   基于链表的阻塞队列，同 ArrayListBlockingQueue 类似，此队列按照先进先出（FIFO）的原则对元素进行排序。而 LinkedBlockingQueue之所以能够高效的处理并发数据，还因为其对于生产者端和消费者端分别采用了独立的锁来控制数据同步，这也意味着在高并发的情况下生产者和消费者可以并行地操作队列中的数据，以此来提高整个队列的并发性能。LinkedBlockingQueue 会默认一个类似无限大小的容量（Integer.MAX_VALUE)

3. PriorityBlockingQueue ：支持优先级排序的无界阻塞队列。 （compareTo 排序实现优先） 

   是一个支持优先级的无界队列。默认情况下元素采取自然顺序升序排列。 可以自定义实现compareTo()方法来指定元素进行排序规则，或者初始化 PriorityBlockingQueue 时，指定构造参数 Comparator 来对元素进行排序。需要注意的是不能保证同优先级元素的顺序

4. DelayQueue：使用优先级队列实现的无界阻塞队列。 （缓存失效、定时任务 ）

   是一个支持延时获取元素的无界阻塞队列。队列使用 PriorityQueue 来实现。队列中的元素必须实现 Delayed 接口，在创建元素时可以指定多久才能从队列中获取当前元素。只有在延迟期满时才能从队列中提取元素

5. SynchronousQueue：不存储元素的阻塞队列。（不存储数据、可用于传递数据）

   是一个不存储元素的阻塞队列。每一个 put 操作必须等待一个 take 操作，否则不能继续添加元素。SynchronousQueue 可以看成是一个传球手，负责把生产者线程处理的数据直接传递给消费者线程。队列本身并不存储任何元素，非常适合于传递性场景,比如在一个线程中使用的数据，传递给另 外 一 个 线 程 使 用 ， SynchronousQueue 的 吞 吐 量 高 于 LinkedBlockingQueue 和ArrayBlockingQueue。

6. LinkedTransferQueue：由链表结构组成的无界阻塞队列。 相 对 于 其 他 阻 塞 队 列 ，LinkedTransferQueue 多了 tryTransfer 和 transfer 方法

   1. transfer 方法：如果当前有消费者正在等待接收元素（消费者使用 take()方法或带时间限制的poll()方法时），transfer 方法可以把生产者传入的元素立刻 transfer（传输）给消费者。如果没有消费者在等待接收元素，transfer 方法会将元素存放在队列的 tail 节点，并等到该元素被消费者消费了才返回

   2. tryTransfer 方法。则是用来试探下生产者传入的元素是否能直接传给消费者。如果没有消费者等待接收元素，则返回 false。和 transfer 方法的区别是 tryTransfer 方法无论消费者是否接收，方法立即返回。而 transfer 方法是必须等到消费者消费了才返回

      对于带有时间限制的 tryTransfer(E e, long timeout, TimeUnit unit)方法，则是试图把生产者传入的元素直接传给消费者，但是如果没有消费者消费该元素则等待指定的时间再返回，如果超时还没消费元素，则返回 false，如果在超时时间内消费了元素，则返回 true

7. LinkedBlockingDeque：由链表结构组成的双向阻塞队列

   是一个由链表结构组成的双向阻塞队列。所谓双向队列指的你可以从队列的两端插入和移出元素。双端队列因为多了一个操作队列的入口，在多线程同时入队时，也就减少了一半的竞争。相比其他的阻塞队列，LinkedBlockingDeque 多了 addFirst，addLast，offerFirst，offerLast，peekFirst，peekLast 等方法，以 First 单词结尾的方法，表示插入，获取（peek）或移除双端队列的第一个元素。以 Last 单词结尾的方法，表示插入，获取或移除双端队列的最后一个元素。另外插入方法 add 等同于 addLast，移除方法 remove 等效于 removeFirst。但是 take 方法却等同于 takeFirst，不知道是不是 Jdk 的 bug，使用时还是用带有 First 和 Last 后缀的方法更清楚

### 什么是多线程中的上下文切换

多线程会共同使用一组计算机上的 CPU，而线程数大于给程序分配的 CPU 数量时，为了让各个线程都有执行的机会，就需要轮转使用 CPU。不同的线程切换使用 CPU发生的切换数据等就是上下文切换

### 死锁与活锁的区别，死锁与饥饿的区别

活锁：一个线程通常会有会响应其他线程的活动。如果其他线程也会响应另一个线程的活动，那么就有可能发生活锁。同死锁一样，发生活锁的线程无法继续执行。然而线程并没有阻塞——他们在忙于响应对方无法恢复工作。这就相当于两个在走廊相遇的人：甲向他自己的左边靠想让乙过去，而乙向他的右边靠想让甲过去。可见他们阻塞了对方。甲向他的右边靠，而乙向他的左边靠，他们还是阻塞了对方。

死锁：两个或更多线程阻塞着等待其它处于死锁状态的线程所持有的锁。死锁通常发生在多个线程同时但以不同的顺序请求同一组锁的时候，死锁会让你的程序挂起无法完成任务

### Java中的死锁

在Java中使用多线程，就会有可能导致死锁问题。死锁会让程序一直卡住，不再程序往下执行。我们只能通过中止并重启的方式来让程序重新执行。这是我们非常不愿意看到的一种现象，我们要尽可能避免死锁的情况发生

- 互斥条件：指进程对所分配到的资源进行排它性使用，即在一段时间内某资源只由一个进程占用。如果此时还有其它进程请求资源，则请求者只能等待，直至占有资源的进程用完释放

- 请求和保持条件：指进程已经保持至少一个资源，但又提出了新的资源请求，而该资源已被其它进程占有，此时请求进程阻塞，但又对自己已获得的其它资源保持不放

- 不剥夺条件：指进程已获得的资源，在未使用完之前，不能被剥夺，只能在使用完时由自己释放

- 环路等待条件：指在发生死锁时，必然存在一个进程——资源的环形链，即进程集合{A，B，C，···，Z} 中的A正在等待一个B占用的资源；B正在等待C占用的资源，……，Z正在等待已被A占用的资源

  

### 在 Java 中 CycliBarriar 和 CountdownLatch 有什么区别

CyclicBarrier 可以重复使用，而 CountdownLatch 不能重复使用。 Java 的 concurrent 包里面的 CountDownLatch 其实可以把它看作一个计数器，只不过这个计数器的操作是原子操作，同时只能有一个线程去操作这个计数器，也就是同时只能有一个线程去减这个计数器里面的值。你可以向 CountDownLatch 对象设置一个初始的数字作为计数值，任何调用这个对象上的 await()方法都会阻塞，直到这个计数器的计数值被其他的线程减为 0 为止。



所以在当前计数到达零之前，await 方法会一直受阻塞。之后，会释放所有等待的线程，await 的所有后续调用都将立即返回。这种现象只出现一次——计数无法被重置。如果需要重置计数，请考虑使用 CyclicBarrier。

CountDownLatch 的一个非常典型的应用场景是：有一个任务想要往下执行，但必须要等到其他的任务执行完毕后才可以继续往下执行。假如我们这个想要继续往下执行的任务调用一个 CountDownLatch 对象的 await()方法，其他的任务执行完自己的任务后调用同一个CountDownLatch 对象上的 countDown()方法，这个调用 await()方法的任务将一直阻塞等待，直到这个 CountDownLatch 对象的计数值减到 0 为止。

CyclicBarrier 一个同步辅助类，它允许一组线程互相等待，直到到达某个公共屏障点 (common barrier point)。在涉及一组固定大小的线程的程序中，这些线程必须不时地互相等待，此时 CyclicBarrier 很有用。因为该 barrie在释放等待线程后可以重用，所以称它为循环 的barrier。

### 为什么使用 Executor 框架比使用应用创建和管理线程好

1. 每次执行任务创建线程 new Thread()比较消耗性能，创建一个线程是比较耗、耗资源的
2. 调用 new Thread()创建的线程缺乏管理，被称为野线程，而且可以无限制的创建，线程之间的相互竞争会导致过多占用系统资源而导致系统瘫痪，还有线程之间的频繁交替也会消耗很多系统资源。
3. 直接使用 new Thread() 启动的线程不利于扩展，比如定时执行、定期执行、定时定期执行、线程中断等都不便实现

### 使用 Executor 线程池框架的优点

1. 能复用已存在并空闲的线程从而减少线程对象的创建从而减少了消亡线程的开
   销。

2. 可有效控制最大并发线程数，提高系统资源使用率，同时避免过多资源竞争。

3. 框架中已经有定时、定期、单线程、并发数控制等功能。

   综上所述使用线程池框架 Executor 能更好的管理线程、提供系统资源使用率

### Linux 上查找哪个线程使用的 CPU 时间最长

1. 使用top 命令，然后使按下 shift + p 查找出cpu 使用率最高的 pid
2. 根据第一步拿到的pid  使用命令 top -H -p pid  然后按下 shift +p 查找cpu 利用率最高的线程
3. 将获取的pid 线程转换为 16 进制  printf '%x\n'  pid
4. 使用 jstack pid > /tmp/d.dat 将进程信息打印输出
5. 编辑 dat 文件，查找线程号对应的信息

### 为什么代码会重排序

在执行程序时，为了提供性能，处理器和编译器常常会对指令进行重排序，但是不能随意重排序，不是你想怎么排序就怎么排序，它需要满足以下两个条件

1. 在单线程环境下不能改变程序运行的结果
2. 存在数据依赖关系的不允许重排序

需要注意的是：重排序不会影响单线程环境的执行结果，但是会破坏多线程的执行语义

### volatile 变量和 atomic 变量有什么不同

Volatile 变量可以确保先行关系，即写操作会发生在后续的读操作之前, 但它并不能保证原子性。例如用 volatile 修饰 count 变量那么 count++ 操作就不是原子性的

 AtomicInteger 类提供的 atomic 方法可以让这种操作具有原子性如getAndIncrement()方法会原子性的进行增量操作把当前值加一，其它数据类型和引用变量也可以进行相似操作

### 什么是 CAS

CAS 是 compare and swap 的缩写，即我们所说的比较交换 

cas 是一种基于锁的操作，而且是乐观锁。在 java 中锁分为乐观锁和悲观锁。悲观锁是将资源锁住，等一个之前获得锁的线程释放锁之后，下一个线程才可以访问。而乐观锁采取了一种宽泛的态度，通过某种方式不加锁来处理资源，比如通过给记录加 version 来获取数据，性能较悲观锁有很大的提高 

CAS 操作包含三个操作数                  内存位置（V）、预期原值（A） 新值(B)

如果内存地址里面的值和 A 的值是一样的，那么就将内存里面的值更新成 B。CAS是通过无限循环来获取数据的，若果在第一轮循环中，a 线程获取地址里面的值被b 线程修改了，那么 a 线程需要自旋，到下次循环才有可能机会执行

CAS  具 有 原 子 性 ， 它 的 原 子 性 由  CPU  硬 件 指 令 实 现 保 证 ， 即 使 用JNI 调 用 Native 方 法 调 用 由 C++ 编 写 的 硬 件 级 别 指 令 ， JDK 中 提供 了 Unsafe 类 执 行 这 些 操 作

### CAS 的问题

1. CAS 容易造成 ABA 问题

   一个线程 a 将数值改成了 b，接着又改成了 a，此时 CAS 认为是没有变化，其实是已经变化过了，而这个问题的解决方案可以使用版本号标识，每操作一次version 加 1

2. 不能保证代码块的原子性

   CAS 机制所保证的知识一个变量的原子性操作，而不能保证整个代码块的原子性。比如需要保证 3 个变量共同进行原子性的更新，就不得不使用synchronized 了

3. CAS 造成 CPU 利用率增加

   CAS 里面是一个循环判断的过程，如果线程一直没有获取到状态，cpu资源会一直被占用 ，可以使用自旋次数的设置，来避免这个耗时问题

### 什么是AQS

AQS 是 AbstactQueuedSynchronizer 的简称，它是一个 Java 提高的底层同步工具类，用一个 int 类型的变量表示同步状态，并提供了一系列的 CAS 操作来管理这个同步状态 

AQS 是一个用来构建锁和同步器的框架，使用 AQS 能简单且高效地构造出应用广泛的大量的同步器，比如我们提到的 ReentrantLock，Semaphore，其他的诸如ReentrantReadWriteLock，SynchronousQueue，FutureTask 等等皆是基于AQS 的

AQS定义了对双向队列所有的操作，而只开放了tryLock和tryRelease方法给开发者使用，开发者可以根据自己的实现重写tryLock和tryRelease方法，以实现自己的并发功能

### AQS 框 架 

1.  AQS 在 内 部 定 义 了 一 个 volatile int state 变 量 ， 表 示 同 步 状 态 ： 当 线程 调 用 lock 方 法 时 ， 如 果 state=0， 说 明 没 有 任 何 线 程 占 有 共 享 资 源的 锁 ， 可 以 获 得 锁 并 将 state=1； 如 果 state=1， 则 说 明 有 线 程 目 前 正 在使 用 共 享 变 量 ， 其 他 线 程 必 须 加 入 同 步 队 列 进 行 等 待
2.  AQS 通 过 Node 内 部 类 构 成 的 一 个 双 向 链 表 结 构 的 同 步 队 列 ， 来 完 成 线程 获 取 锁 的 排 队 工 作 ， 当 有 线 程 获 取 锁 失 败 后 ， 就 被 添 加 到 队 列 末 尾
    - Node  类 是 对 要 访 问 同 步 代 码 的 线 程 的 封 装 ， 包 含 了 线 程 本 身 及 其 状 态 叫waitStatus（ 有 五 种 不 同  取 值 ， 分 别 表 示 是 否 被 阻 塞 ， 是 否 等 待 唤 醒 ，是 否 已 经 被 取 消 等 ） ， 每 个 Node 结 点 关 联 其 prev 结 点 和 next 结点 ， 方 便 线 程 释 放 锁 后 快 速 唤 醒 下 一 个 在 等 待 的 线 程 ， 是 一 个 FIFO 的 过程
    - Node 类 有 两 个 常 量 ， SHARED 和 EXCLUSIVE， 分 别 代 表 共 享 模 式 和 独占 模 式 。 所 谓 共 享 模 式 是 一 个 锁 允 许 多 条 线 程 同 时 操 作 （ 信 号 量Semaphore 就 是 基 于 AQS 的 共 享 模 式 实 现 的 ） ， 独 占 模 式 是 同 一 个 时间 段 只 能 有 一 个 线 程 对 共 享 资 源 进 行 操 作 ， 多 余 的 请 求 线 程 需 要 排 队 等 待（ 如 ReentranLock）
3.  AQS  通 过 内 部 类  ConditionObject  构 建 等 待 队 列 （ 可 有 多 个 ） ， 当Condition  调 用  wait()  方 法 后 ， 线 程 将 会 加 入 等 待 队 列 中 ， 而 当Condition 调 用 signal() 方 法 后 ， 线 程 将 从 等 待 队 列 转 移 动 同 步 队 列 中进 行 锁 竞 争
4.  AQS  和  Condition  各 自 维 护 了 不 同 的 队 列 ， 在 使 用  Lock  和 Condition 的 时 候 ， 其 实 就 是 两 个 队 列 的 互 相 移 动

### 线程类的构造方法、静态块是被哪个线程调用的

线程类的构造方法、静态块是被 new这个线程类所在的线程所调用的，而 run 方法里面的代码才是被线程自身所调用的

### 在多线程中，什么是上下文切换(context-switching)？

单核CPU也支持多线程执行代码，CPU通过给每个线程分配CPU时间片来实现这个机制。时间片是CPU分配给各个线程的时间，因为时间片非常短，所以CPU通过不停地切换线程执行，让我们感觉多个线程时同时执行的，时间片一般是几十毫秒（ms）。

操作系统中，CPU时间分片切换到另一个就绪的线程，则需要保存当前线程的运行的位置，同时需要加载需要恢复线程的环境信息

### 线程安全的级别

###### 不可变

不可变的对象一定是线程安全的，并且永远也不需要额外的同步 （Java类库中大多数基本数值类如Integer、String和BigInteger都是不可变的）

###### 无条件的线程安全

由类的规格说明所规定的约束在对象被多个线程访问时仍然有效，不管运行时环境如何排列，线程都不需要任何额外的同步

如 Random 、ConcurrentHashMap、Concurrent集合、atomic

###### 有条件的线程安全

有条件的线程安全类对于单独的操作可以是线程安全的，但是某些操作序列可能需要外部同步  有条件线程安全的最常见的例子是遍历由 Hashtable 或者 Vector 或者返回的迭代器

###### 非线程安全(线程兼容)

线程兼容类不是线程安全的，但是可以通过正确使用同步而在并发环境中安全地使用

###### 线程对立

线程对立是那些不管是否采用了同步措施，都不能在多线程环境中并发使用的代码   如如System.setOut()、System.runFinalizersOnExit()

### 高并发、任务执行时间短的业务怎样使用线程池？并发不高、任务执行时间长的业务怎样使用线程池？并发高、业务执行时间长的业务怎样使用线程池

1. 高并发、任务执行时间短的业务，线程池线程数可以设置为CPU核数+1，减少线程上下文的切换
2. 并发不高、任务执行时间长的业务要区分开看
   - 假如是业务时间长集中在IO操作上，也就是IO密集型的任务，因为IO操作并不占用CPU，所以不要让所有的CPU闲下来，可以加大线程池中的线程数目，让CPU处理更多的业务
   - 假如是业务时间长集中在计算操作上，也就是计算密集型任务，这个就没办法了，和（1）一样吧，线程池中的线程数设置得少一些，减少线程上下文的切换
3. 并发高、业务执行时间长，解决这种类型任务的关键不在于线程池而在于整体架构的设计，看看这些业务里面某些数据是否能做缓存是第一步，增加服务器是第二步，至于线程池的设置，设置参考（2）

### 线程同步以及线程调度相关的方法

- wait()  调用该方法的线程进入 WAITING 状态，只有等待另外线程的通知或被中断才会返回，需要注意的是调用 wait()方法后，会释放对象的锁。因此，wait 方法一般用在同步方法或同步代码块中
- sleep  使一个正在运行的线程处于睡眠状态，与 wait 方法不同的是 sleep 不会释放当前占有的锁,sleep(long)会导致线程进入 TIMED-WATING 状态，而 wait()方法会导致当前线程进入 WATING 状态
- yield 线程让步  yield 会使当前线程让出 CPU 执行时间片，与其他线程一起重新竞争 CPU 时间片。一般情况下，优先级高的线程有更大的可能性成功竞争得到 CPU 时间片，但这又不是绝对的，有的操作系统对线程优先级并不敏感 
- 线程中断（interrupt） 中断一个线程，其本意是给这个线程一个通知信号，会影响这个线程内部的一个中断标识位。这个线程本身并不会因此而改变状态(如阻塞，终止等)
  - 调用 interrupt()方法并不会中断一个正在运行的线程。也就是说处于 Running 状态的线程并不会因为被中断而被终止，仅仅改变了内部维护的中断标识位而已
  - 若调用 sleep()而使线程处于 TIMED-WATING 状态，这时调用 interrupt()方法，会抛出InterruptedException,从而使线程提前结束 TIMED-WATING 状态
  - 许多声明抛出 InterruptedException 的方法(如 Thread.sleep(long mills 方法))，抛出异常前，都会清除中断标识位，所以抛出异常后，调用 isInterrupted()方法将会返回 false
  - 中断状态是线程固有的一个标识位，可以通过此标识位安全的终止线程。比如,你想终止一个线程thread的时候，可以调用thread.interrupt()方法，在线程的run方法内部可以根据 thread.isInterrupted()的值来优雅的终止线程
- Join    等待其他线程终止  join() 方法，等待其他线程终止，在当前线程中调用一个线程的 join() 方法，则当前线程转为阻塞状态，回到另一个线程结束，当前线程再由阻塞状态变为就绪状态，等待 cpu 的宠幸
- notify 唤醒一个处于等待状态的线程，当然在调用此方法时，并不能确切的唤醒某一个等待状态的线程，而是由JVM 确定唤醒那个线程，而且和优先级无关
- notifyAll  唤醒所有处于等待状态的线程，该方法并不是将对象的锁给所有的线程，而是让他们竞争，只有获得锁的线程，才能进入就绪状态

### 线程上下文切换

巧妙地利用了时间片轮转的方式, CPU 给每个任务都服务一定的时间，然后把当前任务的状态保存下来，在加载下一任务的状态后，继续服务下一任务，任务的状态保存及再加载, 这段过程就叫做上下文切换。时间片轮转的方式使多个任务在同一颗 CPU 上执行变成了可能

1. 上下文

   是指某一时间点 CPU 寄存器和程序计数器的内容

2. 寄存器

   是 CPU 内部的数量较少但是速度很快的内存（与之对应的是 CPU 外部相对较慢的 RAM 主内存）。寄存器通过对常用值（通常是运算的中间值）的快速访问来提高计算机程序运行的速度

3. 程序计数器

   是一个专用的寄存器，用于表明指令序列中 CPU 正在执行的位置，存的值为正在执行的指令的位置或者下一个将要被执行的指令的位置，具体依赖于特定的系统

4. PCB-“切换桢

   上下文切换可以认为是内核（操作系统的核心）在 CPU 上对于进程（包括线程）进行切换，上下文切换过程中的信息是保存在进程控制块（PCB, process control block）中的。PCB 还经常被称作“切换桢”（switchframe）。信息会一直保存到 CPU 的内存中，直到他们被再次使用

5. 上下文切换的活动

   1. 挂起一个进程，将这个进程在 CPU 中的状态（上下文）存储于内存中的某处
   2. 在内存中检索下一个进程的上下文并将其在 CPU 的寄存器中恢复、
   3. 跳转到程序计数器所指向的位置（即跳转到进程被中断时的代码行），以恢复该进程在程序中

6. 引起线程上下文切换 引起线程上下文切换的原因

   1. 当前执行任务的时间片用完之后，系统 CPU 正常调度下一个任务
   2. 当前执行任务碰到 IO 阻塞，调度器将此任务挂起，继续下一任务
   3. 多个任务抢占锁资源，当前任务没有抢到锁资源，被调度器挂起，继续下一任务
   4. 用户代码挂起当前任务，让出 CPU 时间
   5. 硬件中断

# JVM



![image-20210805172149574](https://gitee.com/Sean0516/image/raw/master/img/image-20210805172149574.png)

### JRE JDK JVM 以及JIT 之间有什么不同

JRE 代表 Java 运行时（Java run-time），是运行 Java 引用所必须的。JDK 代表 Java 开发工具（Java development kit），是 Java 程序的开发工具，如 Java编译器，它也包含 JRE。JVM 代表 Java 虚拟机（Java virtual machine），它的责任是运行 Java 应用。JIT 代表即时编译（Just In Time compilation），当代执行的次数超过一定的阈值时，会将 Java 字节码转换为本地代码，如，主要的热点代码会被准换为本地代码，这样有利大幅度提高 Java 应用的性

![image-20210630140457284](http://qvi33264o.hn-bkt.clouddn.com/img/image-20210630140457284.png)

### 程序计数器（线程私有）

一块较小的内存空间, 是当前线程所执行的字节码的行号指示器，每条线程都要有一个独立的程序计数器，这类内存也称为“线程私有” 的内存。

正在执行 java 方法的话，计数器记录的是虚拟机字节码指令的地址（当前指令的地址） 。如果还是 Native 方法，则为空。

这个内存区域是唯一一个在虚拟机中没有规定任何 OutOfMemoryError 情况的区域

### 虚拟机栈（线程私有）

是描述java方法执行的内存模型，每个方法在执行的同时都会创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈、动态链接、方法出口等信息。 每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。

栈帧（ Frame）是用来存储数据和部分过程结果的数据结构，同时也被用来处理动态链接(Dynamic Linking)、 方法返回值和异常分（DispatchException）。 栈帧随着方法调用而创建，随着方法结束而销毁——无论方法是正常完成还是异常完成（抛出了在方法内未被捕获的异常）都算作方法结束。

### 本地方法区(线程私有)

本地方法区和 Java Stack 作用类似, 区别是虚拟机栈为执行 Java 方法服务, 而本地方法栈则为Native 方法服务, 如果一个 VM 实现使用 C-linkage 模型来支持 Native 调用, 那么该栈将会是一个C 栈，但 HotSpot VM 直接就把本地方法栈和虚拟机栈合二为一

### 堆（Heap-线程共享） -运行时数据区

是被线程共享的一块内存区域， 创建的对象和数组都保存在 Java 堆内存中，也是垃圾收集器进行垃圾收集的最重要的内存区域。 由于现代VM 采用分代收集算法, 因此 Java 堆从 GC 的角度还可以细分为: 新生代(Eden 区、 From Survivor 区和 To Survivor 区)和老年代

### 方法区/永久代（线程共享）

即我们常说的永久代(Permanent Generation), 用于存储被 JVM 加载的类信息、 常量、 静态变量、 即时编译器编译后的代码等数据.HotSpot VM把GC分代收集扩展至方法区, 即使用Java堆的永久代来实现方法区, 这样 HotSpot 的垃圾收集器就可以像管理 Java 堆一样管理这部分内存,而不必为方法区开发专门的内存管理器(永久带的内存回收的主要目标是针对常量池的回收和类型的卸载, 因此收益一般很小) 。



运行时常量池（Runtime Constant Pool）是方法区的一部分。 Class 文件中除了有类的版本、字段、方法、接口等描述等信息外，还有一项信息是常量池 （Constant Pool Table），用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。 Java 虚拟机对 Class 文件的每一部分（自然也包括常量池）的格式都有严格的规定，每一个字节用于存储哪种数据都必须符合规范上的要求，这样才会被虚拟机认可、装载和执行

### Java 中堆和栈有什么区别

JVM 中堆和栈属于不同的内存区域，使用目的也不同。栈常用于保存方法帧和局部变量，而对象总是在堆上分配。栈通常都比堆小，也不会在多个线程之间共享，而堆被整个 JVM 的所有线程共享

###### 栈

在函数中定义的一些基本类型的变量和对象的引用变量都是在函数的栈内存中分配，当在一段代码块定义一个变量时，Java 就在栈中为这个变量分配内存空间，当超过变量的作用域后，Java 会自动释放掉为该变量分配的内存空间，该内存空间可以立即被另作它用

###### 堆

堆内存用来存放由 new 创建的对象和数组，在堆中分配的内存，由 Java 虚拟机的自动垃圾回收器来管理。在堆中产生了一个数组或者对象之后，还可以在栈中定义一个特殊的变量，让栈中的这个变量的取值等于数组或对象在堆内存中的首地址，栈中的这个变量就成了数组或对象的引用变量，以后就可以在程序中使用栈中的引用变量来访问堆中的数组或者对象，引用变量就相当于是为数组或者对象起的一个名称

### 垃圾回收器的基本原理是什么

对于GC来说，当程序员创建对象时，GC就开始监控这个对象的地址、大小以及使用情况。通常，GC采用有向图的方式记录和管理堆(heap)中的所有对象。通过这种方式确定哪些对象是"可达的"，哪些对象是"不可达的"。当GC确定一些对象为"不可达"时，GC就有责任回收这些内存空间

### 新生代

是用来存放新生的对象。一般占据堆的 1/3 空间。由于频繁创建对象，所以新生代会频繁触发MinorGC 进行垃圾回收。新生代又分为 Eden区、 ServivorFrom、 ServivorTo 三个区

###### Eden 区

Java 新对象的出生地（如果新创建的对象占用内存很大，则直接分配到老年代）。当 Eden 区内存不够的时候就会触发 MinorGC，对新生代区进行一次垃圾回收

###### Servivor From

上一次 GC 的幸存者，作为这一次 GC 的被扫描者

###### Servivor To

保留了一次 MinorGC 过程中的幸存者

###### MinorGC 的过程（复制->清空->互换）

1. 首先，把 Eden 和 ServivorFrom 区域中存活的对象复制到 ServicorTo 区域（如果有对象的年龄以及达到了老年的标准，则赋值到老年代区），同时把这些对象的年龄+1（如果 ServicorTo 不够位置了就放到老年区）
2. 然后，清空 Eden 和 ServicorFrom 中的对象
3. 最后， ServicorTo 和 ServicorFrom 互换，原 ServicorTo 成为下一次 GC 时的 ServicorFrom区

### 老年代

   主要存放应用程序中生命周期长的内存对象。
   老年代的对象比较稳定，所以 MajorGC 不会频繁执行。在进行 MajorGC 前一般都先进行了一次 MinorGC，使得有新生代的对象晋身入老年代，导致空间够用时才触发。当无法找到足够大的连续空间分配给新创建的较大对象时也会提前触发一次 MajorGC 进行垃圾回收腾出空间。

   MajorGC 采用标记清除算法：首先扫描一次所有老年代，标记出存活的对象，然后回收没有标记的对象。 ajorGC 的耗时比较长，因为要扫描再回收。MajorGC 会产生内存碎片，为了减少内存损耗，我们一般需要进行合并或者标记出来方便下次直接分配。当老年代也满了装不下的时候，就会抛出OOM（Out of Memory）异常。

   

### 永久代 与 元数据（JAVA8）

指内存的永久保存区域，主要存放 Class 和 Meta（元数据）的信息,Class 在被加载的时候被放入永久区域， 它和和存放实例的区域不同,GC不会在主程序运行期对永久区域进行清理。所以这也导致了永久代的区域会随着加载的 Class 的增多而胀满，最终抛出 OOM 异常



在 Java8 中， 永久代已经被移除，被一个称为“元数据区”（元空间）的区域所取代。元空间的本质和永久代类似，元空间与永久代之间最大的区别在于： 元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存限制。 类的元数据放入nativememory, 字符串池和类的静态变量放入 java 堆中， 这样可以加载多少类的元数据就不再由MaxPermSize 控制, 而由系统的实际可用空间来控制。

### 新生代、老年代、持久代都存储哪些东西

###### 新生代

方法中new 一个对象，就会先进入新生代

###### 老年代

1. 新生代中经历了N次垃圾回收仍然存活的对象就会被放到老年代中
2. 大对象一般直接放入老年代
3. 当Survivor空间不足。需要老年代担保一些空间，也会将对象放入老年代

###### 永久代

方法区/ 元数据

![image-20210805172857989](https://gitee.com/Sean0516/image/raw/master/img/image-20210805172857989.png)

### 永久代中是否回发生垃圾回收

垃圾回收不会发生在永代，如果永久代满了或者超过了临界值，会触发 Full  GC   Java 8 中已经移除了永久代，新加了一个叫做元数据区的native 内存区

### Full GC Major GC Minor GC 之间的区别

###### Minor GC 

新生代空间的垃圾回收

###### Major GC 

老年代垃圾回收 ，出现MajorGC 通常会出现至少一次的Minor GC

###### Full GC 

正对整个新生代 老年代  元空间 的全局范围GC

### 如何判断对象可以被回收

1. 引用计数法

   在 Java 中，引用和对象是有关联的。如果要操作对象则必须用引用进行。因此，很显然一个简单的办法是通过引用计数来判断一个对象是否可以回收。

   

   所谓引用计数法就是给每一个对象设置一个引用计数器，每当有一个地方引用这个对象时，就将计数器加一，引用失效时，计数器就减一。当一个对象的引用计数器为零时，说明此对象没有被引用，也就是“死对象”,将会被垃圾回收

   

   引用计数法有一个缺陷就是无法解决循环引用问题，也就是说当对象 A 引用对象 B，对象B 又引用者对象 A，那么此时 A,B 对象的引用计数器都不为零，也就造成无法完成垃圾回收，所以主流的虚拟机都没有采用这种算法

2. 可达性分析

   为了解决引用计数法的循环引用问题， Java 使用了可达性分析的方法。通过一系列的“GC roots”对象作为起点搜索。如果在“GC roots”和一个对象之间没有可达路径，则称该对象是不可达的。要注意的是，不可达对象不等价于可回收对象， 不可达对象变为可回收对象至少要经过两次标记过程。两次标记后仍然是可回收对象，则将面临回收

### Java 垃圾回收机制

在jvm 中，有一个垃圾回收线程，它是低优先级的，在正常情况下是不会执行的，只有在虚拟机空闲或者当前堆内存不足时，才会触发执行，扫面那些没有被任何引用的对象，并将它们添加到要回收的集合中，进行回收

### 垃圾回收算法

- 标记清除算法

  最基础的垃圾回收算法，分为两个阶段，标注和清除。标记阶段标记出所有需要回收的对象，清除阶段回收被标记的对象所占用的空间。  该算法最大的问题是内存碎片化严重，后续可能发生大对象不能找到可利用空间的问题

- 复制算法（copying）

  为了解决 Mark-Sweep 算法内存碎片化的缺陷而被提出的算法。按内存容量将内存划分为等大小的两块。每次只使用其中一块，当这一块内存满后将尚存活的对象复制到另一块上去，把已使用的内存清掉

  这种算法虽然实现简单，内存效率高，不易产生碎片，但是最大的问题是可用内存被压缩到了原本的一半。且存活对象增多的话， Copying算法的效率会大大降低    （算法进行改进。将内存划分为 8:1:1 ） 较大那份内存交 Eden 区，其余是两块较小的内存区叫 Survior 区。每次都会优先使用 Eden 区，若 Eden 区满，就将对象复制到第二块内存区上，然后清除 Eden 区

- 标记整理算法(Mark-Compact)

  结合了以上两个算法，为了避免缺陷而提出。标记阶段和 Mark-Sweep 算法相同， 标记后不是清理对象，而是将存活对象移向内存的一端。然后清除端边界外的对象

- 分代收集算法

  分代收集法是目前大部分 JVM 所采用的方法，其核心思想是根据对象存活的不同生命周期将内存划分为不同的域，一般情况下将 GC 堆划分为老生代(Tenured/Old Generation)和新生代(YoungGeneration)。老生代的特点是每次垃圾回收时只有少量对象需要被回收，没有额外的空间进行分配担保，所以可以采用标记--整理和标记-清除   新生代的特点是每次垃圾回收时都有大量垃圾需要被回收，因此可以采用复制算法

### 新生代与复制算法

目前大部分 JVM 的 GC 对于新生代都采取 Copying 算法，因为新生代中每次垃圾回收都要回收大部分对象，即要复制的操作比较少，但通常并不是按照 1： 1 来划分新生代。一般将新生代划分为一块较大的 Eden 空间和两个较小的 Survivor 空间(From Space, To Space)，每次使用Eden 空间和其中的一块 Survivor 空间，当进行回收时，将该两块空间中还存活的对象复制到另一块 Survivor 空间中

### 老年代与标记复制算法

因为对象存活率高、没有额外空间对它进行分配担保, 就必须采用“标记—清理”或“标记—整理” 算法来进行回收, 不必进行内存复制, 且直接腾出空闲内存



### GC 垃圾收集器

![image-20210630141113554](http://qvi33264o.hn-bkt.clouddn.com/img/image-20210630141113554.png)

- Serial 垃圾收集器（单线程、 复制算法）

  Serial收集器是Hotspot运行在Client模式下的默认新生代收集器, 它在进行垃圾收集时，会暂停所有的工作进程，用一个线程去完成GC工作

  新生代采用复制算法 ， 老年代采用标记-整理算法 

  特点：简单高效，适合jvm管理内存不大的情况（十兆到百兆）

- ParNew 垃圾收集器（Serial+多线程）

  ParNew收集器其实是Serial的多线程版本，回收策略完全一样，但是他们又有着不同 。所以它配合多核心的cpu效果更好，如果是一个cpu，他俩效果就差不多。（可用-XX:ParallelGCThreads参数控制GC线程数）

- CMS

  CMS(Concurrent Mark Sweep)收集器是一款具有划时代意义的收集器, 一款真正意义上的并发收集器,虽然现在已经有了理论意义上表现更好的G1收集器, 但现在主流互联网企业线上选用的仍是CMS(如Taobao),又称多并发低暂停的收集器

  ![](http://qvi33264o.hn-bkt.clouddn.com/img/image-20210729150102785.png)

  它是基于标记-清除算法实现的。整个过程分4个步骤

  1.  初始标记(CMS initial mark):仅只标记一下GC Roots能直接关联到的对象, 速度很快
  2.  并发标记(CMS concurrent mark: GC Roots Tracing过程)
  3.  重新标记(CMS remark):修正并发标记期间因用户程序继续运行而导致标记产生变动的那一部分对象的标记记录
  4.  并发清除(CMS concurrent sweep: 已死对象将会就地释放)

  初始标记、重新标记需要STW(stop the world 即：挂起用户线程)操作。因为最耗时的操作是并发标记和并发清除。所以总体上我们认为CMS的GC与用户线程是并发运行的

  ###### 优点 

  并发收集、低停顿

  ###### 缺点

  1. CMS默认启动的回收线程数=(CPU数目+3)*4  当CPU数>4时, GC线程最多占用不超过25%的CPU资源, 但是当CPU数<=4时, GC线程可能就会过多
     的占用用户CPU资源, 从而导致应用程序变慢, 总吞吐量降低
  2. 无法清除浮动垃圾（GC运行到并发清除阶段时用户线程产生的垃圾），因为用户线程是需要内存的，如果浮动垃圾施放不及时，很可能就造成内存溢出，所以CMS不能像别的垃圾收集器那样等老年代几乎满了才触发，CMS提供了参数 -XX:CMSInitiatingOccupancyFraction 来设置GC触发百分比(1.6后默认92%),当然我们还得设置启用该策略 -XX:+UseCMSInitiatingOccupancyOnly
  3. 因为CMS采用标记-清除算法，所以可能会带来很多的碎片，如果碎片太多没有清理，jvm会因为无法分配大对象内存而触发GC，因此CMS提供了 -XX:+UseCMSCompactAtFullCollection 参数，它会在GC执行完后接着进行碎片整理，但是又会有个问题，碎片整理不能并发，所以必须单线程去处理，所以如果每次GC完都整理用户线程stop的时间累积会很长，所以XX:CMSFullGCsBeforeCompaction 参数设置隔几次GC进行一次碎片整理

- G1 

  G1最大的特点是引入分区的思路，弱化分代的概念，合理利用垃圾收集各个周期的资源，解决了其他收集器甚至CMS的众多缺陷

  因为每个区都有E、S、O代，所以在G1中，不需要对整个Eden等代进行回收，而是寻找可回收对象比较多的区，然后进行回收（虽然也需要STW操作，但是花费的时间是很少的），保证高效率

  ###### 新生代收集

  G1的新生代收集跟ParNew类似，如果存活时间超过某个阈值，就会被转移到S/O区。年轻代内存由一组不连续的heap区组成, 这种方法使得可以动态调整各代区域的大小

  ###### 老年代收集

  1. 初始标记 (Initial Mark: Stop the World Event)

     在G1中, 该操作附着一次年轻代GC, 以标记Survivor中有可能引用到老年代对象的Regions

  2. 扫描根区域 (Root Region Scanning: 与应用程序并发执行)  

     扫描Survivor中能够引用到老年代的references. 但必须在Minor GC触发前执行完

  3. 并发标记 (Concurrent Marking : 与应用程序并发执行)
     在整个堆中查找存活对象, 但该阶段可能会被Minor GC中断

  4. 重新标记 (Remark : Stop the World Event)
     完成堆内存中存活对象的标记. 使用snapshot-at-the-beginning(SATB, 起始快照)算法, 比CMS所用算法要快得多(空Region直接被移除并回收, 并计算所有区域的活跃度).

  5. 清理 (Cleanup : Stop the World Event and Concurrent)
     在含有存活对象和完全空闲的区域上进行统计(STW)、擦除Remembered Sets(使用RememberedSet来避免扫描全堆，每个区都有对应一个Set用来记录引用信息、读写操作记录)(STW)、重置空regions并将他们返还给空闲列表(free list)(Concurrent)

  

### JVM 类加载机制

类加载机制，是指虚拟机把描述类的数据从 Class 文件加载到内存，并对数据进行校验，解析和初始化，最终形成可以被虚拟机直接使用的 java 类型

JVM 类加载机制分为五个部分： 加载 ，验证，准备，解析， 初始化

###### 加载

加载是类加载过程中的一个阶段， 这个阶段会在内存中生成一个代表这个类的 java.lang.Class 对象， 作为方法区这个类的各种数据的入口。注意这里不一定非得要从一个 Class 文件获取，这里既可以从 ZIP 包中读取（比如从 jar 包和 war 包中读取），也可以在运行时计算生成（动态代理），也可以由其它文件生成（比如将 JSP 文件转换成对应的 Class 类）  在这一阶段，将完成以下三件事：

1. 通过一个类的全限定名获取该类的二进制流
2. 将该二进制流中的静态存储结构转换为方法去进行时数据结构
3. 在内存中生成该类的Class 对象，作为该类的数据访问入库

###### 验证

这一阶段的主要目的是为了确保 Class 文件的字节流中包含的信息是否符合当前虚拟机的要求，并且不会危害虚拟机自身的安全，该阶段主要完成以下四种验证

1. 文件格式验证： 验证字节流是否符合 Class 文件的规范，如主次版本号是否在当前虚拟机范围内，常量池中的常量是否有不被支持的类型
2. 元数据验证:对字节码描述的信息进行语义分析，如这个类是否有父类，是否集成了不被继承的类等
3. 字节码验证：是整个验证过程中最复杂的一个阶段，通过验证数据流和控制流的分析，确定程序语义是否正确，主要针对方法体的验证。如：方法中的类型转换是否正确，跳转指令是否正确等
4. 符号引用验证：这个动作在后面的解析过程中发生，主要是为了确保解析动作能正确执行

###### 准备

准备阶段是为类的静态变量分配内存并将其初始化为默认值，这些内存都将在方法区中进行分配。准备阶段不分配类中的实例变量的内存，实例变量将会在对象实例化时随着对象一起分配在 Java 堆中

```java
public static int value=123; //在准备阶段 value 初始值为 0 。在初始化阶段才会变为 123 
```

###### 解析

该阶段主要完成符号引用到直接引用的转换动作。解析动作并不一定在初始化动作完成之前，也有可能在初始化之后

###### 初始化

初始化阶段是类加载最后一个阶段，前面的类加载阶段之后，除了在加载阶段可以自定义类加载器以外，其它操作都由 JVM 主导。到了初始阶段，才开始真正执行类中定义的 Java 程序代码

初始化阶段是执行类构造器<client>方法的过程。<client>方法是由编译器自动收集类中的类变量的赋值操作和静态语句块中的语句合并而成的。虚拟机会保证子<client>方法执行之前，父类的<client>方法已经执行完毕，如果一个类中没有对静态变量赋值也没有静态语句块，那么编译器可以不为这个类生成<client>()方法



### JVM 加载Class 文件的原理机制

类（Class）只有被加载到 JVM 后才能运行。当运行指定程序时，JVM 会将编译生成的 .class 文件按照需求和一定的规则加载到内存中，并组织成为一个完整的 Java 应用程序。这个加载过程是由类加载器完成，具体来说，就是由 ClassLoader 和它的子类来实现的。类加载器本身也是一个类，其实质是把类文件从硬盘读取到内存中

类的加载方式分为隐式加载和显示加载。隐式加载指的是程序在使用 new 等方式创建对象时，会隐式地调用类的加载器把对应的类加载到 JVM 中。显示加载指的是通过直接调用 class.forName()方法来把所需的类加载到 JVM 中

### 类加载器

虚拟机设计团队把加载动作放到 JVM 外部实现，以便让应用程序决定如何获取所需的类， JVM 提供了4 种类加载器：

###### 启动类加载器(Bootstrap ClassLoader)

负责加载 JAVA_HOME\lib 目录中的， 或通过-Xbootclasspath 参数指定路径中的， 且被虚拟机认可（按文件名识别， 如 rt.jar） 的类。

###### 扩展类加载器(Extension ClassLoader)

负责加载 JAVA_HOME\lib\ext 目录中的，或通过 java.ext.dirs 系统变量指定路径中的类库。

###### 应用程序类加载器(Application ClassLoader)：

负责加载用户路径（classpath）上的类库。JVM 通过双亲委派模型进行类的加载， 当然我们也可以通过继承 java.lang.ClassLoader实现自定义的类加载器。

###### 用户自定义类加载器

通过继承 java.lang.ClassLoader 类的方式实现

![image-20210630142521914](http://qvi33264o.hn-bkt.clouddn.com/img/image-20210630142521914.png)

### 双亲委派机制

当一个类收到了类加载请求，他首先不会尝试自己去加载这个类，而是把这个请求委派给父类去完成，每一个层次类加载器都是如此，因此所有的加载请求都应该传送到启动类加载其中，只有当父类加载器反馈自己无法完成这个请求的时候（在它的加载路径下没有找到所需加载的Class）， 子类加载器才会尝试自己去加载。

采用双亲委派的一个好处是比如加载位于 rt.jar 包中的类 java.lang.Object，不管是哪个加载器加载这个类，最终都是委托给顶层的启动类加载器进行加载，这样就保证了使用不同的类加载器最终得到的都是同样一个 Object 对象

![image-20210630142606545](http://qvi33264o.hn-bkt.clouddn.com/img/image-20210630142606545.png)

### 什么时候会触发FullGC

1. 老年代空间不足

   老年代只有在新生代对象转入及创建为大对象、大数组时才会出现不足的现象，当执行Full GC后空间仍然不足，则抛出如下错误：java.lang.OutOfMemoryError: Java heap space

2. 调用System.gc时，系统建议执行Full GC，但是不必然执行

3. 方法区空间不足

4. 通过Minor GC后进入老年代的平均大小大于老年代的剩余空间



### 对象分配规则

1. 对象优先分配在Eden区，如果Eden区没有足够的空间时，虚拟机执行一次Minor GC。
2. 大对象直接进入老年代（大对象是指需要大量连续内存空间的对象）。这样做的目的是避免在Eden区和两个Survivor区之间发生大量的内存拷贝（新生代采用复制算法收集内存）。
3. 长期存活的对象进入老年代。虚拟机为每个对象定义了一个年龄计数器，如果对象经过了1次Minor GC那么对象会进入Survivor区，之后每经过一次Minor GC那么对象的年龄加1，知道达到阀值对象进入老年区。
4. 动态判断对象的年龄。如果Survivor区中相同年龄的所有对象大小的总和大于Survivor空间的一半，年龄大于或等于该年龄的对象可以直接进入老年代。
5. 空间分配担保。每次进行Minor GC时，JVM会计算Survivor区移至老年区的对象的平均大小，如果这个值大于老年区的剩余值大小则进行一次Full GC，如果小于检查HandlePromotionFailure设置，如果true则只进行Monitor GC,如果false则进行Full GC、

### Java对象创建过程

1. 虚拟机遇到一个new 指令，首先将去检查整个指令的参数是否能在常量池中定位到这个类的符号引用，并且检查这个符号引用的类是否已经被加载--解析--初始化
2. 如果类已经被加载，那么进行第三步。 如果没有被加载，则需要先进行类的加载
3. 类加载通过后，进行新生对象的内存分配 （对象生成需要的内存大小在类加载完成后就可以完全确定）
4. 为对象分配内存。一种办法“指针碰撞”、一种办法“空闲列表”，最终常用的办法“本地线程缓冲分配(TLAB)”
5. 空间申请完毕后，JVM 需要将内存的空间都初始化为0 值，如果使用TLAB ，就可以在TLAB 分配的时候进行该进行的工作
6. 对对象头进行必要设置
7. 完成上面的步骤之后，从JVM 来看，一个对象基本完成了， 但是从Java程序代码来看，对象创建才刚刚开始，需要执行 init 方法，按照程序中设定的初始化操作初始化。 

### 简述Java的对象结构

Java对象由三个部分组成：对象头、实例数据、对齐填充。

1. 对象头由两部分组成，第一部分存储对象自身的运行时数据：哈希码、GC分代年龄、锁标识状态、线程持有的锁、偏向线程ID（一般占32/64 bit）
2. 第二部分是指针类型，指向对象的类元数据类型（即对象代表哪个类）。如果是数组对象，则对象头中还有一部分用来记录数组长度。
3. 实例数据用来存储对象真正的有效信息（包括父类继承下来的和自己定义的）对齐填充：JVM要求对象起始地址必须是8字节的整数倍（8字节对齐

### 你知道哪些JVM性能调优

设定堆内存大小   -Xmx：堆内存最大限制。
设定新生代大小。 新生代不宜太小，否则会有大量对象涌入老年代
-XX:NewSize：新生代大小
-XX:NewRatio 新生代和老生代占比
-XX:SurvivorRatio：Eden 区和survivor空间的占比
设定垃圾回收器 年轻代用 -XX:+UseParNewGC 年老代用-XX:+UseConcMarkSweepGC

### OOM 异常排查

1. 使用top 查看服务器系统状态 
2. ps - aux | grep java 找出当前java 进程的pid
3. jstat  -gcutil pid interval 查看当前GC 状态
4. jmap  -histo:live pid 可用统计存活对象的分布情况，从高到低查看占据内存最多的对象
5. jmap -dump:format=b file= 文件名 [pid] 生成dump 文件
6. 使用性能分析工具对 dump 文件进行分析

### 对象是怎么从年轻代进入老年代的

1. 如果对象够老，会通过提升（Promotion）进入老年代，这一般是根据对象的年龄进行判断的
2. 动态对象年龄判定。有的垃圾回收算法，比如G1，并不要求age必须达到15才能晋升到老年代，它会使用一些动态的计算方法。
3. 分配担保。当 Survivor 空间不够的时候，就需要依赖其他内存（指老年代）进行分配担保。这个时候，对象也会直接在老年代上分配
4. 超出某个大小的对象将直接在老年代分配。不过这个值默认为0，意思是全部首选Eden区进行分配

### OOM你遇到过哪些情况，SOF你遇到过哪些情况

1. OutOfMemoryError异常
2. 虚拟机栈和本地方法栈溢出
3. 运行时常量池溢出
4. 方法区溢出
5. 堆栈溢出StackOverflow

### 哪些手段用来排查内存溢出

使用jstat命令，发现Old区在一直增长。我使用jmap命令，导出了一份线上堆栈，然后使用MAT进行分析。通过对GC Roots的分析，我发现了一个非常大的HashMap对象，这个原本是有位同学做缓存用的，但是一个无界缓存，造成了堆内存占用一直上升。后来，将这个缓存改成 guava的Cache，并设置了弱引用，故障就消失了

### 生产上如何配置垃圾收集器的



### safepoint是什么

当发生GC时，用户线程必须全部停下来，才可以进行垃圾回收，这个状态我们可以认为JVM是安全的（safe），整个堆的状态是稳定的

如果在GC前，有线程迟迟进入不了safepoint，那么整个JVM都在等待这个阻塞的线程，造成了整体GC的时间变长

