---
title: Java动态代理
date: 2019-05-10 13:59:32
categories: Java
---



## 动态代理简介

**动态代理是一种方便运行时动态构建代理、动态处理代理方法调用的机制**。很多场景都是利用类似机制做到的，比如用来包装 RPC 调用、面向切面的编程（AOP）。

代理模式以及装饰器模式的一种实现。能够实现**调用者**和**实现者**之间的**解耦**。

基于JDK动态代理的示例代码：

```java
public class MyDynamicProxy {
    public static  void main (String[] args) {
        HelloImpl hello = new HelloImpl();
        MyInvocationHandler handler = new MyInvocationHandler(hello);
        // 动态的生成代理类
        Hello proxyHello = (Hello) Proxy.newProxyInstance(HelloImpl.class.getClassLoader(), HelloImpl.class.getInterfaces(), handler);
        // 调用代理方法，实际上会调用handler的invoke方法
        proxyHello.sayHello();
    }
}

interface Hello {s
    void sayHello();
}

class HelloImpl implements  Hello {
    @Override
    public void sayHello() {
        System.out.println("Hello World");
    }
}

class MyInvocationHandler implements InvocationHandler {
    private Object target;//委托类
    public MyInvocationHandler(Object target) {
        this.target = target;
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        System.out.println("Invoking sayHello");//对委托类进行预处理
        Object result = method.invoke(target, args);//消息转发
      	System.out.println("Invoking sayHello");//对委托类进行后续处理
        return result;
    }
}
```

<!-- more -->

1. 首先创建被调用类的实例hello
2. 然后**以接口Hello为纽带**，创建了一个Proxy的实例
3. 然后调用代理方法



由上可以看出，要使用动态代理需要三个元素：

* 委托类
* 一组接口
* 代理(通过Proxy+委托类+一组接口进行创建)



这种依赖接口去使用动态代理的方式，在API设计和实现的角度上来看，多有不便，如果被调用类没有实现任何接口，这种方法就不适用。在这种情况下，可以使用其他的方式，例如在Spring的AOP当中支持的两种模式的代理方式：

* JDK Proxy
* cglib

cglib就能够克服对接口的依赖。



## 深入一点点

### 代理设计模式

代理设计模式的一个目的就是为对象提供一个代理以控制对某个对象的访问，代理类对委托类有几点需要负责：

- 预处理消息
- 过滤消息并转发消息
- 消息被委托类执行后的后续处理

通过代理类这中间一层，**能有效控制对委托类对象的直接访问**，也可以很好地**隐藏和保护委托类对象**，同时也为**实施不同控制策略预留了空间**，从而在设计上获得了更大的灵活性。

### 静态代理

静态代理和动态代理都是代理设计模式的实现，但是静态代理在程序还没有运行之前，就把所有的代码都写好了，下边是一个静态代理的例子：

```java
/**
 * 生产厂家
 */
public class Vendor implements Sell { 
    public void sell() { 
        System.out.println("In sell method"); 
    } 
    
    public void ad() { 
        System,out.println("ad method");
    }
} 

/**
 * 代理类
 */
public class BusinessAgent implements Sell {
    private Sell vendor;
    
    public BusinessAgent(Sell vendor){
        this.vendor = vendor;
    }

    public void sell() { 
        vendor.sell();
    } 
    
    public void ad() {
        vendor.ad();
    }
} 
```

然而，动态代理使用了一种更为灵活的方式，从之前的代码可以看出，动态代理是在程序的运行过程当中，通过Proxy这个动态代理的核心类，动态的生成了代理类，因此得名**动态代理**。

浅析一下Proxy类的`newProxyInstance`方法。

```java
static Object newProxyInstance(ClassLoader loader, Class[] interfaces, InvocationHandler h);
```

根据静态代理了解到，代理类必须实现和委托类相同的接口，因此`interfaces`就是一个委托类所实现的接口的数组。而InvocationHandler从字面意思来看是代表一个调用处理器，实际上他的功能也是用于处理方法调用，我们需要自己去实现这个接口，从而对委托类进行扩展，下面是InvocationHandler接口当中定义的唯一的一个方法。

```java
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable;
```

* proxy即为我们所自动生成的代理
* method是代理类所实现的方法-----------(可能不准确，这块没有理解透彻)
* args是method方法的参数



总结一下，在动态代理当中一共分了三种角色：

* 代理类(动态生成)
* 委托类(预先定义)
* 中介类(invocationHandler接口的实现类)

所有对动态生成的代理类的调用，最终都会调用中介类的invoke方法，而在invoke方法当中又去调用了委托类的相关方法，在invoke方法里边我们就可以处理对委托类这个方法调用的增强操作。对比一下静态代理，就能看出动态代理的好处。

设想这样一种业务场景，有一个服务接口叫Service，他的实现类是ServiceImpl，需要为ServiceImpl所实现的每个方法都添加log输出。如果我们用，静态代理实现，实现步骤如下：

1. 创建静态代理实现类，并实现Service接口
2. 给每个实现的方法，都添加一个日志输出，并在其中调用委托类的对应方法
3. 客户端调用都调用代理类

而动态代理的实现则是：

1. 创建调用处理器类，其实现了InvocationHandler接口，在其invoke方法中，先打印一个log然后调用委托类的相应方法。
2. 外部调用的地方，通过Proxy动态的生成代理实例，然后调用动态代理的方法。

可见，动态代理节省了大量的日志输出重复代码，这是通过引入中介类实现的。



## 关于技术选型

### JDK Proxy的优势

* 最小化依赖关系，减少依赖意味着简化开发和维护
* JDK本身的支持，可能比cglib更加可靠，并且能够平滑进行JDK版本升级，字节码类库则需要进行更新从而能够保证在新版本的Java上能够使用
* 代码实现比较简单



### 基于类似cglib框架的优势

* 不依赖于接口，减少对开发者代码的侵入性
* 只操作关心的类，不必为其他类做多余的操作
* 高性能



从性能角度，补充几句。记得有人曾经得出结论说 JDK Proxy 比 cglib 或者 Javassist 慢几十倍。坦白说，**不去争论具体的 benchmark 细节，在主流 JDK 版本中，JDK Proxy 在典型场景可以提供对等的性能水平，数量级的差距基本上不是广泛存在的**。而且，**反射机制性能在现代 JDK 中，自身已经得到了极大的改进和优化**，同时，JDK Proxy很多功能也不完全是反射，同样使用了 ASM 进行字节码操作。

我们在选型中，性能未必是唯一考量，**可靠性、可维护性、编程工作量**等往往是更主要的考虑因素，毕竟标准类库和反射编程的门槛要低得多，代码量也是更加可控的，如果**我们比较下不同开源项目在动态代理开发上的投入，也能看到这一点**。



## 动态代理的应用

动态代理目前来看最实用的地方是**完美符合Spring AOP切面编程的要求**。

> 切面编程`是对面向对象编程的一种补充`，可以把纠缠在各个模块间相同的逻辑抽离出来，从来提高代码的抽象度，进而提高开发效率和复用度。

AOP能力的一个图解。

![img](/Users/liuzhe/Documents/project/LZ/TechSnapshot/Snapshot/java/2.java高级/Java 动态代理/ba9a5b6228b188f5b9b15017e29a302b.png)