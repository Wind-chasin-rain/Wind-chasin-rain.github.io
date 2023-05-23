---
title: Java 8 新特性
date:  2023-05-20 14:55:08
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202305201458168.jpg
tags: Java
updated:
categories: 学习笔记
description: Java 8 以来的新特性
mathjax:
toc: true
toc_number: true
---

Java 8 (又称为 jdk 1.8) 是 Java 语言开发的一个主要版本。 Oracle 公司于 2014 年 3 月 18 日发布 Java 8 ，它支持函数式编程，新的 JavaScript 引擎，新的日期 API，新的Stream API 等。

------

# Lambda 表达式

Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。

Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。

使用 Lambda 表达式可以使代码变的更加简洁紧凑。

## 语法

```
(parameters) -> expression
或
(parameters) ->{ statements; }
```

以下是lambda表达式的重要特征:

- **可选类型声明：**不需要声明参数类型，编译器可以统一识别参数值。
- **可选的参数圆括号：**一个参数无需定义圆括号，但多个参数需要定义圆括号。
- **可选的大括号：**如果主体包含了一个语句，就不需要使用大括号。
- **可选的返回关键字：**如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定表达式返回了一个数值。

```java
public class Java8Tester {
   public static void main(String args[]){
      Java8Tester tester = new Java8Tester();
        
      // 类型声明
      MathOperation addition = (int a, int b) -> a + b;
        
      // 不用类型声明
      MathOperation subtraction = (a, b) -> a - b;
        
      // 大括号中的返回语句
      MathOperation multiplication = (int a, int b) -> { return a * b; };
        
      // 没有大括号及返回语句
      MathOperation division = (int a, int b) -> a / b;
        
      System.out.println("10 + 5 = " + tester.operate(10, 5, addition));
      System.out.println("10 - 5 = " + tester.operate(10, 5, subtraction));
      System.out.println("10 x 5 = " + tester.operate(10, 5, multiplication));
      System.out.println("10 / 5 = " + tester.operate(10, 5, division));
        
      // 不用括号
      GreetingService greetService1 = message ->
      System.out.println("Hello " + message);
        
      // 用括号
      GreetingService greetService2 = (message) ->
      System.out.println("Hello " + message);
        
      greetService1.sayMessage("Runoob");
      greetService2.sayMessage("Google");
   }
    
   interface MathOperation {
      int operation(int a, int b);
   }
    
   interface GreetingService {
      void sayMessage(String message);
   }
    
   private int operate(int a, int b, MathOperation mathOperation){
      return mathOperation.operation(a, b);
   }
}
```

使用 Lambda 表达式需要注意以下两点：

- Lambda 表达式主要用来定义行内执行的方法类型接口（例如，一个简单方法接口）。在上面例子中，我们使用各种类型的 Lambda 表达式来定义 MathOperation 接口的方法，然后我们定义了 operation 的执行。
- Lambda 表达式免去了使用匿名方法的麻烦，并且给予 Java 简单但是强大的函数化的编程能力。

</br>

## 函数式接口

- 只包含`一个抽象方法`（Single Abstract Method，简称SAM）的接口，称为函数式接口。当然该接口可以包含其他非抽象方法。
- 你可以通过 Lambda 表达式来创建该接口的对象。（若 Lambda 表达式抛出一个受检异常(即：非运行时异常)，那么该异常需要在目标接口的抽象方法上进行声明）。
- 我们可以在一个接口上使用 `@FunctionalInterface` 注解，这样做可以检查它是否是一个函数式接口。同时 javadoc 也会包含一条声明，说明这个接口是一个函数式接口。
- 在`java.util.function`包下定义了Java 8 的丰富的函数式接口

</br>

## 常用接口

| 函数式接口         | 称谓       | 参数类型 | 用途                                                         |
| ------------------ | ---------- | -------- | ------------------------------------------------------------ |
| `Consumer<T>  `    | 消费型接口 | T        | 对类型为T的对象应用操作，包含方法：  `void accept(T t)  `    |
| `Supplier<T>  `    | 供给型接口 | 无       | 返回类型为T的对象，包含方法：`T get()  `                     |
| `Function<T, R>  ` | 函数型接口 | T        | 对类型为T的对象应用操作，并返回结果。结果是R类型的对象。包含方法：`R apply(T t)  ` |
| `Predicate<T>  `   | 判断型接口 | T        | 确定类型为T的对象是否满足某约束，并返回 boolean 值。包含方法：`boolean test(T t)  ` |

**类型1：消费型接口**

消费型接口的抽象方法特点：有形参，但是返回值类型是void

| 接口名               | 抽象方法                       | 描述                       |
| -------------------- | ------------------------------ | -------------------------- |
| BiConsumer<T,U>      | void accept(T t, U u)          | 接收两个对象用于完成功能   |
| DoubleConsumer       | void accept(double value)      | 接收一个double值           |
| IntConsumer          | void accept(int value)         | 接收一个int值              |
| LongConsumer         | void accept(long value)        | 接收一个long值             |
| ObjDoubleConsumer<T> | void accept(T t, double value) | 接收一个对象和一个double值 |
| ObjIntConsumer<T>    | void accept(T t, int value)    | 接收一个对象和一个int值    |
| ObjLongConsumer<T>   | void accept(T t, long value)   | 接收一个对象和一个long值   |

**类型2：供给型接口**

这类接口的抽象方法特点：无参，但是有返回值

| 接口名          | 抽象方法               | 描述              |
| --------------- | ---------------------- | ----------------- |
| BooleanSupplier | boolean getAsBoolean() | 返回一个boolean值 |
| DoubleSupplier  | double getAsDouble()   | 返回一个double值  |
| IntSupplier     | int getAsInt()         | 返回一个int值     |
| LongSupplier    | long getAsLong()       | 返回一个long值    |

**类型3：函数型接口**

这类接口的抽象方法特点：既有参数又有返回值

