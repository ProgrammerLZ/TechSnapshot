##Netty基础

问题:

> NIO和异步有啥关系？
>
> NioEventLoopGroup,基于Channel的Selector
>
> 分块编码传输机制
>
> HTTP协议相关的东西需要之后去了解一下，找一本书或者视频看看

### Netty中的主要概念

#### Channel

管道，其是对 Socket 的封装，其包含了一组 API，大大简化了直接与 Socket 进行操作的 复杂性。

#### EventLoopGroup

EventLoopGroup 是一个 EventLoop 池，包含很多的 EventLoop。

Netty 为每个 Channel 分配了一个 EventLoop，用于处理用户连接请求、对用户请求的处 理等所有事件。EventLoop 本身只是一个线程驱动，在其生命周期内只会绑定一个线程，让 该线程处理一个 Channel 的所有 IO 事件。

一个 Channel 一旦与一个 EventLoop 相绑定，那么在 Channel 的整个生命周期内是不能 改变的。一个 EventLoop 可以与多个 Channel 绑定。即 Channel 与 EventLoop 的关系是 n:1， 而 EventLoop 与线程的关系是 1:1。

#### ServerBootStrap

用于配置整个 Netty 代码，将各个组件关联起来。服务端使用的是 ServerBootStrap，而

客户端使用的是则 BootStrap。

#### ChannelHandler 与 ChannelPipeline

ChannelHandler 是对 Channel 中数据的处理器，这些处理器可以是系统本身定义好的编 解码器，也可以是用户自定义的。这些处理器会被统一添加到一个 ChannelPipeline 的对象中， 然后按照添加的顺序对 Channel 中的数据进行依次处理。

#### ChannelFuture

Netty 中所有的 I/O 操作都是异步的，即操作不会立即得到返回结果，所以 Netty 中定义 了一个 ChannelFuture 对象作为这个异步操作的“代言人”，表示异步操作本身。如果想获取 到该异步操作的返回值，可以通过该异步操作对象的 addListener()方法为该异步操作添加监听器，为其注册回调:当结果出来后马上调用执行。

Netty 的异步编程模型都是建立在 Future 与回调概念之上的。



### Netty执行流程

![image-20200404110653478](Netty%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200404110653478.png)





## 手写RPC框架

### 了解RPC

远程过程调用

RPC是一种协议。

开发者角度：

框架具有一般性。

用户角度： 

调用远程方法就像调用本地一样，但其实已经经过了网络。

他就是一个概念（协议），webservice也是这种协议的实现。



### 手写RPC

1. 服务端将指定包中的所有提供者进行发布（写入服务注册表，其本身是一个HashMap）。

   > 注意：一旦写到一个Map的时候，并且作为成员变量，就要考虑**并发问题**。虽然这里不用考虑，但是要养成一个习惯

2. 定义一个用于缓存指定包中所有提供者（实现类）的类名。这里使用list进行缓存。

   > 如何让list编程线程安全呢？
   >
   > 答：Collections.synchronizedList(new ArrayList<>()); 



JDK的代理的使用，需要了解一下。

泛型，需要了解下。



#### 手写RPC服务器

##### 服务提供者的发布流程

1. 获取指定包下的所有提供者类型并加入缓存
   1. 获取包的资源路径
   2. 获取该资源路径下的所有类的全限定名称，加入缓存
2. 将上一步骤获取到的信息加入注册表
   1. 循环上一步的缓存数据。
   2. 每次循环都根据全限定名利用Java的反射，获取到相应的对象，然后以全限定名为key，获取到的对象为value这种形式，将信息存入注册表。



##### 启动流程

RPC服务器的底层基于Netty，除了Netty的模式化代码之外，需要开发者自定义处理器来处理来自RPC客户端的请求。

该处理器主要作用是，根据客户端传来的调用信息（告诉服务器我要调用哪个类的哪个方法），进行本地方法调用，然后将调用结果返回给客户端。



#### 手写RPC客户端

利用JDK的Proxy创建一个代理对象。当在该代理对象上进行方法调用的时候，会将这次调用转换成远程调用（在不是调用Object的方法的情况下）。







## 手写Dubbo



## 手写Tomcat

定义sevlet规范

CUstomHttpReuest



