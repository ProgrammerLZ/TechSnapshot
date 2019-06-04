## Java基础

- [ ] 接口和抽象类的区别，如何选择
- [ ] Java中实现多态的机制是什么，动态多态和静态多态的区别
- [ ] Java能不能多继承，可不可以多实现
- [ ] `Static Nested Class` 和 `Inner Class`的不同
- [ ] 重载和重写的区别。
- [ ] 是否可以继承`String`类
- [ ] 构造器是否可被`override`?
- [ ] `public`,`protected`,`private`的区别?
- [ ] `String`和`StringBuffer`、`StringBuilder`的区别
- [ ] `==`和`equals`的区别
- [ ] `hashCode`的作用，和`equals`方法的关系
- [ ] Java的四种引用
- [ ] 序列化与反序列化
- [ ] 正则表达式
- [ ] `int`和`Integer`的区别，什么是自动装箱和自动拆箱
- [ ] JAVA中的几种基本数据类型是什么，各自占用多少字节。
- [ ] String类能被继承吗，为什么。
- [ ] final的用途。
- [ ] 如何在父类中为子类自动完成所有的hashcode和equals实现？这么做有何优劣。
- [ ] 请结合OO设计理念，谈谈访问修饰符public、private、protected、default在应用设计中的作用。 
- [ ] 深拷贝和浅拷贝区别。
- [ ] 数组和链表数据结构描述，各自的时间复杂度。
- [ ] error和exception的区别，CheckedException，RuntimeException的区别。
- [ ] 请列出5个运行时异常。
- [ ] 在自己的代码中，如果创建一个java.lang.String类，这个类是否可以被类加载器加载？为什么。 
- [ ] 说一说你对java.lang.Object对象中hashCode和equals方法的理解。在什么场景下需要重新实现这两个方法。 
- [ ] 在jdk1.5中，引入了泛型，泛型的存在是用来解决什么问题。
- [ ] 这样的a.hashcode() 有什么用，与a.equals(b)有什么关系。
- [ ] 有没有可能2个不相等的对象有相同的hashcode。
- [ ] Java中的HashSet内部是如何工作的。
- [ ] 什么是序列化，怎么序列化，为什么序列化，反序列化会遇到什么问题，如何解决。
- [ ] java8的新特性。
- [ ] Error`和`Exception的区别
- [ ] 异常的类型，什么是运行时异常
- [ ] `final`、`finally`和`finalize`的区别
- [ ] `try-catch-finally`中，如果在`catch`中`return`了，`finally`中的代码还会执行么，原理是什么？
- [ ] 列举3个以上的`RuntimeException`
- [ ] Java中的异常处理机制的简单原理和应用
- [ ] Java9比Java8改进了什么；
- [ ] 说说自定义注解的场景及实现；
- [ ] 反射的原理，反射创建类实例的三种方式是什么。
- [ ] 反射中，Class.forName和ClassLoader区别 。
- [ ] 说说反射的用途及实现，反射是不是很慢，我们在项目中是否要避免使用反射；
- [ ] 描述动态代理的几种实现方式，分别说出相应的优缺点。
- [ ] 动态代理与cglib实现的区别。
- [ ] 为什么CGlib方式可以对接口实现代理。
- [ ] 写出三种单例模式实现 。
- [ ] 哪些情况下的对象会被垃圾回收机制处理掉？
- [ ] 讲一下常见编码方式？
- [ ] utf-8编码中的中文占几个字节；int型几个字节？
- [ ] 静态代理和动态代理的区别，什么场景使用？
- [ ] Java的异常体系
- [ ] 谈谈你对解析与分派的认识。
- [ ] 修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？
- [ ] Java中实现多态的机制是什么？
- [ ] 如何将一个Java对象序列化到文件里？
- [ ] 说说你对Java反射的理解
- [ ] 说说你对Java注解的理解
- [ ] 说说你对依赖注入的理解
- [ ] 说一下泛型原理，并举例说明
- [ ] Java中String的了解
- [ ] String为什么要设计成不可变的？
- [ ] Object类的equal和hashCode方法重写，为什么？