| 接口名                  | 抽象方法                                        | 描述                                                |
| ----------------------- | ----------------------------------------------- | --------------------------------------------------- |
| UnaryOperator<T>        | T apply(T t)                                    | 接收一个T类型对象，返回一个T类型对象结果            |
| DoubleFunction<R>       | R apply(double value)                           | 接收一个double值，返回一个R类型对象                 |
| IntFunction<R>          | R apply(int value)                              | 接收一个int值，返回一个R类型对象                    |
| LongFunction<R>         | R apply(long value)                             | 接收一个long值，返回一个R类型对象                   |
| ToDoubleFunction<T>     | double applyAsDouble(T value)                   | 接收一个T类型对象，返回一个double                   |
| ToIntFunction<T>        | int applyAsInt(T value)                         | 接收一个T类型对象，返回一个int                      |
| ToLongFunction<T>       | long applyAsLong(T value)                       | 接收一个T类型对象，返回一个long                     |
| DoubleToIntFunction     | int applyAsInt(double value)                    | 接收一个double值，返回一个int结果                   |
| DoubleToLongFunction    | long applyAsLong(double value)                  | 接收一个double值，返回一个long结果                  |
| IntToDoubleFunction     | double applyAsDouble(int value)                 | 接收一个int值，返回一个double结果                   |
| IntToLongFunction       | long applyAsLong(int value)                     | 接收一个int值，返回一个long结果                     |
| LongToDoubleFunction    | double applyAsDouble(long value)                | 接收一个long值，返回一个double结果                  |
| LongToIntFunction       | int applyAsInt(long value)                      | 接收一个long值，返回一个int结果                     |
| DoubleUnaryOperator     | double applyAsDouble(double operand)            | 接收一个double值，返回一个double                    |
| IntUnaryOperator        | int applyAsInt(int operand)                     | 接收一个int值，返回一个int结果                      |
| LongUnaryOperator       | long applyAsLong(long operand)                  | 接收一个long值，返回一个long结果                    |
| BiFunction<T,U,R>       | R apply(T t, U u)                               | 接收一个T类型和一个U类型对象，返回一个R类型对象结果 |
| BinaryOperator<T>       | T apply(T t, T u)                               | 接收两个T类型对象，返回一个T类型对象结果            |
| ToDoubleBiFunction<T,U> | double applyAsDouble(T t, U u)                  | 接收一个T类型和一个U类型对象，返回一个double        |
| ToIntBiFunction<T,U>    | int applyAsInt(T t, U u)                        | 接收一个T类型和一个U类型对象，返回一个int           |
| ToLongBiFunction<T,U>   | long applyAsLong(T t, U u)                      | 接收一个T类型和一个U类型对象，返回一个long          |
| DoubleBinaryOperator    | double applyAsDouble(double left, double right) | 接收两个double值，返回一个double结果                |
| IntBinaryOperator       | int applyAsInt(int left, int right)             | 接收两个int值，返回一个int结果                      |
| LongBinaryOperator      | long applyAsLong(long left, long right)         | 接收两个long值，返回一个long结果                    |

**类型4：判断型接口**

这类接口的抽象方法特点：有参，但是返回值类型是boolean结果。

| 接口名           | 抽象方法                   | 描述             |
| ---------------- | -------------------------- | ---------------- |
| BiPredicate<T,U> | boolean test(T t, U u)     | 接收两个对象     |
| DoublePredicate  | boolean test(double value) | 接收一个double值 |
| IntPredicate     | boolean test(int value)    | 接收一个int值    |
| LongPredicate    | boolean test(long  value)  | 接收一个long值   |

</br>

# 引用

Lambda表达式是可以简化函数式接口的变量或形参赋值的语法。而方法引用和构造器引用是为了简化Lambda表达式的。

## 方法引用

- 格式：使用方法引用操作符 “`::`” 将类(或对象) 与 方法名分隔开来。

  - 两个:中间不能有空格，而且必须英文状态下半角输入

  如下三种主要使用情况：

  - 情况1：`对象 :: 实例方法名`
  - 情况2：`类 :: 静态方法名`
  - 情况3：`类 :: 实例方法名`

- 方法引用使用前提

  **要求1：**Lambda体只有一句语句，并且是通过调用一个对象的/类现有的方法来完成的

  例如：System.out对象，调用println()方法来完成Lambda体

  ​           Math类，调用random()静态方法来完成Lambda体

  **要求2：**

  针对情况1：函数式接口中的抽象方法a在被重写时使用了某一个对象的方法b。如果方法a的形参列表、返回值类型与方法b的形参列表、返回值类型都相同，则我们可以使用方法b实现对方法a的重写、替换。


  针对情况2：函数式接口中的抽象方法a在被重写时使用了某一个类的静态方法b。如果方法a的形参列表、返回值类型与方法b的形参列表、返回值类型都相同，则我们可以使用方法b实现对方法a的重写、替换。

  针对情况3：函数式接口中的抽象方法a在被重写时使用了某一个对象的方法b。如果方法a的返回值类型与方法b的返回值类型相同，同时方法a的形参列表中有n个参数，方法b的形参列表有n-1个参数，且方法a的第1个参数作为方法b的调用者，且方法a的后n-1参数与方法b的n-1参数匹配（类型相同或满足多态场景也可以）

  例如：t->System.out.println(t)

  ​        () -> Math.random() 都是无参

  ```java
  public class MethodRefTest {
  
  	// 情况一：对象 :: 实例方法
  	//Consumer中的void accept(T t)
  	//PrintStream中的void println(T t)
  	@Test
  	public void test1() {
  		Consumer<String> con1 = str -> System.out.println(str);
  		con1.accept("北京");
  
  		System.out.println("*******************");
  		PrintStream ps = System.out;
  		Consumer<String> con2 = ps::println;
  		con2.accept("beijing");
  	}
  	
  	//Supplier中的T get()
  	//Employee中的String getName()
  	@Test
  	public void test2() {
  		Employee emp = new Employee(1001,"Tom",23,5600);
  
  		Supplier<String> sup1 = () -> emp.getName();
  		System.out.println(sup1.get());
  
  		System.out.println("*******************");
  		Supplier<String> sup2 = emp::getName;
  		System.out.println(sup2.get());
  
  	}
  
  	// 情况二：类 :: 静态方法
  	//Comparator中的int compare(T t1,T t2)
  	//Integer中的int compare(T t1,T t2)
  	@Test
  	public void test3() {
  		Comparator<Integer> com1 = (t1,t2) -> Integer.compare(t1,t2);
  		System.out.println(com1.compare(12,21));
  
  		System.out.println("*******************");
  
  		Comparator<Integer> com2 = Integer::compare;
  		System.out.println(com2.compare(12,3));
  
  	}
  	
  	//Function中的R apply(T t)
  	//Math中的Long round(Double d)
  	@Test
  	public void test4() {
  		Function<Double,Long> func = new Function<Double, Long>() {
  			@Override
  			public Long apply(Double d) {
  				return Math.round(d);
  			}
  		};
  
  		System.out.println("*******************");
  
  		Function<Double,Long> func1 = d -> Math.round(d);
  		System.out.println(func1.apply(12.3));
  
  		System.out.println("*******************");
  
  		Function<Double,Long> func2 = Math::round;
  		System.out.println(func2.apply(12.6));
  	}
  
  	// 情况三：类 :: 实例方法  (有难度)
  	// Comparator中的int comapre(T t1,T t2)
  	// String中的int t1.compareTo(t2)
  	@Test
  	public void test5() {
  		Comparator<String> com1 = (s1,s2) -> s1.compareTo(s2);
  		System.out.println(com1.compare("abc","abd"));
  
  		System.out.println("*******************");
  
  		Comparator<String> com2 = String :: compareTo;
  		System.out.println(com2.compare("abd","abm"));
  	}
  
  	//BiPredicate中的boolean test(T t1, T t2);
  	//String中的boolean t1.equals(t2)
  	@Test
  	public void test6() {
  		BiPredicate<String,String> pre1 = (s1,s2) -> s1.equals(s2);
  		System.out.println(pre1.test("abc","abc"));
  
  		System.out.println("*******************");
  		BiPredicate<String,String> pre2 = String :: equals;
  		System.out.println(pre2.test("abc","abd"));
  	}
  	
  	// Function中的R apply(T t)
  	// Employee中的String getName();
  	@Test
  	public void test7() {
  		Employee employee = new Employee(1001, "Jerry", 23, 6000);
  
  
  		Function<Employee,String> func1 = e -> e.getName();
  		System.out.println(func1.apply(employee));
  
  		System.out.println("*******************");
  		Function<Employee,String> func2 = Employee::getName;
  		System.out.println(func2.apply(employee));
  	}
  
  }
  ```

  </br>

