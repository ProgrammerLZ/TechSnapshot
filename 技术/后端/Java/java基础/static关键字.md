#  Static

在平时开发当中，我们经常会遇见static关键字。这篇文章就把java中static关键字的使用方法的原理进行一个深入的分析。先给出这篇文章的大致脉络：

首先，描述了static关键字去修饰java类、方法、变量、代码块的方法然后，从底层分析static关键字，接下来，给出static的一些使用场景和案例最后，对static进行一个总结，包括和普通变量的区分。

OK，开始今天的文章。



## 一、static关键字的基本用法

### static关键字基本概念

我们可以一句话来概括：**方便在没有创建对象的情况下来进行调用**。

也就是说：被static关键字修饰的不需要创建对象去调用，直接根据类名就可以去访问。对于这个概念，下面根据static关键字的四个基本使用来描述。然后在下一部分再来去分析static的原理，希望你能认真读完。

### static关键字修饰类

内部类一般都是只跟外部类有关系，没有必要单独搞一个文件存放这个类，所以才设计出了内部类这个概念。

静态内部类只有在被访问的时候，才会被加载。

java里面static一般用来修饰成员变量或函数。但有一种特殊用法是用static修饰内部类，**普通类是不允许声明为静态的，只有内部类才可以**。下面看看如何使用。

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/8644ebf81a4c510f633a3493cb792028d52aa567.jpeg)

如果没有用static修饰InterClass，则只能new 一个外部类实例。再通过外部实例创建内部类。

### static关键字修饰方法

修饰方法的时候，其实跟类一样，可以直接通过类名来进行调用：

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/4afbfbedab64034f7b86b4dc05e37c340b551d41.jpeg)



### static关键字修饰变量

被static修饰的成员变量叫做静态变量，也叫做类变量，说明这个变量是属于这个类的，而不是属于是对象，没有被static修饰的成员变量叫做实例变量，说明这个变量是属于某个具体的对象的。

我们同样可以使用上面的方式进行调用变量：

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/d8f9d72a6059252d0a2aeb289fbb063e5bb5b987.jpeg)

### static关键字修饰代码块

静态代码块在类第一次被载入时执行，在这里主要是想验证一下，类初始化的顺序。

1. 父类静态变量
2. 父类静态代码块
3. 子类静态变量
4. 子类静态代码块
5. 父类普通变量
6. 父类普通代码块
7. 父类构造函数
8. 子类普通变量
9. 子类普通代码块
10. 子类构造函数

代码验证一下：

首先我们定义一个父类

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/faf2b2119313b07ed299fae9a6f7942696dd8cd4.jpeg)

然后定义一个子类

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/37d3d539b6003af36f87b1b99f0ac3591138b6cb.jpeg)

看个结果

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/d009b3de9c82d158c5bd9c3e2a2a1cddbc3e4223.jpeg)



## 二、深入分析static关键字

上面我们只是描述了一下static关键字的基本使用场景，下面主要解析一下static关键字的深层原理。要理解static为什么会有上面的特性，首先我们还需要从jvm内存说起。我们先给出一张java的内存结构图，然后通过案例描述一下static修饰的变量存放在哪。

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/024f78f0f736afc33409f1471839eec1b74512b4.jpeg)

从上图我们可以发现，**静态变量存放在方法区中**，**并且是被所有线程所共享**的。这里要说一下java堆，java堆存放的就是我们创建的一个个实例变量。

堆区:

- 存储的全部是对象，每个对象都包含一个与之对应的class的信息。(class的目的是得到操作指令)
- jvm只有一个堆区(heap)被所有线程共享，堆中不存放基本类型和对象引用，只存放**对象本身**

栈区:

- 每个线程包含一个栈区，栈中只保存**基础数据类型的对象和自定义对象的引用**(不是对象)，对象都存放在堆区中
- 每个栈中的数据(原始类型和对象引用)都是私有的，其他栈不能访问。
- 栈分为3个部分
  - 基本类型变量区
  - 执行环境上下文
  - 操作指令区(存放操作指令)

方法区:

- 又叫静态区，跟堆一样，被所有的线程共享。方法区包含所有的class和static变量。

- 方法区中包含的都是在整个程序中永远**唯一**的元素，如class，static变量。

下面通过一个案例说明一下，从内存的角度来看，static关键字为什么会有这样的特性。

首先我们定义一个类

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/359b033b5bb5c9ea023ba6fe7e19b3053bf3b38c.jpeg)

接下来我们从内存的角度出发，看看

![img](static%E5%85%B3%E9%94%AE%E5%AD%97.assets/f3d3572c11dfa9ec028d9199c8f0f206908fc147.jpeg)

从上面可以看到，我们的方法在调用的时候，是从方法区调用的，但是堆内存不一样，堆内存中的成员变量lastname是随着对象的产生而产生。随着对象的消失而消失。静态变量是所有线程共享的，所以不会消失。这也就能解释上面static关键字的真正原因。



## 三、小结

### **特点**

- static是一个修饰符，用于修饰成员。（成员变量，成员函数）static修饰的成员变量 称之为静态变量或类变量。
- static修饰的成员被所有的对象共享。
- static优先于对象存在，因为static的成员随着类的加载就已经存在。
- static修饰的成员多了一种调用方式，可以直接被类名所调用，（类名.静态成员）。
- static修饰的数据是共享数据，对象中的存储的是特有的数据。

### **成员变量和静态变量的区别**：

1. 生命周期的不同

- 成员变量随着对象的创建而存在随着对象的回收而释放。
- 静态变量随着类的加载而存在随着类的消失而消失。

2. 调用方式不同：

- 成员变量只能被对象调用。
- 静态变量可以被对象调用，也可以用类名调用。（推荐用类名调用）

3. 别名不同：

- 成员变量也称为实例变量。
- 静态变量称为类变量。

4. 数据存储位置不同：

- 成员变量数据存储在堆内存的对象中，所以也叫对象的特有数据。
- 静态变量数据存储在方法区（共享数据区）的静态区，所以也叫对象的共享数据。

5. 静态使用时需要注意的事项：

- 静态方法只能访问静态成员。（非静态既可以访问静态，又可以访问非静态）
- 静态方法中不可以使用this或者super关键字。
- 主函数是静态的