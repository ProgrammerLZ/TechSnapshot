# Java基本语法

### 1.Object&Classes

#### Constructors

```java
Public class Puppy{

    public Puppy(){

    }

    public Puppy(String name){

    }

}
```



#### Creating an Object

```java
public class Puppy{

        public Puppy(String name){

            System.out.println("Passed Name is :"+ name);

        }

        public static void main(String []args){

            Puppy myPupy = new Puppy("tommy");

        }

}
```



#### Accessing Instance Variables and Methods

ObjectReference = new Construtor();

ObjectReference.variableName;

ObjectReference.MethodName;

#### Source File Declaration Rules

- 每个源文件只有一个公共类。
- 一个源文件可以有多个非公共的类。
- 公共类的名字应该是源文件的名字。
- 如果一个类定义在一个包（文件夹？）里边，包的声明应该在这个源文件的第一行。
- import 声明应该在 package声明和类定义之间。

其他类：

- 内部类
- 私有类

### 2.Modifier Types

#### Access Control Modifiers

- Visible to the package, the default. No modifiers are needed.
- Visible to the class only (private).
- Visible to the world (public).
- Visible to the package and all subclasses (protected).

#### Non-Access Modifiders

- The static modifier for creating class methods and variables.
- The final modifier for finalizing the implementations of classes, methods, and variables.
- The abstract modifier for creating abstract classes and methods.
- The synchronized and volatile modifiers, which are used for threads.

### 3.Basic Operators

### 4.Loop Control