## 构造器引用

当Lambda表达式是创建一个对象，并且满足Lambda表达式形参，正好是给创建这个对象的构造器的实参列表，就可以使用构造器引用。

格式：`类名::new`

```java
import java.util.function.BiFunction;
import java.util.function.Function;

public class Test07 {
    public static void main(String[] args) {
        Function<Integer, Dog> func1 = Dog::new;
        System.out.println(func1.apply(11));
        BiFunction<Integer, String, Dog> func2 = Dog::new;
        System.out.println(func2.apply(11, "Tom"));
    }
}

class Dog {
    private int id;
    private String name;

    public Dog() {

    }

    public Dog(int id) {
        this.id = id;
    }

    public Dog(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return "dog{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

</br>

## 数组构造引用

当Lambda表达式是创建一个数组对象，并且满足Lambda表达式形参，正好是给创建这个数组对象的长度，就可以数组构造引用。

格式：`数组类型名::new`

```Java
public void test4(){
    Function<Integer,String[]> func1 = length -> new String[length];
    String[] arr1 = func1.apply(5);
    System.out.println(Arrays.toString(arr1));

    System.out.println("*******************");

    Function<Integer,String[]> func2 = String[] :: new;
    String[] arr2 = func2.apply(10);
    System.out.println(Arrays.toString(arr2));
}
```

</br>

# Stream API

## 什么是Stream

Stream 是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。

Stream 和 Collection 集合的区别：**Collection 是一种静态的内存数据结构，讲的是数据，而 Stream 是有关计算的，讲的是计算。**前者是主要面向内存，存储在内存中，后者主要是面向 CPU，通过 CPU 实现计算。

注意：

①Stream 自己不会存储元素。

②Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。

③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。即一旦执行终止操作，就执行中间操作链，并产生结果。

④ Stream一旦执行了终止操作，就不能再调用其它中间操作或终止操作了。

</br>

## Stream的操作三个步骤

**1- 创建 Stream**
一个数据源（如：集合、数组），获取一个流

**2- 中间操作**
每次处理都会返回一个持有结果的新Stream，即中间操作的方法返回值仍然是Stream类型的对象。因此中间操作可以是个`操作链`，可对数据源的数据进行n次处理，但是在终结操作前，并不会真正执行。

**3- 终止操作(终端操作)**
终止操作的方法返回值类型就不再是Stream了，因此一旦执行终止操作，就结束整个Stream操作了。一旦执行终止操作，就执行中间操作链，最终产生结果并结束Stream。

### 创建Stream实例

**方式一：通过集合**

Java8 中的 Collection 接口被扩展，提供了两个获取流的方法：

- default Stream<E> stream() : 返回一个顺序流

- default Stream<E> parallelStream() : 返回一个并行流

```java
@Test
public void test01(){
    List<Integer> list = Arrays.asList(1,2,3,4,5);

    //JDK1.8中，Collection系列集合增加了方法
    Stream<Integer> stream = list.stream();
}
```



**方式二：通过数组**

Java8 中的 Arrays 的静态方法 stream() 可以获取数组流：

- static <T> Stream<T> stream(T[] array): 返回一个流
- public static IntStream stream(int[] array)
- public static LongStream stream(long[] array)
- public static DoubleStream stream(double[] array)

```java
@Test
public void test02(){
    String[] arr = {"hello","world"};
    Stream<String> stream = Arrays.stream(arr); 
}

@Test
public void test03(){
    int[] arr = {1,2,3,4,5};
    IntStream stream = Arrays.stream(arr);
}
```



**方式三：通过Stream的of()**

可以调用Stream类静态方法 of(), 通过显示值创建一个流。它可以接收任意数量的参数。

- public static<T> Stream<T> of(T... values) : 返回一个流

```java
@Test
public void test04(){
    Stream<Integer> stream = Stream.of(1,2,3,4,5);
    stream.forEach(System.out::println);
}
```



**方式四：创建无限流(了解)**

可以使用静态方法 Stream.iterate() 和 Stream.generate(), 创建无限流。

- 迭代
  public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f) 

- 生成
  public static<T> Stream<T> generate(Supplier<T> s) 

```java
// 方式四：创建无限流
@Test
public void test05() {
	// 迭代
	// public static<T> Stream<T> iterate(final T seed, final
	// UnaryOperator<T> f)
	Stream<Integer> stream = Stream.iterate(0, x -> x + 2);
	stream.limit(10).forEach(System.out::println);

	// 生成
	// public static<T> Stream<T> generate(Supplier<T> s)
	Stream<Double> stream1 = Stream.generate(Math::random);
	stream1.limit(10).forEach(System.out::println);
}

