## 一、Java多线程初探

### 1. Create Thread

#### 1.1 Create a Thread by Implementing a Runnable Interface

```java
class RunnableDemo implements Runnable {

   private Thread t;

   private String threadName;

   RunnableDemo( String name) {

      threadName = name;

      System.out.println("Creating " +  threadName );

   }

   public void run() {

      System.out.println("Running " +  threadName );

      try {

         for(int i = 4; i > 0; i--) {

            System.out.println("Thread: " + threadName + ", " + i);

            // Let the thread sleep for a while.

            Thread.sleep(50);

         }

      } catch (InterruptedException e) {

         System.out.println("Thread " +  threadName + " interrupted.");

      }

      System.out.println("Thread " +  threadName + " exiting.");

   }

   public void start () {

      System.out.println("Starting " +  threadName );

      if (t == null) {

         t = new Thread (this, threadName);

         t.start ();

      }

   }

}

public class TestThread {

   public static void main(String args[]) {

      RunnableDemo R1 = new RunnableDemo( "Thread-1");

      R1.start();

      RunnableDemo R2 = new RunnableDemo( "Thread-2");

      R2.start();

   }  

}
```



### 1.2 Create a Thread by Extending a Thread Class

```java
class ThreadDemo extends Thread {

   private Thread t;

   private String threadName;

   ThreadDemo( String name) {

      threadName = name;

      System.out.println("Creating " +  threadName );

   }

   public void run() {

      System.out.println("Running " +  threadName );

      try {

         for(int i = 4; i > 0; i--) {

            System.out.println("Thread: " + threadName + ", " + i);

            // Let the thread sleep for a while.

            Thread.sleep(50);

         }

      } catch (InterruptedException e) {

         System.out.println("Thread " +  threadName + " interrupted.");

      }

      System.out.println("Thread " +  threadName + " exiting.");

   }

   public void start () {

      System.out.println("Starting " +  threadName );

      if (t == null) {

         t = new Thread (this, threadName);

         t.start ();

      }

   }

}

public class TestThread {

   public static void main(String args[]) {

      ThreadDemo T1 = new ThreadDemo( "Thread-1");

      T1.start();

      ThreadDemo T2 = new ThreadDemo( "Thread-2");

      T2.start();

   }  

}
```



### 2.Thread Synchronization

Synchronization in java is the capability to control the access of multiple threads to any shared resource.

There are two types of thread synchronization mutual exclusive and inter-thread communication.

- Mutual Exclusive
- - Synchronized method.
  - Synchronized block.
  - static synchronization.
- Cooperation (Inter-thread communication in java)

#### 2.1 Mutual Exclusive

Mutual Exclusive helps keep threads from interfering with one another while sharing data. This can be done by three ways in java:

- by synchronized method
- by synchronized block
- by static synchronization

#### 2.2 Concept of Lock in Java

Synchronization is built around an internal entity known as the lock or monitor. Every object has an lock associated with it. By convention, a thread that needs consistent access to an object's fields has to acquire the object's lock before accessing them, and then release the lock when it's done with them.

#### 2.3 Java synchronized method

If you declare any method as synchronized, it is known as synchronized method.

Synchronized method is used to lock an object for any shared resource.

When a thread invokes a synchronized method, it automatically acquires the lock for that object and releases it when the thread completes its task.

```java
//example of java synchronized method 

class Table{ 

synchronized void printTable(int n){//synchronized method 

   for(int i=1;i<=5;i++){ 

     System.out.println(n*i); 

     try{ 

      Thread.sleep(400); 

     }catch(Exception e){System.out.println(e);} 

   } 

} 

} 

class MyThread1 extends Thread{ 

Table t; 

MyThread1(Table t){ 

this.t=t; 

} 

public void run(){ 

t.printTable(5); 

} 

} 

class MyThread2 extends Thread{ 

Table t; 

MyThread2(Table t){ 

this.t=t; 

} 

public void run(){ 

t.printTable(100); 

} 

} 

public class TestSynchronization2{ 

public static void main(String args[]){ 

Table obj = new Table();//only one object 

MyThread1 t1=new MyThread1(obj); 

MyThread2 t2=new MyThread2(obj); 

t1.start(); 

t2.start(); 

} 

} 

//Program of synchronized method by using annonymous class 

class Table{ 

synchronized void printTable(int n){//synchronized method 

   for(int i=1;i<=5;i++){ 

     System.out.println(n*i); 

     try{ 

      Thread.sleep(400); 

     }catch(Exception e){System.out.println(e);} 

   } 

} 

} 
```



#### 2.4 Example of synchronized method by using annonymous class

```java
public class TestSynchronization3{

    public static void main(String args[]){

        final Table obj = new Table();//only one object

        Thread t1=new Thread(){

            public void run(){

                obj.printTable(5);

            }

        };

        Thread t2=new Thread(){

            public void run(){

                obj.printTable(100);

            }

        };

        t1.start();

        t2.start();

    }

}  

```