- while loop
- for loop
- do...while loop
- for(declaration : expression) { // Statements }

### 5.Number

### 6.String

#### Accessor Method

Method used to obtain information about an object are known as accessor methods.One accessor method that you can use with strings is the length() method,which returns the number of characters contained in the string object.

#### Creating Format Strings

There are two methods to creating fromat strings as follows:

```java
//The first method.

System.out.printf("The value of the float variable is"

                    + "%f,while the value of the integer"

                    + "variable is %d,and the string"

                    + is%s",floatVar,intVar,stringVar);

//The second method.

String fs;

fs = String.format("The value of the float variable is"

                    + "%f,while the value of the integer"

                    + "variable is %d,and the string"

                    + is%s",floatVar,intVar,stringVar);

System.out.println(fs);
```



### 7.Arrays

Array stores a fixedsize sequential collection of elements of the same type.

#### Declaring Array Variables

To use an array in a program,you must declare a variable to reference the array,and you must specify the type of array the vaiable can reference.

dataType [] arrayRefVar;

or

dataType arrayRefVar[];

#### Creating Arrays

You can create an array by using the new aperator with the following syntax -

arrayRefVar = new dataType[arraySize];

- It creates an array using new dataType[arraySize].
- It assigns the reference of the newly creted array to the catiable arrayRefVar.

The array elements are accessed through the index. 

#### Processing Arrays

When processing array elements,we often use either for loop or foreach loop because all of the elements in an array are of the sanme type and the size of the array is known. 

```java
public class TestArray{

    public static void main(String[] args){

    double[] myList = {1.9,2.9,3.4,2.5};

        //Print all the array elements

        for(int i = 0;i <myList.length;i++){

            System.out.println(myList[i] + "");

        }  

        //Summing all elements

     double total = 0;

        for (int i = 0;i < myList.length;i++){

         total += myList[i];

     }  

        System.out.println("Total is" +total);

        //Finding the largest element

        double max = myList[0];

        for (int i = 1; i < myList.length;i++){

            if(myList[i]> max) max = myList[i];

     }  

     System.out.println("Max is" + max);

  }     

}
```



#### The foreach Loops

Foreach loops enables you to traverse the complete array sequentially without using an index variable.

```java
public class TestArray{

    public static void main(String [] args){

        double[] myList = {1.9,2.9,3.4,3.5};

        //print all the array elements

        for (double element: myList){

            System.out.println(element);

        }

    }  

}
```



#### Passing Arrays to Methods

```java
public static void printArray(int[] array){

    for (int i = 0; i < array.length;i++){

        System.out.print(array[i]+"");

    }

}
```



#### Returing an Array from a Method

```java
public static int[] reverse(int[] list){

    int[] result = new int[list.length];

    for (int i =0,j=result.length - 1;i<list.length;i++,j--){

        result[j] = list[i];

    }

    return result;

}
```



### 8.Date and Time

#### Geting Current Date and Time

```java
import java.util.Date;

public class TestDate{

public static void main(String arg[]){

    //Instantiate a Date objec

    Date date = new Date();

    //Dispaly time and date using to String()

    System.out.println(date.toString());

}  

}
```



#### Date Comparison

Following are the three ways to compare two dates-

- Using the getTime() to obtain the number of milliseconds that have elapsed since midnight,January 1,1970,for both objects and the compare thes tow values.

- You can use the mehotds before(),after(),and equals().Because the 12th of the month comes before the 18th,for example

  new Date(99,2,12.before(new Date(99,2,18));

  return true.

- You can use the compareTo() method,which is defined by the Comparable interface and implemented by Date.

#### Date Formatting Using SimpleDateFormat

```java
public static void TestDateFormat(){

    Date dNow = new Date();

    SimpleDateFormat ft = new SimpleDateFormat("E yyy.MM.dd 'at' hh:mm:ss a zzz");

    System.out.println("current Date:"+ft.format(dNow));

}  
```



#### Date Formatting Using printf

   

```java
 public static void TestDateFormatUsingPrintf(){

        Date date = new Date();

        String str = String.format("Current Date/Time : %tc",date);

        System.out.printf(str);

    }
```

It would be a bit silly if you had to supply the date multiple times to format each part.For that reason,a format string acan indicate the index of the argument to be formatted.

  

```java
  public static void TestDateFormatUsingFormatString(){

    Date date = new Date();

    //System.out.printf("%1$s %2$tB %2td ,%2$tY","Due date:",date);

    System.out.printf("%s %tB %<te, %<tY", "Due date:", date);

}  
```



#### Parsing Strings into Dates

The SimpleDateFormat class has some additional methods,notably parse(),which tries to parse a string according to the format stored in the given SimpledateFormat object.

```java
public static void TestParsingStringIntoDates(){

    SimpleDateFormat ft = new SimpleDateFormat("yyyy-MM-dd");

    String input = mArg.length == 0 ? "1818-11-11":mArg[0];

    System.out.print(input +"Parses as");

    Date t;

    try{

        t = ft.parse(input);

        System.out.println(t);

    }catch(ParseException e){

        System.out.println("Unparseable using"+ ft);

    }  

}  
```



#### Sleepingfor a While

The following program would sleep for 3 seconds.

   

```java
 public static void TestSleep(){

    try{

        System.out.println(new Date() +"\n");

        Thread.sleep(5*60*10);//unit is millisecond.

        System.out.println(new Date() +"\n");

    }catch(Exception e){

        System.out.println("Got an exception!");

    }  

}  
```



#### Measuring Elapsed Time

   

```java
 public static void TestElapsedTime(){

    try{

        long start = System.currentTimeMillis();

        System.out.println(new Date() +"\n");

        Thread.sleep(5*60*10);

        System.out.println(new Date() +"\n");

        long end = System.currentTimeMillis();

        long diff = end - start;

        System.out.println("Difference is :" + diff);

    }catch(Exception e){

        System.out.println("Got a exception!");

    }

}
```



#### GregorianCalendar Class

GregoranCalendar is a concrete implementation of a Calendar clas that implements the normal Gregorian calendar with which you are familar.You can look up standard Java documentation for Calendar class.

### 9.Regular Expressions

正则表达式属于锦上添花的一种技术，他很强大，但是暂时先不要浪费时间去学，等到有大块时间的时候，一次性搞定。目前的目的是，先把Java的主要内容消化掉。

### 10.Methods

A java method is a collection of statements that are grouped together to perform an opreation.Now,we will do three things:

1. Create you own method with or without return values.
2. Invoke a method with or without parameters.
3. Apply method abstraction in the program design.

#### Creating Method

public static int methodName(int a,int b){

​    //Body

}

- public static——modifier
- int——return type
- methodNamne——name of the method
- a,b——formal parameters
- int a,int b —— list of parameters

#### Method Calling

For using a method,it should be called. There are two ways in which a method is called i.e., method returns a value or returning nothing.

public class ExampleMinNumber {

​    public static void main(String[] args) {

  int a = 11;

  int b = 6;

  int c = minFunction(a, b);

  System.out.println("Minimum Value = " + c);

}

/** returns the minimum of two numbers */

public static int minFunction(int n1, int n2) {

  int min;

  if (n1 > n2)

​     min = n2;

  else

​     min = n1;

  return min;

  }

#### Method Overloading

When a class has two or more methods by the same name but different parameter,it is known as method overloading.It is different from overriding. In overriding, a method has the same method name,type,number of parameters,etc.

```java
public class TestMethod{

public static void main(String[] args){

    int a = 11;

    int b = 6;

    double c = 7.3;

    double d = 9.4;

    int result1 = minFunction(a,b);

    //same function name with different parameters

    double result2 = minFunction(c,d);

    System.out.println("Minimum value =" +result1);

    System.out.println("Minimum value =" +result2);

}  

public static int minFunction(int n1,int n2){

    int min;

    if (n1 > n2)

        min = n2;

    else

        min = n1;

    return min;

}  

//方法重载

public static double minFunction(double n1, double n2) {

    double min;

    if (n1 > n2)

        min = n2;

    else

        min = n1;

    return min;

}

}
```

Overloading methods makes program readable.Here, two methods are given by the same name but with different parameters. The minimum number from integer and double types is result.

#### The Constructors

A constructor initializes an object when it is created.It has the same name as its class and is syntactically similar to a method. However, constructors have no explicit return type.

Typically, you will use a constructor to give initial values to the instance variables defined by the class,or to perform any other starup procedures required to create a fully fored object.

All classes have constructors,whether you define one or not,because Java automatically provides a default constructor that initializes all member variables to zero. However, once you define your own constructor,the default constructor is no longer used.

```java
class MyClass {

   int x;

   // Following is the constructor

   MyClass() {

      x = 10;

   }

}

public class ConsDemo {

   public static void main(String args[]) {

      MyClass t1 = new MyClass();

      MyClass t2 = new MyClass();

      System.out.println(t1.x + " " + t2.x);

   }

}
```



#### Parameterized Constructor

```java
class MyClass {

   int x;

   // Following is the constructor

   MyClass(int i ) {

      x = i;

   }

}

public class ConsDemo {

   public static void main(String args[]) {

      MyClass t1 = new MyClass( 10 );

      MyClass t2 = new MyClass( 20 );

      System.out.println(t1.x + " " + t2.x);

   }

}
```



#### The 'this' keyword

This is a keyword in java which is used as a reference to the object of the current class, with in an instance method or a constructor.Using this you can refer the members of a class such as constructors, variables and methods.

The keyword this is used to -

- Differentiate(区分) the instance variables from local variables if they have same names, within a constructor or a method.

  ```java
  - class Student {

       int age;  

       Student(int age) {

          this.age = age;  

       }

    }

  - 
  ```

  Call one type of constructor (parametrized(用参数表示) constructor or default) from other in a class. It is known as explicit constructor invocation(祈祷).

  ```java
  class Student {

     int age

     Student() {

        this(20);

     }

     Student(int age) {

        this.age = age;  

     }

  }
  ```

  ​

#### Variable Arguments

JDK1.5 enables you to pass a variable number of arguments of the same type to a method.

> typeName... parameterName

```java
public class VarargsDemo {

   public static void main(String args[]) {

      // Call method with variable args 

       printMax(34, 3, 3, 2, 56.5);

      printMax(new double[]{1, 2, 3});

   }

   public static void printMax( double... numbers) {

      if (numbers.length == 0) {

         System.out.println("No argument passed");

         return;

      }

      double result = numbers[0];

      for (int i = 1; i <  numbers.length; i++)

      if (numbers[i] >  result)

      result = numbers[i];

      System.out.println("The max value is " + result);

   }

}
```



#### The finalize() Method

It is possible to define a method that will be called just before an object's final destruction by the garbage collector. This method is called finalize( ), and it can be used to ensure that an object terminates cleanly.

### 11.Files and I/O

The [java.io](http://java.io) package contains nearly every class you might ever need to perform input and output in Java.All these streams represent an input source and an output destination.

#### 11.1 Stream

A stream can be defined as a sequence of data.There are two kinds of Streams:

- InputStream - Used to read data from source.
- OutPutStream - Used for writing data to a destination.

Java provides strong but flexible support for I/O related to files and networks.

##### 11.1.1 Byte Stream

Byte stream are used to perform input and output of 8-bit bytes. Though there are many classes related to byte streams but the most frequently used classes are FileInputStream and FileOutputStream.

```java
import java.io.*;

public class CopyFile {

   public static void main(String args[]) throws IOException { 

      FileInputStream in = null;

      FileOutputStream out = null;

      try {

         in = new FileInputStream("input.txt");

         out = new FileOutputStream("output.txt");

         int c;

         while ((c = in.read()) != -1) {

            out.write(c);

         }

      }finally {

         if (in != null) {

            in.close();

         }

         if (out != null) {

            out.close();

         }

      }

   }

}
```



##### 11.1.2 Character Streams

Java Character Streams are used to perform input and output for 16-bit unicode.The most frequently used classes are FileReade and FilwWriter.

```java
import java.io.*;

public class CopyFile {

   public static void main(String args[]) throws IOException {

      FileReader in = null;

      FileWriter out = null;

      try {

         in = new FileReader("input.txt");

         out = new FileWriter("output.txt");

         int c;

         while ((c = in.read()) != -1) {

            out.write(c);

         }

      }finally {

         if (in != null) {

            in.close();

         }

         if (out != null) {

            out.close();

         }

      }

   }

}
```



#### 11.2 Standard Stream

All the programming languages provide support for standard I/O where the user's program can take input from a keyboard and then produce an output on the computer [screen.In](http://screen.In) C and C++ programming languages,have three standard devices STDIN,STDOUT and STDERR. Similarly,Java provides the following three standard streams -

- Standard Input - This is used to feed the data to user's pogram and usually a keyboard is used as standard input stream and represented as [System.in](http://System.in).

- Standard Output - This is used to output the data produced by the user's program and usually a computer screen is used for standard output stream and represented as System.out.

- Standard Error - This is used to output the error data produced by the user' s program and usually a computer screen is used for standard error stream and represented as Ststem.err.

  ```java
  import java.io.*;

  public class ReadConsole {

     public static void main(String args[]) throws IOException {

        InputStreamReader cin = null;

        try {

           cin = new InputStreamReader(System.in);

           System.out.println("Enter characters, 'q' to quit.");

           char c;

           do {

              c = (char) cin.read();

              System.out.print(c);

           } while(c != 'q');

        }finally {

           if (cin != null) {

              cin.close();

           }

        }

     }

  }
  ```

  ​

#### 11.3 Reading and Writing Files

The two important streams are FileInputStream and FileOutPutStream.Here is the example to demonstrate InputStream and OutputStream -

```java
import java.io.*;

public class fileStreamTest {

   public static void main(String args[]) {

      try {

         byte bWrite [] = {11,21,3,40,5};

         OutputStream os = new FileOutputStream("test.txt");

         for(int x = 0; x < bWrite.length ; x++) {

            os.write( bWrite[x] );   // writes the bytes

         }

         os.close();

         InputStream is = new FileInputStream("test.txt");

         int size = is.available();

         for(int i = 0; i < size; i++) {

            System.out.print((char)is.read() + "  ");

         }

         is.close();

      } catch (IOException e) {

         System.out.print("Exception");

      }

   }

}
```



#### 11.4 Directories inJava

A directory is a File which can contain a list of other files and directories. You use File object to create directories, to list down files available in a directory.

##### 11.4.1 Creating Directories

There are two useful File utility methods, which can be used to create directories - 

- mkdir() - Creates a directory, returning true on succ and false on failure.

- mkdirs() - Creates both a directory and all the parents of the directory. 

  ```java
  import java.io.File;

  public class CreateDir {

     public static void main(String args[]) {

        String dirname = "/tmp/user/java/bin";

        File d = new File(dirname);

        // Create directory now.

        d.mkdirs();

     }

  }
  ```

  ​

##### 11.4.2 Listing Directories

You can use list() method provided by File object to list down all the files and directories available in a directory as follows -

​    

```java
import java.io.File;

    public class ReadDir {

       public static void main(String[] args) {

          File file = null;

          String[] paths;

          try {     

             // create new file object

             file = new File("/tmp");

             // array of files and directory

             paths = file.list();

             // for each name in the path array

             for(String path:paths) {

                // prints filename and directory name

                System.out.println(path);

             }

          } catch (Exception e) {

             // if any error occurs

             e.printStackTrace();

          }

       }

}
```



### 12.Exceptions

A exception (or exceptional event) is a problem that arises during the execution of a program.

An exception can occur form any different reasons - 

- A user has entered an invalid data.
- A file that needs to be opened cannot be found.
- A netword connection haas been lost in the middle of communications or the JVM has run out of memory.

We have three categories of Exception - 

1. Checked exceptions

   A checked exception is an exception that occurs at the compile time, these are also called as compile time exceptions.

2. Unchecked exceptions

   A unchecked exception is an exception that occurs at the time of execution.These are also called as Runtime Exceptions.These include programming bugs, such as logic errors or improper(不正确的) use of an API. Runtime exceptions are ignored at the time of compilation.

3. Errors

   These are not exceptions at all, but problems that arise beyond the control of the user or the programmer. Errors are typically ignored in your code because you can rarely do anything about an error. For example, if a stack overflow occurs, an error will arise. They are also ignored at the time of compilation.

#### 12.1 Exception Hierarchy

![img](data:image/png;base64,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)

Errors are abnormal conditions that happen in case of severe failures, these are not handled by the Java programs. Errors are generated to indicate errors generated by the runtime environment. Example: JVM is out of memory. Normally, programs cannot recover from errors.

The Exception class has two main subclasses(亚纲): IOException class and RuntimeException Class.

#### 12.2 Catching Exceptions

A method catches an exception using a combination of the try and catch keywords. Code within a try/catch block is referred to as protected code, and the syntax for using try/catch looks like the following − 

​    

```java
try {

       // Protected code

    } catch (ExceptionName e1) {

       // Catch block

    }
```

The following is an array declared with 2 elements. Then the code tries to access the 3rd element of the array which throws an exception.

   

```java
 // File Name : ExcepTest.java

    import java.io.*;

    public class ExcepTest {

       public static void main(String args[]) {

          try {

             int a[] = new int[2];

             System.out.println("Access element three :" + a[3]);

          } catch (ArrayIndexOutOfBoundsException e) {

             System.out.println("Exception thrown  :" + e);

          }

          System.out.println("Out of the block");

       }

    }
```

Every try block should be immediately followed either by a catch block or finally block.

#### 12.3 Multiple Catch Blocks

A try block can be followed by multiple catch blocks. The syntax for multiple catch blocks looks like the following −

   

```java
 try {

       // Protected code

    } catch (ExceptionType1 e1) {

       // Catch block

    } catch (ExceptionType2 e2) {

       // Catch block

    } catch (ExceptionType3 e3) {

       // Catch block

    }
```

Here is code segment showing how to use multiple try/catch statements.

​    

```java
try {

       file = new FileInputStream(fileName);

       x = (byte) file.read();

    } catch (IOException i) {

       i.printStackTrace();

       return -1;

    } catch (FileNotFoundException f) // Not valid! {

       f.printStackTrace();

       return -1;

    }
```



#### 12.4 Catching Multiple Type of Exceptions

Here is how you would do it −

​    

```java
catch (IOException|FileNotFoundException ex) {

       logger.log(ex);

       throw ex;

      }
```



#### 12.5 The Throws/Throw Keywords

If a method does not handle a checked exception, the method must declare it using the throws keyword. You can throw an exception, either a newly instantiated(例示) one or an exception that you just caught, by using the throw keyword. 

​    

```java
import java.io.*;

    public class className {

       public void deposit(double amount) throws RemoteException {

          // Method implementation

          throw new RemoteException();

       }

       // Remainder of class definition

    }
```

A method can declare that it throws more than one exception, in which case the exceptions are declared in a list separated by commas.

   

```java
 import java.io.*;

    public class className {

       public void withdraw(double amount) throws RemoteException,

          InsufficientFundsException {

          // Method implementation

       }

       // Remainder of class definition

    }
```



#### 12.6 The Finally Block

Using a finally block allows you to run any cleanup-type statements that you want to execute, no matter what happens in the protected code.

```java
public class ExcepTest {

   public static void main(String args[]) {

      int a[] = new int[2];

      try {

         System.out.println("Access element three :" + a[3]);

      } catch (ArrayIndexOutOfBoundsException e) {

         System.out.println("Exception thrown  :" + e);

      }finally {

         a[0] = 6;

         System.out.println("First element value: " + a[0]);

         System.out.println("The finally statement is executed");

      }

   }

}


```

Note the following −

- A catch clause cannot exist without a try statement.
- It is not compulsory(义务的) to have finally clauses whenever a try/catch block is present.
- The try block cannot be present without either catch clause or finally clause.
- Any code cannot be present in between the try, catch, finally blocks.

#### 12.7 The try-with-resources

try-with-resources, also referred as automatic resource management, is a new exception handling mechanism that was introduced in Java 7, which automatically closes the resources used within the try catch block.

```java
import java.io.FileReader;

import java.io.IOException;

public class Try_withDemo {

   public static void main(String args[]) {

      try(FileReader fr = new FileReader("<E://file.txt>")) {

         char [] a = new char[50];

         fr.read(a);   // reads the contentto the array

         for(char c : a)

         System.out.print(c);   // prints the characters one by one

      } catch (IOException e) {

         e.printStackTrace();

      }

   }

}
```



- To use a class with try-with-resources statement it should implement AutoCloseable interface and the close() method of it gets invoked automatically at runtime.
- You can declare more than one class in try-with-resources statement.
- While you declare multiple classes in the try block of try-with-resources statement these classes are closed in reverse order.
- Except the declaration of resources within the parenthesis(插入语) everything is the same as normal try/catch block of a try block.
- The resource declared in try gets instantiated(例示) just before the start of the try-block.
- The resource declared at the try block is implicitly(含蓄地) declared as final.

#### 12.8 User-defined Exceptions

You just need to extend the predefined(预先确定) Exception class to create your own Exception. These are considered to be checked exceptions.The following InsufficientFundsException class is a user-defined exception that extends the Exception class, making it a checked exception. An exception class is like any other class, containing useful fields and methods.

   

```java
 // File Name InsufficientFundsException.java

    import java.io.*;

    public class InsufficientFundsException extends Exception {

       private double amount;

       public InsufficientFundsException(double amount) {

          this.amount = amount;

       }

       public double getAmount() {

          return amount;

       }

    }
```

the following CheckingAccount class contains a withdraw() method that throws an InsufficientFundsException.

```java
// File Name CheckingAccount.java

import java.io.*;

public class CheckingAccount {

   private double balance;

   private int number;

   public CheckingAccount(int number) {

      this.number = number;

   }

   public void deposit(double amount) {

      balance += amount;

   }

   public void withdraw(double amount) throws InsufficientFundsException {

      if(amount <= balance) {

         balance -= amount;

      }else {

         double needs = amount - balance;

         throw new InsufficientFundsException(needs);

      }

   }

   public double getBalance() {

      return balance;

   }

   public int getNumber() {

      return number;

   }

}
```

The following BankDemo program demonstrates invoking the deposit() and withdraw() methods of CheckingAccount.

```java
// File Name BankDemo.java

public class BankDemo {

   public static void main(String [] args) {

      CheckingAccount c = new CheckingAccount(101);

      System.out.println("Depositing $500...");

      c.deposit(500.00);

      try {

         System.out.println("\nWithdrawing $100...");

         c.withdraw(100.00);

         System.out.println("\nWithdrawing $600...");

         c.withdraw(600.00);

      } catch (InsufficientFundsException e) {

         System.out.println("Sorry, but you are short $" + e.getAmount());

         e.printStackTrace();

      }

   }

}
```



- All exceptions must be a child of Throwable.
- If you want to write a checked exception that is automatically enforced by the Handle or Declare Rule, you need to extend the Exception class.
- If you want to write a runtime exception, you need to extend the RuntimeException class.

#### 12.9 Common Exceptions

In Java, it is possible to define two catergories of Exceptions and Errors.

- JVM Exceptions − These are exceptions/errors that are exclusively or logically thrown by the JVM. Examples: NullPointerException, ArrayIndexOutOfBoundsException, ClassCastException.
- Programmatic Exceptions − These exceptions are thrown explicitly by the application or the API programmers. Examples: IllegalArgumentException, IllegalStateException.

### 13.Inner classes

In Java, just like methods, variables of a class too can have another class as its member. Writing a class within another is allowed in Java. The class written within is called the nested class, and the class that holds the inner class is called the outer class.

```
class Outer_Demo {

   class Nested_Demo {

   }

}
```

Nested classes are divided into two types −

- Non-static nested classes − These are the non-static members of a class.
- Static nested classes − These are the static members of a class.

![img](data:image/png;base64,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)

#### 13.1 Inner Classes(Non-static Nested Classes)

Inner classes are a security mechanism in Java.Inner classes are of three types depending on how and where you define them. They are −

- Inner Class
- Method-local Inner Class
- Anonymous Inner Class

##### 13.1.1 Inner CLass

Following is the program to create an inner class and access it. In the given example, we make the inner class private and access the class through a method.

c

```
lass Outer_Demo {

   int num;

   // inner class

   private class Inner_Demo {

      public void print() {

         System.out.println("This is an inner class");

      }

   }

   // Accessing he inner class from the method within

   void display_Inner() {

      Inner_Demo inner = new Inner_Demo();

      inner.print();

   }

}

public class My_class {

   public static void main(String args[]) {

      // Instantiating the outer class

      Outer_Demo outer = new Outer_Demo();

      // Accessing the display_Inner() method.

      outer.display_Inner();

   }


```

}

##### 13.1.2 Accessing the Private Members

As mentioned earlier, inner classes are also used to access the private members of a class.The following program shows how to access the private members of a class using inner class.

c

```
lass Outer_Demo {

   // private variable of the outer class

   private int num = 175; 

   // inner class

   public class Inner_Demo {

      public int getNum() {

         System.out.println("This is the getnum method of the inner class");

         return num;

      }

   }

}

public class My_class2 {

   public static void main(String args[]) {

      // Instantiating the outer class

      Outer_Demo outer = new Outer_Demo();

      // Instantiating the inner class

      Outer_Demo.Inner_Demo inner = outer.new Inner_Demo();

      System.out.println(inner.getNum());

   }

}
```



> 问题：为什么要用私有类访问私有变量？这样做的意义是什么？还有，为什么要用访问器方法来封装 变量的改变，这样设计的好处是什么？

##### 13.1.3 Method-local Inner Class

In Java, we can write a class within a method and this will be a local type. Like local variables, the scope of the inner class is restricted within the method.

A method-local inner class can be instantiated only within the method where the inner class is defined. The following program shows how to use a method-local inner class.

```
public class Outerclass {

   // instance method of the outer class

   void my_Method() {

      int num = 23;

      // method-local inner class

      class MethodInner_Demo {

         public void print() {

            System.out.println("This is method inner class "+num);    

         }  

      } // end of inner class

      // Accessing the inner class

      MethodInner_Demo inner = new MethodInner_Demo();

      inner.print();

   }

   public static void main(String args[]) {

      Outerclass outer = new Outerclass();

      outer.my_Method();          

   }

}
```



##### 13.1.4Anonymous Inner Class

An inner class declared without a class name is known as an anonymous inner class. In case of anonymous inner classes, we declare and instantiate them at the same time. Generally, they are used whenever you need to override the method of a class or an interface.The following program shows how to override the method of a class using anonymous inner class.

```
abstract class AnonymousInner {

   public abstract void mymethod();

}

public class Outer_class {

   public static void main(String args[]) {

      AnonymousInner inner = new AnonymousInner() {

         public void mymethod() {

            System.out.println("This is an example of anonymous inner class");

         }

      };

      inner.mymethod();

   }

}
```



##### 13.1.5Anonymous Inner Class as Argument

```
// interface

interface Message {

   String greet();

}

public class My_class {

   // method which accepts the object of interface Message

   public void displayMessage(Message m) {

      System.out.println(m.greet() +

         ", This is an example of anonymous inner class as an argument"); 

   }

   public static void main(String args[]) {

      // Instantiating the class

      My_class obj = new My_class();

      // Passing an anonymous inner class as an argument

      obj.displayMessage(new Message() {

         public String greet() {

            return "Hello";

         }

      });

   }

}
```



#### 13.2 Static Nested Class

A static inner class is a nested class which is a static member of the outer class. It can be accessed without instantiating(例示) the outer class, using other static members. Just like static members, a static nested class does not have access to the instance variables and methods of the outer class.The syntax of static nested class is as follows −

```
public class Outer {

   static class Nested_Demo {

​      public void my_method() {

​         System.out.println("This is my nested class");

​      }

   }

   public static void main(String args[]) {

​      Outer.Nested_Demo nested = new Outer.Nested_Demo();   

​      nested.my_method();

   }

}
```