```

### 一系列中间操作

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”。

1-筛选与切片

| **方   法**             | **描   述**                                                  |
| ----------------------- | ------------------------------------------------------------ |
| **filter(Predicatep)**  | 接收  Lambda ， 从流中排除某些元素                           |
| **distinct()**          | 筛选，通过流所生成元素的  hashCode() 和 equals() 去除重复元素 |
| **limit(long maxSize)** | 截断流，使其元素不超过给定数量                               |
| **skip(long n)**        | 跳过元素，返回一个扔掉了前  n 个元素的流。<br>若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补 |

2-映射

| **方法**                            | **描述**                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| **map(Function f)**                 | 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。 |
| **mapToDouble(ToDoubleFunction f)** | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 DoubleStream。 |
| **mapToInt(ToIntFunction  f)**      | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的  IntStream。 |
| **mapToLong(ToLongFunction  f)**    | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的  LongStream。 |
| **flatMap(Function  f)**            | 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流 |

3-排序

| **方法**                       | **描述**                           |
| ------------------------------ | ---------------------------------- |
| **sorted()**                   | 产生一个新流，其中按自然顺序排序   |
| **sorted(Comparator** **com)** | 产生一个新流，其中按比较器顺序排序 |

代码举例：

```java
package com.atguigu.stream;

import org.junit.Test;

import java.util.Arrays;
import java.util.stream.Stream;

public class StreamMiddleOperate {
	@Test
    public void test01(){
        //1、创建Stream
        Stream<Integer> stream = Stream.of(1,2,3,4,5,6);

        //2、加工处理
        //过滤：filter(Predicate p)
        //把里面的偶数拿出来
        /*
         * filter(Predicate p)
         * Predicate是函数式接口，抽象方法：boolean test(T t)
         */
        stream = stream.filter(t -> t%2==0);

        //3、终结操作：例如：遍历
        stream.forEach(System.out::println);
    }
    @Test
    public void test02(){
        Stream.of(1,2,3,4,5,6)
                .filter(t -> t%2==0)
                .forEach(System.out::println);
    }
    @Test
    public void test03(){
        Stream.of(1,2,3,4,5,6,2,2,3,3,4,4,5)
                .distinct()
                .forEach(System.out::println);
    }
    @Test
    public void test04(){
        Stream.of(1,2,3,4,5,6,2,2,3,3,4,4,5)
                .limit(3)
                .forEach(System.out::println);
    }
    @Test
    public void test05(){
        Stream.of(1,2,2,3,3,4,4,5,2,3,4,5,6,7)
                .distinct()  //(1,2,3,4,5,6,7)
                .filter(t -> t%2!=0) //(1,3,5,7)
                .limit(3)
                .forEach(System.out::println);
    }
    @Test
    public void test06(){
        Stream.of(1,2,3,4,5,6,2,2,3,3,4,4,5)
                .skip(5)
                .forEach(System.out::println);
    }
    @Test
    public void test07(){
        Stream.of(1,2,3,4,5,6,2,2,3,3,4,4,5)
                .skip(5)
                .distinct()
                .filter(t -> t%3==0)
                .forEach(System.out::println);
    }
    @Test
    public void test08(){
        long count = Stream.of(1,2,3,4,5,6,2,2,3,3,4,4,5)
                .distinct()
                .peek(System.out::println)  //Consumer接口的抽象方法  void accept(T t)
                .count();
        System.out.println("count="+count);
    }
    @Test
    public void test09(){
        //希望能够找出前三个最大值，前三名最大的，不重复
        Stream.of(11,2,39,4,54,6,2,22,3,3,4,54,54)
                .distinct()
                .sorted((t1,t2) -> -Integer.compare(t1, t2))//Comparator接口  int compare(T t1, T t2)
                .limit(3)
                .forEach(System.out::println);
    }
    @Test
    public void test10(){
        Stream.of(1,2,3,4,5)
                .map(t -> t+=1)//Function<T,R>接口抽象方法 R apply(T t)
                .forEach(System.out::println);
    }
    @Test
    public void test11(){
        String[] arr = {"hello","world","java"};

        Arrays.stream(arr)
                .map(t->t.toUpperCase())
                .forEach(System.out::println);
    }
    @Test
    public void test12(){
        String[] arr = {"hello","world","java"};
        Arrays.stream(arr)
                .flatMap(t -> Stream.of(t.split("|")))//Function<T,R>接口抽象方法 R apply(T t)  现在的R是一个Stream
                .forEach(System.out::println);
    } 
}

