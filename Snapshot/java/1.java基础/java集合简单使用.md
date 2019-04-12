# Java集合

## 数组

数组实质上是相同类型的元素的集合

可以通过两种方式声明数组：

- 创建一个具有一定大小的数组，这个大小在数组的整个生存期中是固定的。
- 创建一个具有一组初始值的数组。此集合的大小确定了数组的大小 — 它的大小恰好能够包含所有这些值，而且的它的大小在数组的整个生存期中是固定的。

## 声明一个数组

一般而言，可以像这样声明一个数组：

```java
new elementType [arraySize]
```

可以通过两种方式创建一个整数元素数组。这条语句创建一个拥有 5 个元素的空间的数组，但它是空的：

```java
// creates an empty array of 5 elements:

int[] integers = new int[5];
```

这条语句创建该数组并一次性初始化它：

```java
// creates an array of 5 elements with values:

int[] integers = new int[] { 1, 2, 3, 4, 5 };
```

或

```java
// creates an array of 5 elements with values (without the new operator):
int[] integers = { 1, 2, 3, 4, 5 };
```

初始值放在花括号内，使用逗号分隔。

另一种创建数组的方式是首先创建它，然后编写一个循环来初始化它：

前

```java
int[] integers = new int[5];for (int aa = 0; aa < integers.length; aa++) {integers[aa] = aa+1;}
```