由代码可见，匿名类要比上一个例子写起来更加方便，代码更少，可读性更高。

#### 2.5 Synchronized block in java

Synchronized block can be used to perform synchronization on any specific resource of the method.

Suppose you have 50 lines of code in your method, but you want to synchronize only 5 lines, you can use synchronized block. 

Points to remember for Synchronized block:

- Synchronized block is used to lock an object for any shared resource.
- Scope of synchronized block is smaller than the method.

##### 2.5.1 Example of synchronized block

同步的意义是，该资源此时此刻由我占有，其他线程靠边站，等我用完了再说。 

```java
class Table{

    void printTable(int n){

        synchronized(this){//synchronized block

            for(int i=1;i<=5;i++){

                System.out.println(n*i);

                try{

                    Thread.sleep(400);

                }catch(Exception e){System.out.println(e);}

            }

        }

    }//end of the method

}

class MyThread1 extends Thread{

    Table t;

    MyThread1(Table t){

        this.t=t;

    }

    public void run(){

        t.printTable(5);

    }

}

class MyThread2 extends Thread{

    Table t;

    MyThread2(Table t){

        this.t=t;

    }

    public void run(){

        t.printTable(100);

    }

}

public class TestSynchronizedBlock1{

    public static void main(String args[]){

        Table obj = new Table();//only one object

        MyThread1 t1=new MyThread1(obj);

        MyThread2 t2=new MyThread2(obj);

        t1.start();

        t2.start();

    }

}
```



#### 2.6 Static synchronization

If you make any static method as synchronized, the lock will be on the class not on object. 

##### 2.6.1 Problem without static synchronization

