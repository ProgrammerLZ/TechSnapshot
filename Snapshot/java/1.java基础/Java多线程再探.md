## 并发

### 一、线程状态

* New
* Runnable
* Blocked
* Waiting
* Times waiting
* Terminated



#### 1.1 新创建线程

新建状态，并没有运行。



####1.2 可运行线程

调用完start()方法之后，是否真正开始运行取决于操作系统调度。抢占式操作系统采用CPU时间分片的方式分配给每一个线程执行时间，时间到了，再分配给其他线程——这取决于CPU数量，如果CPU数量>线程数量则不用进行时间分片，因为每个CPU都可以执行一条单一的线程。



#### 1.3 被阻塞和等待线程

![Snip20181126_11](/LZMac/Personal/Learn/笔记/web/1.java基础/assets/Snip20181126_11.png)



### 二、线程属性

* 线程优先级
* 守护线程
* 未捕获异常处理器



#### 2.1 线程优先级

* **setPriority()**

* 不要将程序的正确性依赖于优先级。因为Java虚拟机依赖于宿主机的线程实现时，java优先级将映射到宿主机平台的优先级，而宿主机的优先级设置可能有java优先级不同。



#### 2.2 守护线程

* **setDaemon()**
* 守护线程应该永远不去访问固有资源， 如文件、 数据库， 因为它会在**任何时候**甚至在一个操作的中间发生中断。



#### 2.3 未捕获异常处理器

用于捕获线程的run() 方法中的出现的非受查异常。

* **Thread.UncaughtExceptionHandler**-->是一个接口
  * void uncaughtException(Thread t, Throwable e) 
* **setUncaughtExceptionHandler()**——>为某个线程配置一个异常处理器
* **setDefaultUncaughtExceptionHandler()**——>为所有线程配置一个标准的异常处理器



### 三、同步

#### 3.1 竞争

![Snip20181126_12](/Users/liuzhe/Desktop/Snip20181126_12.png)



同一个资源，多个线程都要修改。由于修改操作是一个非原子操作，他在计算机底层分为多个指令：

1. 取数到寄存器
2. 修改寄存器当中的值
3. 把寄存器中的值放回内存。

在多线程的情况下，操作系统可能在这个线程还没有执行完所有的的指令的情况下，就剥夺了其运行权而去执行其他线程，这是出现竞争的**根本原因**。如果不让一个线程**执行完所有的指令**，竞争就始终存在。



#### 3.2 锁

* ReentrantLock

  ```java
  myLock.lockO; // a ReentrantLock object try
  {
  	critical section
  }
  finally
  {
  	myLock.unlockO;// make sure the lock is unlocked even if an exception is thrown 
  }
  ```



#### 3.3 条件

* 一个锁对象可以有多个相关的条件对象。
* 在对象的状态有利于等待线程的方向改变时调用 **signalAll()**
* 注意调用 signalAll 不会立即激活一个等待线程。它仅仅解除等待线程的阻塞
* 通常， 对 await 的调用应该在如下形式的循环体中

```java
while ((!ok)) 
    condition.await;
```

* **使用条件是一个有挑战的选择，非常容易造成死锁**



#### 3.4 	synchronized关键字

```java
public synchronized void method {

}
```

* 这是得益于java语言的一种同步机制
* 每个对象都有一个内部锁
* 内部锁只有一个相关条件
* 使代码更简洁
* synchronzed和锁/条件，都不是被推荐的线程同步方式，只有的非常适合他们的场合下才使用他们
* 每一个条件，管理一个单一的线程集合



关于条件的一点总结：

情景1：

```java
if(a < b){
	lock.lock();
    //excute some code...
    lock.unlock();
}
	
```

线程A和B。

当线程A在执行某一段代码的时候，他要做一个条件判断(a < b)，条件判断过后，操作系统可能剥夺了线程的执行权而去执行线程B，此时A处于可执行状态。线程B所执行的操作破坏了本来线程A已经判断通过了的条件，使得条件对于A来说不再成立，但是A不会再去判断条件是否可以通过，这就导致了线程A的执行出现错误。



情景2：

```java
lock.lock();
while(a > b){
    //waite...
}
//excute some code...
lock.unlock();
```

假设线程A执行这段代码，他先持有锁，然后判断条件不满足，判断完条件后线程有两种情况：

1. 满足条件，那么就执行代码，然后退出。

2. 不满足条件又分两种情况：

   a 执行完剩余的代码，退出。

   b 就地等待，等待其他线程改变我现在的处境。

这里选择b这种情况，当条件不满足的时候我们不想让线程退出而是让他处于等待状态。然而，由于线程A拥有锁，其他线程不可能再拥有这段代码的执行权，而只有这段代码才能改变线程A的现状，因此，才有了**条件**

这个技术。





