## Spring

### 一、Spring 要解决的核心问题

IOC（控制反转）或者DI（依赖注入）是Spring的核心能力，当IOC或者DI被适当的使用的时候，我们就能够创建具有松耦合度的程序。松耦合度具有很多的好处，对象之间如果具有松耦合度，以目前的经验看来，能够更方便的进行**单元测试**，并且使得程序对于修改不敏感，当修改功能的时候而不至于牵一发而动全身。关于松耦合的设计的好处肯定远远不止这些，可以作为另外一个点作为研究。

因此，可以看出，Spring所要解决的核心问题就是**对象之间的解耦，使得程序获得松耦合的设计带来的各种好处。**

这里遗留了一个问题：

> 为什么松耦合度的程序更加便于测试？



###二、Spring解决的其他问题。

1. 重复的模板代码。

   Spring JDBC 以及 Spring JMS是构建的Spring Core之上的两个模块，他们都解决了重复模板代码的问题，以Spring JDBC为例，它将重复的JDBC编程操作进行了框架级别的封装，使得程序开发者能够简化数据库编程的重复代码。

2. 与其他的框架很好的集成。

   Spring没有去解决现在已经解决的很好的问题了，一些框架已经在他们的领域当中很好的解决了一些问题，Spring选择了与他们共生，开发者可以很方便的将这些框架与Spring进行集成，从而利用这个以Spring为核心的框架组合去提高开发效率。共赢的思想在这点上很好的提现出来，可见Spring的决策层具有很好的大局观。



## Spring Boot

纵然Spring已经能够为我们提供种种我们所需要的功能了，但是它依然还是不完美的。**基于Spring的程序需要进行很多的配置**。比如我们要开发web应用程序，我们需要考虑使用哪些Spring模块以及其他的框架，并且还需要考虑框架之间的版本是否兼容。除此之外，我们还要进行大量的基础配置，配置来配置去，这种重复操作总会让人闻到一股怀味道。

Spring的开发者们注意到了这些问题，于是Spring Boot诞生了。

Spring Boot通过两个方式使得我们从上述的重复操作中得以解脱：

* Spring Boot Auto Configuration
* Spring Boot Starter Project



### 一、Spring Boot Starter Project

Spring Boot像是一个大管家，我们只需要告诉他，我们要做什么类型的程序，由他来负责帮助我们去做具体的事情。还是以web开发为例，我们只需要告诉Spring Boot我要干一个web项目，去给我准备相关的东西，Spring Boot就会去干事了。具体到操作上就是，我们提供给Spring Boot一个**spring-boot-starter-web**的选项，Spring Boot就会知道我们的意思，然后为我们生成出来一个工程，这个工程就已经具备了所有web应用程序开发所需要的所有依赖，完全没必要我们自己再去到pom中手动配置。Spring Boot的文档是这样解释的：

> Starters are a set of convenient dependency descriptors that you can include in your application. You get a one-stop-shop for all the Spring and related technology that you need, without having to hunt through sample code and copy paste loads of dependency descriptors. For example, if you want to get started using Spring and JPA for database access, just include the spring-boot-starter-data-jpa dependency in your project, and you are good to go.



### 二、Spring Boot Auto Configuration

除了自动管理项目依赖之外，Spring Boot也提供了自动配置的能力。

当我们开发web应用程序时，我们需要在xml中或java以代码的形式来配置基于Spring的开发环境。比如我们需要配置Dispatcher Servlet、DataSource、ViewResolver等等这些必须的bean，这些操作每次都是重复的，所以Spring Boot就把它们也自动化掉了。Spring Boot会检测ClassPath当中的jar包，如果包含了web开发所需要使用jar包（Spring MVC），Spring就会把上述所说的配置都配置好，不用我们再去配置。同样贴上官方的解释：

> *Spring Boot looks at a) Frameworks available on the CLASSPATH b) Existing configuration for the application. Based on these, Spring Boot provides basic configuration needed to configure the application with these frameworks. This is called* `Auto Configuration`.



