![oop](https://gitee.com/Sean0516/image/raw/master/img/oop.png)

### 面向对象的特征

封装： 将描述对象的属性和行为的代码封装在一个模块中。 

抽象： 将具体的对象抽象为类 ，分为 过程抽象和数据抽象

继承： 子类继承父类的特性和行为， 子类可以有父类的方法， 子类可以对父类进行扩展，也可也重写父类的方法

多态： 多态是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定的。 （向上转型，只有运行才能确定其对象属性）

### 类和对象的关系

类是对象的抽象，对象是类的具体，类是对象的模板，对象是类的实例

### java中有没有指针？

有指针，但是隐藏了，开发人员无法直接操作指针，由jvm来操作指针

### java中是值传递引用传递

理论上说，java都是引用传递，对于基本数据类型，传递是值的副本，而不是值本身。对于对象类型，传递是对象的引用，当在一个方法操作操作参数的时候，其实操作的是引用所指向的对象

### 形参和实参的区别

### 构造方法能不能显式调用

不能，构造方法当成普通方法调用，只有在创建对象的时候他才会被系统调用

### java 创建对象的几种方式

1. new 创建新对象
2. 通过反射机制
3. 采用clone机制
4. 通过序列化机制

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

### Java 中各种数据默认值

- Byte,short,int,long默认是都是0
- Boolean默认值是false
- Char类型的默认值是’’
- Float与double类型的默认是0.0
- 对象类型的默认值是null

### ++i  和 i++ 的区别

i++ ： 先赋值，后计算

++i : 先计算，后赋值

### 数组实例化有几种方式

- 静态实例化。 在创建数组的时候，指定数组中的元素
- 动态实例化， 实例会数组的时候，只指定数组的长度，数组中所有的元素都是数组类型的默认值

### instanceof 关键字的作用

instanceof 严格来说是Java中的一个双目运算符，用来测试一个对象是否为一个类的实例，用法为

```java
boolean result = obj instanceof Class
```

### 什么是隐式转换，什么是显式转换

显示转换就是类型强转，把一个大类型的数据强制赋值给小类型的数据；隐式转换就是大范围的变量能够接受小范围的数据；隐式转换和显
式转换其实就是自动类型转换和强制类型转换

### 深拷贝浅拷贝

深拷贝和浅拷贝是指对象的拷贝，一个对象中存在两种类型的属性，一种是基本数据类型，一种是实例对象的引用

1. 浅拷贝 是指， 只会拷贝基本数据类型的值，以及实例对象的引用地址，并不会复制一份引用地址所指向的对象进行复制，也就是浅拷贝出来的对象，内部的类属性指向的是同一个对象
2. 深拷贝是指，既拷贝基本数据类型的值，也会针对实例对象的引用地址所指向的对象进行复制。 深拷贝出来的对象，内部的类指向的不是同一个对象

### 什么是自动拆箱装箱

装箱就是自动将基本数据类型转换为包装器类型 （int-->Integer）   new Integer(6); ，底层调用: Integer.valueOf(6)

拆箱 将包装类型转换为基本类型 （Integer-> int ）int i = new Integer(6); ，底层调用 i.intValue(); 方法实现。

​	区别

1. 声明方式不同  基本类型不使用new关键字，而包装类型需要使用new关键字来**在堆中分配存储空间**
2. 存储方式及位置不同：基本类型是直接将变量值存储在栈中，而包装类型是将对象放在堆中，然后通过引用来使用
3. 初始值不同：基本类型的初始值如int为0，boolean为false，而包装类型的初始值为null
4. 使用方式不同：基本类型直接赋值直接使用就好，而包装类型在集合如Collection、Map时会使用到



```java
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

### String str=”aaa”,与String str=new String(“aaa”)一样吗？

```java
String x = "张三";
String y = "张三";
String z = new String("张三");
System.out.println(x == y); // true
System.out.println(x == z); // false
```

String x = "张三" 的方式，Java 虚拟机会将其分配到常量池中，而常量池中没有重复的元素，比如当执行“张三”时，java虚拟机会先在常量池中检索是否已经有“张三”,如果有那么就将“张三”的地址赋给变量，如果没有就创建一个，然后在赋给变量；而 String z = new String(“张三”) 则会被分到堆内存中，即使内容一样还是会创建新的对象

### 有没有可能两个不相等的对象有相同的hashcode

在产生hash 冲突时，两个不相等的对象，就会有相同的hashcode 。 当hash 冲突产生时，一般用一下方法来处理

1. 拉链法 。 每个hash 表节点都要有一个next 指针，多个哈希表节点可以用next 指针构成一个单向链表。 被分配到同一个索引上的多个节点可以用这个单向链表进行存储
2. 开发定址法  一旦发生了冲突，就去寻找下一个空的散列地址，只要散列表足够大，空的散列表地址总能找到
3. 再哈希:又叫双哈希法,有多个不同的Hash函数.当发生冲突时,使用第二个,第三个….等哈希函数计算地址,直到无冲突

### a=a+b与a+=b有什么区别吗

+= 操作符会进行隐式自动类型转换,此处a+=b隐式的将加操作的结果类型强制转换为持有结果的类型, 而a=a+b则不会自动进行类型转换.

### 两个对象值相同(x.equals(y) == true)，但却可有不同的hash code，这句话对不对

不对，如果两个对象 x 和 y 满足 x.equals(y) == true，它们的哈希码（hash code）应当相同。Java 对于 eqauls 方法和 hashCode 方法是
这样规定的：

1. 如果两个 对象相同（equals 方法返回 true），那么它们的 hashCode 值一定要相同
2. 如果两个对象的 hashCode 相同，它们并不一定相同。当然，你未必要按照要求去做，但是如果你违背了上述原则就会发现在使用容器时，相同的对象可以出现在 Set 集合中，同时增加新元素的效率会大大下降（对于使用哈希存储的系统，如果哈希码频繁的冲突将会造成存取性能急剧下降）

### 两个对象的hashcode 相同，那么equals 也一定为true 吗

不对 , 两个对象的hash code 相同  equals 不一定为true  

### char 型变量中能不能存贮一个中文汉字，为什么？

char 类型可以存储一个中文汉字，因为 Java 中使用的编码是 Unicode（不选择任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一个 char 类型占 2 个字节（16 比特），所以放一个中文是没问题的

### 当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递

是值传递。Java 语言的方法调用只支持参数的值传递。当一个对象实例作为一个参数被传递到方法中时，参数的值就是对该对象的引用。对象的属性可以在被调用过程中被改变，但对对象引用的改变是不会影响到调用者的

### 阐述静态变量和实例变量的区别

静态变量是被 static 修饰符修饰的变量，也称为类变量，它属于类，不属于类的任何一个对象，一个类不管创建多少个对象，静态变量在内存中有且仅有一个拷贝；实例变量必须依存于某一实例，需要先创建对象然后通过对象才能访问到它。静态变量可以实现让多个对象共享内存

### String s = new String(“xyz”);创建了几个字符串对象

两个或者一个， 一个是常量池的的xyz  一个是用new 创建在堆上的对象 ，如果常量池存在xyz ，则只需要在 堆上创建一个 对象

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
3. static 修饰的方法是静态方法，表示该方法属于当前类。 而不属于某个对象，静态方法也不能被重写。 可以直接用类名来调用。 
4. static 修饰变量是静态变量或者类变量。 静态变量被所有实例所共享。 不会依赖于对象， 静态变量在内存中只有一份拷贝，在JVM 加载类的时候，只为静态分配一次内存
5. static 修饰的代码块叫做静态代码块。通常用来做程序优化。静态代码块中的代码在整个类加载的时候只会执行一次

### final 在Java中的作用。

final作为Java中的关键字可以用于三个地方。用于修饰类、类属性和类方法。特征：凡是引用final关键字的地方皆不可修改

1. 被final 修饰的类不可以被继承
2. final 修饰的方法不能被重写
3. final 修饰的变量不可以被改变。 如果修饰是的引用，则引用不可变，引用指向的内容可变
4. final 修饰的方法,JVM 会尝试将其内联，以提高运行效率
5. final修饰的常量，在编译阶段会存入常量池中



### final、finalize()、finally

1. final为关键字  final为用于标识常量的关键字，final标识的关键字存储在常量池中
2. finalize 为方法  finalize()方法在Object中进行了定义，用于在对象“消失”时，由JVM进行调用用于对对象进行垃圾回收，类似于C++中的析构函数；用户自定义时，用于释放对象占用的资源（比如进行I/0操作）
3. finally 为区块标志。 用于try catch 语句中  finally{}用于标识代码块，与try{}进行配合，不论try中的代码执行完或没有执行完（这里指有异常），该代码块之中的程序必定会进行



### 抽象的（abstract）方法是否可同时是静态的（static）,是否可同时是本地方法（native），是否可同时被 synchronized修饰

都不能。抽象方法需要子类重写，而静态的方法是无法被重写的，因此二者是矛盾的。本地方法是由本地代码（如 C 代码）实现的方法，而抽象方法是没有实现的，也是矛盾的。synchronized 和方法的实现细节有关，抽象方法不涉及实现细节，因此也是相互矛盾的



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



6. 

# 泛型

### 什么是泛型

所谓泛型，顾名思义， 泛指的类型，我们提供了泛指的概念，但是具体执行的时候却可以有具体的规则来约束，使用泛型我们不必因为添加元素的类型不同而定义不同类型的集合。可以通过规则按照自己的想法来控制存储的数据类型

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

### 泛型中 extends  和 super  的区别

1. <? extends  T> 表示包括T 在内的任何T的子类
2. <? super T > 表示包亏T在内的任何T 的父类



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

### IO流的分类

#### 按照读写的单位大小来分

- 字符流  以字符为单位输入输出数据，字符流按照16位传输  其只能读取字符类型数据
- 字节流  以字节为单位输入输出数据，字节流按照8位传输 ，可以是任何类型数据 （Java代码只能以byte 数组接收）

#### 按照实际IO 操作来分

- 输入流  从文件读取到内存
- 输出流 从内存读出到文件



### 字节流如何转字符流

InputStreamReader  InputStreamWriter 

### 阻塞IO 模型 非阻塞IO模型 多路复用IO模型 信号驱动IO 模型 异步IO 模型

### BIO、NIO、AIO 有什么区别

- BIO：Block IO 同步阻塞式 IO， IO 是面向流
- NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。面向缓冲区的
- AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制

### Select poll epoll 有什么区别

他们是NIO 多路复用的三种实现机制

- select 无差别轮询所有流，找出能读出数据，或者写入数据的流，对他们进行操作，会维护一个文件描述符FD 的集合 fd_set 将fd_set 从用户空间复制到内核空间  x86 fd_set 是一个数组结构
- poll 与 select  机制相似 ，但是在fd_set 结构上进行了优化，由链表结构的pollfd 来替代 fd_set
- epoll  不再扫描所有的文件描述符 。只将用户关心的事件放在内核的一个事件表中，减少用户空间和内核空间的数据拷贝, epoll 可以理解尾 event poll ，epoll  会把那个流发生了怎么样的I/O 事件通知我们 

# NIO

### Java NIO

### 什么是NIO

NIO 是指同步非阻塞IO ，是在BIO 同步阻塞IO 上的升级。NIO 主要由 Channel(通道)， Buffer(缓冲区), Selector。三部分组成。传统 IO 基于字节流和字符流进行操作， 而 NIO 基于 Channel 和Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。 Selector(选择区)用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个线程可以监听多个数据通道。 NIO 和传统 IO 之间第一个最大的区别是， IO 是面向流的， NIO 是面向缓冲区的 



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

### 什么是反射

反射是指在运行时，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意个对象，都能够调用它的任意一个方法。在java中，只要给定类的名字，就可以通过反射机制来获得类的所有信息 。 这种动态获取信息以及动态调用对象方法的功能成为 Java 语言的反射机制 

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