```

###  终止操作

- 终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是 void 。

- 流进行了终止操作后，不能再次使用。

1-匹配与查找

| **方法**                        | **描述**                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| **allMatch(Predicate  p)**      | 检查是否匹配所有元素                                         |
| **anyMatch(Predicate  p)  **    | 检查是否至少匹配一个元素                                     |
| **noneMatch(Predicate**  **p)** | 检查是否没有匹配所有元素                                     |
| **findFirst()**                 | 返回第一个元素                                               |
| **findAny()**                   | 返回当前流中的任意元素                                       |
| **count()**                     | 返回流中元素总数                                             |
| **max(Comparator c)**           | 返回流中最大值                                               |
| **min(Comparator c)**           | 返回流中最小值                                               |
| **forEach(Consumer c)**         | 内部迭代(使用  Collection  接口需要用户去做迭代，称为外部迭代。<br>相反，Stream  API 使用内部迭代——它帮你把迭代做了) |

2-归约

| **方法**                                  | **描述**                                                 |
| ----------------------------------------- | -------------------------------------------------------- |
| **reduce(T  identity, BinaryOperator b)** | 可以将流中元素反复结合起来，得到一个值。返回  T          |
| **reduce(BinaryOperator  b)**             | 可以将流中元素反复结合起来，得到一个值。返回 Optional<T> |

备注：map 和 reduce 的连接通常称为 map-reduce 模式，因 Google 用它来进行网络搜索而出名。

3-收集

| **方   法**               | **描   述**                                                  |
| ------------------------- | ------------------------------------------------------------ |
| **collect(Collector  c)** | 将流转换为其他形式。接收一个  Collector接口的实现，<br>用于给Stream中元素做汇总的方法 |

Collector 接口中方法的实现决定了如何对流执行收集的操作(如收集到 List、Set、Map)。

另外， Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例，具体方法与实例如下表：

| **方法**   | **返回类型**             | **作用**             |
| ---------- | ------------------------ | -------------------- |
| **toList** | Collector<T, ?, List<T>> | 把流中元素收集到List |

```java
List<Employee> emps= list.stream().collect(Collectors.toList());
```

| **方法**  | **返回类型**            | **作用**            |
| --------- | ----------------------- | ------------------- |
| **toSet** | Collector<T, ?, Set<T>> | 把流中元素收集到Set |

```java
Set<Employee> emps= list.stream().collect(Collectors.toSet());
```

| **方法**         | **返回类型**       | **作用**                   |
| ---------------- | ------------------ | -------------------------- |
| **toCollection** | Collector<T, ?, C> | 把流中元素收集到创建的集合 |

```java
Collection<Employee> emps =list.stream().collect(Collectors.toCollection(ArrayList::new));
```

| **方法**     | **返回类型**          | **作用**           |
| ------------ | --------------------- | ------------------ |
| **counting** | Collector<T, ?, Long> | 计算流中元素的个数 |

```java
long count = list.stream().collect(Collectors.counting());
```

| **方法**       | **返回类型**             | **作用**                 |
| -------------- | ------------------------ | ------------------------ |
| **summingInt** | Collector<T, ?, Integer> | 对流中元素的整数属性求和 |

```java
int total=list.stream().collect(Collectors.summingInt(Employee::getSalary));
```

| **方法**         | **返回类型**            | **作用**                        |
| ---------------- | ----------------------- | ------------------------------- |
| **averagingInt** | Collector<T, ?, Double> | 计算流中元素Integer属性的平均值 |

```java
double avg = list.stream().collect(Collectors.averagingInt(Employee::getSalary));
```

| **方法**           | **返回类型**                          | **作用**                                |
| ------------------ | ------------------------------------- | --------------------------------------- |
| **summarizingInt** | Collector<T, ?, IntSummaryStatistics> | 收集流中Integer属性的统计值。如：平均值 |

```java
int SummaryStatisticsiss= list.stream().collect(Collectors.summarizingInt(Employee::getSalary));
```

| **方法**    | **返回类型**                       | **作用**           |
| ----------- | ---------------------------------- | ------------------ |
| **joining** | Collector<CharSequence, ?, String> | 连接流中每个字符串 |

```java
String str= list.stream().map(Employee::getName).collect(Collectors.joining());
```

| **方法**  | **返回类型**                 | **作用**             |
| --------- | ---------------------------- | -------------------- |
| **maxBy** | Collector<T, ?, Optional<T>> | 根据比较器选择最大值 |

```java
Optional<Emp>max= list.stream().collect(Collectors.maxBy(comparingInt(Employee::getSalary)));
```

| **方法**  | **返回类型**                 | **作用**             |
| --------- | ---------------------------- | -------------------- |
| **minBy** | Collector<T, ?, Optional<T>> | 根据比较器选择最小值 |

```java
Optional<Emp> min = list.stream().collect(Collectors.minBy(comparingInt(Employee::getSalary)));
```

| **方法**     | **返回类型**                 | **作用**                                                     |
| ------------ | ---------------------------- | ------------------------------------------------------------ |
| **reducing** | Collector<T, ?, Optional<T>> | 从一个作为累加器的初始值开始，利用BinaryOperator与流中元素逐个结合，从而归约成单个值 |

```java
int total=list.stream().collect(Collectors.reducing(0, Employee::getSalar, Integer::sum));
```

| **方法**              | **返回类型**      | **作用**                           |
| --------------------- | ----------------- | ---------------------------------- |
| **collectingAndThen** | Collector<T,A,RR> | 包裹另一个收集器，对其结果转换函数 |

```java
int how= list.stream().collect(Collectors.collectingAndThen(Collectors.toList(), List::size));
```

| **方法**       | **返回类型**                     | **作用**                               |
| -------------- | -------------------------------- | -------------------------------------- |
| **groupingBy** | Collector<T, ?, Map<K, List<T>>> | 根据某属性值对流分组，属性为K，结果为V |

```java
Map<Emp.Status, List<Emp>> map= list.stream().collect(Collectors.groupingBy(Employee::getStatus));
```

| **方法**           | **返回类型**                           | **作用**                |
| ------------------ | -------------------------------------- | ----------------------- |
| **partitioningBy** | Collector<T, ?, Map<Boolean, List<T>>> | 根据true或false进行分区 |

```java
Map<Boolean,List<Emp>> vd = list.stream().collect(Collectors.partitioningBy(Employee::getManage));
```

举例：

```java
package com.atguigu.stream;

import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;
import java.util.stream.Stream;

import org.junit.Test;

