# 面试总结7——Java基础

## 基本类型

### Java随机数

1, double b = Math.random();   //返回值是[0.0，1.0)

​	生成 1~100  int I = (int) (Math.random()*100+1);

​	生成n~m之间的随机整数：int num = (int) (Math.random()*(m-n+1)+n);

2, Random类：

```java
Random r1 = new Random(); // 不带种子的构造器；
Random r2 = new Random(System.currentTimeMillis());  //获取当前时间的毫秒数作为随机数种子。种子数只是随机算法的起源数字，和生成的随机数的区间没有任何关系。
int I = r1.nextInt(n);  // [0，n)之间的随机数
int j = r2.nextInt(n);  // 随机数种子一样时，生成的随机数序列顺序是一样的。
```

3, currentTimeMillis():

### 8大基本类型（内置数据类型）：

​	boolean: 1bit，只有false和true。

​	byte：8bit，1字节，有符号的二进制补码表示 -128~127

​	short: 2字节，有符号的二进制补码表示

​	char: 2字节，unicode字符。

​	int:（默认整型） 4字节，有符号的二进制补码表示

​	float: 4字节，单精度

​	long:  8字节，有符号的二进制补码表示

​	double（默认浮点数）：8字节，双精度

包装类型: 自动包箱、自动拆箱

​	Boolean，Byte, Short, Character, Integer, Float, Long, Double.

**！！Integer包装型，整数字面量在-128~127之间时不会new新的Integer对象，而是直接引用常量池中的Integer对象。**

其他都为引用类型：

​	对象

​	数组

​	String

​	枚举

 

### 常量

​	Final修饰，一次赋值，不可修改

​	String属于常量字符串

### 类型转换   

转换从低到高：byte,short,char -> int -> long -> float -> double

1. 自动类型转换：转换前的数据类型的位数低于转换后的。
2. 强制类型转换：转换的数据类型必须是兼容的。

```java
short s1 = 1; s1=s1+1; // 错误，1默认为int型，需要强制转换。
Short s1 =1 ; s1+=1;    //正确，隐含强制类型转换。
```

## String

### 创建字符串

用字符串直接赋值

使用String的构造器来创建字符串

```java
String str = “This is String”;
String str = new String(“this is String”);
```

### String的不可变

String被final修饰，不可被继承；

没有提供任何会修改对象状态的方法；

所有域都是final，并且是private

 

Java中String类其实就是对字符数组的封装

Jdk1.6中，string的成员变量，都是private final的：

char数组value，

String在这个value数组中的起始位置offset，

String所占的字符的个数count，

String对象的哈希值的缓存hash。

Jdk1.7中，string的主要成员变量变为了，都是private final的：

char数组value（value中的所有字符都是属于String这个对象），

hash。

### 通过反射改变String

因为成员变量都是private final，所以一旦初始化不能被改变，但是value是一个引用变量，可以通过反射，获取value属性，改变访问权限，然后通过获得的value引用改变数组内容。

#### 不可变类的好处：

1. 实现了字符串池，运行时节约堆空间，不同的字符串变量都指向池中的同一个字符串。
2. 字符串可变会引起严重的安全问题，比如用户名和密码都是字符串形式传入获得数据库的连接，或者socket编程中的主机名和端口。
3. 字符串不可变可以实现多线程安全。
4. 字符串创建的时候hashcode就被缓存了，不必重新计算，所以字符串很适合做map的键。

#### 区分对象和对象的引用：

```java
String s = “ABCabc”; 
s=”123456”;
// s只是一个String对象的引用，不是对象本身，所以s的引用是可以变得。
```

### String对象的存储/String的比较

字符串常量池：jvm为了字符串的复用，减少字符串对象的重复创建。

```java
String str1 = “123”;  // 直接在字符串常量池中查找是否存在123，若存在返回引用，若不存在，新建一个string对象存入字符串常量池中。
String str2 = new String(“123”); // new关键字，直接在堆上生成一个string对象。不理会常量池中是否有这个值。
str2 = str2.intern(); // String类提供了一个native方法intern()用来将这个对象加入字符串常量池。首先也是检查字符串常量池中是否存在这个对象，若存在直接返回引用，不存在加入str2并返回。
```

### String类提供了一个native方法intern()用来将这个对象加入字符串常量池

#### 常用方法：

​	length（）

​	charAt（）

​	toLowerCase()

​	toUpperCase()

​	toCharArray()

​	trim()

#### 比较方法：

​	equals（）

​	equalsIgnoreCase()

​	compareTo()

​	compareToIgnoreCase()

​	startsWith(String prefis)

​	startsWith(String prefis,int toffset)

​	endsWith(String suffix)

​	contains(String s)

#### (String), String.valueOf(), toString()方法：

​	将非String类型的对象，转换成String

​	(String)：强制转换

​	toString()：类.toString()，比如Integer.toString(), StringBuffer.toString()等等。

​	toString()是在Object中定义的，因此，任何继承Object的类都具有这个方法。

​			使用toString()的对象不能为null，否则会抛出异常java.lang.NullPointerException。

​	String.valueOf()：

​		当参数不是类对象时，比如String.valueOf( int i), 字符数组等。

​		首先对转换的对象进行判空检测，如果为空，则返回“null”字符串，以至于不会报出空指针异常。解决了toString()使用对象不能为空的问题

 

#### String对象比较 “==”和”equals”的区别

​	用equals() 比较，不要用‘==’

​	1，对于==

​			如果作用于基本数据类型的变量，则直接比较其存储的“值”是否相等。

　		如果作用于引用类型的变量，则比较的是所指向的对象的地址。

​	2，对于equals方法，

​			equals方法不能作用于基本数据类型的变量，equals继承Object类，比较的是是否是同一个对象。

　  		如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址；

​		诸如String、Date等类对equals方法进行了重写的话，比较的是所指向的对象的内容。

​	3，Equals对于String的底层源码：

​		首先，判断地址是否相等“==”，若相等，直接返回。

​		其次，在判断第二个类是否是String类型，不是直接返回false。

​		然后，判断字符串长度是否一样，若不一样则返回。

