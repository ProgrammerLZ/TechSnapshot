slf4j是一个简单的一直门面抽象，采用门面模式抽象了底层各种不同的日志记录框架，LogBack就是底层日志记录框架之一。官方也比较推荐把logback和slf4j配合起来使用。



## LogBack结构

* logback-core 实现了核心功能，是下边两个模块的基础
* logback-classic 对slf4j的API进行实现，配合slf4j使用的时候，需要导入这个模块
* logback-access 为了集成servlet环境而准备的



## 配置

```xml
<configuration scan="true" scanPeriod="60 second" debug="false">  
      <!-- 其他配置省略-->  
</configuration>
```