## 多线程
- [ ] 进程和线程的区别
- [ ] 并行和并发的区别和联系
- [ ] 同步与异步
- [ ] 多线程的实现方式，有什么区别
- [ ] 什么叫`守护线程`
- [ ] 如何停止一个线程？
- [ ] 什么是`线程安全`？
- [ ] synchronized` 和 `lock`的区别
- [ ] 当一个线程进入一个对象的一个`synchronized`方法后，其它线程是否可进入此对象的其它方法?
- [ ] 启动一个线程是用`run()`还是`start()`?
- [ ] wait和sleep的区别
- [ ] notify和notifyAll的区别
- [ ] 线程池的作用
- [ ] Java中线程池相关的类
- [ ] 线程池的原理，为什么要创建线程池？
- [ ] 线程的生命周期，什么时候会出现僵死进程；
- [ ] 什么是现线程安全，如何实现线程安全；
- [ ] 创建线程池有哪几个核心参数？ 如何合理配置线程池的大小？
- [ ] synchronized、volatile区别、synchronized锁粒度、模拟死锁场景、原子性与可见性；
- [ ] 多线程的几种实现方式，什么是线程安全。
- [ ] volatile的原理，作用，能代替锁么。
- [ ] 画一个线程的生命周期状态图。
- [ ] sleep和wait的区别。
- [ ] sleep和sleep(0)的区别。
- [ ] Lock与Synchronized的区别 。
- [ ] synchronized的原理是什么，一般用在什么地方(比如加在静态方法和非静态方法的区别，静态方法和非静态方法同时执行的时候会有影响吗)，解释以下名词：重排序，自旋锁，偏向锁，轻量级锁，可重入锁，公平锁，非公平锁，乐观锁，悲观锁。
- [ ] 用过哪些原子类，他们的原理是什么。
- [ ] JUC下研究过哪些并发工具，讲讲原理。
- [ ] 用过线程池吗，如果用过，请说明原理，并说说newCache和newFixed有什么区别，构造函数的各个参数的含义是什么，比如coreSize，maxsize等。 
- [ ] 线程池的关闭方式有几种，各自的区别是什么。
- [ ] 假如有一个第三方接口，有很多个线程去调用获取数据，现在规定每秒钟最多有10个线程同 时调用它，如何做到。 
- [ ] spring的controller是单例还是多例，怎么保证并发的安全。
- [ ] 用三个线程按顺序循环打印abc三个字母，比如abcabcabc。
- [ ] ThreadLocal用过么，用途是什么，原理是什么，用的时候要注意什么。
- [ ] 如果让你实现一个并发安全的链表，你会怎么做。
- [ ] 有哪些无锁数据结构，他们实现的原理是什么。
- [ ] 讲讲java同步机制的wait和notify。
- [ ] CAS机制是什么，如何解决ABA问题。
- [ ] 多线程如果线程挂住了怎么办。
- [ ] countdowlatch和cyclicbarrier的内部原理和用法，以及相互之间的差别(比如countdownlatch的await方法和是怎么实现的)。 
- [ ] 对AbstractQueuedSynchronizer了解多少，讲讲加锁和解锁的流程，独占锁和公平所加锁有什么不同。 
- [ ] 使用synchronized修饰静态方法和非静态方法有什么区别。
- [ ] 简述ConcurrentLinkedQueue和LinkedBlockingQueue的用处和不同之处。
- [ ] 导致线程死锁的原因？怎么解除线程死锁。
- [ ] 非常多个线程（可能是不同机器），相互之间需要等待协调，才能完成某种工作，问怎么设计这种协调方案。
- [ ] 用过读写锁吗，原理是什么，一般在什么场景下用。
- [ ] 开启多个线程，如果保证顺序执行，有哪几种实现方式，或者如何保证多个线程都执行完再拿到结果。 
- [ ] 延迟队列的实现方式，delayQueue和时间轮算法的异同。
- [ ] 开启线程的三种方式？
- [ ] 线程和进程的区别？
- [ ] 为什么要有线程，而不是仅仅用进程？
- [ ] run()和start()方法区别
- [ ] 如何控制某个方法允许并发访问线程的个数？
- [ ] 在Java中wait和seelp方法的不同；
- [ ] 谈谈wait/notify关键字的理解
- [ ] 什么导致线程阻塞？
- [ ] 线程如何关闭？
- [ ] 讲一下java中的同步的方法
- [ ] 数据一致性如何保证？
- [ ] 如何保证线程安全？
- [ ] 如何实现线程同步？
- [ ] 两个进程同时要求写或者读，能不能实现？如何防止进程的同步？
- [ ] 线程间操作List
- [ ] Java中对象的生命周期
- [ ] Synchronized用法
- [ ] synchronize的原理
- [ ] 谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
- [ ] static synchronized 方法的多线程访问和作用
- [ ] 同一个类里面两个synchronized方法，两个线程同时访问的问题
- [ ] volatile的原理
- [ ] 谈谈volatile关键字的用法
- [ ] 谈谈volatile关键字的作用
- [ ] 谈谈NIO的理解
- [ ] synchronized 和volatile 关键字的区别
- [ ] synchronized与Lock的区别
- [ ] ReentrantLock 、synchronized和volatile比较
- [ ] ReentrantLock的内部实现
- [ ] 死锁的四个必要条件？
- [ ] 怎么避免死锁？
- [ ] 对象锁和类锁是否会互相影响？
- [ ] 什么是线程池，如何使用?
- [ ] Java的并发、多线程、线程模型
- [ ] 谈谈对多线程的理解
- [ ] 多线程有什么要注意的问题？
- [ ] 谈谈你对并发编程的理解并举例说明
- [ ] 谈谈你对多线程同步机制的理解？
- [ ] 如何保证多线程读写文件的安全？




## IO

- [ ] `Input/OutputStream`和`Reader/Writer`有什么区别

  > 一个是基于字节读写，一个是基于字符的读写。

- [ ] 如何在字符流和字节流之间转换？

  > 

- [ ] `switch`可以使用那些数据类型

- [ ] IO模型有哪些，讲讲你理解的nio ，他和bio，aio的区别是啥，谈谈reactor模型。

- [ ] NIO、AIO和BIO 之间的区别

- [ ] `IO`和`NIO`常用用法



## 集合
- [ ] ArrayList和LinkedList有什么区别。
- [ ] 用过哪些Map类，都有什么区别，HashMap是线程安全的吗,并发下使用的Map是什么，他们内部原理分别是什么，比如存储方式，hashcode，扩容，默认容量等。
- [ ] JAVA8的ConcurrentHashMap为什么放弃了分段锁，有什么问题吗，如果你来设计，你如何设计。 
- [ ] 有没有有顺序的Map实现类，如果有，他们是怎么保证有序的。 
- [ ] 抽象类和接口的区别，类可以继承多个类么，接口可以继承多个接口么,类可以实现多个接口么。
- [ ] 继承和聚合的区别在哪。 
- [ ] 讲讲类的实例化顺序，比如父类静态数据，构造函数，字段，子类静态数据，构造函数，字段，当new的时候，他们的执行顺序。
- [ ]  Arraylist、linkedlist差异，应用场景；
- [ ]  HashMap在JDK1.8有哪些改动？
- [ ]  HashCurrentMap和HashMap的区别在哪里？
- [ ] Hashmap什么时候使用红黑树？
- [ ] 列举几个Java中`Collection`类库中的常用类
- [ ] List`、`Set`、`Map`是否都继承自`Collection`接口？存储特点分别是什么？
- [ ] ArrayList`、`LinkedList`和`Vector`之间的区别与联系
- [ ] HashMap`和`Hashtable`、`TreeMap`以及`ConcurrentHashMap`的区别
- [ ] `Collection` 和 `Collections`的区别
- [ ] 其他的集合类：`treeset`,`linkedhashmap`等。
- [ ] List和Map区别，Arraylist与LinkedList区别，ArrayList与Vector 区别；
- [ ] 红黑树的实现原理和应用场景；
- [ ] HashMap内部的数据结构是什么？底层是怎么实现的？


