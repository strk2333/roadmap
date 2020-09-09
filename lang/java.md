# Java

























## Java8 特性

### lambda

Lambda 表达式主要用来定义行内执行的方法类型接口，例如，一个简单方法接口。在上面例子中，我们使用各种类型的Lambda表达式来定义MathOperation接口的方法。然后我们定义了sayMessage的执行。



lambda 表达式只能引用标记了 final 的外层局部变量，这就是说不能在 lambda 内部修改定义在域外的局部变量，否则会编译错误。



lambda 表达式的局部变量可以不用声明为 final，但是必须不可被后面的代码修改（即隐性的具有 final 的语义）



在 Lambda 表达式当中不允许声明一个与局部变量同名的参数或者局部变量。



### 方法引用

- **构造器引用：**它的语法是Class::new，或者更一般的Class< T >::new实例如下：
  final Car car = Car.create( Car::new );final List< Car > cars = Arrays.asList( car );
- **静态方法引用：**它的语法是Class::static_method，实例如下：
  cars.forEach( Car::collide );
- **特定类的任意对象的方法引用：**它的语法是Class::method实例如下：
  cars.forEach( Car::repair );
- **特定对象的方法引用****：**它的语法是instance::method实例如下：

final Car police = Car.create( Car::new );cars.forEach( police::follow );





### 函数式接口

lambda

@FunctionalInterface

interface GreetingService 

{

  void sayMessage(String message);

}



### 默认方法

```java
public interface Vehicle {
   default void print(){
      System.out.println("我是一辆车!");
   }
    // 静态方法
   static void blowHorn(){
      System.out.println("按喇叭!!!");
   }
}
```



### 流

```java
OptionalDouble avg = people.stream()
                .mapToInt(p -> p.getAge())
                .average();

avg.getAsDouble()
```







## ENV

JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_152.jdk/Contents/Home

CLASSPAHT=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

export JAVA_HOME

export CLASSPATH

export PATH=$JAVA_HOME/bin:$PATH:





## GC

1. 简介

​     Java GC 对JVM (Java Virtual Machine) 中的内存进行标记, 确定哪些内存需要回收, 根据一定的回收策略, 自动回收内存, 永不停息 (Nerver Stop) 地保证JVM中的内存空间, 防止出现内存泄露和溢出问题。

 本文中默认介绍的虚拟机是HotSpot.

 Java GC机制主要完成3件事: 确定*哪些内存需要回收*, 确定*什么时候需要执行GC*, *如何执行GC*. 

​     我们将从4个方面学习Java GC机制: 1. 内存是如何分配的 2. 如何保证内存不被错误回收 (哪些内存需要回收) 3. 在什么情况下执行GC以及执行GC的方式 4. 如何监控和优化GC机制

1. Java内存区域

​     程序计数器 虚拟机栈 本地方法栈 堆区 方法区 直接内存

- 程序计数器 (Program Counter Register)

​     程序计数器是一个比较小的内存区域, 用于指示当前线程所执行的字节码执行到了第几行, 可以理解为是当前线程的行号指示器. 字节码解释器在工作时, 会通过改变这个计数器的值来取下一条语句指令.

 每个程序计数器只用来记录一个线程的行号, 所以它是线程私有 (一个线程有一个程序计数器) 的。

 如果程序执行的是一个Java方法, 则计数器记录的是正在执行的虚拟机字节码指令地址. 如果正在执行的是一个本地方法 (native 由C语言编写) , 则计数器的值为Undefined, 由于程序计数器只是记录当前指令地址, 所以不存在内存溢出的情况, 因此, 程序计数器也是所有JVM内存区域中唯一一个没有定义OutOfMemoryError的区域. 

- 虚拟机栈 (JVM Stack)