Suppose there are two objects of a shared class(e.g. Table) named object1 and [object2.In](http://object2.In) case of synchronized method and synchronized block there cannot be interference between t1 and t2 or t3 and t4 because t1 and t2 both refers to a common object that have a single lock.But there can be interference between t1 and t3 or t2 and t4 because t1 acquires another lock and t3 acquires another lock.I want no interference between t1 and t3 or t2 and t4.Static synchronization solves this problem.

意思就是：假如一个类的两个对象，关于static的方法和域，这两个对象是共享的。当两个线程，在两个不同的对象上进行操作的时候，普通的锁只能保证两个对象的实例方法和实例域保持同步，而不能保证静态方法和静态域的同步。Static Synchronization就是用来解决这个问题的：他会将类进行同步。

##### 2.6.2 Example of static synchronization

```java
class Table{

    synchronized static void printTable(int n){

        for(int i=1;i<=10;i++){

            System.out.println(n*i);

            try{

                Thread.sleep(400);

            }catch(Exception e){}

        }

    }

}

class MyThread1 extends Thread{

    public void run(){

        Table.printTable(1);

    }

}

class MyThread2 extends Thread{

    public void run(){

        Table.printTable(10);

    }

}

class MyThread3 extends Thread{

    public void run(){

        Table.printTable(100);

    }

}

class MyThread4 extends Thread{

    public void run(){

        Table.printTable(1000);

    }

}

public class TestSynchronization4{

    public static void main(String t[]){

        MyThread1 t1=new MyThread1();

        MyThread2 t2=new MyThread2();

        MyThread3 t3=new MyThread3();

        MyThread4 t4=new MyThread4();

        t1.start();

        t2.start();

        t3.start();

        t4.start();

    }

}
```



### 3.Deadlock in java

Deadlock can occur in a situation when a thread is waiting for an object lock, that is acquired by another thread and second thread is waiting for an object lock that is acquired by first thread. Since, both threads are waiting for each other to release the lock, the condition is called deadlock.

#### 3.1 Example of Deadlock in java

```java
public class TestDeadlockExample1 {

       public static void main(String[] args) {

           final String resource1 = "ratan jaiswal";

           final String resource2 = "vimal jaiswal";

           // t1 tries to lock resource1 then resource2

           Thread t1 = new Thread() {

               public void run() {

                   synchronized (resource1) {

                       System.out.println("Thread 1: locked resource 1");

                       try { Thread.sleep(100);} catch (Exception e) {}

                       synchronized (resource2) {

                           System.out.println("Thread 1: locked resource 2");

                       }

                   }

               }

           };

           // t2 tries to lock resource2 then resource1

           Thread t2 = new Thread() {

               public void run() {

                   synchronized (resource2) {

                       System.out.println("Thread 2: locked resource 2");

                       try { Thread.sleep(100);} catch (Exception e) {}

                       synchronized (resource1) {

                           System.out.println("Thread 2: locked resource 1");

                       }

                   }

               }

           };

           t1.start();

           t2.start();

       }

}
```



### 4.Inter-thread communicvation in Java

Inter-thread communication or Co-operation is all about allowing synchronized(同步的) threads to communicate with each other.

Cooperation (Inter-thread communication) is a mechanism in which a thread is paused running in its critical section and another thread is allowed to enter (or lock) in the same critical section to be [executed.It](http://executed.It) is implemented by following methods of Object class:

- wait()

Causes current thread to release the lock and wait until either another thread invokes the notify() method or the notifyAll() method for this object, or a specified amount of time has elapsed.

The current thread must own this object's monitor, so it(wait()) must be called from the synchronized method only otherwise it will throw exception.

- notify()
- notifyAll()

#### 4.1 Understanding the process of inter-thread communication

![img](data:image/png;base64,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)

The point to point explanation of the above diagram is as follows:

1. Threads enter to acquire lock.
2. Lock is acquired by on thread.
3. Now thread goes to waiting state if you call wait() method on the object. Otherwise it releases the lock and exits.
4. If you call notify() or notifyAll() method, thread moves to the notified state (runnable(可追捕的) state).
5. Now thread is available to acquire lock.
6. After completion of the task, thread releases the lock and exits the monitor state of the object.

#### 4.2 Example of inter thread communication in java

```java
class Customer{

    int amount=10000;

    //取款

    synchronized void withdraw(int amount){

        System.out.println("going to withdraw...");

        if(this.amount<amount){

            System.out.println("Less balance; waiting for deposit...");

            try{

                wait();

            }catch(Exception e){}

        }

        this.amount-=amount;

        System.out.println("withdraw completed...");

    }

    //存款

    synchronized void deposit(int amount){

        System.out.println("going to deposit...");

        this.amount+=amount;

        System.out.println("deposit completed... ");

        notify();

    }

}

class Test{

    public static void main(String args[]){

        final Customer c=new Customer();

        new Thread(){

            public void run()

            {

                c.withdraw(15000);

            }

        }.start();

        new Thread(){

            public void run()

            {

                c.deposit(10000);

            }

        }.start();

    }

}
```



### 5 Interrupting a Thread

If any thread is in sleeping or waiting state (i.e. sleep() or wait() is invoked), calling the interrupt() method on the thread, breaks out the sleeping or waiting state throwing InterruptedException. If the thread is not in the sleeping or waiting state, calling the interrupt() method performs normal behaviour and doesn't interrupt the thread but sets the interrupt flag to true. Let's first see the methods provided by the Thread class for thread interruption.

#### 5.1 The 3 methods provided by the Thread class for interrupting a thread

- public void interrupt()
- public static boolean interrupted()
- public boolean isInterrupted()

#### 5.2 Example of interrupting a thread that stops working

```java
class TestInterruptingThread1 extends Thread{

    public void run(){

        try{

            Thread.sleep(1000);

            System.out.println("task");

        }catch(InterruptedException e){

            throw new RuntimeException("Thread interrupted..."+e);

        }

    }

    public static void main(String args[]){

        TestInterruptingThread1 t1=new TestInterruptingThread1();

        t1.start();

        try{

            t1.interrupt();

        }catch(Exception e){System.out.println("Exception handled "+e);}

    }

}
```



#### 5.3 Example of interrupting thread that behaves normally

class TestInterruptingThread3 extends Thread{

​    public void run(){

​        for(int i=1;i<=5;i++)

​            System.out.println(i);

​    }

​    public static void main(String args[]){

​        TestInterruptingThread3 t1=new TestInterruptingThread3();

​        t1.start();

​        t1.interrupt();

​    }

}

#### 5.4 What about isInterrupted and interrupted method?

The isInterrupted() method returns the interrupted flag either true or false. The static interrupted() method returns the interrupted flag afterthat it sets the flag to false if it is true.

```java
public class TestInterruptingThread4 extends Thread{

    public void run(){

        for(int i=1;i<=2;i++){

            if(Thread.interrupted()){

                System.out.println("code for interrupted thread");

            }

            else{

                System.out.println("code for normal thread");

            }

        }//end of for loop

    }

    public static void main(String args[]){

        TestInterruptingThread4 t1=new TestInterruptingThread4();

        TestInterruptingThread4 t2=new TestInterruptingThread4();

        t1.start();

        t1.interrupt();

        t2.start();

    }

}
```



### 6 Reentrant Monitor in Java

According to Sun Microsystems, Java monitors are reentrant means java thread can reuse the same monitor for different synchronized(同步的) methods if method is called from the method.

#### 6.1 Advantage of Reentrant Monitor

It eliminates the possibility of single thread deadlocking.

```java
class Reentrant {

    public synchronized void m() {

        n();

        System.out.println("this is m() method");

    }

    public synchronized void n() {

        System.out.println("this is n() method");

    }

}

public class ReentrantExample{

    public static void main(String args[]){

        final ReentrantExample re=new ReentrantExample();

        Thread t1=new Thread(){

            public void run(){

                re.m();//calling method of Reentrant class

            }

        };

        t1.start();

    }

}
```