​		最后一次比较String中的每个字符，若有不一样的则返回。
![img](https://img-blog.csdnimg.cn/20200605221132164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

#### String，StringBuilder 以及 StringBuffer

关于这三个字符串类的异同之处主要关注可变不可变、安全不安全两个方面：

​	1）StringBuffer(同步的)和String(不可变的)都是线程安全的，StringBuilder是线程不安全的；

​	2）String是不可变的，StringBuilder和StringBuffer是不可变的；

​	3）String的连接操作的底层是由StringBuilder实现的；

三者都是final的，不允许被继承；

​	StringBuilder 以及 StringBuffer 都是抽象类AbstractStringBuilder的子类，它们的接口是相同的。

## Integer

### Integer类的parseInt和valueOf的区别

​	Integer/Double/Float/.parseInt(String s, int radix)方法: 返回的是基本类型。

​	Integer/Double/Float/.valueOf(String s, int radix)方法: 返回的是包装类型, valueOf方法实际上是调用了parseInt方法。

### Integer和Int的区别

​	区别：

1. Integer是int的包装类，默认初始值是null，必须先实例化，自带很多方法可以用；int是基本数据类型，默认初始值为0，可以直接使用，能做一些基本的加减乘除赋值等操作。
2. 存储方法和位置：基本类型是直接存储变量的值保存在堆栈中能高效的存取，封装类型需要通过引用指向实例，具体的实例保存在堆中。
3. 二者交叉会出现自动装箱和拆箱操作。
4. 集合类都是装object的，用int不可以，只能用Integer，泛型定义也不可以用基本类型。

### 什么时候自动装箱和拆箱

编译期进行的自动装箱和拆箱。

1. 自动装箱： 如Integer I = 100；基本数据类型赋值给引用数据类型的时候；执行的是Integer I = Integer.valueOf(100)；valueOf对于-128到127之间的数，会进行缓存。
2. 自动拆箱：如，Integer a = new Integer(100); int m = a；基本数据类型和引用数据类型做运算的时候；执行的是int m = a.intValue()；

#### 比较

1. Int和Integer（无论new否），都为true，（integer自动拆箱为int再去比较，实际为两个int变量的比较）
2. 两个new出来的Integer，比较结果为false，（new生成的是两个对象，其内存地址不同）；
3. Integer和new出来的Integer，比较结果为false，（因为非new生成的Integer变量指向的是java常量池中的对象，而new Integer()生成的变量指向堆中新建的对象，两者在内存中的地址不同）
4. 两个非new出来的Integer，如果数值在-128~127之间，则为true，否则为false。 （valueOf()函数会对-128到127之间的数进行缓存。范围之外的数实际是new得到的，地址不同）

#### 涉及自动类型转换时的integer和int的比较：

1. equals 运算符不会进行基本类型转换
2. ==、+-*/可以进行基本类型转换

如：

```java
Integer a = 1;
Integer b = 2;
Long g = 3L;
Long h = 2L;
System.out.println(g==(a+b));   // true；a+b：自动拆箱；== 自动类型转换（int转为long）
System.out.println(g.equals(a+b));   // false； a+b：自动拆箱； equals不进行自动类型转换。
System.out.println(g.equals(a+h));   // true；a+h：自动拆箱，自动类型转换为long。
```

## Object

### Equals

​	Object中equals和==是相同的，判断是否具有相同的引用。

​	String，Integer等类对equals进行了重写，判读对象所含内容是否一致。

### Hashcode

​	对象导出的一个整数值，object中指对象的存储地址。

### 为什么重写equals就要重写hashcode

#### Hashcode三条约定：

1. Equals方法中比较所用信息没有改变时，则同一对象多次调用hashcode返回相同结果。
2. 想等对象具有相同的hashcode
3. 不想等对象不要求必须产生不同的hashcode

如果重写equals不重写hashcode，违反了第二条约定。

### toString

返回表示对象值的字符串

###  Clone()

参考：

https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247484930&amp;idx=1&amp;sn=68bcd988c1658377e288a26a8effa17d&source=41#wechat_redirect

 

根据其成员对象是否也克隆，对象克隆可以分为两种形式：深克隆 和 浅克隆 。

 

#### **1）对象创建：new创建或者clone()复制**

**区别：虽然都是创建了一个新对象，但是clone创建的对象里引用类型的数据与原对象引用类型的对象使用的是一个引用地址。所以clone()方法又叫浅拷贝。**

new: 首先去看new操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后，再调用构造函数，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。

Clone():首先分配内存，分配的内存和源对象（即调用clone方法的对象）大小相同，然后再使用原对象中对应的各个域，填充新对象的域， 填充完成之后，clone方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部 。

！！ clone复制的对象里面若有引用形实例数据是，新对象和原对象引用型实例数据是同一个引用！并没有新建。
![img](https://img-blog.csdnimg.cn/2020060522131021.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

#### 2）如果想要实现深拷贝，实现Clonable接口，覆盖并实现clone方法。

除了调用父类中的clone方法得到新的对象， 还要将该类中的引用变量也clone出来。如果只是用Object中默认的clone方法，是浅拷贝的。

下面的例子：

克隆body的时候，实例域中有一个引用型数据head，如果要使Body对象在clone时进行深拷贝， 那么就要在Body的clone方法中，将源对象引用的Head对象也clone一份。（新克隆的head和原来的head不是同一个内存了。）

浅拷贝：
![img](https://img-blog.csdnimg.cn/20200605221337433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

深拷贝

![img](https://img-blog.csdnimg.cn/20200605221512512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

#### **3）彻底的深拷贝：如果在拷贝一个对象时，要想让这个拷贝的对象和源对象完全彼此独立，那么在引用链上的每一级对象都要被显式的拷贝。**

上面的2）中实现的body拷贝还是不彻底的深拷贝，若head类中可能还有引用型数据face，则face没有实现深拷贝。

![img](https://img-blog.csdnimg.cn/2020060522152545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

所以需要在head中也要重写clone()方法，将源对象引用的face对象也clone一份。

![img](https://img-blog.csdnimg.cn/20200605221547101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

创建彻底的深拷贝是非常麻烦的，尤其是在引用关系非常复杂的情况下， 或者在引用链的某一级上引用了一个第三方的对象， 而这个对象没有实现clone方法， 那么在它之后的所有引用的对象都是被共享的。

所以彻底深拷贝，几乎是不可能实现的。

## 类

### 面向对象的特征

​	1）继承：从已有类继承信息创建新类的过程。子类自动共享父类的非私有数据和方法的机制，是一种类之间的关系，提高了软件的可重用性和扩展性。

​	2）封装：是把数据和操作数据的方法绑定起来，对数据的访问只能通过已经定义的接口。封装的目的是实现高内聚低耦合，对象是封装的基本单位。

​	3）多态：允许不同子类型的对象对同一消息做出不同的相应。分为编译时多态（重载：方法名必须相同，参数个数和类型，返回类型可以不同）和运行时多态（重写）。运行时多态需要做：方法重写（子类继承父类并重写父类中已有的方法），对象的构造（用父类型引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同表现出不同的行为）。

​	4）抽象：将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象，抽象是关注对象有哪些属性和行为，而不关注行为的细节。

### 访问修饰符的区别

​	Public: 当前类、同一包中的类、子类、其他包

​	Protected: 当前类、同一包中的类、子类

​	Default: 当前类、同一包中的类

​	Private: 当前类

#### Final关键字

1. 修饰实例域：一次初始化不能更改，修饰的是引用类型时，指引用的地址值不能修改。
2. 修饰方法：不能被重写，类的private方法隐式指为final。
3. 修饰类：不能被继承，如String，System。

### Static

创建独立于对象的域变量或方法。

随着类的加载而加载，存放在静态常量池里。

1. static方法：没有this，不能访问非静态方法和成员变量。
2. static变量：所有对象同享，内存中只有一个副本。
3. static常量：final修饰。
4. static块：提升程序性能，类初次被加载时，按顺序执行静态块，且只执行一次。
5. 静态内部类：

### 接口和抽象类的区别

#### 用法：

1. 接口只声明了方法，不实现，抽象类可以有实现
2. 接口中不能有构造函数
3. 接口中只能由静态常量，抽象类中成员标量类型任意。
4. 可以实现多个接口

#### 应用：

1. 接口主要用于定义模块之间的通信契约，提供了一个准则。
2. 而抽象类在代码实现方面发挥作用，可以实现代码的重用.

#### 构造器

​	构造器不能被继承，因此不能被重写，但是可以被重载。

 

#### 重载（Overload）和重写（Override）的区别。重载的方法能否根据返回类型进行区分？

​	重载和重写都是实现多态的方法，重载编译时多态，重写运行时多态。

​	不能只根据返回类型进行区分。

![img](https://img-blog.csdnimg.cn/20200605221621378.png)

### 内部类

#### 定义在类中的类

1. 成员内部类：可以获得外部类的引用。
2. 局部内部类：定义在一个方法或者一个作用域中的类。
3. 匿名内部类：在实现父类或者接口中的方法中同时产生一个相应的对象。
4. 静态内部类：不依赖于外部类，不能使用外部类的非静态方法或属性。

## Java集合

Java集合框架包含两种类型的容器：collection接口和map接口

collection包含三种子接口set，list，queue

![img](https://img-blog.csdnimg.cn/20200605221642679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

### List

1. 有序，可重复的集合
2. 实现类：

Arraylist：底层实现数组，查询快，增删慢，线程不安全，效率高。

Vector：底层实现数组，查询快，增删慢，线程安全，效率低。

Linkedlist：底层实现双向链表，查询慢，增删快，线程不安全，效率高。

### Set

1. 无序、不可重复的集合
2. 实现类：

HashSet：无序，不可重复，线程不安全，集合元素可以为null

LinkedHashSet：底层实现链表和哈希表，有序（顺序），不可重复，线程不安全。

TreeSet：底层使用红黑树算法，擅长于范围查询，元素有序（排序），不可重复，线程不安全。自然排序或者比较器排序实现元素排序；compareTo或compare返回值是否为0保证唯一性。

### Map

1. Key-value的键值对，key不允许重复，value可以
2. 实现类：

hashMap：采用哈希表算法，key无序且不允许重复，key判断重复的标准是：key1和key2是否equals为true，并且hashCode相等

hashtable：安全

linkedHashMap：

TreeMap：采用红黑树算法，key有序且不允许重复，key判断重复的标准是：compareTo或compare返回值是否为0

 

### Queue

1. 实现类：

Linkedlist

PriorityQueue

Queue<Object> queuesample = new LinkedList<>();

方法：

Offer()

Poll()

Size()

isEmpty();

### PriorityQueue

类 PriorityQueue

java.lang.Object

——继承者 java.util.AbstractCollection

———– 继承者 java.util.AbstractQueue

—————–继承者 java.util.PriorityQueue

 

1. 默认为最小堆
2. 不允许使用null或者不可比较对象
3. 初始容量为11，自动扩容(先申请一个更大的数组，然后复制过去)，无界
4. 线程不安全
5. 数组实现，并拥有优先级
6. 插入元素：插入最后一个，并向上调节

删除：将最后一个元素放在堆顶，向下调整。

 

方法：

1. add()和offer(): Queue接口规定二者对插入失败时的处理不同，前者在插入失败时抛出异常，后则则会返回false。对于PriorityQueue这两个方法其实没什么差别。
2. element() / peek()：获取但不删除队首元素，也就是队列中权值最小的那个元素，二者唯一的区别是当方法失败时前者抛出异常，后者返回null。
3. remove（）/ poll（）：获取并删除队首元素，区别是当方法失败时前者抛出异常，后者返回null。


PriorityQueue:  JDK1.5开始提供的新的数据结构接口，默认内部是自然排序，结果为小顶堆，也可以自定义排序器，比如下面反转比较，完成大顶堆。

PriorityQueue 一个基于优先级的无界优先级队列。优先级队列的元素按照其自然顺序进行排序，或者根据构造队列时提供的 Comparator 进行排序，具体取决于所使用的构造方法。该队列不允许使用 null 元素也不允许插入不可比较的对象(没有实现Comparable接口的对象)。

PriorityQueue 队列的头指排序规则最小那哥元素。如果多个元素都是最小值则随机选一个。

PriorityQueue 是一个无界队列，但是初始的容量(实际是一个Object[])，随着不断向优先级队列添加元素，其容量会自动扩容，无需指定容量增加策略的细节。

 

//PriorityQueue默认是小顶堆，实现大顶堆，需要反转默认排序器
![img](https://img-blog.csdnimg.cn/20200605221716252.png)

## 异常

### 异常

异常机制是指当程序出现错误后，程序如何处理。具体来说，异常机制提供了程序退出的安全通道。

Throwable：error，exception（受检异常IOException、SQLException、ClassNotFoundException，非受检异常RuntimeException）

Exception可以被程序本身处理，Error无法处理
![img](https://img-blog.csdnimg.cn/20200605221836695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

### Error

是程序无法处理的错误，表示运行应用程序中较严重问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。

如OutOfMemoryError，Java虚拟机运行错误（Virtual MachineError）、类定义错误（NoClassDefFoundError）等。这些错误是不可查的

### Exception

1. 非受检异常RuntimeException：可以选择捕获处理，也可以不处理。如空指针错误，数据下标越界，方法传递参数错误，字符串转数字错误，数据类型转换错误；算术条件异常
2. 受检异常IOException、SQLException、ClassNotFoundException：必须进行处理的异常，不处理不能编译通过。文件已结束异常，文件未找到异常。

### 异常处理机制

1. Throws：声明异常，不处理，交给上层。
2. Throw：实例化异常对象，抛出异常。
3. Try，catch，finally：捕获异常。

#### 什么时候捕获异常，什么时候向上抛出异常？

应该考虑当前作用域是否有有能力处理这一异常，如果没有，则应将该异常继续向上抛出，交由更上层的作用域来处理。

### 断言

调试方法，只用于开发测试阶段，保证正确性，异常保证健壮性

assertionError：不能捕获的异常。

Assert 条件：表达式     条件为false时，抛出异常

## 其他

### 泛型

1. 参数化类型，提高重用率，安全性（编译时检查），可读性
2. 泛型出现之前：object实现参数的任意性。缺点：强制转换，留到运行时检查。
3. 泛型类：具有一个或多个类型变量的类（方法的返回类型，域，及局部变量）

泛型方法：放在返回类型之前，与使用了泛型的成员变量区别！

#### 泛型接口：

1. 类型变量的限定：上界 extends， 下界 super，无限通配符 <?>
2. 泛型的实现：类型擦除。编译器在编译时擦除了所有泛型类型相关的信息，运行时不保存任何泛型类型相关的信息。编译为字节码时先进行类型检查，类型参数用限定类型的第一个或者object代替。

#### 自动装箱和自动拆箱

​	基本数据类型与包装器类型的自动转化

 

### 如何实现对象克隆

1. 实现cloneable接口并重写clone方法；
2. 实现serializable接口，通过对象的序列化和反序列化实现克隆

### Java反射机制

1. Java 反射机制是在运行状态中，对于任意一个类，都能够获得这个类的所有属性和方法，对于任意一个对象都能够调用它的任意一个属性和方法。
2. 这种在运行时动态的获取信息以及动态调用对象的方法的功能称为Java 的反射机制。
3. Class 类与java.lang.reflect 类库一起对反射的概念进行了支持，该类库包含了Field,Method,Constructor类(每个类都实现了Member 接口)。这些类型的对象时由JVM 在运行时创建的，用以表示未知类里对应的成员。
4. 在Java 中可以通过三种方法获取类的字节码(Class)对象：

Object 类中的getClass() 方法，想要用这种方法必须要明确具体的类并且创建该类的对象

所有数据类型都具备一个静态的属性.class 来获取对应的Class 对象。但是还是要明确到类，

通过Class.forName() 方法完成，必须要指定类的全限定名

### Java反射中Class.forName()加载类和使用ClassLoader加载类的区别

在java中Class.forName()和ClassLoader都可以对类进行加载。

ClassLoader就是遵循双亲委派模型最终调用启动类加载器的类加载器，实现的功能是“通过一个类的全限定名来获取描述此类的二进制字节流”，获取到二进制流后放到JVM中；只是将class文件加载到jvm中，不执行static的内容；

Class.forName()方法实际上也是调用的CLassLoader来实现的；forName 会初始化Class，而 loadClass 不会。

### 代理

静态代理：代理对象和被代理对象实现相同的接口或者继承相同的父类
动态代理：运行时创建一组给定接口的新类。
Cglib代理：运行时在内存中动态生成一个子类对象从而实现对目标对象功能的扩展。


# 面试总结8——Java虚拟机

##  平台无关性

Java 源文件----> 编译器-----> 字节码文件

字节码文件-----> JVM(解释器)-----> 机器码

每一种平台的解释器是不同的，但是实现的虚拟机是相同的

当一个程序从开始运行，这时虚拟机就开始实例化了，

多个程序启动就会存在多个虚拟机实例。

多个虚拟机实例之间数据不 能共享。

## Java内存区域

### 运行时数据区域

1. 程序计数器：行号指示器，用于保证线程切换
2. Java虚拟机栈：java方法执行的内存模型
3. 本地方法栈：native方法对应的，类似虚拟机栈
4. Java堆：存放对象实例，
5. 方法区：已被虚拟机加载的类信息、常量、静态变量。
6. 运行时常量池：方法区的一部分，存放编译器生成的各种字面量和符号引用。
7. 直接内存
   ![img](https://img-blog.csdnimg.cn/20200606143708700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

### 对象的创建

加载并初始化类和创建对象

 

创建对象

​	1、在堆区分配对象需要的内存：分配的内存包括本类和父类的所有实例变量，但不包括任何静态变量

​	2、对所有实例变量赋默认值：将方法区内对实例变量的定义拷贝一份到堆区，然后赋默认值

​	3、执行实例初始化代码：初始化顺序是先初始化父类再初始化子类，初始化时先执行实例代码块然后是构造方法

​	4、如果有类似于Child c = new Child()形式的c引用的话，在栈区定义Child类型引用变量c，然后将堆区对象的地址赋值给它

### 对象分配内存

​	1，指针碰撞

​	2，空闲列表

### 对象的内存布局：

1. 对象头
2. 实例数据
3. 对齐填充

### 对象的访问定位

1. 句柄
2. 直接指针访问

### OOM异常

解决方法：
修改JVM启动参数，直接增加内存。

检查错误日志，查看“OutOfMemory”错误前是否有其它异常或错误；

对代码进行走查和分析，找出可能发生内存溢出的位置。

1. java堆：创建的对象太多；检查程序，看是否有死循环或不必要地重复创建大量对象；增加Java虚拟机中Xms（初始堆大小）和Xmx（最大堆大小）参数的大小；
2. java虚拟机栈和本地方法栈
3. 方法区和运行时常量池：类定义过多，常量过多；增加java虚拟机中的XX:PermSize和XX:MaxPermSize参数的大小；
4.  程序计数器是唯一不会发生OOM异常的区域

### 内存区域控制参数及对应溢出异常

#### 辅助参数说明

​	-XX:+HeapDumpOnOutOfMemoryError 打印堆内存异常时打印出快照信息

​	-XX:+HeapDumpPath 快照输出路径

​	-Xmn指定eden区的大小

​	-XX:SurvirorRation来调整幸存区的大小

​	-XX:PretenureSizeThreshold设置进入老年代的阀值

 

#### 堆内存参数

​	-Xms：堆最小值

​	-Xmx：堆最大值

​	最小值等于最大值时，堆内存不可扩展。

#### 栈内存参数

​	-Xss：

#### 方法区参数

​	-XX:PermSize方法区内存最小值

​	-XX:MaxPermSize方法区内存最大值

#### 本机直接内存参数

​	-XX:MaxDirectMemorySize

### 垃圾回收

#### 需要回收的区域：java堆，方法区

#### 如何判断对象已死

1. 引用计数：
2. 可达性分析：

#### GC Roots包括那些对象

虚拟机栈(栈桢中的本地变量表)中的引用的对象，本地方法栈中JNI的引用的对象

，方法区中的类静态属性引用的对象，方法区中的常量引用的对象

#### 引用类型

1. 强引用
2. 软引用
3. 弱引用
4. 虚引用：它不能单独使用，必须和引用队列联合使用。虚 引用的主要作用是跟踪对象被垃圾回收的状态。

#### 垃圾回收过程中的两次标记

 

#### 回收方法区：性价比低

1. 回收废弃常量
2. 回收无用的类

#### 垃圾回收算法

1. 标记-清除
2. 复制
3. 标记-整理

#### 垃圾回收器

新生代垃圾回收：

1. Serial：单线程，复制算法，可以获得最高的单线程垃圾收集效率。
2. ParNew：serial的多线程版本，复制算法
3. Parallel Scavenge:达到可控制的吞吐量（高吞吐量为目标，即减少垃圾收集时间，就是每次垃圾收集时间短，但是收集次数多），多线程，复制算法。

老年代垃圾回收

1. Serial old：单线程，标记-整理
2. Parallel old
3. CMS
4. G1:可控制的停顿时间
   ![img](https://img-blog.csdnimg.cn/20200606143929488.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

##### CMS和G1的区别：

1. CMS是并发标记清除。以获取最短回收停顿时间为目标。只能回收老年代；产生内存碎片；无法处理浮动垃圾；
2. G1是并发标记整理。除了追求低停顿外，还能建立可预测的停顿时间模型。不分新生代和老年代，分region；
3. CMS的fullGC原因：在年轻代晋升的时候老年代没有足够的连续空间容纳；在并发过程中jvm觉得在并发过程结束前堆就会满了，需要提前触发Full GC。
4. G1的初衷就是要避免Full GC的出现，fullGC的原因：region复制整理的时候空闲的region了。

### 什么时候触发MinorGC?什么时候触发FullGC?

#### 触发MinorGC(Young GC)

虚拟机在进行minorGC之前会判断老年代最大的可用连续空间是否大于新生代的所有对象总空间

​	1、如果大于的话，直接执行minorGC

​	2、如果小于，判断是否开启HandlerPromotionFailure，没有开启直接FullGC

​	3、如果开启了HanlerPromotionFailure, JVM会判断老年代的最大连续内存空间是否大于历次晋升的大小，如果小于直接执行FullGC

​	4、如果大于的话，执行minorGC

#### 触发FullGC

老年代空间不足

YGC出现promotion failure

统计YGC发生时晋升到老年代的平均总大小大于老年代的空闲空间

显示调用System.gc

#### 新生代

是用来存放新生的对象。

一般占据堆的 1/3 空间。

由于频繁创建对象，所以新生代会频繁触发 MinorGC 进行垃圾回收。

新生代又分为 Eden 区、ServivorFrom、ServivorTo 三个区。

MinorGC采用复制算法

#### 老年代

主要存放应用程序中生命周期长的内存对象。

老年代的对象比较稳定，所以 MajorGC 不会频繁执行。

在进行 MajorGC 前一般都先进行了一次 MinorGC，使得有新生代的对象晋身入老年代，导致空间不够用时才触发。

当无法找到足够大的连续空间分配给新创建的较大对象时也会提前触发一次 MajorGC 进行垃圾回收腾出空间。

MajorGC采用标记清除算法

#### 永久代

指内存的永久保存区域，主要存放 Class 和 Meta（元数据）的信息,Class 在被加载的时候被 放入永久区域，

它和和存放实例的区域不同,GC 不会在主程序运行期对永久区域进行清理。

所以这 也导致了永久代的区域会随着加载的 Class 的增多而胀满，最终抛出 OOM 异常。

在 Java8 中，永久代已经被移除，被一个称为“元数据区”（元空间）的区域所取代。元空间的本质和永久代类似，元空间与永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用 本地内存。

#### Eden: Survivor: Survivor=8：1：1

IBM公司的专项研究表明，新生代中的对象98%都是“朝生夕死”的（即：将被回收的对象：存活的对象 > 9：1），所以如果根据复制算法完成按照1：1的比例划分新生代的内存空间，将会造成相当大的浪费。

这样即使所有的对象都不会存活，那么也只会“浪费”10%的内存空间。不过我们也无法保证存活的对象一定<2%或10%，当新生代中Survivor to区内存不够用时，就会触发老年代的担保机制进行分配担保。

#### 为什么要有survivor区

##### 1） 如果没有survivor

如果没有Survivor，Eden区每进行一次Minor GC，存活的对象就会被送到老年代。老年代很快被填满，触发Major GC（因为Major GC一般伴随着Minor GC，也可以看做触发了Full GC）。

老年代的内存空间远大于新生代，进行一次Full GC消耗的时间比Minor GC长得多。频发的Full GC消耗的时间是非常可观的，这一点会影响大型程序的执行和响应速度。

##### 2）解决

增加老年代空间：降低full gc频率，一旦full gc发生，执行需要的时间更长。

减少老年代空间：full gc所需要时间减少，full gc频率增加。

##### 3） survivor存在的意义

减少被送到老年代的对象，进而减少Full GC的发生，Survivor的预筛选保证，只有经历16次Minor GC还能在新生代中存活的对象，才会被送到老年代。

#### 为什么设置两个survivor

解决了碎片化。

##### 1，如果只有一个survivor

新建的对象在Eden中，一旦Eden满了，触发一次Minor GC，Eden中的存活对象就会被移动到Survivor区。下一次Eden满了的时候，此时在Eden和Survivor都进行Minor GC， survivor垃圾回收之后剩下的空间是不连续的，也就导致了内存碎片化。

碎片化带来的风险是极大的，严重影响JAVA程序的性能。

##### 2，两块servivor

刚刚新建的对象在Eden中，经历一次Minor GC，Eden中的存活对象就会被移动到第一块survivor1，Eden被清空；等Eden区再满了，就再触发一次Minor GC，Eden和S1中的存活对象又会被复制送入第二块survivor2，S0和Eden被清空，然后下一轮S0与S1交换角色，如此循环往复。如果对象的复制次数达到16次，该对象就会被送到老年代中。

上述机制最大的好处就是，整个过程中，永远有一个survivor space是空的，另一个非空的survivor space无碎片。

##### 3，设置多个survivor

如果Survivor区再细分下去，每一块的空间就会比较小，很容易导致Survivor区满，因此，我认为两块Survivor区是经过权衡之后的最佳方案。

#### 内存分配和回收策略（调优方法）

1. 优先
2. 大对象（通过XX:PretenureSizeThreshold设置）
3. 长期存活
4. 存活时间
5. 空间分配担保


获取GC信息的方法

​	-verbose:gc或者-XX:+PrintGC　　获取gc信息

​	-XX:+PrintGCDetails　　获取更加详细的gc信息

​	-XX:+PrintGCTimeStamps　　获取GC的频率和间隔

​	-XX:+PrintHeapAtGC　　获取堆的使用情况

​	-Xloggc:D:\gc.log　　指定日志情况的保存路径

### JVM调优

在tomcat的bin/catalina.bat文件的开头添加相关的配置。

Jstatd：jvm监控服务，提供应用程序信息

Jps：列出所有jvm实例；

Jconsole：观察java进程信息

Jstack：所有线程中断信息和状态

Jinfo：运行环境参数

Jmap：jvm物理内存信息

Jstat：类加载，编译器，gc信息

#### Jps

列举正在运行的虚拟机进程并显示主类及这些进程的唯一ID

​	Jps [option] [hostid]

​	jps -q 只输出LVMID

​	jps -m 输出JVM启动时传给主类的方法

​	jps -l 输出主类的全名，如果是Jar则输出jar的路径

​	jps -v 输出JVM的启动参数

#### Jstat

jstat主要用于监控虚拟机的各种运行状态信息，如类的装载、内存、垃圾回收、JIT编译器等，在没有GUI的服务器上，这款工具是首选的一款监控工具。

#### Jinfo

jinfo的作用是实时查看虚拟机的各项参数信息

#### Jmap

用于生成堆快照（heapdump）

#### Jstack

Jstack用于JVM当前时刻的线程快照，又称threaddump文件，它是JVM当前每一条线程正在执行的堆栈信息的集合。生成线程快照的主要目的是为了定位线程出现长时间停顿的原因，如线程死锁、死循环、请求外部时长过长导致线程停顿的原因。

#### Jconsole

在JDK的bin目录下,监控内存,thread,堆栈等

#### Jprofile

类似于jconsole,比jconsole监控信息更全面，内存，线程，包,cup 类，堆栈，等等

### 类加载机制

#### 类加载定义

Class文件加载，

#### 类加载的过程

1. 加载
2. 验证
3. 准备
4. 解析
5. 初始化
6. 使用
7. 卸载

#### 立即对类“初始化”的情况

New，静态变量，静态方法

反射调用

父类初始化

主类

#### 类的加载器（安全性）

类加载阶段中通过类的全限定名来获取此类的二进制字节流，这一过程放在jvm外部实现，实现这个代码的动作模块成为类加载器。

类加载器就是负责检索并加载其他Java类或者资源（如文件）的对象，它一般继承于java.lang.ClassLoader这个抽象类（除了BootstrapClassLoader）。

采用双亲委派的一个好处是比如加载位于 rt.jar 包中的类 java.lang.Object，不管是哪个加载 器加载这个类，最终都是委托给顶层的启动类加载器进行加载，这样就保证了使用不同的类加载 器最终得到的都是同样一个 Object 对象。

#### 类加载器的分类

1. 启动类加载器（bootstrapClassLoader）：作为Java虚拟机的一部分，它使用C++语言实现，因此不继承自ClassLoader加载器，其他的类加载都必须继承自ClassLoader加载器。在程序刚启动时就被加载进来，负责Java标准库的加载，并且只有它能完成该任务。
2. 扩展类加载器（ExtensionClassLoader）：负责加载Java_Home /lib/ext或者由系统变量 java.ext.dir指定位置中的类库
3. 应用进程类加载器（Application）：负责加载java扩展库(例如sun公司专门为连接数据库设计的JDBC的一组API)。类加载器负责加载系统类路径（CLASSPATH）中指定的类库。同时它常被称为系统（System）加载器，因为我们可以通过getSystemClassLoader()方法来获取它。
4. 自定义类加载器（User）：由程序员自己编写的类加载器被称为自定义类加载器，如果生成自定义类加载器时没有明确地指出父类加载器，会默认把应用程序（Application）类加载器作为自己的父亲。

组合关系，并非继承关系

#### 线程上下文类加载器

1. 定义：线程上下文类加载器（context class loader）是从 JDK 1.2 开始引入的。类 java.lang.Thread中的方法 getContextClassLoader()和 setContextClassLoader(ClassLoader cl)用来获取和设置线程的上下文类加载器。如果没有通过 setContextClassLoader(ClassLoader cl)方法进行设置的话，线程将继承其父线程的上下文类加载器。Java 应用运行的初始线程的上下文类加载器是系统类加载器。在线程中运行的代码可以通过此类加载器来加载类和资源。
2. 应用场景：Java提供了很多服务提供者接口（SPI，Service Provider Interface），允许独立厂商（第三方）为此提供实现。常见的SPI有：JNDI、JDBC、JAXP等。这些接口由Java的核心库来提供，所以问题就在于，SPI的接口是Java核心库的一部分，它们是由启动类加载器来加载的。SPI实现的Java类一般是由应用程序类加载器（Application ClassLoader）来加载的。启动类无法找到SPI的实现类，因为它只加载核心库（SPI的实现类由第三方提供）。它也不能代理给应用程序类加载器，因为它又是应用程序类加载器的父类，双亲委派模型又会将它交给启动类来加载。所以在这个时候我们就要“打破”这个“双亲委派模型”。
3. 使用线程上下文类加载器，可以在执行线程中抛弃双亲委派加载链模式，使用线程上下文里的类加载器加载类。

#### 双亲委派模型

如何破

1. 自定义一个类加载器，重写classLoader的loadClass方法。
2. 基础类都是右上层加载器加载的，当基础类又要调用用户的代码时，线程上下文类加载器，父类加载器请求子类加载器去完成类加载动作。
   

# 面试总结9——Java多线程

## Semaphore信号量

Semaphore 是一种基于计数的信号量。它可以设定一个阈值，基于此，多个线程竞争获取许可信号，做完自己的申请后归还，超过阈值后，线程申请许可信号将会被阻塞。Semaphore 可以用来 构建一些对象池，资源池之类的，比如数据库连接池

1. 实现互斥锁（计数器为 1）

我们也可以创建计数为 1 的 Semaphore，将其作为一种类似互斥锁的机制，这也叫二元信号量， 表示两种互斥状态。

### Semaphore 与 ReentrantLock

Semaphore 基本能完成 ReentrantLock 的所有工作，使用方法也与之类似，通过 acquire()与 release()方法来获得和释放临界资源。

默认为可响应中断锁

实现了可轮询的锁请求与定时锁的功能

提供了公平与非公平锁的机制

释放锁的操作也必须在 finally 代码块中完成。

### ReadWriteLock 读写锁

为了提高性能，Java 提供了读写锁，在读的地方使用读锁，在写的地方使用写锁，灵活控制，如果没有写锁的情况下，读是无阻塞的,在一定程度上提高了程序的执行效率。读写锁分为读锁和写 锁，多个读锁不互斥，读锁与写锁互斥，这是由 jvm 自己控制的，你只要上好相应的锁即可。

### Java中，死锁怎么避免

1. 调整申请锁的顺序：确保所有的线程都是按照相同的顺序获得锁。
2. 在尝试获取锁的时候加一个超时时间，在尝试获取锁的过程中若超过了这个时限该线程则放弃对该锁请求。若一个线程没有在给定的时限内成功获得所有需要的锁，则会进行回退并释放所有已经获得的锁，然后等待一段随机的时间再重试。ReentrantLock.tryLock()方法。
3. 缩小加锁的范围；


死锁的检测：

用jstack检测究竟时候那个线程死锁；

 

### 线程同步

同步就是协同步调，按预定的先后次序进行运行。

 

同步方法：

1. 临界区
2. 互斥量
3. 信号量
4. 信号


Java实现同步：

1. synchronized同步方法和同步代码块
2. volatile关键字
3. ReentrantLock
4. 局部变量：Threadlocall
5. 阻塞队列

### java中有几种方法可以实现一个线程？

1） 继承Thread类（java.lang.Thread）创建（本质Thread也实现了Runable接口），重写run方法，start启动线程。

2） 实现Runable接口：定义一个实现java.lang.Runnable接口的类，重写run方法，将类的对象传递给Thread构造器，调用start方法。

3） 实现Callable接口：定义一个实现callable接口的类，将类的对象传递给FutureTask构造器，再将FutureTask对象传递给Thread构造器，调用start方法启动线程，通过FutureTask对象的get方法获取线程运行的结果。

4） 使用线程池：使用executors工具类中的静态工厂方法用于闯将线程池，使用execute方法启动线程，使用shutdown方法等待提交的任务执行完成并关闭线程。

![img](https://img-blog.csdnimg.cn/20200606144551727.png)



### Callable和Runnable的区别如下：

1) Callable定义的方法是call，而Runnable定义的方法是run。

2) Callable的call方法可以有返回值，而Runnable的run方法不能有返回值。

3) Callable的call方法可抛出异常，而Runnable的run方法不能抛出异常。

FutureTask为Runnable的实现类。

### FutureTask类

FutureTask可用于异步获取执行结果或取消执行任务的场景。通过传入Runnable或者Callable的任务给FutureTask，直接调用其run方法或者放入线程池执行，之后可以在外部通过FutureTask的get方法异步获取执行结果，因此，FutureTask非常适合用于耗时的计算，主线程可以在完成自己的任务后，再去获取结果。

### Runnable实现和Thread继承的区别

1. 可以实现多个接口
2. 适合资源共享，Thread类线程是分别处理的。

### 终止线程

1，程序运行结束，线程自动结束。

2，使用退出标志退出线程。有些线程是伺服线程。

它们需要长时间的运行，只有在外部某些条件满足的情况下，才能关闭这些线程。使用一个变量来控制循环，例如： 最直接的方法就是设一个 boolean 类型的标志，并通过设置这个标志为 true 或 false 来控制 while 循环是否退出。

![img](https://img-blog.csdnimg.cn/20200606144643142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)



3，使用interrupt方法中断线程：设置中断标志位，具体怎么反应依赖于线程本身。

​	线程处于阻塞状态：抛出异常。通过代码捕获该异常，然后 break 跳出循环状态，从而让 我们有机会结束这个线程的执行。

​	线程未处于阻塞状态：使用 isInterrupted()判断线程的中断标志来退出循环。和使用自定义的标志来控制循环是一样的道理。

4，stop方法：直接中断线程不安全，可能会产生不可预料的结果，已弃用。

5，run方法完成后线程中止

 

### 中断的API支持：

1. public boolean isInterrupted(): 判断中断标志位是否被标记。
2. public void interrupte(): 设置中断标志位
3. public static Boolean interrupted(): 判断并清空中断标志位。

### 线程状态

![img](https://img-blog.csdnimg.cn/20200606144814172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)



### 线程对中断产生的反应：

1. new，terminated：无任何影响
2. runnable，blocked：设置中断标志位
3. waiting，timed waiting：抛出InterruptException异常，并清空中断标志位。、
4. synchronized在获锁的过程中不能被中断
5. 死锁状态的线程无法被中断（收不到中断信号）

### Start()和run()的区别

​	1，调用start启动一个线程，只要得到cpu就可以执行run方法。

​	2，调用run方法，就是普通的调用方法，运行在当前线程中，没有启动另一个线程。

### notify()和notifyAll()有什么区别？

1. notify方法只唤醒一个等待（对象的）线程并使该线程开始执行。所以如果有多个线程等待一个对象，这个方法只会唤醒其中一个线程，进入锁池，选择哪个线程取决于操作系统对多线程管理的实现。
2. notifyAll 会唤醒所有等待(对象的)线程（等待池中的所有线程），全部进入锁池，尽管哪一个线程将会第一个处理取决于操作系统的实现。

### wait()和sleep()的区别

1. sleep来自Thread类，wait来自Object类。
2. sleep使线程暂停执行一段时间的方法，把执行机会（CPU）让给其他线程; wait，一种使线程暂停执行的方法。
3. 调用sleep()方法的过程中，线程不会释放对象锁（监控状态monitor仍然保持）。调用 wait 方法线程会释放对象锁，进入等待此对象的等待锁定池，只有调用notify()/notifyAll方法后本线程才进入锁池，通过竞争对象锁进入运行状态。
4. sleep睡眠后不出让系统资源，sleep(milliseconds)需要指定一个睡眠时间，时间一到会自动唤醒。wait让出系统资源其他线程可以占用CPU。
5. sleep必须捕获异常。有可能被其他对象调用他的interrupt（），产生Interrupted Exception。wait，notify和notifyAll不需要捕获异常。
6. sleep可以在任何地方使用，wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用

### yield()和join()

​	1）yield()方法是停止当前线程，让同等优先权的线程或更高优先级的线程有执行的机会。如果没有的话，那么yield()方法将不会起作用，并且由可执行状态后马上又被执行。   

​	2）join方法是用于在某一个线程的执行过程中调用另一个线程执行，等到被调用的线程执行结束后，再继续执行当前线程。如：`t.join();`//主要用于等待t线程运行结束，若无此句，main则会执行完毕，导致结果不可预测。

### 什么是Daemon线程？它有什么意义？

指在程序运行的时候在后台提供一种通用服务的线程，如计时线程和垃圾回收线程。

只剩下守护线程，虚拟机退出。

永远不去访问固有资源，因为可能在任何情况下发生中断。

### java如何实现多线程之间的通讯和协作？

1. 共享内存：JMM（Java内存模型）：java线程<->工作内存<->主内存

2. 消息传递：
   syncrhoized加锁的线程的Object类的wait()/notify()/notifyAll():  Object类中声明的方法，因为每个对象都可以拥有锁；是本地方法，且为final方法，无法被重写；调用wait时，当前线程必须拥有此对象的锁，否则抛出错误。

   ReentrantLock类加锁的线程的Condition类的await()/signal()/signalAll()：实现线程间协作更加安全和高效，调用Condition的await()和signal()方法，都必须在lock保护之内。

### 为什么线程通信的方法wait(), notify()和notifyAll()被定义在Object类里？

​	Wait-notify机制是在获取对象锁的前提下不同线程间的通信机制。在Java中，任意对象都可以当作锁来使用，由于锁对象的任意性，所以这些通信方法需要被定义在Object类里。

### 为什么wait(), notify()和notifyAll()必须在同步方法或者同步块中被调用？

​	wait/notify机制是依赖于Java中Synchronized同步机制的，其目的在于确保等待线程从Wait()返回时能够感知通知线程对共享变量所作出的修改。如果不在同步范围内使用，就会抛出java.lang.IllegalMonitorStateException的异常。

### 什么是线程池？如何创建一个Java线程池？

　　一个线程池管理了一组工作线程，同时它还包括了一个用于放置等待执行的任务的队列。线程池可以避免线程的频繁创建与销毁，降低资源的消耗，提高系统的反应速度。java.util.concurrent.Executors提供了几个java.util.concurrent.Executor接口的实现用于创建线程池，其主要涉及四个角色：

线程池：Executor

工作线程：Worker线程，Worker的run()方法执行Job的run()方法

任务Job：Runable和Callable

阻塞队列：BlockingQueue

![img](https://img-blog.csdnimg.cn/20200606144915216.png)

### 构造函数的参数及意义

1. corePoolSize：核心线程池的大小，核心线程默认情况下会一直存活在线程池中，即使这个核心线程啥也不干(闲置状态)。
2. maximunPoolSize：线程池能创建最大的线程数量。
3. keepAliveTime：非核心线程能够空闲的最长时间，超过时间，线程终止。只要线程数量不超过核心线程大小，就不会起作用。但是，如果设置了  allowCoreThreadTimeOut = true，则会作用于核心线程。
4. unit：时间单位，和keepAliveTime配合使用。
5. workQueue：缓存队列，用来存放等待被执行的任务。当所有的核心线程都在干活时，新添加的任务会被添加到这个队列中等待处理，如果队列满了，则新建非核心线程执行任务。
6. threadFactory：线程工厂，用来创建线程，这是一个接口，new它的时候需要实现他的Thread newThread(Runnable r)方法。
7. handler：拒绝处理策略，线程数量大于最大线程数就会采用拒绝处理策略。主要是用来抛异常的。

### workQueue类型

​	SynchronousQueue：同步队列，这个队列接收到任务的时候，会直接提交给线程处理，而不保留它。假设所有线程都在工作怎么办？这种情况下，SynchronousQueue就会新建一个线程来处理这个任务。所以为了保证不出现（线程数达到了maximumPoolSize而不能新建线程）的错误，使用这个类型队列的时候，maximumPoolSize一般指定成Integer.MAX_VALUE，即无限大，去规避这个使用风险。

​	LinkedBlockingQueue：链表阻塞队列，这个队列接收到任务的时候，如果当前线程数小于核心线程数，则新建线程(核心线程)处理任务；如果当前线程数等于核心线程数，则进入队列等待。由于这个队列没有最大值限制，即所有超过核心线程数的任务都将被添加到队列中，这也就导致了maximumPoolSize的设定失效，因为总线程数永远不会超过corePoolSize。

​	ArrayBlockingQueue：数组阻塞队列，可以限定队列的长度（既然是数组，那么就限定了大小），接收到任务的时候，如果没有达到corePoolSize的值，则新建线程(核心线程)执行任务，如果达到了，则入队等候，如果队列已满，则新建线程(非核心线程)执行任务，又如果总线程数到了maximumPoolSize，并且队列也满了，则发生错误

​	DelayQueue：延迟队列，队列内元素必须实现Delayed接口，这就意味着你传进去的任务必须先实现Delayed接口。这个队列接收到任务时，首先先入队，只有达到了指定的延时时间，才会执行任务

### 线程池是如何处理这些批量任务？

1：如果线程数量未达到corePoolSize，则新建一个线程(核心线程)执行任务

2：如果线程数量达到了corePools，则将任务移入队列等待

3：如果队列已满，新建线程(非核心线程)执行任务

4：如果队列已满，总线程数又达到了maximumPoolSize，就会由RejectedExecutionHandler抛出异常

#### 线程数量大于最大线程数就会采用拒绝处理策略

1. AbortPolicy:丢弃任务并抛出RejectedExecutionException
2. CallerRunsPolicy：只要线程池未关闭，该策略直接在调用者线程中，运行当前被丢弃的任务。显然这样做不会真的丢弃任务，但是，任务提交线程的性能极有可能会急剧下降。
3. DiscardOldestPolicy：丢弃队列中最老的一个请求，也就是即将被执行的一个任务，并尝试再次提交当前任务。
4. DiscardPolicy：丢弃任务，不做任何处理。

#### 四种常用的线程池

1. SingleThreadExecutor：用于创建一个单线程的线程池. 此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
2. FixedThreadPool：用于创建使用固定线程数的ThreadPool，只有核心线程。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
3. CachedThreadPool：用于创建一个可缓存的线程池，无核心线程，无限大，如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60 秒不执行任务）线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。
4. ScheduledThreadPoolExecutor：用于创建一个大小无限的线程池，此线程池支持定时以及周期性执行任务的需求。核心线程池固定，大小无限的线程池。

#### 线程池状态

​	在ThreadPoolExecutor类中定义了一个volatile变量runState来表示线程池的状态，线程池有四种状态，分别为RUNNING、SHURDOWN、STOP、TERMINATED。

1. 线程池创建后处于RUNNING状态。
2. 调用shutdown后处于SHUTDOWN状态，线程池不能接受新的任务，会等待缓冲队列的任务完成。
3. 调用shutdownNow后处于STOP状态，线程池不能接受新的任务，并尝试终止正在执行的任务。
4. 当线程池处于SHUTDOWN或STOP状态，并且所有工作线程已经销毁，任务缓存队列已经清空或执行结束后，线程池被设置为TERMINATED状态。

#### 如何在 Java 线程池中提交线程？

1. execute()：ExecutorService.execute 方法接收一个 Runnable 实例

2. submit()：ExecutorService.submit() 方法返回的是 Future 对象。

#### ThreadLocal及其引发的内存泄露

​	ThreadLocal（本地线程副本）为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。

​	ThreadLocal是如何做到为每一个线程维护变量的副本的呢：在ThreadLocal类中有一个Map，用于存储每一个线程的变量副本，Map中元素的键为线程对象，而值对应线程的变量副本。


# 面试总结10——Java 线程安全

## 同步类容器和并发类容器

### 同步类容器

如古老的Vector、Hashtable。这些容器的同步功能其实都是有JDK的Collection.synchronized等工厂方法去创建实现的。其底层的机制无非就是用传统的synchronized关键字对每个公共的方法都进行同步，使得每次操作只能有一个线程去访问容器的状态。

### 并发类容器

jdk5.0以后提供了多种并发类容器来替代同步类容器从而改善性能，并发类容器时专门针对并发设计的：使用ConcurrentHashMap来代替基于散列的Hashtable，并添加了一些常见符合操作的支持；使用CopyOnWriteArrayList代替Vector，并发的CopyOnWriteArraySet，以及并发的Queue：ConcurrentLinkedQueue和LinkedBlockingQueue，前者是高性能的队列，后者是以阻塞形式的队列。具体形式的Queue还有很多如：ArrayBlockingQueue、PriorityBlockingQueue、SynchronousQueue等。

### Copy-On-Write容器

JDK中的COW容器有两种，CopyOnWriteArrayList和CopyOnWriteArraySet，COW容器非常的有用，可以在非常多的并发场景中使用到。

CopyOnWrite容器即写时复制的容器，通俗的理解是当我们往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行Copy，复制出一个新的容器，然后往新的容器里添加元素。元素添加完成之后，再将原容器的引用指向新的容器。这样做的好处是我们可以对CopyOnWrite容器进行并发的读，而不需要加锁，因为当前容器不会添加任何的元素，所以CopyOnWrite容器也是一种读写分离的思想，读和写不同的容器，非常适合读多写少的情况

### 何为线程安全的类？

在线程安全性的定义中，最核心的概念就是 正确性。当多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在主调代码中不需要任何额外的同步或协同，这个类都能表现出正确的行为，那么这个类就是线程安全的。

 

### Java中常见的线程安全的类

1，通过synchronized 关键字给方法加上内置锁来实现线程安全：

Timer，TimerTask，Vector，Stack，HashTable，StringBuffer

2，原子类Atomicxxx—包装类的线程安全类：

如AtomicLong，AtomicInteger等等，通过Unsafe 类的native方法实现线程安全的。

3，BlockingQueue 和BlockingDeque：

通过使用定义为final的ReentrantLock作为类属性显式加锁实现同步的

4，CopyOnWriteArrayList和 CopyOnWriteArraySet

ReentrantLock实现同步

5，Concurrentxxx：分段锁

6，ThreadPoolExecutor：使用了ReentrantLock显式加锁同步

### 多线程编程的三个核心概念

原子性：一个操作（有可能包含有多个子操作）要么全部执行（生效），要么全部都不执行（都不生效）。
可见性（缓存一致性问题）：当多个线程并发访问共享变量时，一个线程对共享变量的修改，其它线程能够立即看到。
顺序性：程序执行的顺序按照代码的先后顺序执行。

### 如何解决多线程并发

保证原子性：锁和同步方法/同步代码块（通过排他性进而保证了目标代码的原子性）；CAS（开销比锁小）。
保证可见性：volatile（不能保证原子性，开销比锁小）；锁和synchronized（利用happen-before原则）；final关键字；
保证顺序性：volatile(禁止指令重排)；synchronized和锁（排他性，保证某一时刻只允许一条线程操作）。
除了从应用层面保证目标代码段执行的顺序性外，JVM还通过被称为happens-before原则隐式地保证顺序性。两个操作的执行顺序只要可以通过happens-before推导出来，则JVM会保证其顺序性，反之JVM对其顺序性不作任何保证，可对其进行任意必要的重新排序以获取高效率。

### happen-before原则

Java内存模型中定义的两项操作之间的偏序关系。

A操作先行发生于操作B，是说在发生操作B之前，操作A产生的影响都能被操作B观察到。

#### 具体的定义为：

1）如果一个操作happens-before另一个操作，那么第一个操作的执行结果将对第二个操作可见，而且第一个操作的执行顺序排在第二个操作之前。

2）两个操作之间存在happens-before关系，并不意味着Java平台的具体实现必须要按照happens-before关系指定的顺序来执行。如果重排序之后的执行结果，与按happens-before关系来执行的结果一致，那么这种重排序并不非法（也就是说，JMM允许这种重排序）。

### 八大原则：

1. 单线程：在同一个线程中，书写在前面的操作happen-before后面的操作。
2. 锁的：同一个锁的unlock操作happen-before此锁的lock操作。
3. volatile的：对一个volatile变量的写操作happen-before对此变量的任意操作(当然也包括写操作了)。
4. 传递性原则：如果A操作 happen-before B操作，B操作happen-before C操作，那么A操作happen-before C操作。
5. 线程启动：同一个线程的start方法happen-before此线程的其它方法。
6. 线程中断：对线程interrupt方法的调用happen-before被中断线程的检测到中断发送的代码。
7. 线程终止：主线程A等待子线程B完成，当子线程B执行完毕后，主线程A可以看到线程B的所有操作。也就是说，子线程B中的任意操作，happens-before join()的返回。
8. 对象创建：一个对象的初始化完成先于他的finalize方法调用。

### Java内存模型

Java内存模型（JMM）规定了所有的变量都存储在主内存中，每条线程还有自己的工作内存（本地线程副本），线程的工作内存中保存了该线程中是用到的变量的主内存副本拷贝，线程对变量的所有操作都必须在工作内存中进行，而不能直接读写主内存。不同的线程之间也无法直接访问对方工作内存中的变量，线程间变量的传递均需要自己的工作内存和主存之间进行数据同步进行。

JMM是一种规范，目的是解决由于多线程通过共享内存进行通信时，存在的本地内存数据不一致、编译器会对代码指令重排序、处理器会对代码乱序执行等带来的问题。

### 内存模型怎么解决缓存一致性问题

1、通过在总线加LOCK#锁的方式。如果对总线加LOCK#锁的话，也就是说阻塞了其他CPU对其他部件访问（如内存），从而使得只能有一个CPU能使用这个变量的内存。

2、通过缓存一致性协议（Cache Coherence Protocol）。当CPU写数据时，如果发现操作的变量是共享变量，即在其他CPU中也存在该变量的副本，会发出信号通知其他CPU将该变量的缓存行置为无效状态，因此当其他CPU需要读取这个变量时，发现自己缓存中缓存该变量的缓存行是无效的，那么它就会从内存重新读取。

### 如何确保线程安全？

在Java中可以有很多方法来保证线程安全，诸如：

1）通过加锁(Lock/Synchronized)保证对临界资源的同步互斥访问；

2） volatile关键字，轻量级同步机制，但不保证互斥性原子性；**（不能确保实现线程安全！！）**

3）使用不变类 和 线程安全类(原子类，并发容器，同步容器等)。

4）利用CAS操作（非阻塞同步）

### 什么是Java Timer类？如何创建一个有特定时间间隔的任务？

Timer是一个调度器，可以用于安排一个任务在未来的某个特定时间执行或周期性执行。TimerTask是一个实现了Runnable接口的抽象类，我们需要去继承这个类来创建我们自己的定时任务并使用Timer去安排它的执行。

### 阻塞队列、非阻塞队列和普通队列

实现一个线程安全的队列有两种方式：一种是使用阻塞算法，另一种是使用非阻塞算法。

阻塞队列是与普通队列的区别有两点

1.阻塞队列获取元素时，如果队列为空，则会等待队列有元素，否则就阻塞队列（普通队列返回结果，无元素）

2.阻塞队列放入元素时，如果队列满，则等待队列，直到有空位置，然后插入。（普通队列，要么直接扩容，要么直接无法插入，不阻塞）

#### 阻塞队列和非阻塞队列区别：

1，使用阻塞算法的队列可以用一个锁（入队和出队用同一把锁）或两个锁（入队和出队用不同的锁）等方式来实现。

2，非阻塞的实现方式是使用循环CAS的方式来实现。

### 非阻塞队列ConcurrentLinkedQueue

ConcurrentLinkedQueue是一个基于链接节点的无界线程安全队列，它采用先进先出的规则对节点进行排序，当我们添加一个元素的时候，它会添加到队列的尾部；当我们获取一个元素时，它会返回队列头部的元素。

入队和出队操作均利用CAS（compare and set）更新，这样允许多个线程并发执行，并且不会因为加锁而阻塞线程，使得并发性能更好。

### 阻塞队列类型

1. ArrayBlockingQueue ：一个由数组结构组成的有界阻塞队列。
2. LinkedBlockingQueue ：一个由链表结构组成的无界阻塞队列。
3. PriorityBlockingQueue ：一个支持优先级排序的无界阻塞队列。
4. DelayQueue：一个使用优先级队列实现的无界阻塞队列。layQueue中的元素只有当其指定的延迟时间到了，才能够从队列中获取到该元素。
5. SynchronousQueue：一个不存储元素的阻塞队列。无界、无缓冲的等待队列，可以认为SynchronousQueue是一个缓存值为1的阻塞队列。
6. LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。

### 什么是阻塞队列？如何使用阻塞队列来实现生产者-消费者模型？

java.util.concurrent.BlockingQueue的特性是：当队列是空的时，从队列中获取或删除元素的操作将会被阻塞，或者当队列是满时，往队列里添加元素的操作会被阻塞。特别地，阻塞队列不接受空值，当你尝试向队列中添加空值的时候，它会抛出NullPointerException。另外，阻塞队列的实现都是线程安全的，所有的查询方法都是原子的并且使用了内部锁或者其他形式的并发控制。

### 同步容器（强一致性）

同步容器指的是 Vector、Stack、HashTable及Collections类中提供的静态工厂方法创建的类。1）Vector实现了List接口，Vector实际上就是一个数组，和ArrayList类似，但是Vector中的方法都是synchronized方法，即进行了同步措施

2）Stack也是一个同步容器，它的方法也用synchronized进行了同步，它实际上是继承于Vector类；

3）HashTable实现了Map接口，它和HashMap很相似，但是HashTable进行了同步处理，而HashMap没有。



# 面试总结11——Java 锁

## Synchronized底层实现原理

### Java对象头+Monitor

1. Synchronized 通过在对象头中（MarkWord）设置标记实现了这一目的，是由 JVM 实现的一种实现互斥同步的一种方式，JVM基于进入和退出Monitor对象来实现方法同步和代码同步，被 Synchronized 修饰过的程序块，在编译前后被编译器生成了 monitorenter 和 monitorexit 两个字节码指令。
2. 在虚拟机执行到 monitorenter 指令时，首先要尝试获取对象的锁，如果这个对象没有锁定，或者当前线程已经拥有了这个对象的锁，把锁的计数器 +1；当执行 monitorexit 指令时将锁计数器 -1；当计数器为 0 时，锁就被释放了。如果获取对象失败了，那当前线程就要阻塞等待，直到对象锁被另外一个线程释放为止。
3. 执行到该代码块时，程序的运行级别从用户态切换到内核态中完成（挂起线程和恢复线程的操作），让cpu通过操作系统指令，去调度多线程之间，谁执行代码块，谁进入阻塞状态。这样会频繁出现程序运行状态的切换，这样就会大量消耗资源，程序运行的效率低下。

为了优化synchronized机制，从java6开始引入偏向锁，和轻量级锁，尽量让多线程访问公共资源的时候，不进行程序运行状态的切换。

### 锁的四种状态

根据重量级由低到高的次序，随着锁的竞争，锁可以从偏向锁升级到轻量级锁，再升级到重量级锁（但是锁的升级是单向的，也就是说只能从低级到高级，不会出现锁降级）。

#### 1，无锁状态：

#### 2，偏向锁状态

Why：加锁和解锁不需要额外的消耗，消除了同步，提高性能；

What：本质是指向互斥量的指针，锁标志位设为01，一个线程持有锁，其他竞争时，再释放；

Where：只有一个线程进入临界区

缺点：线程间存在锁竞争时，有额外的锁撤销消耗。

#### 3，轻量级锁状态

Why：竞争的线程不会阻塞，提高程序响应速度；

What：指向栈中锁记录的指针，锁标志位设为00，线程a和b交替进入临界区；

Where：多个线程交替进入临界区，追求响应时间，同步快执行速度非常快；

缺点：得不到锁竞争的线程使用自旋消耗cpu。

#### 4，重量级锁状态

Why：线程竞争不适用自旋，不消耗cpu；

What：synchronized；

Where：多个线程进入临界区，追求吞吐量，同步快执行速度比较长；

缺点：线程阻塞，响应时间缓慢。

 ![https://img-blog.csdnimg.cn/20190620153844690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ptaDQ1OA==,size_16,color_FFFFFF,t_70](https://img-blog.csdnimg.cn/20190620153844690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ptaDQ1OA==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20200606145228650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)

### 三种锁状态详解

#### 偏向锁：偏向锁的获取方式是将对象头的 MarkWord 部分中，标记上线程ID。

1. 首先读取目标对象的markword，判断是否处于可偏向状态（可偏向标志位位1，对象头末尾标志为01）
2. 如果是可偏向状态，则尝试用CAS操作，将自己线程ID写入markword。若cas成功，则认为已经获取到该对象的偏向锁，执行同步块代码，并且在执行完同步代码块以后， 并不会尝试将 MarkWord 中的 thread ID 赋回原值。若cas失败，则说明， 有另外一个线程 Thread B 抢先获取了偏向锁。 这种状态说明该对象的竞争比较激烈， 此时需要撤销 Thread B 获得的偏向锁，将 Thread B 持有的锁升级为轻量级锁。
3. 如果是已偏向状态，则检测 MarkWord 中存储的 thread ID 是否等于当前 thread ID。若想等，则证明本线程已经获取到偏向锁，可以直接继续执行同步代码块。若不等，则证明该对象目前偏向于其他线程，需要撤销偏向锁。
4. 偏向锁的撤销：在偏向锁的获取过程中，发现了竞争时，直接将一个被偏向的对象“升级到”被加了轻量级锁的状态。操作过程：偏向锁cas更新操作失败以后， 等待到达全局安全点。通过 MarkWord 中已经存在的 Thread Id 找到成功获取了偏向锁的那个线程, 然后在该线程的栈帧中补充上轻量级加锁时， 会保存的锁记录（Lock Record）， 然后将被获取了偏向锁对象的 MarkWord 更新为指向这条锁记录的指针。

#### 轻量级锁：

1. 轻量级锁是指当锁是偏向锁的时候，却被另外的线程所访问，此时偏向锁就会升级为轻量级锁，其他线程会通过自旋（关于自旋的介绍见文末）的形式尝试获取锁，线程不会阻塞，从而提高性能
2. 轻量级锁的获取主要由两种情况：① 当关闭偏向锁功能时；② 由于多个线程竞争偏向锁导致偏向锁升级为轻量级锁。
3. 在代码进入同步块的时候，如果同步对象锁状态为无锁状态，虚拟机将首先在当前线程的栈帧中建立一个名为锁记录（Lock Record）的空间，用于存储锁对象目前的 Mark Word 的拷贝，然后将对象头中的 Mark Word 复制到锁记录中。拷贝成功后，虚拟机将使用 CAS 操作尝试将对象的 Mark Word 更新为指向 Lock Record 的指针，并将 Lock Record 里的 owner 指针指向对象的 Mark Word。如果这个更新动作成功了，那么这个线程就拥有了该对象的锁，并且对象 Mark Word 的锁标志位设置为“00”，表示此对象处于轻量级锁定状态。
4. 如果轻量级锁的更新操作失败了，虚拟机首先会检查对象的 Mark Word 是否指向当前线程的栈帧，如果是就说明当前线程已经拥有了这个对象的锁，那就可以直接进入同步块继续执行，否则说明多个线程竞争锁。
5. 若当前只有一个等待线程，则该线程将通过自旋进行等待。但是当自旋超过一定的次数时，轻量级锁便会升级为重量级锁（锁膨胀）。另外，当一个线程已持有锁，另一个线程在自旋，而此时又有第三个线程来访时，轻量级锁也会升级为重量级锁（锁膨胀）。

#### 重量级锁

1. 重量级锁是指当有一个线程获取锁之后，其余所有等待获取该锁的线程都会处于阻塞状态。
2. 重量级锁通过对象内部的监视器（monitor）实现，而其中 monitor 的本质是依赖于底层操作系统的 Mutex Lock 实现，操作系统实现线程之间的切换需要从用户态切换到内核态，切换成本非常高。
3. 简言之，就是所有的控制权都交给了操作系统，由操作系统来负责线程间的调度和线程的状态变更。而这样会出现频繁地对线程运行状态的切换，线程的挂起和唤醒，从而消耗大量的系统资源，导致性能低下。

### Synchronized锁对象

Java中的每一个对象都可以作为锁

1.对于普通同步方法，锁是当前实例对象

2.对于静态同步方法，锁是当前类的Class对象（在程序运行期间，Java运行时系统始终为所有的对象维护一个被称为运行时的类型标识，这些信息的类称为Class）

3.对于同步方法块，锁是Synchronized括号里配置的对象

当一个线程驶入访问同步块时，它首先必须得到锁。退出或者抛出异常时必须释放锁。

#### 当一个线程A进入一个对象的一个synchronized方法A后，其他线程B是否可以进此对象的其他方法B？

1） 如果方法B前加了synchronized关键字，就不能，如果没加synchronized，则能够进去。

2） 如果方法A内部调用了wait()，则可以进入其他加synchronized的方法。

3） 如果方法B加了synchronized关键字，并且没有调用wai方法，则不能。

#### 为什么说 Synchronized 是可重入锁

可重入性是锁的一个基本要求，是为了解决自己锁死自己的情况。在执行 monitorenter 指令时，如果这个对象没有锁定，或者当前线程已经拥有了这个对象的锁（而不是已拥有了锁则不能继续获取），就把锁的计数器 +1，其实本质上就通过这种方式实现了可重入性。

#### Synchronized锁优化技术（是什么，为什么）

##### 1）自旋锁(上下文切换代价大)与自适应锁：互斥锁 -> 阻塞 –> 释放CPU，线程上下文切换代价较大（挂起和恢复线程要转入内核态完成） + 共享变量的锁定时间较短 == 让线程通过自旋（忙循环）等一会儿 ；

​	缺点：自旋等待本身虽然避免了线程切换的开销，但它是要占用处理器时间的， 所以如果锁被占用的时间很短，自旋等待的效果就会非常好，反之如果锁被占用的时间很长，那么自旋的线程只会白白消耗处理器资源，带来性能的浪费。

自适应意味着自旋的时间不再固定了，而是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。

##### 2）锁粗化(一个大锁优于若干小锁)：一系列连续操作对同一对象的反复频繁加锁/解锁会导致不必要的性能损耗，建议粗化锁。一般而言，同步范围越小越好，这样便于其他线程尽快拿到锁，但仍然存在特例。

##### 3）偏向锁(有锁但当前情形不存在竞争)：消除数据在无竞争情况下的同步原语，提高带有同步但无竞争的程序性能。锁偏向于第一个获得它的线程。

##### 4）锁消除(有锁但不存在竞争，锁多余)：JVM编译优化，对一些代码上要求同步，但是被检测到不可能存在共享数据竞争的锁进行削除。

锁削除的主要判定依据来源于逃逸分析的数据支持，如果判断到一段代码中，在堆上的所有数据都不会逃逸出去被其他线程访问到，那就可以把它们当作栈上数据对待， 认为它们是线程私有的，同步加锁自然就无须进行。

#### Synchronized 是非公平锁

非公平主要表现在获取锁的行为上，并非是按照申请锁的时间前后给等待线程分配锁的，每当锁被释放后，任何一个线程都有机会竞争到锁，这样做的目的是为了提高执行性能，缺点是可能会产生线程饥饿现象。

#### 为什么说 Synchronized 是一个悲观锁？ReetrantLock乐观锁？

1）不管是否会产生竞争，任何的数据操作都必须要加锁；其他线程只能依靠阻塞来等待线程释放锁，也称未阻塞同步。

2）乐观锁的核心算法是 CAS，先进性操作，如果没有其他线程争用共享数据，那操作就成功了，如果共享数据被争用，产生了冲突，那就再进行其他的补偿措施（最常见的补偿措施就是不断地重试，直到试成功为止），这种乐观的并发策略的许多实现都不需要把线程挂起，因此这种同步被称为非阻塞同步。

#### CAS

CAS，Compare and Swap即比较并交换，设计并发算法时常用到的一种技术。CAS有3个操作数，内存值V，旧的预期值A，新值B。当且仅当预期值A和内存值V相同时，将内存值V修改为B，否则什么都不做。CAS是通过unsafe类的compareAndSwap (JNI, Java Native Interface) 方法实现的，该方法包括四个参数：第一个参数是要修改的对象，第二个参数是对象中要修改变量的偏移量，第三个参数是修改之前的值，第四个参数是预想修改后的值。

#### CAS仍然存在三大问题：

ABA问题：ABA问题的解决思路就是使用版本号。在变量前面追加上版本号，每次变量更新的时候把版本号加一。

不适用于竞争激烈的情形中：：并发越高，失败的次数会越多，CAS如果长时间不成功，会极大的增加CPU的开销。因此CAS不适合竞争十分频繁的场景。

只能保证一个共享变量的原子操作：当对一个共享变量执行操作时，我们可以使用循环CAS的方式来保证原子操作，但是对多个共享变量操作时，循环CAS就无法保证操作的原子性，这个时候就可以用锁，或者有一个取巧的办法，就是把多个共享变量合并成一个共享变量来操作。

#### ReentrantLock 底层实现原理

ReentrantLock 以及所有的基于 Lock 接口的实现类，都是通过用一个 volitile 修饰的 int 型变量，并保证每个线程都能拥有对该 int 的可见性和原子修改，其本质是基于所谓的 AQS 框架。

先通过CAS尝试获取锁。如果此时已经有线程占据了锁，那就加入CLH队列并且被挂起。当锁被释放之后，排在CLH队列队首的线程会被唤醒，然后CAS再次尝试获取锁。

#### AQS框架

1. AQS = CLH(FIFO双向队列) + volatile state + CAS。AQS就是基于CLH队列，用volatile修饰共享变量state，线程通过CAS去改变状态符，成功则获取锁成功，失败则进入等待队列，等待被唤醒。

2. 定义：AQS是AbstractQueuedSynchronizer的简称。是JDK下提供的一套用于实现基于FIFO等待队列的阻塞锁和相关的同步器的一个同步框架。这个抽象类被设计为作为一些可用原子int值来表示状态的同步器的基类，并提供了一系列的CAS操作来管理这个同步状态。例如ReentrantLock，CountdowLatch就是基于AQS实现的。

3. 使用：同步队列是AQS很重要的组成部分，它是一个双端队列，是一个虚拟的双向队列，虚拟的双向队列即不存在队列实例，仅存在节点之间的关联关系。遵循FIFO原则，主要作用是用来存放在锁上阻塞的线程，当一个线程尝试获取锁时，如果已经被占用，那么当前线程就会被构造成一个Node节点假如到同步队列的尾部，队列的头节点是成功获取锁的节点，当头节点线程释放锁时，会唤醒后面的节点并释放当前头节点的引用。

4. 对state操作的方法：final int getState(), final void setState(int newState), final Boolean compareAndSetState(int expect, int update)

5. AQS定义两种资源共享方式：Exclusive（独占，只有一个线程能执行，如ReentrantLock）和Share（共享，多个线程可同时执行，如Semaphore/CountDownLatch）。

6. 实现独占锁要重写的方法：

   ```java
   protected Boolean tryRelease(int arg)  // 释放锁
   protected Boolean tryAcquire(int arg)  //以独占方式获取获取同步状态
   protected Boolean isHeldExclusively()  //是否在独占模式下被线程占用，true为被占用。
   ```

   7，实现共享锁要重写的方法：

   ```java
   protected int tryAcquireShared(int arg) //返回值大于0则是成功占用锁，小于0则是阻塞
   protected Boolean tryReleaseShared(int arg)    //释放锁方法，返回true代表成功释放
   ```

#### 对比下 Synchronized 和 Lock 的异同

相同点：独占锁、可重入（Synchronized由jvm实现可重入，ReentrantLock基于AQS实现）

不同点：

1）Lock是一个接口（ReentrantLock是lock接口的一个实现类），而synchronized是Java中的关键字，synchronized是内置的语言实现；

2）synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；

3）Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；

4）通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。

5）Lock可以提高多个线程进行读操作的效率，既就是实现读写锁等。

5）ReentrantLock提供公平锁（等待时间最长的线程将获得锁的使用权）和非公平锁两种模式。

6）ReentrantLock通过Condition可以绑定多个条件。

7）底层实现不一样， synchronized 是同步阻塞，使用的是悲观并发策略，lock 是同步非阻塞，采用的是乐观并发策略

#### Lock的几种方法

void lock();   在等待获取锁的过程中休眠并禁止一切线程调度

void lockInterruptibly() throws InterruptedException;    在等待获取锁的过程中可被中断

boolean tryLock();     获取到锁并返回true；获取不到并返回false

boolean tryLock(long time, TimeUnit unit) throws InterruptedException;   在指定时间内等待获取锁；过程中可被中断

#### Volatile和Synchronized

1. 粒度不同，前者针对变量 ，后者锁对象和类
2. Volatile不需要加锁，比Synchronized更轻量级，并不会阻塞线程（volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
3. syn保证三大特性（原子性，可见性，顺序性），volatile不保证原子性
4. volatile标记的变量不会被编译器优化,而synchronized标记的变量可以被编译器优化（如编译器重排序的优化）

#### volatile关键字在Java中有什么作用

volatile用于多线程环境下的单次操作(单次读或者单次写)。volatile关键字不能提供原子性，不具备互斥性。volatile关键字为实例域的同步访问提供了一种免锁机制。

1） volatile的特殊规则保证了新值能立即同步到主内存，以及每次使用前立即从主内存刷新，即保证了内存的可见性，除此之外还能 禁止指令重排序。此外，synchronized关键字也可以保证内存可见性。

2） 指令重排序问题在并发环境下会导致线程安全问题，volatile关键字通过禁止指令重排序来避免这一问题。而对于Synchronized关键字，其所控制范围内的程序在执行时独占的，指令重排序问题不会对其产生任何影响，因此无论如何，其都可以保证最终的正确性。

#### 不具有原子性的体现：自增自减操作。

i++操作可以被拆分为三步：

1. 线程读取i的值
2. 计算i+1（没有刷新到内存，此时阻塞的时候）
3. 将i+1之后的值写回内存

一个变量i被volatile修饰，两个线程想对这个变量修改，都对其进行自增操作也就是i++，i++的过程可以分为三步，首先获取i的值，其次对i的值进行加1，最后将得到的新值写会到缓存中。

Volatile修饰的变量i=100，线程A，B对其进行自增操作，都进行到第二步时，线程A阻塞，B将i=101刷新回内存并通知其他线程保存的i值失效，A唤醒重新执行，但是A已经不需要再从内存中读取i值了，所以无效，A继续将i=101写回内存。

#### Volatile与原子变量的区别

Volatile只保证了可见性，原子变量既保证了可见性又保证了原子性。

##### 原子变量：

1，变量都是volatile修饰的，保证了内存可见性

2，CAS操作保证数据变量的原子性：依赖当前值得原子修改。

 

java.util.concurrent.atomic 包下提供了一些原子变量以及对应的数组。

![img](https://img-blog.csdnimg.cn/20200606145348269.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V3cTIxcXdl,size_16,color_FFFFFF,t_70)



#### Volatile的实现原理

Lock前缀指令（只实现了立即可见型）、内存屏障(分为load barrier和store barrier)

没有实现原子性：对任意单个volatile变量的读/写具有原子性，但类似于volatile++这种复合操作不具有原子性。

#### 可见性：

当变量被volatile修饰，编译成汇编时，会增加一个Lock前缀指令。作用：

写一个volatile时，JMM会把该线程对应的本地内存中的共享变量值刷新到主内存；
当读一个volatile变量时，JMM会把该线程对应的本地内存置为无效（嗅探技术），线程接下来从主内存中读取共享变量。

#### 禁止指令重排序

内存屏障是一组CPU处理指令，用来实现对内存操作的顺序限制。内存屏障之后的指令不能越过内存屏障。

#### 如何保证该线程对应的本地内存置为无效的

处理器使用嗅探技术保证它的内部缓存，系统内存和其他处理器的缓存在总线上保持一致。

每个处理器通过嗅探在总线上传播的数据来检查自己缓存的值是不是过期了，当处理器发现自己缓存行对应的内存地址被修改，就会将当前处理器的缓存行设置为无效状态， 当处理器对这个数据进行修改操作的时候，会重新从系统内存中吧数据读到处理器缓存行里。

#### 为什么指令重排序

在执行程序时为了提高性能，编译器和处理器经常会对指令进行重排序。重排序分成三种类型：

1）编译器优化的重排序。编译器在不改变单线程程序语义放入前提下，可以重新安排语句的执行顺序。

2）指令级并行的重排序。现代处理器采用了指令级并行技术来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。

3）内存系统的重排序。由于处理器使用缓存和读写缓冲区，这使得加载和存储操作看上去可能是在乱序执行。

#### 什么是死锁(Deadlock)？如何分析和避免死锁？

死锁是指两个以上的线程永远阻塞的情况，这种情况产生至少需要两个以上的线程和两个以上的资源。

　　分析死锁，我们需要查看Java应用程序的线程转储。我们需要找出那些状态为BLOCKED的线程和他们等待的资源。每个资源都有一个唯一的id，用这个id我们可以找出哪些线程已经拥有了它的对象锁。下面列举了一些JDK自带的死锁检测工具：

Jconsole：JDK自带的图形化界面工具，主要用于对 Java 应用程序做性能分析和调优。
Jstack：JDK自带的命令行工具，主要用于线程Dump分析。

VisualVM：JDK自带的图形化界面工具，主要用于对 Java 应用程序做性能分析和调优。



# 面试总结12-场景题

## 高并发解决

1. 尽量使用缓存，包括用户缓存，信息缓存等，可以大量减少与数据库的交互，提高性能。
2. 页面静态化
3. 使用高性能的服务器、高性能的数据库、高效率的编程语言、还有高性能的Web容器。
4. 使用Ngnix负载均衡
5. 优化数据库查询语句，优化数据库结构，索引，提高查询效率，数据库集群和库表散列
6. 使用线程安全的集合对象
7. 使用线程池。

## 秒杀

### 1，优化：

将请求尽量拦截在系统上游（不要让锁冲突落到数据库上去）

充分利用缓存：秒杀买票，这是一个典型的读多写少的应用场景

### 2，基本架构

（1）浏览器端，最上层，会执行到一些JS代码
（2）站点层，这一层会访问后端数据，拼html页面返回给浏览器
（3）服务层，向上游屏蔽底层数据细节，提供数据访问
（4）数据层，最终的库存是存在这里的，mysql是一个典型（当然还有会缓存）

![img](https://img-blog.csdnimg.cn/20200606150624395.png)

### 3，各层优化细节：

1）客户端（浏览器，app）：做限速

禁止重复提交请求：重复提交的话增加系统的负载

X秒之内只能提交一次请求——将请求尽量拦截在系统上游

2）站点层次：按照uid做限速，页面缓存

对uid进行请求计数和去重，一个uid，5秒只准透过1个请求。拦住for循环请求。

页面缓存：同一个uid，x秒内到达站点层的请求，均返回同一页面。

3）服务层（知道库存有多少）：按照业务做写请求队列控制流量，做数据缓存

写请求，做请求队列，每次只透有限的写请求（最多库存数量个）去数据层。读请求：cache，

分时分段售票，将流量平摊

数据粒度的优化，对于余票查询这个业务，流量大的时候，做一个粗粒度的“有票”“无票”缓存即可。不用具体到还有多少余量

业务逻辑的异步，下单业务与支付业务的分离。

## 数据库优化

### 1，优化sql和索引

(1)用慢查询日志定位执行效率低的SQL语句。

(2)用explain分析SQL的执行计划。

(3)确定问题，采取相应的优化措施，建立索引。

(4)避免全局扫描：where xx is null, !=, <>, in(可替代为between，exists), not in, or, %开头模糊索引，where中对字段进行表达式操作，

### 2，搭建缓存

缓存和数据库一致性问题

缓存的击穿，穿透，雪崩

### 3，读写分离，主从复制

### 4，分区表

### 5，垂直拆分

(1)把不常用的字段单独放在一张表。

(2)把常用的字段单独放一张表

(3)经常组合查询的列放在一张表中（联合索引）。

### 6，水平拆分：各模块间耦合性太强，成本太大

## Mysql查询缓慢原因

### 1，查询一直很慢的情况

- 没有索引或者没有用到索引
- 统计出错，走的是全表扫描
- I/O吞吐量小，形成瓶颈。
- 网络速度慢

### 2，查询偶尔很慢的情况

- 锁或者死锁
- 刷脏页