## JVM
### 内存相关
- [ ] JVM内存模型
- [ ] JVM内存模型的相关知识了解多少，比如重排序，内存屏障，happen-before，主内存，工作内存等。 
- [ ] JVM收集器G1的内存模型和CMS的内存模型有什么不同？
- [ ] 如何查看java内存使用情况（jconsole、命令jmap、jstack等等）
- [ ] Eden和Survivor比例。
- [ ] JVM内存为什么要分成新生代，老年代，持久代。新生代中为什么要分为Eden和Survivor。
- [ ] 什么情况下会发生栈内存溢出。当出现了内存溢出，你怎么排错。
- [ ] Java中的内存溢出是什么，和内存泄露有什么关系
- [ ] 方法区和直接内存什么时候会oom？
- [ ] JVM堆和栈的区别
- [ ] JVM内存区域，开线程影响哪块内存
- [ ] 内存对象的循环引用及避免
- [ ] 虚拟机原理，如何自己设计一个虚拟机(内存管理，类加载，双亲委派)
- [ ] JVM堆的分代

### GC
- [ ] JVM中**一次完整的GC流程**是怎样的，对象如何晋升到老年代，说说你知道的几种主要的JVM参数。 
- [ ] 你知道哪几种垃圾收集器，各自的优缺点，重点讲下cms和G1，包括原理，流程，优缺点。
- [ ] 垃圾回收算法的实现原理
- [ ] GC的概念，如果A和B对象循环引用，是否可以被GC？
- [ ] gc如何判断对象是否需要回收，有哪几种方式？
- [ ] Java中能不能主动触发GC
- [ ] g1和cms区别,吞吐量优先和响应优先的垃圾收集器选择
- [ ] 垃圾回收机制与调用System.gc()区别
- [ ] GC机制和原理；GC分哪两种；什么时候会触发Full GC？
- [ ] 内存回收机制、GC回收策略、GC原理时机以及GC对象

