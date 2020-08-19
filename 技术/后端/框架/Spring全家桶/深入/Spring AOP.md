`学习笔记`

TODO：表示需要进行深入的但不耽误对现在主题理解的知识点。

##  一、AOP的由来

1. OOP的缺陷
2. AOP对OOP的补充
3. OOP属于横向扩展
4. AOP属于纵向扩展



## 二、AOP的家庭成员

- Aspect: A modularization of a concern that cuts across multiple classes. Transaction management is a good example of a crosscutting concern in enterprise Java applications. In Spring AOP, aspects are implemented by using regular classes (the [schema-based approach](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-schema)) or regular classes annotated with the `@Aspect` annotation (the [@AspectJ style](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-ataspectj)).
- Join point: A point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.
- Advice: Action taken by an aspect at a particular join point. Different types of advice include “around”, “before” and “after” advice. (Advice types are discussed later.) Many AOP frameworks, including Spring, model an advice as an interceptor and maintain a chain of interceptors around the join point.
- Pointcut: A predicate that matches join points. Advice is associated with a pointcut expression and runs at any join point matched by the pointcut (for example, the execution of a method with a certain name). The concept of join points as matched by pointcut expressions is central to AOP, and Spring uses the AspectJ pointcut expression language by default.
- Introduction: Declaring additional methods or fields on behalf of a type. Spring AOP lets you introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an `IsModified` interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community.)
- Target object: An object being advised by one or more aspects. Also referred to as the “advised object”. Since Spring AOP is implemented by using runtime proxies, this object is always a proxied object.
- AOP proxy: An object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). In the Spring Framework, an AOP proxy is a JDK dynamic proxy or a CGLIB proxy.
- Weaving: linking aspects with other application types or objects to create an advised object. This can be done at compile time (using the AspectJ compiler, for example), load time, or at runtime. Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.



## 三、AOP的技术实现

Proxy是Java当中对AOP的技术实现。

1. 静态代理
2. 动态代理，对静态代理的一种优化，必须与被代理的类实现相同的接口
3. CGLib，动态代理的另外一种实现。避免了动态代理对接口的限制，但是仍然存在局限性，final类无法被代理，因为CGLib是要生成被代理的类的子类的
4. **基于反射的动态代理的实现——TODO**
5. Spring已经封装了上述AOP的实现



### Spring AOP中的Joinpoint

Spring AOP中，仅支持方法级别的JoinPoint。原因有三：

1. 保持框架简单。
2. 字段级别的拦截破坏了类的封装。
3. 需求特殊，可采用其他的AOP产品，Spring框架本身对他们的继承也有很好的支持。



### Spring AOP中的Pointcut







## 四、在Spring Boot当中使用AOP

1. pom引入相关依赖

2. 用@Aspect注解声明切面类

3. 在切面类中用@PointCut声明切点，表明切点在哪里

4. 利用一下的注解说明应该在切点的什么位置做增强。

   * @Aspect 表明是一个切面类
   * @Component 将当前类注入到Spring容器内
   * @Pointcut 切入点，其中execution用于使用切面的连接点。使用方法：execution(方法修饰符(可选) 返回类型 方法名 参数 异常模式(可选)) ，可以使用通配符匹配字符，*可以匹配任意字符。
   * @Before 在方法前执行
   * @After 在方法后执行
   * @AfterReturning 在方法执行后返回一个结果后执行
   * @AfterThrowing 在方法执行过程中抛出异常的时候执行

   



## 五、深入

再继续深入可追溯Spring是如何实现AOP的，但暂时不对源码做过多深入了，理解了AOP的技术实现是动态代理目前来说就够了。

关于Spring AOP的源码阅读如果哪天需要的时候，会在这里补上。