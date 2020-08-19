## Java内部类

为什么需要内部类呢？原因有3：

* 内部类方法可以访问该类定义所在的作用域中的数据， 包括私有的数据。
* 内部类可以对同一个包中的其他类隐藏起来。
* 当想要定义一个回调函数且不想编写大量代码时，使用匿名(anonymous) 内部类比较便捷。



### 内部类访问外围类对象状态

内部类既可以访问自身的数据域，也可以访问创建它的外围类对象的数据域。

内部类总有一个隐式引用，它指向了创建它的外部类对象。**编译器**修改了所有的内部类的构造器，添加一个**外围类引用**的参数。 

<img src="Java%E5%86%85%E9%83%A8%E7%B1%BB.assets/image-20200811094638284.png" alt="image-20200811094638284" style="zoom:50%;" />





### 内部类的特殊语法规则

* 创建内部类对象的语法

```java
outerObject.new InnerClass{constructionparameters)
```

* 外围类的引用

```java
OuterClass.this
```

* 在外围类的作用域之外， 可以这样引用内部类

```java
OuterClass.InnerClass
```

* 在外围类的作用于中创建内部类

> 内部类对象的**外围类引用被设置为创建内部类对象的方法中的this引用**。（每个java的方法都隐式包含一个this引用，这是编译器的魔法）

```java
ActionListener listener = this.new TimePrinter();
```

* 在外围类的作用域外创建内部类。

```java
TalkingClock jabberer = new Ta1kingClock(1000, true);
TalkingOock.TiiePrinter listener = jabberer.new TimePrinter();
```

* 内部类声明的静态域必须是final
* 内部类不能有static方法



### 局部内部类

某个类只在某个方法中使用的时候可以定义局部内部类。局部内部类具有以下特点：

* 不能用`public`和`private`说明符进行声明
* 作用域被限定在声明这个局部类的块中
* 对外部世界完全隐藏
* 访问的局部变量必须是`final`



### 匿名内部类

将局部内部类的使用再深入一步，就变成了匿名内部类。

假设只需要创建局部内部类的一个对象，那就不需要进行命名了。这种类就叫做**匿名内部类**。

```java
public void start(int interval, boolean beep)
{
    ActionListener listener = new ActionListener()
    {
      //Do sth.
    {
    Timer t = new Timer(interval, listener);
    t.start();
}
```

多年来， Java 程序员习惯的做法是用匿名内部类实现事件监听器和其他回调。 如今最好 还是使用**lambda表达式**。

```java
public void start(int interval, boolean beep)
{
    Timer t = new Timer(interval, event ->
    {
				//Do sth.
    });
    t.start();
}
```



### 静态内部类

有时候， 使用内部类只是为了把一个类隐藏在另外一个类的内部， 并**不需要内部类引用外围类对象**。为此，可以将内部类声明为static, 以便取消产生的引用。

当然， 只有内部类可以声明为 static。静态内部类的对象除了没有对生成它的外围类对象的引用特权外， 与其他所冇内部类完全一样。

* 在内部类不需要访问外围类对象的时候， 应该使用静态内部类
* 与常规内部类不同， 静态内部类可以有静态域和方法
* 声明在接口中的内部类自动成为 static 和 public 类