面的代码声明一个包含 5 个元素的整数数组。如果尝试在该数组中放入 5 个以上的元素，Java 运行时就会抛出一个*异常*。您将在[第 14 单元](http://www.ibm.com/developerworks/cn/java/j-perry-exceptions/index.html) 中学习异常和如何处理它们。

### 加载数组

要加载数组，需要对从 1 一直到数组长度的整数执行循环（可以在数组上调用.length来获取它的长度，稍后将更详细地介绍此内容）。在本例中，循环在到达 5 时停止。

加载数组后，您可以像之前一样访问它：

```java
Logger l = Logger.getLogger("Test");  
for (int aa = 0; aa < integers.length; aa++) {      
	l.info("This little integer's value is: " + integers[aa]);  
}
```

此语法也有效，而且（因为它更容易使用）本单元全部使用此语法：

```java
   Logger l = Logger.getLogger("Test");     

   for (int i : integers) {         

   	l.info("This little integer's value is: " + i);     

}
```

### 元素索引

可以将数组想象为一系列的桶，每个桶中放入一个某种类型的元素。每个桶均可通过一个元素*索引* 来访问：

```java
element = arrayName [elementIndex];
```

要访问一个元素，您需要数组的引用（它的名称）和包含您想要的元素的索引。

### length 属性

每个数组有一个length属性，该属性具有public可视性，可用于确定可将多少个元素放入该数组中。要访问此属性，可使用数组引用、一个句点 (.) 和关键字 length，就象这样：

 int arraySize = arrayName.length; 

Java 语言中的数组*从 0 开始建立索引*。也就是说，对于所有数组，数组中的第一个元素始终位于 *arrayName*[0] ，最后一个元素位于*arrayName*[*arrayName*.length - 1] 。

### 对象数组

您已了解数组如何包含原语类型，但值得一提的是，它们还可以包含对象。创建一个java.lang.Integer对象数组与创建一个原语类型数组没有太大区别，您可以通过两种方式进行创建：

```java
// creates an empty array of 5 elements:Integer[] integers = new Integer[5]; 

// creates an array of 5 elements with values:
Integer[] integers = new Integer[] { 
	Integer.valueOf(1), 
	Integer.valueOf(2)
	Integer.valueOf(3)
	Integer.valueOf(4)
	Integer.valueOf(5));
}
```



## 装箱和拆箱

Java 语言中的每种原语类型都有一个对应的 JDK 类，如表 1 所示。

| 原语    | 对应的 JDK 类       |
| ------- | ------------------- |
| boolean | java.lang.Boolean   |
| byte    | java.lang.Byte      |
| char    | java.lang.Character |
| short   | java.lang.Short     |
| int     | java.lang.Integer   |
| long    | java.lang.Long      |
| float   | java.lang.Float     |
| double  | java.lang.Double    |

每个 JDK 类都提供了相应方法来解析它的内部表示，并将其转换为相应的原语类型。例如，下面这段代码将十进制值 238 转换为一个Integer ：

```java
int value = 238;
Integer boxedValue = Integer.valueOf(value);
```

此技术称为***装箱***，因为您将原语放在一个包装器或箱子中。

类似地，要将Integer表示转换为对应的int类，可以对它执行***拆箱***：

```java
Integer boxedValue = Integer.valueOf(238); //装箱
int intValue = boxedValue.intValue(); //拆箱
```



### 自动装箱和自动拆箱

严格地讲，您**不需要显式装箱和拆箱原语**。可以使用 Java 语言的自动装箱和自动拆箱特性：

```java
int intValue = 238; Integer boxedValue = intValue;//intValue = boxedValue; 
```

但是，建议您**避免使用自动装箱和自动拆箱**，因为它们可能导致代码可读性问题。装箱和拆箱代码段中的代码比自动装箱的代码更加一目了然，因此可读性更高；我相信投入这些额外工作是值得的。



### 解析和转换装箱的类型

您已经了解了如何获取一个装箱的类型，但如何将一个您怀疑拥有装箱类型的数字String解析到它的正确箱子中？JDK 包装器类还提供了一些方法来实现此目的：

```java
String characterNumeric = "238"; 
Integer convertedValue = Integer.parseInt(characterNumeric);
```

也可以将 JDK 包装类型的内容转换为String ：

```java
Integer boxedValue = Integer.valueOf(238);
String characterNumeric = boxedValue.toString(); 
```

请注意，在String表达式中使用串联运算符时（您应该已在对Logger的调用中看到过），原语类型已自动装箱，而且包装器类型会自动在它们之上调用toString() 。非常方便。



## 列表

List是一种**有序集合**，也称为***序列***。因为List是有序的，所以您能够完全控制项目进入List中的何处。JavaList**集合只能包含对象**（不能包含像int这样的原语类型），而且它为其行为方式定义了严格的契约。

List是一个**接口**，所以不能直接实例化它。这里将使用它的最常用实现**ArrayList** 。可通过两种方式执行声明。第一种方式使用显式语法： 

```java
List<String> listOfStrings = new ArrayList<String>(); 
```

第二种方式使用 “菱形” 运算符（在 JDK 7 中引入）：

```java
 List<String> listOfStrings = new ArrayList<>(); 
```

请注意， ArrayList实例化过程中没有指定对象类型。这是因为表达式右侧的类的类型必须与左侧的类型匹配。在本学习路径的剩余部分中，两种类型都会用到，因为您可能在实际应用中看到这两种用法。

请注意，我将ArrayList对象赋给了一个List类型的变量。在 Java 编程中，可以将某种类型的变量赋给另一种类型，**只要被赋值的变量是赋值变量所实现的超类或接口**。在后面的单元中，将进一步介绍这些变量赋值类型的约束规则。

### 正式类型

前面的代码段中的<Object>被称为*正式类型*。 <Object>告诉编译器，这个List包含一个Object类型的集合，这意味着您可将您喜欢的任何实体放在该List中。

如果您想对能或不能放入List中的实体进行更严格的约束，可通过不同方式定义该正式类型：

```java
 List<Person> listOfPersons = new ArrayList<Person>(); 
```

现在您的List仅能包含Person实例。

### 使用列表

使用List （一般来讲就像使用 Java 集合一样）非常简单。以下是可对List执行的一些操作：

- 将实体放入List中。
- 询问List目前有多大。
- 从List中获取实体。

要将实体放在List中，可调用**add()**方法：

```java
List<Integer> listOfIntegers = new ArrayList<>();

listOfIntegers.add(Integer.valueOf(238)); 
```

add()方法将元素添加到List的**末尾**处。

要询问List有多大，可调用**size()** ：

```java
List<Integer> listOfIntegers = new ArrayList<>();
listOfIntegers.add(Integer.valueOf(238));
Logger l = Logger.getLogger("Test");
l.info("Current List size: " + listOfIntegers.size()); 
```

要从List中检索某一项，可调用get()并向它传递您想要的项的索引：

```java
List<Integer> listOfIntegers = new ArrayList<>();
listOfIntegers.add(Integer.valueOf(238));
Logger l = Logger.getLogger("Test");
l.info("Item at index 0 is: " listOfIntegers.get(0)); 
```

在真实的应用程序中，一个List将包含记录或业务对象，您可能希望在处理过程中查看所有这些对象。您通常如何实现此目的？答案：您希望***迭代*** 该集合，您可以这么做是因为List实现了**java.lang.Iterable**接口。



## 迭代变量 (Iterable)

如果一个集合实现了java.lang.Iterable ，那么该集合被称为***迭代变量集合***。您可从一端开始，逐项地处理集合，直到处理完所有项。

在 [循环](http://www.ibm.com/developerworks/cn/java/j-perry-loops/index.html) 单元中，我简要提及了迭代实现Iterable接口的集合的特殊语法。这里更详细地介绍了该语法：

```java
for (objectType varName : collectionReference) {
	// Start using objectType (via varName) right away...
} 
```

之前的代码比较抽象，这是一个更真实的示例：

```java
 List<Integer> listOfIntegers = obtainSomehow();
 Logger l = Logger.getLogger("Test");
 for (Integer i : listOfIntegers) {
 	l.info("Integer value is : " + i);
 } 
```

这个小代码段所做的事情与下面这个长代码段相同：

```java
 List<Integer> listOfIntegers = obtainSomehow();
 Logger l = Logger.getLogger("Test");
 for (int aa = 0; aa < listOfIntegers.size(); aa++) {
	 Integer I = listOfIntegers.get(aa);
	 l.info("Integer value is : " + i);
 } 
```

第一个代码段使用了简写语法：它没有index变量（在本例中为aa ）要初始化，也没有调用List的get()方法。

因为**List扩展了java.util.Collection** （它实现Iterable ），所以可以使用简写语法来迭代任何List 。



## 集 (Set)

**根据定义， Set是一种包含唯一元素的集合结构 — 即没有重复元素。** List可包含同一个对象数百次，而 Set仅可包含某个特定实例一次。JavaSet集合**仅可包含对象**，而且它为它的行为方式定义了严格的契约。

因为Set是一个接口，所以不能直接实例化它。我最喜欢的实现之一是**HashSet** ，它很容易使用且**类似于List** 。

以下是可对Set执行的一些操作：

- 将实体放入Set中。
- 询问Set目前有多大。
- 从Set中获取实体。

 Set的一个独特特征是，它可保证其元素中的**唯一性**，但不关心元素的顺序。考虑以下代码：

```java
Set<Integer> setOfIntegers = new HashSet<Integer();
setOfIntegers.add(Integer.valueOf(10));
setOfIntegers.add(Integer.valueOf(11));
setOfIntegers.add(Integer.valueOf(10));
for (Integer i : setOfIntegers) {
	l.info("Integer value is: " + i);
} 
```

您可能已料到，该Set中有 3 个元素，但它仅有两个，因为包含值10的Integer对象仅添加了一次。

在迭代Set时请注意此行为，类似于：

```java
Set<Integer> setOfIntegers = new HashSet();
setOfIntegers.add(Integer.valueOf(10));
setOfIntegers.add(Integer.valueOf(20));
setOfIntegers.add(Integer.valueOf(30));
setOfIntegers.add(Integer.valueOf(40));
setOfIntegers.add(Integer.valueOf(50));
Logger l = Logger.getLogger("Test");
for (Integer i : setOfIntegers) {
	l.info("Integer value is : " + i);
} 
```

对象可能按照**不同于添加它们的顺序**打印出来，因为Set可以确保唯一性，但**不保证顺序**。



## 映射 (Map)

 Map是一种方便的集合构造，可以使用它**将一个对象（*键*）与另一个对象（*值*）相关联**。您可能已想象到， Map的键必须是唯一的，而且可在以后用于检索值。JavaMap集合仅可包含对象，而且它为其行为方式定义了严格的契约。

因为Map是一个接口，所以不能直接实例化它。我最喜欢的实现之一是**HashMap** 。

可对Map执行的操作包括：

- 将实体放入Map中。
- 从Map获取实体。
- 向Map提供一个键Set — 用于迭代它。

要将实体放入Map中，需要拥有一个表示它的键的对象和一个表示它的值的对象：

```java
public Map<String, Integer> createMapOfIntegers() {
	Map<String, Integer> mapOfIntegers = new HashMap<>();
	mapOfIntegers.put("1", Integer.valueOf(1));
    mapOfIntegers.put("2", Integer.valueOf(2));
	mapOfIntegers.put("3", Integer.valueOf(3));
	//...mapOfIntegers.put("168", Integer.valueOf(168));
} 
```

在这个示例中， Map包含Integer ，采用一个String作为键，这恰好是它们的String表示。要检索某个特定的Integer值，需要使用它的String表示：

```java
 mapOfIntegers = createMapOfIntegers();
 Integer oneHundred68 = mapOfIntegers.get("168"); 
```



## 结合使用 Set 和 Map

有时，您可能发现您拥有一个Map的引用，而且您希望遍历它的整个内容集合。在这种情况下，您需要向Map提供一个键Set ：

```java
Set<String> keys = mapOfIntegers.keySet();
	Logger l = Logger.getLogger("Test");
	for (String key : keys) {
		Integervalue = mapOfIntegers.get(key);
		l.info("Value keyed by '" + key + "' is '" + value + "'");
	} 
}
```

请注意，在用于Logger调用时，会自动调用从Map检索的Integer的toString()方法。 Map返回它的键的List ，因为Map具有键，而且每个键是唯一的。**唯一性**（而不是顺序）是Set的独特特征（这可以解释为什么**没有keyList()方法**）。