​     一个线程的每个方法在执行的同时, 都会创建一个栈帧 (Statck Frame), 栈帧中存储的有局部变量表, 操作栈, 动态链接, 方法出口等. 当方法被调用时, 栈帧在JVM栈中入栈, 当方法执行完成时, 栈帧出栈.

 局部变量表中存储着方法的相关局部变量, 包括各种基本数据类型, 对象的引用, 返回地址等. 在局部变量表中, 只有long和double类型会占用2个局部变量空间 (slot 对于32位机器, 一个slot就是32个bit), 其它都是1个slot. 需要注意的是, 局部变量表是在编译时就已经确定好的, 方法运行所需要分配的空间在栈帧中是完全确定的, 在方法的生命周期内都不会改变.

 虚拟机栈中定义了两种异常, 如果线程调用的栈深度大于虚拟机允许的最大深度, 则抛出StatckOverFlowError (栈溢出), 不过多数Java虚拟机都允许动态扩展虚拟机栈的大小 (有少部分是固定长度的), 所以线程可以一直申请栈, 知道内存不足, 此时, 会抛出 OutOfMemoryError (内存溢出). 

​      每个线程对应着一个虚拟机栈, 因此虚拟机栈也是线程私有的.

- 本地方法栈 (Native Method Stack)

​     本地方法栈在作用, 运行机制, 异常类型等方面都与虚拟机栈相同, 唯一的区别是: 虚拟机栈是执行Java方法的, 而本地方法栈是用来执行native方法的, 在很多虚拟机中 (如HotSpot), 会将本地方法栈与虚拟机栈放在一起使用. 

​      本地方法栈也是线程私有的.

- 堆区 (Heap)

​     堆区是理解Java GC机制最重要的区域, 没有之一. 在JVM所管理的内存中, 堆区是最大的一块, 也是Java GC机制所管理的主要内存区域. 堆区由所有线程共享, 在虚拟机启动时创建. 堆区的存在是为了存储对象实例, 原则上讲, 所有的对象都在堆区上分配内存 (不过现代技术里, 也不是这么绝对的, 也有栈上直接分配的). 

　　一般的, 根据Java虚拟机规范规定, 堆内存需要在逻辑上是连续的 (在物理上不需要), 在实现时, 可以是固定大小的, 也可以是可扩展的, 目前主流的虚拟机都是可扩展的. 如果在执行垃圾回收之后, 仍没有足够的内存分配, 也不能再扩展, 将会抛出OutOfMemoryError:Java heap space异常. 

- 方法区 (Method Area)

​     在Java虚拟机规范中, 将方法区作为堆的一个逻辑部分来对待, 但实际上, 方法区并不是堆 (Non-Heap). 另外, 不少人的博客中, 将Java GC的分代收集机制分为3个代: 青年代, 老年代, 永久代, 这些作者将方法区定义为”永久代", 这是因为在HotSpot的实现方式中, 将分代收集的思想扩展到了方法区, 并将方法区设计成了永久代. 不过, 除HotSpot之外的多数虚拟机, 并不将方法区当做永久代, HotSpot本身, 也计划取消永久代. 本文使用JDK6, 将使用永久代一词.

　　方法区是各个线程共享的区域, 用于存储已经被虚拟机加载的类信息 (加载类时需要加载的信息, 包括版本, field, 方法, 接口等信息), final常量, 静态变量, 编译器即时编译的代码等.

　　方法区在物理上也不需要是连续的, 可以选择固定大小或可扩展大小, 并且方法区比堆还多了一个选项: 可以选择是否执行垃圾收集. 一般的, 方法区上执行的垃圾收集是很少的, 这也是方法区被称为永久代的原因之一, 但这也不代表着在方法区上完全没有垃圾收集, 其上的垃圾收集主要是针对常量池的内存回收和对已加载类的卸载.

　　在方法区上进行垃圾收集, 条件苛刻而且相当困难, 效果也不令人满意, 所以一般不做太多考虑, 可以留作以后进一步深入研究时使用. 

　　在方法区上定义了OutOfMemoryError:PermGen space异常, 在内存不足时抛出。

​     运行时常量池 (Runtime Constant Pool) 是方法区的一部分, 用于存储编译期就生成的字面常量, 符号引用, 翻译出来的直接引用 (符号引用是用字符串表示某个变量, 接口的位置, 直接引用是根据符号引用翻译出来的地址, 将在类链接阶段完成翻译). 运行时常量池除了存储编译期常量外, 也可以存储在运行时间产生的常量 (如String类的intern()方法, String维护了一个常量池, 如果调用的字符“abc”已经在常量池中, 则返回池中的字符串地址. 否则, 新建一个常量加入池中, 并返回地址).