### 双亲委派
- [ ] 简单说说你了解的类加载器，可以打破双亲委派么，怎么打破。
- [ ] 什么是双亲委派机制？介绍一些运作过程，双亲委派模型的好处；
- [ ] 什么情况下我们需要破坏双亲委派模型；
- [ ] Java的类加载机制，什么是双亲委派
- [ ] 谈谈你对双亲委派模型理解

### ClassLoader
- [ ] JVM里的有几种classloader，为什么会有多种？
- [ ] ClassLoader的类加载方式
- [ ] 谈谈对ClassLoader(类加载器)的理解
- [ ] 类加载机制
- [ ] 谈谈对动态加载（OSGI）的理解

### JVM调优
- [ ] 常见的JVM调优方法有哪些？可以具体到调整哪个参数，调成什么值？
- [ ] jvm调优用过吗？
- [ ] 你们线上应用的JVM参数有哪些。
- [ ] 请解释如下jvm参数的含义： 
- [ ] -XX:PermSize=256m -XX:MaxPermSize=512m -
XX:+UseCMSInitiatingOccupancyOnly。

### 其他
- [ ] 讲讲JAVA的反射机制。
- [ ] 怎么打出线程栈信息。
- [ ] java虚拟机的特性
- [ ] 对Dalvik、ART虚拟机有什么了解？
- [ ] Art和Dalvik对比

## 开源项目
### Spring
- [ ] 使用Spring的好处是什么，Spring的核心理念
- [ ] 什么是`AOP`和`IOC`，实现原理是什么
- [ ] spring bean的初始化过程
- [ ] Spring的`事务管理` ，`Spring bean注入`的几种方式
- [ ] spring四种依赖注入方式
- [ ] Spring AOP的实现原理和场景；（应用场景很重要）
- [ ] Spring Boot比Spring做了哪些改进？ Spring 5比Spring4做了哪些改进；（惭愧呀，我们还在用Spring4，高版本的没关心过）
- [ ] SpringMVC、动态代理、反射、AOP原理、事务隔离级别；
- [ ] 什么是DI、为什么DI、DI的类型（构造器注入、方法注入）；
- [ ] Spring如何解决循环依赖问题；

### ORM
### Tomcat
### Netty
## 