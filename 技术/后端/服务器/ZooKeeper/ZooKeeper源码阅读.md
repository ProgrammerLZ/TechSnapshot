### Zookeeper客户端会话超时管理

### 问题区

静态内部类的作用是什么？ 



### 源码理解区

会话超时管理由服务端进行管理。

##### 大流程

1. 有客户端连接上，创建一个SessionTracker用于追踪这个Session是否过期。





#### SessionTrackerImpl

**expirationInterval = tickTime**  一个session的过期时间间隔

> 时钟滴答一下认为是一个tick，所以tickTime就可以理解为一个时钟（不一定是生活中的时钟）的单位时间，Zookeeper把它设置为2，这是Zookeeper自己的时钟的滴答一下的时间。

**nextExpirationTime** 



### 源码技巧区

#### 一个更精确的计算时间间隔的方法

```java
long time = Time.currentElapsedTime();
......
long duration = Time.currentElapsedTime() - time;
```

