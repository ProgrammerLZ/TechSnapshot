## Java I/O

### 基于字节的I/O操作接口

### ByteStream

* 字节流是最底层的I/O留，尽量避免使用。

* 每次读取/写入一个字节，如图所示。

![Simple byte stream input and output.](Java IO/byteStream.gif)





### Java NIO

​							 	 put

​							   	 ↑

Everywhere  => ChannelA===read===>Buffer===write===>ChannelB => Everywhere

​							    	↓

​							 	 get





待解决：

* 内部类，静态内部类的熟练应用。
* 内部类中使用外部变量为什么必须声明为final。
* 递归。
* 标准I/O的概念。
* JDK 类图自动生成。
* 对象如何在内存当中存放。
* IO操作的缓冲有什么用？没有缓冲貌似更高效。换句话说，**块IO和流IO在性能上的差距主要产生在什么地方**？
* Socket概念上的理解，以及简单使用。
* Java socket/socketServer。
* 劝告式的锁？与排它锁的区别是什么？
* MappedByteBuffer的理解。
* 异步与多线程的关系。
* 异步与非阻塞有什么区别？