public class StreamEndding {
    @Test
    public void test01(){
        Stream.of(1,2,3,4,5)
                .forEach(System.out::println);
    }
    @Test
    public void test02(){
        long count = Stream.of(1,2,3,4,5)
                .count();
        System.out.println("count = " + count);
    }
    @Test
    public void test03(){
        boolean result = Stream.of(1,3,5,7,9)
                .allMatch(t -> t%2!=0);
        System.out.println(result);
    }
	@Test
    public void test04(){
        boolean result = Stream.of(1,3,5,7,9)
                .anyMatch(t -> t%2==0);
        System.out.println(result);
    }
	@Test
    public void test05(){
        Optional<Integer> opt = Stream.of(1,3,5,7,9).findFirst();
        System.out.println(opt);
    }
	@Test
    public void test06(){
        Optional<Integer> opt = Stream.of(1,2,3,4,5,7,9)
                .filter(t -> t%3==0)
                .findFirst();
        System.out.println(opt);
    }
	@Test
    public void test07(){
        Optional<Integer> opt = Stream.of(1,2,4,5,7,8)
                .filter(t -> t%3==0)
                .findFirst();
        System.out.println(opt);
    }
    @Test
    public void test08(){
        Optional<Integer> max = Stream.of(1,2,4,5,7,8)
                .max((t1,t2) -> Integer.compare(t1, t2));
        System.out.println(max);
    }
    @Test
    public void test09(){
        Integer reduce = Stream.of(1,2,4,5,7,8)
                .reduce(0, (t1,t2) -> t1+t2);//BinaryOperator接口   T apply(T t1, T t2)
        System.out.println(reduce);
    }
    @Test
    public void test10(){
        Optional<Integer> max = Stream.of(1,2,4,5,7,8)
                .reduce((t1,t2) -> t1>t2?t1:t2);//BinaryOperator接口   T apply(T t1, T t2)
        System.out.println(max);
    }
    @Test
    public void test11(){
        List<Integer> list = Stream.of(1,2,4,5,7,8)
                .filter(t -> t%2==0)
                .collect(Collectors.toList());

        System.out.println(list);
    }   
}
```

</br>

# 新语法结构

##  Java的REPL工具： jShell命令

**JDK9的新特性**

Java 终于拥有了像Python 和 Scala 之类语言的REPL工具（交互式编程环境，read - evaluate - print - loop）：`jShell`。以交互式的方式对语句和表达式进行求值。`即写即得`、`快速运行`。

利用jShell在没有创建类的情况下，在命令行里直接声明变量，计算表达式，执行语句。无需跟人解释”public static void main(String[] args)”这句"废话"。

## 异常处理之try-catch资源关闭

**JDK7的新特性**

在try的后面可以增加一个()，在括号中可以声明流对象并初始化。try中的代码执行完毕，会自动把流对象释放，就不用写finally了。

格式：

```java
try(资源对象的声明和初始化){
    业务逻辑代码,可能会产生异常
}catch(异常类型1 e){
    处理异常代码
}catch(异常类型2 e){
    处理异常代码
}
```

说明：

1、在try()中声明的资源，无论是否发生异常，无论是否处理异常，都会自动关闭资源对象，不用手动关闭了。

2、这些资源实现类必须实现AutoCloseable或Closeable接口，实现其中的close()方法。Closeable是AutoCloseable的子接口。Java7几乎把所有的“资源类”（包括文件IO的各种类、JDBC编程的Connection、Statement等接口…）都进行了改写，改写后资源类都实现了AutoCloseable或Closeable接口，并实现了close()方法。

3、写到try()中的资源类的变量默认是final声明的，不能修改。

**JDK9的新特性**

try的前面可以定义流对象，try后面的()中可以直接引用流对象的名称。在try代码执行完毕后，流对象也可以释放掉，也不用写finally了。

格式：

```java
A a = new A();
B b = new B();
try(a;b){
    可能产生的异常代码
}catch(异常类名 变量名){
    异常处理的逻辑
}
```

举例：

```java
@Test
public void test04() {
    InputStreamReader reader = new InputStreamReader(System.in);
    OutputStreamWriter writer = new OutputStreamWriter(System.out);
    try (reader; writer) {
        //reader是final的，不可再被赋值
        //   reader = null;

    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

</br>

## 局部变量类型推断

**JDK 10的新特性**

局部变量的显示类型声明，常常被认为是不必须的，给一个好听的名字反而可以很清楚的表达出下面应该怎样继续。本新特性允许开发人员省略通常不必要的局部变量类型声明，以增强Java语言的体验性、可读性。

- 使用举例


```java
//1.局部变量的实例化
var list = new ArrayList<String>();

var set = new LinkedHashSet<Integer>();

//2.增强for循环中的索引
for (var v : list) {
    System.out.println(v);
}

//3.传统for循环中
for (var i = 0; i < 100; i++) {
    System.out.println(i);
}

//4. 返回值类型含复杂泛型结构
var iterator = set.iterator();
//Iterator<Map.Entry<Integer, Student>> iterator = set.iterator();

```

- 不适用场景
  - 声明一个成员变量
  - 声明一个数组变量，并为数组静态初始化（省略new的情况下）
  - 方法的返回值类型
  - 方法的参数类型
  - 没有初始化的方法内的局部变量声明
  - 作为catch块中异常类型
  - Lambda表达式中函数式接口的类型
  - 方法引用中函数式接口的类型

</br>

## instanceof的模式匹配

**JDK14中预览特性：**

instanceof 模式匹配通过提供更为简便的语法，来提高生产力。有了该功能，可以减少Java程序中显式强制转换的数量，实现更精确、简洁的类型安全的代码。

Java 14之前旧写法：

```java
if(obj instanceof String){
    String str = (String)obj; //需要强转
    .. str.contains(..)..
}else{
    ...
}
```

Java 14新特性写法：

```java
if(obj instanceof String str){
    .. str.contains(..)..
}else{
    ...
}
```

举例：

```java
/**
 * instanceof的模式匹配（预览）
 *
 * @author shkstart
 * @create 上午 11:32
 */
public class Feature01 {
    @Test
    public void test1(){

        Object obj = new String("hello,Java14");
        obj = null;//在使用null 匹配instanceof 时，返回都是false.
        if(obj instanceof String){
            String str = (String) obj;
            System.out.println(str.contains("Java"));
        }else{
            System.out.println("非String类型");
        }

        //举例1：
        if(obj instanceof String str){ //新特性：省去了强制类型转换的过程
            System.out.println(str.contains("Java"));
        }else{
            System.out.println("非String类型");
        }
    }
}

// 举例2
class InstanceOf{

    String str = "abc";

    public void test(Object obj){

        if(obj instanceof String str){//此时的str的作用域仅限于if结构内。
            System.out.println(str.toUpperCase());
        }else{
            System.out.println(str.toLowerCase());
        }

    }

}

//举例3：
class Monitor{
    private String model;
    private double price;

//    public boolean equals(Object o){
//        if(o instanceof Monitor other){
//            if(model.equals(other.model) && price == other.price){
//                return true;
//            }
//        }
//        return false;
//    }


    public boolean equals(Object o){
        return o instanceof Monitor other && model.equals(other.model) && price == other.price;
    }

}
```

</br>

## switch表达式

传统switch声明语句的弊端：

- 匹配是自上而下的，如果忘记写break，后面的case语句不论匹配与否都会执行； --->case穿透
- 所有的case语句共用一个块范围，在不同的case语句定义的变量名不能重复；
- 不能在一个case里写多个执行结果一致的条件；
- 整个switch不能作为表达式返回值；

**JDK12中预览特性：**

- Java 12将会对switch声明语句进行扩展，使用`case L ->`来替代以前的`break;`，省去了 break 语句，避免了因少写 break 而出错。

- 同时将多个 case 合并到一行，显得简洁、清晰，也更加优雅的表达逻辑分支。

- 为了保持兼容性，case 条件语句中依然可以使用字符` :` ，但是同一个 switch 结构里不能混用` ->` 和` :` ，否则编译错误。

```java
public class SwitchTest2 {
    public static void main(String[] args) {
        Fruit fruit = Fruit.GRAPE;
        int numberOfLetters = switch(fruit){
            case PEAR -> 4;
            case APPLE,MANGO,GRAPE -> 5;
            case ORANGE,PAPAYA -> 6;
            default -> throw new IllegalStateException("No Such Fruit:" + fruit);
        };
        System.out.println(numberOfLetters);
    }
}
```

**JDK13中二次预览特性：**

JDK13中引入了yield语句，用于返回值。这意味着，switch表达式(返回值)应该使用yield，switch语句(不返回值)应该使用break。

yield和return的区别在于：return会直接跳出当前循环或者方法，而yield只会跳出当前switch块。

在JDK13中：

```java
@Test
public void testSwitch2(){
    String x = "3";
    int i = switch (x) {
        case "1" -> 1;
        case "2" -> 2;
        default -> {
            yield 3;
        }
    };
    System.out.println(i);
}
```

或者

```java
@Test
public void testSwitch3() {
    String x = "3";
    int i = switch (x) {
        case "1":
            yield 1;
        case "2":
            yield 2;
        default:
            yield 3;
    };
    System.out.println(i);
}
```

</br>

## 文本块

现实问题：

在Java中，通常需要使用String类型表达HTML，XML，SQL或JSON等格式的字符串，在进行字符串赋值时需要进行转义和连接操作，然后才能编译该代码，这种表达方式难以阅读并且难以维护。

**JDK13的新特性**

使用"""作为文本块的开始符和结束符，在其中就可以放置多行的字符串，不需要进行任何转义。因此，文本块将提高Java程序的可读性和可写性。

基本使用：

```java
"""
line1
line2
line3
"""
```

相当于：

```java
"line1\nline2\nline3\n"
```

或者一个连接的字符串：

```java
"line1\n" +
"line2\n" +
"line3\n"
```

如果字符串末尾不需要行终止符，则结束分隔符可以放在最后一行内容上。例如：

```java
"""
line1
line2
line3"""
```

相当于

```java
"line1\nline2\nline3"
```

文本块可以表示空字符串，但不建议这样做，因为它需要两行源代码：

```java
String empty = """
""";
```

**JDK14中二次预览特性**

JDK14的版本主要增加了两个escape sequences，分别是` \  取消换行 `与`\s  空格`。

</br>

## Record

`record` 是一种全新的类型，它本质上是一个 `final` 类，同时所有的属性都是 `final` 修饰，它会自动编译出 `public get` 、`hashcode` 、`equals`、`toString`、构造器等结构，减少了代码编写量。

具体来说：当你用`record` 声明一个类时，该类将自动拥有以下功能：

- 获取成员变量的简单方法，比如例题中的 name() 和 partner() 。注意区别于我们平常getter()的写法。
- 一个 equals 方法的实现，执行比较时会比较该类的所有成员属性。
- 重写 hashCode() 方法。
- 一个可以打印该类所有成员属性的 toString() 方法。
- 只有一个构造方法。

此外：

- 还可以在record声明的类中定义静态字段、静态方法、构造器或实例方法。
- 不能在record声明的类中定义实例字段；类不能声明为abstract；不能声明显式的父类等。

举例：

```java
public record Dog(String name, Integer age) {
}
```

```java
public class Java14Record {

    public static void main(String[] args) {
        Dog dog1 = new Dog("牧羊犬", 1);
        Dog dog2 = new Dog("田园犬", 2);
        Dog dog3 = new Dog("哈士奇", 3);
        System.out.println(dog1);
        System.out.println(dog2);
        System.out.println(dog3);
    }
}
```

`记录不适合哪些场景`

record的设计目标是提供一种将数据建模为数据的好方法。它也不是 JavaBeans 的直接替代品，因为record的方法不符合 JavaBeans 的 get 标准。另外 JavaBeans 通常是可变的，而记录是不可变的。尽管它们的用途有点像，但记录并不会以某种方式取代 JavaBean。

</br>

## 密封类

背景：

在 Java 中如果想让一个类不能被继承和修改，这时我们应该使用 `final` 关键字对类进行修饰。不过这种要么可以继承，要么不能继承的机制不够灵活，有些时候我们可能想让某个类可以被某些类型继承，但是又不能随意继承，是做不到的。Java 15 尝试解决这个问题，引入了 `sealed` 类，被 `sealed` 修饰的类可以指定子类。这样这个类就只能被指定的类继承。

**JDK15的预览特性：**

通过密封的类和接口来限制超类的使用，密封的类和接口限制其它可能继承或实现它们的其它类或接口。

具体使用：

- 使用修饰符`sealed`，可以将一个类声明为密封类。密封的类使用保留关键字`permits`列出可以直接扩展（即extends）它的类。


-  `sealed` 修饰的类的机制具有传递性，它的子类必须使用指定的关键字进行修饰，且只能是 `final`、`sealed`、`non-sealed` 三者之一。


举例：

```java
package com.atguigu.java;
public abstract sealed class Shape permits Circle, Rectangle, Square {...}

public final class Circle extends Shape {...} //final表示Circle不能再被继承了

public sealed class Rectangle extends Shape permits TransparentRectangle, FilledRectangle {...}

public final class TransparentRectangle extends Rectangle {...}

public final class FilledRectangle extends Rectangle {...}

public non-sealed class Square extends Shape {...} //non-sealed表示可以允许任何类继承
```

</br>

# 其它变化

## Optional类

**JDK8的新特性**

到目前为止，臭名昭著的空指针异常是导致Java应用程序失败的最常见原因。以前，为了解决空指针异常，Google在著名的Guava项目引入了Optional类，通过检查空值的方式避免空指针异常。受到Google的启发，Optional类已经成为Java 8类库的一部分。

`Optional<T>` 类(java.util.Optional) 是一个容器类，它可以保存类型T的值，代表这个值存在。或者仅仅保存null，表示这个值不存在。如果值存在，则isPresent()方法会返回true，调用get()方法会返回该对象。

Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

- `创建Optional类对象的方法：`
- static <T> Optional<T> empty() ：用来创建一个空的Optional实例
  - static <T> Optional<T> of(T value) ：用来创建一个Optional实例，value必须非空
  - `static <T> Optional<T> ofNullable(T value)` ：用来创建一个Optional实例，value可能是空，也可能非空

- `判断Optional容器中是否包含对象：`
  - boolean isPresent() : 判断Optional容器中的值是否存在
  - void ifPresent(Consumer<? super T> consumer) ：判断Optional容器中的值是否存在，如果存在，就对它进行Consumer指定的操作，如果不存在就不做

- `获取Optional容器的对象：`
- T get(): 如果调用对象包含值，返回该值。否则抛异常。T get()与of(T value)配合使用

- `T orElse(T other) `：orElse(T other) 与ofNullable(T value)配合使用，如果Optional容器中非空，就返回所包装值，如果为空，就用orElse(T other)other指定的默认值（备胎）代替

- T orElseGet(Supplier<? extends T> other) ：如果Optional容器中非空，就返回所包装值，如果为空，就用Supplier接口的Lambda表达式提供的值代替

- T orElseThrow(Supplier<? extends X> exceptionSupplier) ：如果Optional容器中非空，就返回所包装值，如果为空，就抛出你指定的异常类型代替原来的NoSuchElementException

举例：

```java
import java.util.Optional;

import org.junit.Test;

public class TestOptional {
	@Test
    public void test1(){
        String str = "hello";
        Optional<String> opt = Optional.of(str);
        System.out.println(opt);
    }
    @Test
    public void test2(){
        Optional<String> opt = Optional.empty();
        System.out.println(opt);
    }
    @Test
    public void test3(){
        String str = null;
        Optional<String> opt = Optional.ofNullable(str);
        System.out.println(opt);
    }
    @Test
    public void test4(){
        String str = "hello";
        Optional<String> opt = Optional.of(str);

        String string = opt.get();
        System.out.println(string);
    }
    @Test
    public void test5(){
        String str = null;
        Optional<String> opt = Optional.ofNullable(str);
//		System.out.println(opt.get());//java.util.NoSuchElementException: No value present
    }
    @Test
    public void test6(){
        String str = "hello";
        Optional<String> opt = Optional.ofNullable(str);
        String string = opt.orElse("atguigu");
        System.out.println(string);
    }
    @Test
    public void test7(){
        String str = null;
        Optional<String> opt = Optional.ofNullable(str);
        String string = opt.orElseGet(String::new);
        System.out.println(string);
    }
    @Test
    public void test8(){
        String str = null;
        Optional<String> opt = Optional.ofNullable(str);
        String string = opt.orElseThrow(()->new RuntimeException("值不存在"));
        System.out.println(string);
    }
    @Test
    public void test9(){
        String str = "Hello1";
        Optional<String> opt = Optional.ofNullable(str);
        //判断是否是纯字母单词，如果是，转为大写，否则保持不变
        String result = opt.filter(s->s.matches("[a-zA-Z]+"))
                .map(s -> s.toUpperCase()).orElse(str);
        System.out.println(result);
    }
}

```

**这是JDK9-11的新特性**

| **新增方法**                                                 | **描述**                                                     | **新增的版本** |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------- |
| boolean isEmpty()                                            | 判断value是否为空                                            | JDK  11        |
| ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction) | value非空，执行参数1功能；如果value为空，执行参数2功能       | JDK  9         |
| Optional<T> or(Supplier<? extends Optional<? extends T>> supplier) | value非空，返回对应的Optional；value为空，返回形参封装的Optional | JDK  9         |
| Stream<T> stream()                                           | value非空，返回仅包含此value的Stream；否则，返回一个空的Stream | JDK  9         |
| T orElseThrow()                                              | value非空，返回value；否则抛异常NoSuchElementException       | JDK  10        |

</br>

## GC方面新特性

###  G1 GC

JDK9以后默认的垃圾回收器是G1GC。

**JDK10 : 为G1提供并行的Full GC**

G1最大的亮点就是可以尽量的避免full gc。但毕竟是“尽量”，在有些情况下，G1就要进行full gc了，比如如果它无法足够快的回收内存的时候，它就会强制停止所有的应用线程然后清理。

在Java10之前，一个单线程版的标记-清除-压缩算法被用于full gc。为了尽量减少full gc带来的影响，在Java10中，就把之前的那个单线程版的标记-清除-压缩的full gc算法改成了支持多个线程同时full gc。这样也算是减少了full gc所带来的停顿，从而提高性能。

你可以通过`-XX:ParallelGCThreads`参数来指定用于并行GC的线程数。

**JDK12：可中断的 G1 Mixed GC**

**JDK12：增强G1，自动返回未用堆内存给操作系统**

### henandoah GC

**JDK12：Shenandoah GC：低停顿时间的GC**

Shenandoah 垃圾回收器是 Red Hat 在 2014 年宣布进行的一项垃圾收集器研究项目 Pauseless GC 的实现，旨在**针对 JVM 上的内存收回实现低停顿的需求**。

###  ZGC

**JDK11：引入革命性的 ZGC**

ZGC，这应该是JDK11最为瞩目的特性，没有之一。 

ZGC是一个并发、基于region、压缩型的垃圾收集器。

ZGC的设计目标是：支持TB级内存容量，暂停时间低（<10ms），对整个程序吞吐量的影响小于15%。 将来还可以扩展实现机制，以支持不少令人兴奋的功能，例如多层堆（即热对象置于DRAM和冷对象置于NVMe闪存），或压缩堆。

**JDK13：ZGC:将未使用的堆内存归还给操作系统**

**JDK14：ZGC on macOS和windows**

**JDK15：ZGC 功能转正**

**JDK16：ZGC 并发线程处理**