- 直接内存 (Direct Memory)

​     直接内存不是JVM管理的内存, 它是JVM以外的机器内存, 比如, 你有4G的内存, JVM占用了1G, 则其余的3G就是直接内存. JDK中有一种基于通道 (Channel) 和缓冲区 (Buffer) 的内存分配方式, 将由C语言实现的native函数库分配在直接内存中, 用存储在JVM堆中的DirectByteBuffer来引用.  由于直接内存收到本机器内存的限制, 所以也可能出现OutOfMemoryError的异常.

1. Java对象的访问方式

​     





## 面试题

Java基础

- HashMap 的数据结构是什么
- HashSet 是如何保证不重复的
- HashMap 是线程安全的吗, 为什么不是线程安全的 (最好画图说明多线程环境下不安全) 
- HashMap 的扩容过程 
- HashMap 1.7 与 1.8 的区别, 说明 1.8 做了哪些优化, 如何优化的
- final finally finalize 
- 强引用 、软引用、 弱引用、虚引用
- Java 反射的实现原理
- Arrays.sort 实现原理和 Collection 实现原理
- LinkedHashMap 的应用 
- cloneable 接口实现原理
- 异常分类以及处理机制 
- 数组在内存中如何分配
- io 的模型和 nio selectionkey 是什么

线程池

- 解释线程池的作用
- 线程池的处理流程
- jdk 提供的线程池工具类有哪些, 区别是什么
- 关闭线程池的方法有哪些, 区别是什么

MySqL

- sql优化方法
- 建索引有哪些策略和原则
- 索引存储原理
- mysql数据库锁有哪几种
- 写一个数据库死锁的sql
- 如何做数据库分库分表 (mycat) 

消息队列

- RabbitMQ的exchange有哪几种
- mq的使用场景有哪些
- RabbitMQ的系统架构
- RabbitMQ的任务分发机制有哪些

Redis

- 使用redis有哪些好处
- redis相比memcached有哪些优势
- redis常见性能问题和解决方案
- redis集群有哪些模式
- redis中穿透, 击穿与雪崩的预防及解决
- redis哨兵模式集群的原理

Spring

- IOC和AOP的实现原理
- AOP的应用场景有哪些, 以及动态代理原理是什么
- 事务的传播属性有哪几种
- bean的生命周期
- Spring有哪些模块, 分别有哪些作用和功能

SpringMVC

- SpringMVC的工作原理, 举例说明流程

MyBatis

- Mybatis的二级缓存

Zookeeper

- zk的作用和原理
- zk设计要满足哪些特性, 分别解释一下
- zk的选举机制是什么, 是否有了解Paxos算法

Nginx

- 什么是Nginx, Nginx的作用是什么
- Nginx 有哪些特点

分布式

- 什么是分布式系统, 解决什么问题
- 如何提升系统吞吐量
- 如何降低延迟
- 如何做故障恢复
- 如何做日志统一系统
- 怎么实现通讯编程, 如rpc服务, webService服务等
- 高并发秒杀解决方案有哪些
- 分布式系统有哪些优势
- 分布式系统会面临什么挑战
- 如何设计分布式系统
- 如何做分布式事务

其他问题

- 如何将一个请求由原来的10s减少到3s, 可以从哪些地方优化
- 如何支持大量流量的访问, 可以在哪些地方进行优化
- 双11流量怎么控制
- 1亿无序的数据文件, 如何找出最小的10个数并去重 (topk算法
- 分布式环境下, 如何对一个web请求的做监控

HR问题

- 请说一下你为什么想跳槽, 为什么选择我们
- 你最近在关注那些领域的知识
- 你的职业规划是什么, 你对自己未来的定位是怎样的
- 最近是否有打算深造提升自己, 你平时是如何自学的, 你喜欢读书吗，都有那些书
- 你除了工作之外还有哪些兴趣爱好
- 工作中遇到挑战你通常是怎么处理的, 工作时是否遇到沟通中发生争执
- 你如何看待加班这件事
- 你觉得自己有哪些优势, 生活中别人是如何评价你的, 自己有哪些优点和缺点
- 你的期望薪资是多少, 最低能接受多少