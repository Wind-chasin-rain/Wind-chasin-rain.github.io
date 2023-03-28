---
title: Java学习笔记02
date: 
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208040005348.png
tags: Java
categories: 学习笔记
description: 类、枚举、注解、异常、常用类、集合、泛型、多线程、IO流
updated:
mathjax:
highlight_shrink:
---

# 面向对象编程（高级部分）

## 类变量和类方法

### 类变量

类变量也叫静态变量／静态属性，是该类的所有对象共享的变量，任何一个该类的对象去访问它时，取到的都是相同的值，同样任何一个该类的对象去修改它时，修改的也是同一个变量。

**类变量定义语法：**

访问修饰符 static 数据类型 变量名； *［推荐］*

static 访问修饰符 数据类型 变量名：

**访问类变量**

类名.类变量名   *［推荐］*

对象名.类变量名

【静态变量的访问修饰符的访问权限和范围和普通属性是一样的】

### 类方法

类方法也叫静态方法。

**类方法定义语法：**

访问修饰符 static 数据返回类型 方法名（）｛ ｝*【推荐】*static 访问修饰符 数据返回类型 方法名（）｛ ｝

**类方法的调用**

使用方式：类名.类方法名 或者 对象名.类方法名

【满足访问修饰符的访问权限和】

```java
public class StaticMethod {
    public static void main(String[] args) {
        //创建2个学生对象，叫学费
        Stu tom = new Stu("tom");
        //tom.payFee(100);
        Stu.payFee(100);//对不对?对

        Stu mary = new Stu("mary");
        //mary.payFee(200);
        Stu.payFee(200);//对


        //输出当前收到的总学费
        Stu.showFee();//300

        //如果我们希望不创建实例，也可以调用某个方法(即当做工具来使用)
        //这时，把方法做成静态方法时非常合适
        System.out.println("9开平方的结果是=" + Math.sqrt(9));


        System.out.println(MyTools.calSum(10, 30));
    }
}
//开发自己的工具类时，可以将方法做成静态的，方便调用
class MyTools  {
    //求出两个数的和
    public static double calSum(double n1, double n2) {
        return  n1 + n2;
    }
    //可以写出很多这样的工具方法...
}
class Stu {
    private String name;//普通成员
    //定义一个静态变量，来累积学生的学费
    private static double fee = 0;

    public Stu(String name) {
        this.name = name;
    }
    //说明
    //1. 当方法使用了static修饰后，该方法就是静态方法
    //2. 静态方法就可以访问静态属性/变量
    public static void payFee(double fee) {
        Stu.fee += fee;//累积到
    }
    public static void showFee() {
        System.out.println("总学费有:" + Stu.fee);
    }
}
```

**小结**

1）类方法和普通方法都是随着类的加载而加载，将结构信息存储在方法区：类方法中无this的参数，普通方法中隐含着this的参数。

2）类方法可以通过类名调用，也可以通过对象名调用。［举例］

3）普通方法和对象有关，需要通过对象名调用，比如对象名．方法名（参数），不能通过类名调用。

4）类方法中不允许使用和对象有关的关键字，比如this和super。普通方法（成员方法）可以。

5）类方法（静态方法）中只能访问静态变量或静态方法 。

6）普通成员方法，既可以访问非静态成员，也可以访问静态成员。



## 理解main方法

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121128569.png)

1) 在 main()方法中，我们可以直接调用 main 方法所在类的静态方法或静态属性。 
2) 但是，不能直接访问该类中的非静态成员，必须创建该类的一个实例对象后，才能通过这个对象去访问类中的非静态成员。

```java
public class Main01 {
    //静态的变量/属性
    private static String name = "JAVA学习";
    //非静态的变量/属性
    private int n1 = 10000;

    //静态方法
    public static void hi() {
        System.out.println("Main01的 hi方法");
    }

    //非静态方法
    public void cry() {
        System.out.println("Main01的 cry方法");
    }

    public static void main(String[] args) {

        //可以直接使用 name
        //1. 静态方法main 可以访问本类的静态成员
        System.out.println("name=" + name);
        hi();
        //2. 静态方法main 不可以访问本类的非静态成员
        //System.out.println("n1=" + n1);//错误
        //cry();
        //3. 静态方法main 要访问本类的非静态成员，需要先创建对象 , 再调用即可
        Main01 main01 = new Main01();
        System.out.println(main01.n1);//ok
        main01.cry();
    }
}

```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121143429.png)

```java
public class Main02 {
    public static void main(String[] args) {
        for ( int i = 0; i < args.length; i++ ) {
            System.out.println("args[" + i + "] = " + args[i]);
        }
    }
}
//run
args[0] = 北京
args[1] = 天津
args[2] = 上海
args[3] = Jack
args[4] = Tom
```



## 代码块

代码化块又称为初始化块,属于类中的成员[即是类的一部分]，类似于方法，将逻辑语句封装在方法体中，通过{}包围起来。
但和方法不同，没有方法名，没有返回，没有参数，只有方法体，而且不用通过对象或类显式调用,而是加载类时,或创建对象时隐式调用。

[修饰符]{



};

*说明注意*

1) 修饰符 可选，要写的话，也只能写static

2) 代码块分为两类，使用static修饰的叫静态代码块，没有static修饰的，叫普通代码块/非静态代码块

3) 逻辑语句可以为任何逻辑语句(输入、输出、方法调用、循环、判断等)

4) ;号可以写上，也可以省略

*好处*

1)相当于另外一种形式的构造器(对构造器的补充机制)，可以做初始化的操作

2)场景:如果多个构造器中都有重复的语句，可以抽取到初始化块中,提高代码的重用性

*细节*

1. static代码块也叫静态代码块，作用就是对类进行初始化，而且它随着类的加载而执行，并且只会执行一次。如果是普通代码块，每创建一个对象,就执行。

1) 类什么时候被加载
①创建对象实例时(new)
②创建子类对象实例，父类也会被加载
③使用类的静态成员时(静态属性,静态方法)

3. 普通的代码块,在创建对象实例时,会被隐式的调用。
    被创建一次，就会调用一次。
    如果只是使用类的静态成员时，普通代码块并不会执行。

  ```java
  public class CodeBlock {
      public static void main(String[] args) {
          //①创建对象实例时(new)
          //②创建子类对象实例，父类也会被加载
          AA a = new AA();
          //③使用类的静态成员时(静态属性,静态方法)
          System.out.println(CC.n1);
          //static代码块随着类的加载而执行，并且只会执行一次
          DD d = new DD();
          DD d2 = new DD();
          //如果只是使用类的静态成员时，普通代码块并不会执行。
          System.out.println(DD.n2);
      }
  }
  
  class DD{
      public static int n2 = 888;
      static {
          System.out.println("DD的静态代码块...");
      };
      {
          System.out.println("DD的普通代码块...");
      }
  }
  class CC {
      public static int n1 = 999;
  
      static {
          System.out.println("CC的静态代码块...");
      }
  }
  
  class BB {
      static {
          System.out.println("BB的静态代码块...");
      }
  }
  
  class AA extends BB {
      static {
          System.out.println("AA的静态代码块...");
      }
  }
  ```

  

4. 创建一个对象时，在一个类调用顺序是:

    ①调用静态代码块和静态属性初始化(注意:静态代码块和静态属性初始化调用的优先级一样,如果有多个静态代码块和多个静态变量初始化,则按他们定义的顺序调用）

    ②调用普通代码块和普通属性的初始化(注意:普通代码块和普通属性初始化调用的优先级一样,如果有多个普通代码块和多个普通属性初始化,则按定义顺序调用)

    ③调用构造方法

  ```java
  public class CodeBlock02 {
      public static void main(String[] args) {
          A a = new A();
      }
  }
  
  class A {
      { //普通代码块
          System.out.println("A 普通代码块01");
      }
  
      //普通属性的初始化
      private int n2 = getN2();
  
  
      static { //静态代码块
          System.out.println("A 静态代码块01");
      }
  
      //静态属性的初始化
      private static int n1 = getN1();
  
      public static int getN1() {
          System.out.println("getN1被调用...");
          return 100;
      }
  
      public int getN2() { //普通方法/非静态方法
          System.out.println("getN2被调用...");
          return 200;
      }
  
      //无参构造器
      public A() {
          System.out.println("A() 构造器被调用");
      }
  
  }
  
  //run
  A 静态代码块01
  getN1被调用...
  A 普通代码块01
  getN2被调用...
  A() 构造器被调用
  ```

5. 构造器的最前面其实隐含了super()和调用普通代码块，静态相关的代码块，属性初始化，在类加载时，就执行完毕，因此是优先于构造器和普通代码块执行的。

6. 创建一个子类对象时(继承关系)，他们的静态代码块，静态属性初始化，普通代码块，普通属性初始化，构造方法的调用顺序如下:

  ①父类的静态代码块和静态属性(优先级一样,按定义顺序执行)

  ②子类的静态代码块和静态属性(优先级一样,按定义顺序执行)
  ③父类的普通代码块和普通属性初始化(优先级一样，按定义顺序执行)

  ④父类的构造方法
  ⑤子类的普通代码块和普通属性初始化(优先级一样，按定义顺序执行)

  ⑥子类的构造方法

7. 静态代码块只能直接调用静态成员(静态属性和静态方法)，普通代码块可以调用任意成员。

  ```java
  public class CodeBlock03 {
      public static void main(String[] args) {
  
          //(1) 进行类的加载
          //1.1 先加载 父类 A02 1.2 再加载 B02
          //(2) 创建对象
          //2.1 从子类的构造器开始
          //new B02();//对象
  
          new C02();
      }
  }
  
  class A02 { //父类
      private static int n1 = getVal01();
  
      static {
          System.out.println("A02的一个静态代码块..");//(2)
      }
  
      {
          System.out.println("A02的第一个普通代码块..");//(5)
      }
  
      public int n3 = getVal02();//普通属性的初始化
  
      public static int getVal01() {
          System.out.println("getVal01");//(1)
          return 10;
      }
  
      public int getVal02() {
          System.out.println("getVal02");//(6)
          return 10;
      }
  
      public A02() {//构造器
          //隐藏
          //super()
          //普通代码和普通属性的初始化......
          System.out.println("A02的构造器");//(7)
      }
  
  }
  
  class C02 {
      private int n1 = 100;
      private static int n2 = 200;
  
      private void m1() {
  
      }
  
      private static void m2() {
  
      }
  
      static {
          //静态代码块，只能调用静态成员
          //System.out.println(n1);错误
          System.out.println(n2);//ok
          //m1();//错误
          m2();
      }
  
      {
          //普通代码块，可以使用任意成员
          System.out.println(n1);
          System.out.println(n2);//ok
          m1();
          m2();
      }
  }
  
  class B02 extends A02 { //
  
      private static int n3 = getVal03();
  
      static {
          System.out.println("B02的一个静态代码块..");//(4)
      }
  
      public int n5 = getVal04();
  
      {
          System.out.println("B02的第一个普通代码块..");//(9)
      }
  
      public static int getVal03() {
          System.out.println("getVal03");//(3)
          return 10;
      }
  
      public int getVal04() {
          System.out.println("getVal04");//(8)
          return 10;
      }
  
      //一定要慢慢的去品..
      public B02() {//构造器
          //隐藏了
          //super()
          //普通代码块和普通属性的初始化...
          System.out.println("B02的构造器");//(10)
          // TODO Auto-generated constructor stub
      }
  }
  
  ```

### 单例设计模式

单例(单个的实例)

1. 所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法
2. 单例模式有两种方式:1)饿汉式2)懒汉式

饿汉式和懒汉式单例模式的实现。步骤如下:
1)构造器私有化=》防止直接new

2)类的内部创建对象

3)向外暴露一个静态的公共方法。getInstance

4)代码实现

```java
//饿汉式
public class SingleTon01 {
    public static void main(String[] args) {
        GirlFridend instance = GirlFridend.getInstance();
        System.out.println(instance);

        GirlFridend instance2 = GirlFridend.getInstance();
        System.out.println(instance2);

        System.out.println(instance == instance2);//T
    }
}

class GirlFridend {
    private String name;
    private static GirlFridend gf = new GirlFridend("小红红");

    private GirlFridend(String name) {
        this.name = name;
    }

    public static GirlFridend getInstance() {
        return gf;
    }

    @Override
    public String toString() {
        return "GirlFridend{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

```java
//懒汉式
public class SingleTon02 {
    public static void main(String[] args) {
        //不会调用构造器
        //System.out.println(Cat.n1);

        Cat cat = Cat.getInstance();
        System.out.println(cat);

        Cat cat2 = Cat.getInstance();
        System.out.println(cat2);

        System.out.println(cat2 == cat);//T

    }
}

class Cat {
    private String name;
    public static int n1 = 777;
    private static Cat cat;

    private Cat(String name) {
        System.out.println("构造器被调用...");
        this.name = name;
    }

    public static Cat getInstance() {
        if (cat == null) {
            cat = new Cat("阿白");
        }
        return cat;
    }

    @Override
    public String toString() {
        return "Cat{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

饿汉式和懒汉式的区别

1. 二者最主要的区别在于创建对象的时机不同:饿汉式是在类加载就创建了对象实例,而懒汉式是在使用时才创建。

2. 饿汉式不存在线程安全问题，懒汉式存在线程安全问题。

3. 饿汉式存在浪费资源的可能。因为如果程序员一个对象实例都没有使用，那么饿汉式创建的对象就浪费了，懒汉式是使用时才创建，就不存在这个问题。
   

## final关键字

final可以修饰类、属性、方法和局部变量。
在某些情况下，可能有以下*需求*，就会使用到final:

1)当不希望类被继承时,可以用final修饰。

2)当不希望父类的某个方法被子类覆盖/重写(override)时，可以用final关键字修饰。【访问修饰符 final 返回类型 方法名】

3)当不希望类的的某个属性的值被修改,可以用final修饰。

4)当不希望某个局部变量被修改，可以使用final修饰

*注意*

1. final修饰的属性又叫常量，一般用XX_XX_X来命名

2. final修饰的属性在定义时，必须赋初值，并且以后不能再修改，赋值可以在如下位置之一：
   ①定义时：如public final double TAX_RATE=0.08;

   ②在构造器中。

   ③在代码块中。

3. 如果final修饰的属性是静态的，则初始化的位置只能是①定义时②在静态代码块不能在构造器中赋值。

4. final类不能继承,但是可以实例化对象。
5. 如果类不是final类，但是含有final方法，则该方法虽然不能重写，但是可以被继承。

```java
public class Final01 {
    public static void main(String[] args) {
        CC cc = new CC();

        new EE().cal();
    }
}

class AA {
    /*
    1. 定义时：如 public final double TAX_RATE=0.08;
    2. 在构造器中
    3. 在代码块中
     */
    public final double TAX_RATE = 0.08;//1.定义时赋值
    public final double TAX_RATE2 ;
    public final double TAX_RATE3 ;

    public AA() {//构造器中赋值
        TAX_RATE2 = 1.1;
    }
    {//在代码块赋值
        TAX_RATE3 = 8.8;
    }
}

class BB {
    /*
    如果final修饰的属性是静态的，则初始化的位置只能是
    1 定义时  2 在静态代码块 不能在构造器中赋值。
     */
    public static final double TAX_RATE = 99.9;
    public static final double TAX_RATE2 ;

    static {
        TAX_RATE2 = 3.3;
    }


}

//final类不能继承，但是可以实例化对象
final class CC { }


//如果类不是final类，但是含有final方法，则该方法虽然不能重写，但是可以被继承
//即，仍然遵守继承的机制.
class DD {
    public final void cal() {
        System.out.println("cal()方法");
    }
}
class EE extends DD { }
```

6) final不能修饰构造方法(即构造器)
6) 一般来说，如果一个类已经是final类了，就没有必要再将方法修饰成final方法。
6) final和static往往搭配使用，效率更高，不会导致类加载.底层编译器做了优化处理。
6) 包装类(Integer,Double,Float，Boolean等都是final),String也是final类。

```java
public class Final02 {
    public static void main(String[] args) {
        System.out.println(BBB.num);

        //包装类,String 是final类，不能被继承

    }
}

//final 和 static 往往搭配使用，效率更高，不会导致类加载.底层编译器做了优化处理
class BBB {
    public final static  int num = 10000;
    static {
        System.out.println("BBB 静态代码块被执行");
    }
}
final class AAA{
    //一般来说，如果一个类已经是final类了，就没有必要再将方法修饰成final方法
    //public final void cry() {}
}

```



## 抽象类

1)用abstract关键字来修饰一个类时，这个类就叫抽象类访问。【修饰符 abstract 类名{}】
2)用abstract关键字来修饰一个方法时，这个方法就是抽象方法。【访问修饰符 abstract 返回类型 方法名(参数列表); //没有方法体】
3)抽象类的价值更多作用是在于设计，是设计者设计好后，让子类继承并实现抽象类（）
4)抽象类，在框架和设计模式使用较多。

*细节*

1. 抽象类不能被实例化
2. 抽象类不一定要包含abstract方法。也就是说,抽象类可以没有abstract方法
3. 一旦类包含了abstract方法,则这个类必须声明为abstract
4. abstract只能修饰类和方法，不能修饰属性和其它的
5. 抽象类可以有任意成员【抽象类本质还是类】，比如:非抽象方法,构造器、静态属性等等
6. 抽象方法不能有主体，即不能实现
7. 如果一个类继承了抽象类，则它必须实现抽象类的所有抽象方法，除非它自己也声明为abstract类
8. 抽象方法不能使用private、final和static来修饰，因为这些关键字都是和重写相违背的

### 模板设计模式

*基本介绍*
抽象类体现的就是一种模板模式的设计，抽象类作为多个子类的通用模板，子类在抽象类的基础上进行扩展、改造，但子类总体上会保留抽象类的行为方式。
*模板设计模式能解决的问题*
1)当功能内部一部分实现是确定，一部分实现是不确定的。这时可以把不确定的部分暴露出去，让子类去实现。
2)编写一个抽象父类，父类提供了多个子类的通用方法，并把一个或多个方法留给其子类实现，就是一种模板模式。

```java
public class TestTemplate {
    public static void main(String[] args) {
        Sub sub = new Sub();
        sub.calculateTime();
    }
}
```

```java
abstract public class Template {
    public abstract void job();

    public void calculateTime() {
        long start = System.currentTimeMillis();
        job(); //动态绑定
        long end = System.currentTimeMillis();
        System.out.println("runtimes: " + (end - start) + " ms");
    }

}

```

```Java
public class Sub extends Template {
    @Override
    public void job() {
        long num = 0;
        for (long i = 0; i < 100000; i++) {
            num += i;
        }
    }
}
```



## 接口

接口就是给出一些没有实现的方法,封装到一起,到某个类要使用的时候,在根据具体情况把这些方法写出来。

*语法:*
interface 接口名{

​	//属性

​	//抽象方法

}
class 类名 implements 接口{
		自己属性;
		自己方法;
		必须实现的接口的抽象方法

}

*注意*

1)接口不能被实例化

2)接口中所有的方法是public方法，接口中抽象方法，可以不用abstract修饰

3)一个普通类实现接口,就必须将该接口的所有方法都实现

4)抽象类实现接口,可以不用实现接口的方法

5)一个类同时可以实现多个接口

6)接口中的属性,只能是final的，而且是 public static final修饰符。比如int a=1;实际上是 public static final int a=1; (必须初始化)

7)接口中属性的访问形式:接口名.属性名

8)接口不能继承其它的类,但是可以继承多个别的接口

9)接口的修饰符只能是 public和默认，这点和类的修饰符是一样的。

```Java
public class ExtendsVsInterface {
    public static void main(String[] args) {
        LittleMonkey wuKong = new LittleMonkey("悟空");
        wuKong.climbing();
        wuKong.swimming();
        wuKong.flying();
    }
}

//猴子
class Monkey {
    private String name;

    public Monkey(String name) {
        this.name = name;
    }

    public void climbing() {
        System.out.println(name + " 会爬树...");
    }

    public String getName() {
        return name;
    }
}

//接口
interface Fishable {
    void swimming();
}

interface Birdable {
    void flying();
}

//继承
//小结:  当子类继承了父类，就自动的拥有父类的功能
//      如果子类需要扩展功能，可以通过实现接口的方式扩展.
//      可以理解 实现接口 是 对java 单继承机制的一种补充.
class LittleMonkey extends Monkey implements Fishable, Birdable {

    public LittleMonkey(String name) {
        super(name);
    }

    @Override
    public void swimming() {
        System.out.println(getName() + " 通过学习，可以像鱼儿一样游泳...");
    }

    @Override
    public void flying() {
        System.out.println(getName() + " 通过学习，可以像鸟儿一样飞翔...");
    }
}

```

### 接口多态

多态参数

```java
public class InterfacePolyParameter {
    public static void main(String[] args) {

        //接口的多态体现
        //接口类型的变量 if01 可以指向 实现了IF接口类的对象实例
        IF if01 = new Monster();
        if01 = new Car();

        //继承体现的多态
        //父类类型的变量 a 可以指向 继承AAA的子类的对象实例
        AAA a = new BBB();
        a = new CCC();
    }
}

interface IF {}
class Monster implements IF{}
class Car implements  IF{}

class AAA {

}
class BBB extends AAA {}
class CCC extends AAA {}
```

多态数组

```java
package senior.interface_;

public class InterfacePolyArr {
    public static void main(String[] args) {

        //多态数组 -> 接口类型数组
        Usb[] usbs = new Usb[2];
        usbs[0] = new Phone_();
        usbs[1] = new Camera_();
        /*
        给Usb数组中，存放 Phone  和  相机对象，Phone类还有一个特有的方法call（），
        请遍历Usb数组，如果是Phone对象，除了调用Usb 接口定义的方法外，
        还需要调用Phone 特有方法 call
         */
        for (int i = 0; i < usbs.length; i++) {
            usbs[i].work();//动态绑定..
            //和前面一样，我们仍然需要进行类型的向下转型
            if (usbs[i] instanceof Phone_) {//判断他的运行类型是 Phone_
                ((Phone_) usbs[i]).call();
            }
        }

    }
}

interface Usb {
    void work();
}

class Phone_ implements Usb {
    public void call() {
        System.out.println("手机可以打电话...");
    }

    @Override
    public void work() {
        System.out.println("手机工作中...");
    }
}

class Camera_ implements Usb {

    @Override
    public void work() {
        System.out.println("相机工作中...");
    }
}
```

多态传递

```java
public class InterfacePolyPass {
    public static void main(String[] args) {
        //接口类型的变量可以指向，实现了该接口的类的对象实例
        IG ig = new Teacher();
        //如果IG 继承了 IH 接口，而Teacher 类实现了 IG接口
        //那么，实际上就相当于 Teacher 类也实现了 IH接口.
        //这就是所谓的 接口多态传递现象.
        IH ih = new Teacher();
    }
}

interface IH {
    void hi();
}
interface IG extends IH{ }
class Teacher implements IG {
    @Override
    public void hi() {
    }
}

```

```java
public class InterfaceExercise {
    public static void main(String[] args) {

    }
}

interface A { 
    int x = 0;
}  //想到 等价 public static final int x = 0;

class B {
    int x = 1;
} //普通属性

class C extends B implements A {
    public void pX() {
        //System.out.println(x); //错误，原因不明确x
        //可以明确的指定x
        //访问接口的 x 就使用 A.x
        //访问父类的 x 就使用 super.x
        System.out.println(A.x + " " + super.x);
    }

    public static void main(String[] args) {
        new C().pX();
    }
}
```



## 内部类

一个类的内部又完整的嵌套了另一个类结构。被嵌套的类称为内部类(inner class)，嵌套其他类的类称为外部类(outer class)。是我们类的第五大成员【类的五大成员是哪些?[属性、方法、构造器、代码块、内部类]】，内部类最大的特点就是可以直接访问私有属性，并且可以体现类与类之间的包含关系。

```java
public class InnerClass01 {
    public static void main(String[] args) {

    }
}
class Outer { //外部类
    private int n1 = 100;//属性

    public Outer(int n1) {//构造器
        this.n1 = n1;
    }

    public void m1() {//方法
        System.out.println("m1()");
    }

    {//代码块
        System.out.println("代码块...");
    }

    class Inner { //内部类, 在 Outer 类的内部
    }
}
```

*分类*

- 如果定义类在局部位置(方法中/代码块)：(1) 局部内部类 (2) 匿名内部类 
- 定义在成员位置 ：(1) 成员内部类 (2)静态内部类

### 局部内部类

说明:局部内部类是定义在外部类的局部位置，比如方法中，并且有类名

1.可以直接访问外部类的所有成员,包含私有的

2.不能添加访问修饰符,因为它的地位就是一个局部变量。局部变量是不能使用修饰符的。但是可以使用final修饰，因为局部变量也可以使用final

3.作用域:仅仅在定义它的方法或代码块中。

4.局部内部类---访问---->外部类的成员[访问方式:直接访间]

5.外部类---访问---->局部内部类的成员访问方式:创建对象，再访问(注意：必须在作用域内)记住：(1)局部内部类定义在方法中/代码块(2)作用域在方法体或者代码块中(3)本质仍然是一个类

6.外部其他类---不能访问----->局部内部类（因为局部内部类地位是一个局部变量)

7.如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想访问外部类的成员，则可以使用(外部类名.this.成员)去访问

```java
public class LocalInnerClass {//
    public static void main(String[] args) {
        Outer02 outer02 = new Outer02();
        outer02.m1();
        System.out.println("outer02的hashcode=" + outer02);
    }
}


class Outer02 {//外部类
    private int n1 = 100;
    private void m2() {
        System.out.println("Outer02 m2()");
    }//私有方法
    public void m1() {//方法
        //1.局部内部类是定义在外部类的局部位置,通常在方法
        //3.不能添加访问修饰符,但是可以使用final 修饰
        //4.作用域 : 仅仅在定义它的方法或代码块中
        final class Inner02 {//局部内部类(本质仍然是一个类)
            //2.可以直接访问外部类的所有成员，包含私有的
            private int n1 = 800;
            public void f1() {
                //5. 局部内部类可以直接访问外部类的成员，比如下面 外部类n1 和 m2()
                //7. 如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想访问外部类的成员，
                //   使用 外部类名.this.成员）去访问
                //    .this 本质就是外部类的对象, 即哪个对象调用了m1, Outer02.this就是哪个对象
                System.out.println("n1=" + n1 + " 外部类的n1=" + Outer02.this.n1);
                System.out.println("Outer02.this hashcode=" + Outer02.this);
                m2();
            }
        }
        //6. 外部类在方法中，可以创建Inner02对象，然后调用方法即可
        Inner02 inner02 = new Inner02();
        inner02.f1();
    }

}
```

### 匿名内部类

```java
public class AnonymousInnerClass {
    public static void main(String[] args) {
        Outer04 outer04 = new Outer04();
        outer04.method();

    }
}

class Outer04 { //外部类
    private int n1 = 10;//属性

    public void method() {//方法
        //基于接口的匿名内部类
        //1. 需求：想使用IA接口,并创建对象
        //2. 传统方式，是写一个类，实现该接口，并创建对象
        //3. 需求是 Tiger/Dog 类只是使用一次，后面再不使用
        //4. 可以使用匿名内部类来简化开发
        //5. tiger的编译类型 ? IA
        //6. tiger的运行类型 ? 就是匿名内部类  Outer04$1
        /*
            我们看底层 会分配 类名 Outer04$1
            class Outer04$1 implements IA {
                @Override
                public void cry() {
                    System.out .println("老虎叫唤...");
                }
            }
         */
        //7. jdk底层在创建匿名内部类 Outer04$1,立即马上就创建了 Outer04$1实例，并且把地址
        //   返回给 tiger
        //8. 匿名内部类使用一次，就不能再使用
        IA tiger = new IA() {
            @Override
            public void cry() {
                System.out.println("老虎叫唤...");
            }
        };
        System.out.println("tiger的运行类型=" + tiger.getClass());
        tiger.cry();
        tiger.cry();
        tiger.cry();

//        IA tiger = new Tiger();
//        tiger.cry();

        //演示基于类的匿名内部类
        //分析
        //1. father编译类型 Father
        //2. father运行类型 Outer04$2
        //3. 底层会创建匿名内部类
        /*
            class Outer04$2 extends Father{
                @Override
                public void test() {
                    System.out.println("匿名内部类重写了test方法");
                }
            }
         */
        //4. 同时也直接返回了 匿名内部类 Outer04$2的对象
        //5. 注意("jack") 参数列表会传递给 构造器
        Father father = new Father("jack") {

            @Override
            public void test() {
                System.out.println("匿名内部类重写了test方法");
            }
        };
        System.out.println("father对象的运行类型=" + father.getClass());//Outer04$2
        father.test();

        //基于抽象类的匿名内部类
        Animal animal = new Animal() {
            @Override
            void eat() {
                System.out.println("小狗吃骨头...");
            }
        };
        animal.eat();
    }
}

interface IA {//接口

    public void cry();
}
//class Tiger implements IA {
//
//    @Override
//    public void cry() {
//        System.out.println("老虎叫唤...");
//    }
//}
//class Dog implements  IA{
//    @Override
//    public void cry() {
//        System.out.println("小狗汪汪...");
//    }
//}

class Father {//类

    public Father(String name) {//构造器
        System.out.println("接收到name=" + name);
    }

    public void test() {//方法
    }
}

abstract class Animal { //抽象类
    abstract void eat();
}

```

```java
public class AnonymousInnerClassDetail {
    public static void main(String[] args) {

        Outer05 outer05 = new Outer05();
        outer05.f1();
        //外部其他类---不能访问----->匿名内部类
        System.out.println("main outer05 hashcode=" + outer05);
    }
}

class Outer05 {
    private int n1 = 99;
    public void f1() {
        //创建一个基于类的匿名内部类
        //不能添加访问修饰符,因为它的地位就是一个局部变量
        //作用域 : 仅仅在定义它的方法或代码块中
        Person p = new Person(){
            private int n1 = 88;
            @Override
            public void hi() {
                //可以直接访问外部类的所有成员，包含私有的
                //如果外部类和匿名内部类的成员重名时，匿名内部类访问的话，
                //默认遵循就近原则，如果想访问外部类的成员，则可以使用 （外部类名.this.成员）去访问
                System.out.println("匿名内部类重写了 hi方法 n1=" + n1 +
                        " 外部内的n1=" + Outer05.this.n1 );
                //Outer05.this 就是调用 f1的 对象
                System.out.println("Outer05.this hashcode=" + Outer05.this);
            }
        };
        p.hi();//动态绑定, 运行类型是 Outer05$1

        //也可以直接调用, 匿名内部类本身也是返回对象
        // class 匿名内部类 extends Person {}
//        new Person(){
//            @Override
//            public void hi() {
//                System.out.println("匿名内部类重写了 hi方法,哈哈...");
//            }
//            @Override
//            public void ok(String str) {
//                super.ok(str);
//            }
//        }.ok("jack");


    }
}

class Person {//类
    public void hi() {
        System.out.println("Person hi()");
    }
    public void ok(String str) {
        System.out.println("Person ok() " + str);
    }
}
//抽象类/接口...

```

### 成员内部类

成员内部类是定义在外部类的成员位置，并且没有static修饰。

1.可以直接访问外部类的所有成员，包含私有的

2.可以添加任意访问修饰符(public、protected、默认、private)，因为它的地位就是一个成员。

3.作用域和外部类的其他成员一样，为整个类体。在外部类的成员方法中创建成员内部类对象，再调用方法。

4.成员内部类---访问---->外部类成员(比如：属性) [访问方式：直接访问]

5.外部类---访问------>成员内部类(说明)访问方式：创建对象，再访问

6.外部其他类---访问---->成员内部类

```java
public class MemberInnerClass01 {
    public static void main(String[] args) {
        Outer08 outer08 = new Outer08();
        outer08.t1();

        //外部其他类，使用成员内部类的2种方式
        // 第一种方式
        // outer08.new Inner08(); 相当于把 new Inner08()当做是outer08成员
        // 这就是一个语法，不要特别的纠结.
        Outer08.Inner08 inner08 = outer08.new Inner08();
        inner08.say();
        // 第二方式 在外部类中，编写一个方法，可以返回 Inner08对象
        Outer08.Inner08 inner08Instance = outer08.getInner08Instance();
        inner08Instance.say();


    }
}

class Outer08 { //外部类
    private int n1 = 10;
    public String name = "张三";

    private void hi() {
        System.out.println("hi()方法...");
    }

    //1.注意: 成员内部类，是定义在外部内的成员位置上
    //2.可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员
    public class Inner08 {//成员内部类
        private double sal = 99.8;
        private int n1 = 66;
        public void say() {
            //可以直接访问外部类的所有成员，包含私有的
            //如果成员内部类的成员和外部类的成员重名，会遵守就近原则.
            //，可以通过  外部类名.this.属性 来访问外部类的成员
            System.out.println("n1 = " + n1 + " name = " + name + " 外部类的n1=" + Outer08.this.n1);
            hi();
        }
    }
    //方法，返回一个Inner08实例
    public Inner08 getInner08Instance(){
        return new Inner08();
    }


    //写方法
    public void t1() {
        //使用成员内部类
        //创建成员内部类的对象，然后使用相关的方法
        Inner08 inner08 = new Inner08();
        inner08.say();
        System.out.println(inner08.sal);
    }
}
```

### 静态内部类

说明:静态内部类是定义在外部类的成员位置，并且有static修饰
1.可以直接访问外部类的所有静态成员，包含私有的，但不能直接访问非静态成员

2.可以添加任意访问修饰符(public.protected、默认、private),因为它的地位就是一个成员。

3.作用域:同其他的成员，为整个类体

4.静态内部类---访问---->外部类(比如:静态属性)[访问方式:直接访问所有静
态成员]

5.外部类---访问------>静态内部类访问方式:创建对象，再访问

6.外部其他类---访问----->静态内部类

7.如果外部类和静态内部类的成员重名时，静态内部类访问的时，默认遵循就近原则，如果想访问外部类的成员，则可以使用(外部类名.成员)去访向

```java
public class StaticInnerClass01 {
    public static void main(String[] args) {
        Outer10 outer10 = new Outer10();
        outer10.m1();

        //外部其他类 使用静态内部类
        //方式1
        //因为静态内部类，是可以通过类名直接访问(前提是满足访问权限)
        Outer10.Inner10 inner10 = new Outer10.Inner10();
        inner10.say();
        //方式2
        //编写一个方法，可以返回静态内部类的对象实例.
        Outer10.Inner10 inner101 = outer10.getInner10();
        System.out.println("============");
        inner101.say();

        Outer10.Inner10 inner10_ = Outer10.getInner10_();
        System.out.println("************");
        inner10_.say();
    }
}

class Outer10 { //外部类
    private int n1 = 10;
    private static String name = "张三";
    private static void cry() {}
    //Inner10就是静态内部类
    //1. 放在外部类的成员位置
    //2. 使用static 修饰
    //3. 可以直接访问外部类的所有静态成员，包含私有的，但不能直接访问非静态成员
    //4. 可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员
    //5. 作用域 ：同其他的成员，为整个类体
    static class Inner10 {
        private static String name = "JAVA 学习";
        public void say() {
            //如果外部类和静态内部类的成员重名时，静态内部类访问的时，
            //默认遵循就近原则，如果想访问外部类的成员，则可以使用 （外部类名.成员）
            System.out.println(name + " 外部类name= " + Outer10.name);
            cry();
        }
    }

    public void m1() { //外部类---访问------>静态内部类 访问方式：创建对象，再访问
        Inner10 inner10 = new Inner10();
        inner10.say();
    }

    public Inner10 getInner10() {
        return new Inner10();
    }

    public static Inner10 getInner10_() {
        return new Inner10();
    }
}
```



# 枚举和注解

## 枚举

枚举属于一种特殊的类，里面只包含一组有限的特定的对象。

### 自定义实现枚举

进行自定义类实现枚举，有如下特点： 

1) 构造器私有化 
2)  本类内部创建一组对象[四个 春夏秋冬] 
3) 对外暴露对象（通过为对象添加 public final static 修饰符） 
4) 可以提供 get 方法，但是不要提供 set

```java
public class Enumeration02 {
    public static void main(String[] args) {
        System.out.println(Season.AUTUMN);
        System.out.println(Season.SPRING);
    }
}
//演示字定义枚举实现
class Season {//类
    private String name;
    private String desc;//描述
    //定义了四个对象, 固定.
    public static final Season SPRING = new Season("春天", "温暖");
    public static final Season WINTER = new Season("冬天", "寒冷");
    public static final Season AUTUMN = new Season("秋天", "凉爽");
    public static final Season SUMMER = new Season("夏天", "炎热");
    //1. 将构造器私有化,目的防止 直接 new
    //2. 去掉 setXxx 方法, 防止属性被修改
    //3. 在 Season 内部，直接创建固定的对象
    //4. 优化，可以加入 final 修饰符
    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }
    public String getDesc() {
        return desc;
    }
    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

### enum关键字实现枚举

```java
public class Enumeration03 {
    public static void main(String[] args) {
        System.out.println(Season03.AUTUMN);
        System.out.println(Season03.SUMMER);
    }
}
enum Season03 {
    //如果使用了enum 来实现枚举类
    //1. 使用关键字 enum 替代 class
    //2. public static final Season SPRING = new Season("春天", "温暖") 直接使用
    //   SPRING("春天", "温暖") 解读 常量名(实参列表)
    //3. 如果有多个常量(对象)， 使用 ,号间隔即可
    //4. 如果使用enum 来实现枚举，要求将定义常量对象，写在前面
    //5. 如果我们使用的是无参构造器，创建常量对象，则可以省略 ()
    SPRING("春天", "温暖"), WINTER("冬天", "寒冷"), AUTUMN("秋天", "凉爽"),
    SUMMER("夏天", "炎热")/*, What()*/;

    private String name;
    private String desc;//描述

    private Season03() {//无参构造器

    }

    private Season03(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

```java
public class EnumExercise01 {
    public static void main(String[] args) {
        Gender2 boy = Gender2.BOY;//OK
        Gender2 boy2 = Gender2.BOY;//OK
        System.out.println(boy);//输出BOY //本质就是调用 Gender2 的父类Enum的 toString()
//        public String toString() {
//            return name;
//        }
        System.out.println(boy2 == boy);  //True
    }
}

enum Gender2{ //父类 Enum 的toString
    BOY , GIRL;
}
```

### enum常用方法

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207131000867.png)



1) toString:Enum 类已经重写过了，返回的是当前对象 名,子类可以重写该方法，用于返回对象的属性信息 
2) name：返回当前对象名（常量名），子类中不能重写 
3) ordinal：返回当前对象的位置号，默认从 0 开始 
4) values：返回当前枚举类中所有的常量 
5)  valueOf：将字符串转换成枚举对象，要求字符串必须 为已有的常量名，否则报异常！ 
6)  compareTo：比较两个枚举常量，比较的就是编号！

```java
public class EnumMethod {
    public static void main(String[] args) {
        //使用Season03 枚举类，来演示各种方法
        Season03 autumn = Season03.AUTUMN;

        //输出枚举对象的名字
        System.out.println(autumn.name());
        //ordinal() 输出的是该枚举对象的次序/编号，从0开始编号
        //AUTUMN 枚举对象是第三个，因此输出 2
        System.out.println(autumn.ordinal());
        //从反编译可以看出 values方法，返回 Season2[]
        //含有定义的所有枚举对象
        Season03[] values = Season03.values();
        System.out.println("===遍历取出枚举对象(增强for)====");
        for (Season03 season: values) {//增强for循环
            System.out.println(season);
        }

        //valueOf：将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报异常
        //执行流程
        //1. 根据你输入的 "AUTUMN" 到 Season2的枚举对象去查找
        //2. 如果找到了，就返回，如果没有找到，就报错
        Season03 autumn1 = Season03.valueOf("AUTUMN");
        System.out.println("autumn1=" + autumn1);
        System.out.println(autumn == autumn1);

        //compareTo：比较两个枚举常量，比较的就是编号
        //1. 就是把 Season2.AUTUMN 枚举对象的编号 和 Season2.SUMMER枚举对象的编号比较
        //2. 看看结果
        /*
        public final int compareTo(E o) {
            //...
            return self.ordinal - other.ordinal;
        }
        Season2.AUTUMN的编号[2] - Season2.SUMMER的编号[3]
         */
        System.out.println(Season03.AUTUMN.compareTo(Season03.SUMMER));

    }
}
```

### enum 实现接口 

1) 使用 enum 关键字后，就不能再继承其它类了，因为 enum 会隐式继承 Enum，而 Java 是单继承机制。 
2) 枚举类和普通类一样，可以实现接口。

```java
public class EnumDetail {
    public static void main(String[] args) {
        Music.CLASSICMUSIC.playing();
    }
}
class A {

}

//1.使用enum关键字后，就不能再继承其它类了，因为enum会隐式继承Enum，而Java是单继承机制
//enum Season3 extends A {
//
//}
//2.enum实现的枚举类，仍然是一个类，所以还是可以实现接口的.
interface IPlaying {
    public void playing();
}
enum Music implements IPlaying {
    CLASSICMUSIC;
    @Override
    public void playing() {
        System.out.println("播放好听的音乐...");
    }
}
```



## 注解

1) 注解(Annotation)也被称为元数据(Metadata)，用于修饰解释 包、类、方法、属性、构造器、局部变量等数据信息。 
2) 和注释一样，注解不影响程序逻辑，但注解可以被编译或运行，相当于嵌入在代码中的补充信息。 
3)  在 JavaSE 中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在 JavaEE 中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替 java EE 旧版中所遗留的繁冗代码和 XML 配置等。

### 基本的 Annotation 介绍

使用 Annotation 时要在其前面增加 @ 符号, 并把该 Annotation 当成一个修饰符使用。用于修饰它支持的程序元素 三个基本的 Annotation: 

1) @Override: 限定某个方法，是重写父类方法, 该注解只能用于方法 
2) @Deprecated: 用于表示某个程序元素(类, 方法等)已过时 
3) @SuppressWarnings: 抑制编译器警告

```java
public class Override_ {
    public static void main(String[] args) {

    }
}
class Father{//父类

    public void fly(){
        int i = 0;
        System.out.println("Father fly...");
    }
    public void say(){}

}

class Son extends Father {//子类
    //1. @Override 注解放在fly方法上，表示子类的fly方法时重写了父类的fly
    //2. 这里如果没有写 @Override 还是重写了父类fly
    //3. 如果你写了@Override注解，编译器就会去检查该方法是否真的重写了父类的
    //   方法，如果的确重写了，则编译通过，如果没有构成重写，则编译错误
    //4. 看看 @Override的定义
    //   解读： 如果发现 @interface 表示一个 注解类
    /*
        @Target(ElementType.METHOD)
        @Retention(RetentionPolicy.SOURCE)
        public @interface Override {
        }
     */
    @Override   //说明
    public void fly() {
        System.out.println("Son fly....");
    }
    @Override
    public void say() {}
}
```

```java
public class Deprecated_ {
    public static void main(String[] args) {
        A a = new A();
    }
}
//1. @Deprecated 修饰某个元素，表示该元素已经过时
//2. 即不推荐使用，但是还可以使用。
//3. 可以修饰方法，类，字段, 包, 参数 等
//4. @Deprecated 可以做版本升级过渡使用
/*
 @Documented
 @Retention(RetentionPolicy.RUNTIME)
 @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
 public @interface Deprecated {}
 */
@Deprecated
class A{
    public int n1 =10;
    public void hi(){

    }
}
```

```javascript
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings({"rawtypes", "unchecked", "unused"})
public class SuppressWarnings_ {

    //1. 当我们不希望看到这些警告的时候，可以使用 SuppressWarnings注解来抑制警告信息
    //2. 在{""} 中，可以写入你希望抑制(不显示)警告信息
    //3. 可以指定的警告类型有
    //          all，抑制所有警告
    //          boxing，抑制与封装/拆装作业相关的警告
    //        //cast，抑制与强制转型作业相关的警告
    //        //dep-ann，抑制与淘汰注释相关的警告
    //        //deprecation，抑制与淘汰的相关警告
    //        //fallthrough，抑制与switch陈述式中遗漏break相关的警告
    //        //finally，抑制与未传回finally区块相关的警告
    //        //hiding，抑制与隐藏变数的区域变数相关的警告
    //        //incomplete-switch，抑制与switch陈述式(enum case)中遗漏项目相关的警告
    //        //javadoc，抑制与javadoc相关的警告
    //        //nls，抑制与非nls字串文字相关的警告
    //        //null，抑制与空值分析相关的警告
    //        //rawtypes，抑制与使用raw类型相关的警告
    //        //resource，抑制与使用Closeable类型的资源相关的警告
    //        //restriction，抑制与使用不建议或禁止参照相关的警告
    //        //serial，抑制与可序列化的类别遗漏serialVersionUID栏位相关的警告
    //        //static-access，抑制与静态存取不正确相关的警告
    //        //static-method，抑制与可能宣告为static的方法相关的警告
    //        //super，抑制与置换方法相关但不含super呼叫的警告
    //        //synthetic-access，抑制与内部类别的存取未最佳化相关的警告
    //        //sync-override，抑制因为置换同步方法而遗漏同步化的警告
    //        //unchecked，抑制与未检查的作业相关的警告
    //        //unqualified-field-access，抑制与栏位存取不合格相关的警告
    //        //unused，抑制与未用的程式码及停用的程式码相关的警告

    //4. 关于SuppressWarnings 作用范围是和你放置的位置相关
    //   比如 @SuppressWarnings放置在 main方法，那么抑制警告的范围就是 main
    //   通常我们可以放置具体的语句, 方法, 类.
    //5.  看看 @SuppressWarnings 源码
    //(1) 放置的位置就是 TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE
    //(2) 该注解类有数组 String[] values() 设置一个数组比如 {"rawtypes", "unchecked", "unused"}
    /*
        @Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
            @Retention(RetentionPolicy.SOURCE)
            public @interface SuppressWarnings {

                String[] value();
        }
     */
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("jack");
        list.add("tom");
        list.add("mary");
        int i;
        System.out.println(list.get(1));

    }

    public void f1() {
//        @SuppressWarnings({"rawtypes"})
        List list = new ArrayList();


        list.add("jack");
        list.add("tom");
        list.add("mary");
//        @SuppressWarnings({"unused"})
        int i;
        System.out.println(list.get(1));
    }
}
```

### 元注解

1) Retention //指定注解的作用范围，三种 SOURCE,CLASS,RUNTIME 
2) Target // 指定注解可以在哪些地方使用 
3) Documented //指定该注解是否会在 javadoc 体现
4)  Inherited //子类会继承父类注解

@Retention 的三种值 

1）RetentionPolicy.SOURCE: 编译器使用后，直接丢弃这种策略的注解。

2）RetentionPolicy.CLASS: 编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 不会保留注解。

3）RetentionPolicy.RUNTIME:编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 会保留注解. 程序可以通过反射获取该注解。



# 异常

*基本概念*
Java语言中，将程序执行中发生的不正常情况称为“异常”。(开发过程中的语法错误和逻辑错误不是异常

执行过程中所发生的异常事件可分为*两大类*

1) Error(错误)：Java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重情况。比如：StackOverflowError[栈溢出]和OOM(out of memory). Error是严重错误，程序会崩溃。
2) Exception：其它因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。例如空指针访问，试图读取不存在的文件，网络连接中断等等，Exception 分为两大类：运行时异常[程序运行时，发生的异常]和编译时异常[编程时，编译器检查出的异常]。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207132125956.png)

## 运行时异常

常见的运行时异常包括 

1) NullPointerException 空指针异常 
2) ArithmeticException 数学运算异常 
3) ArrayIndexOutOfBoundsException 数组下标越界异常
4) ClassCastException 类型转换异常 
5)  NumberFormatException 数字格式不正确异常

## 编译时异常

编译异常是指在编译期间，就必须处理的异常，否则代码不能通过编译。

SQLException //操作数据库时，查询表可能发生异常

IOException //操作文件时，发生的异常

FileNotFoundException //当操作一个不存在的文件时，发生异常

ClassNotFoundException //加载类，而该类不存在时，异常

EOFException//操作文件，到文件未尾，发生异常

IllegalArguementException //参数异常

## 异常处理

异常处理就是当异常发生时，对异常处理的方式。

### try-catch-finally

程序员在代码中捕获发生的异常，自行处理。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207132022327.png)

```java
public class TryCatch01 {
    public static void main(String[] args) {
        //ctrl + atl + t
        //1. 如果异常发生了，则异常发生后面的代码不会执行，直接进入到 catch 块
        //2. 如果异常没有发生，则顺序执行 try 的代码块，不会进入到 catch
        //3. 如果希望不管是否发生异常，都执行某段代码(比如关闭连接，释放资源等)则使用如下代码- finally
        try {
            String str = "JAVA学习";
            int a = Integer.parseInt(str);
            System.out.println("数字：" + a);
        } catch (NumberFormatException e) {
            System.out.println("异常信息=" + e.getMessage());
        } finally {
            System.out.println("finally 代码块被执行...");
        }
        System.out.println("程序继续...");
    }

}
```

可以有多个catch语句，捕获不同的异常(进行不同的业务处理)，要求父类异常在后，子类异常在前，比如(Exception在后，NullPointerException在前)，如果发生异常，只会匹配一个catch。

```java
public class TryCatch02 {
    public static void main(String[] args) {
        //1.如果try代码块有可能有多个异常
        //2.可以使用多个catch 分别捕获不同的异常，相应处理
        //3.要求子类异常写在前面，父类异常写在后面
        try {
            Person person = new Person();
            person = null;
            System.out.println(person.getName());//NullPointerException
            int n1 = 10;
            int n2 = 0;
            int res = n1 / n2;//ArithmeticException
        } catch (NullPointerException e) {
            System.out.println("空指针异常=" + e.getMessage());
        } catch (ArithmeticException e) {
            System.out.println("算术异常=" + e.getMessage());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        } finally {
        }
    }
}

class Person {
    private String name = "jack";

    public String getName() {
        return name;
    }
}
```

```java
public class TryCatchExercise03 {
}

class ExceptionExe01 {
    public static int method() {
        int i = 1;//i = 1
        try {
            i++;// i=2
            String[] names = new String[3];
            if (names[1].equals("tom")) { //空指针
                System.out.println(names[1]);
            } else {
                names[3] = "hspedu";
            }
            return 1;
        } catch (ArrayIndexOutOfBoundsException e) {
            return 2;
        } catch (NullPointerException e) {
            return ++i;  // i = 3 => 保存临时变量 temp = 3;
        } finally {
            ++i; //i = 4
            System.out.println("i=" + i);// i = 4
        }
    }

    public static void main(String[] args) {
        System.out.println(method());// 3
    }
}
```

> catch可以省略，try的形式有三种：
>
> try-catch
>
> try-finally
>
> try-catch-finally
>
> 但**catch和finally语句不能同时省略！**



### throws

将发生的异常抛出，交给调用者（方法）来处理，最顶级的处理者就是JVM。

1)如果一个方法(中的语句执行时)可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出异常，表明该方法将不对这些异常进行处理，而由该方法的调用者负责处理。
2)在方法声明中用throws语句可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是它的父类。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207132024005.png)



```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class Throws01 {
    public static void main(String[] args) {
        f2();
    }

    public static void f2() /*throws ArithmeticException*/ {
        //1.对于编译异常，程序中必须处理，比如 try-catch 或者 throws
        //2.对于运行时异常，程序中如果没有处理，默认就是throws的方式处理

        int n1 = 10;
        int n2 = 0;
        double res = n1 / n2;
    }

    public static void f1() throws FileNotFoundException {
        //这里大家思考问题 调用f3() 报错
        //1. 因为f3() 方法抛出的是一个编译异常
        //2. 即这时，就要f1() 必须处理这个编译异常
        //3. 在f1() 中，要么 try-catch-finally ,或者继续throws 这个编译异常
        f3(); // 抛出异常
    }

    public static void f3() throws FileNotFoundException {
        FileInputStream fis = new FileInputStream("d://aa.txt");
    }

    public static void f4() {
        //1. 在f4()中调用方法f5() 是OK
        //2. 原因是f5() 抛出的是运行异常
        //3. 而java中，并不要求程序员显示处理,因为有默认处理机制
        f5();
    }

    public static void f5() throws ArithmeticException {

    }
}

class Father { //父类
    public void method() throws RuntimeException {
    }
}

class Son extends Father {//子类

    //3. 子类重写父类的方法时，对抛出异常的规定:子类重写的方法，
    //   所抛出的异常类型要么和父类抛出的异常一致，要么为父类抛出的异常类型的子类型
    //4. 在throws 过程中，如果有方法 try-catch , 就相当于处理异常，就可以不必throws
    @Override
    public void method() throws ArithmeticException {
    }
}

```

## 自定义异常

```java
public class CustomException {
    public static void main(String[] args) /*throws AgeException*/ {

        int age = 180;
        //要求范围在 18 – 120 之间，否则抛出一个自定义异常
        if(!(age >= 18 && age <= 120)) {
            //这里我们可以通过构造器，设置信息
            throw new AgeException("年龄需要在 18~120之间");
        }
        System.out.println("你的年龄范围正确.");
    }
}
//自定义一个异常
//1. 一般情况下，我们自定义异常是继承 RuntimeException
//2. 即把自定义异常做成 运行时异常，好处时，我们可以使用默认的处理机制
//3. 即比较方便
class AgeException extends RuntimeException {
    public AgeException(String message) {//构造器
        super(message);
    }
}
```



# 常用类

## 包装类

1) 针对八种基本数据类型相应的引用类型—包装类 

2) 有了类的特点，就可以调用类中的方法。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207141750241.png)

```java
public class Wrapper01 {
    public static void main(String[] args) {

        //包装类(Integer)->String
        Integer i = 100;//自动装箱
        //方式 1
        String str1 = i + "";
        //方式 2
        String str2 = i.toString();
        //方式 3
        String str3 = String.valueOf(i);
        //String -> 包装类(Integer)
        String str4 = "12345";
        Integer i2 = Integer.parseInt(str4);//使用到自动装箱
        Integer i3 = new Integer(str4);//构造器
        System.out.println("ok~~");
    }
}
```

```java
public class WrapperMethod {
    public static void main(String[] args) {
        System.out.println(Integer.MIN_VALUE); //返回最小值
        System.out.println(Integer.MAX_VALUE);//返回最大值
        System.out.println(Character.isDigit('a'));//判断是不是数字
        System.out.println(Character.isLetter('a'));//判断是不是字母
        System.out.println(Character.isUpperCase('a'));//判断是不是大写
        System.out.println(Character.isLowerCase('a'));//判断是不是小写
        System.out.println(Character.isWhitespace('a'));//判断是不是空格
        System.out.println(Character.toUpperCase('a'));//转成大写
        System.out.println(Character.toLowerCase('A'));//转成小写
    }
}
```

```java
public class Wrapper02 {
    public static void main(String[] args) {
        //false
        Integer i = new Integer(1);
        Integer j = new Integer(1);
        System.out.println(i == j);
        
        /*
        如果i不在-128~127内，就new Integer()
        public static Integer valueOf(int i) {
            if (i >= IntegerCache.low && i <= IntegerCache.high)
                return IntegerCache.cache[i + (-IntegerCache.low)];
            return new Integer(i);
    }
         */
        //true
        Integer m = 1;
        Integer n = 1;
        System.out.println(m == n);
        //false
        Integer x = 128;
        Integer y = 128;
        System.out.println(x == y);
    }
}

//只要有基本数据类型，判断的是值是否相同
```

## String

两种创建 String 对象的区别

方式一:直接赋值String s = "Java";

方式二:调用构造器String s2 = new String("Java");

1．方式一:先从常量池查看是否有"Java"数据空间，如果有，直接指向；如果没有则重新创建，然后指向。s最终指向的是常量池的空间地址。

2.方式二:先在堆中创建空间，里面维护了value属性，指向常量池的Java空间。如果常量池没有""Java""，重新创建，如果有，直接通过value指向。最终指向的是堆中的空间地址。

```java
public class String01 {
    public static void main(String[] args) {
        String a = "study";
        String b = new String("study");
        System.out.println(a.equals(b));//T
        System.out.println(a==b);//F
        //当调用intern方法时，如果池已经包含等于此String对象的字符串，则返回来自池的字符串。
        // 否则，此String对象将添加到池中，并返回对此String对象的引用。
        //b.intern()方法最终返回的是常量池的地址。
        System.out.println(a==b.intern());//T
        System.out.println(b==b.intern());//F
    }
}
```

```java
public class String01 {
    public static void main(String[] args) {
        String a = "study";
        String b = new String("study");
        System.out.println(a.equals(b));//T
        System.out.println(a == b);//F
        //当调用intern方法时，如果池已经包含等于此String对象的字符串，则返回来自池的字符串。
        // 否则，此String对象将添加到池中，并返回对此String对象的引用。
        //b.intern()方法最终返回的是常量池的地址。
        System.out.println(a == b.intern());//T
        System.out.println(b == b.intern());//F

        String s = "good " + "evening"; //创建一个对象

        String s1 = "haha";
        s1 = "hello";
        String s2 = "world";
        // s3指向堆中对象(String) value[] ->池中"helloworld"
        String s3 = s1 + s2;//一共有3个对象
        //String c1 = "ab"+"cd";常量相加，看的是池
        //String c2 = a+b;变量相加，是在堆中

        Test01 ex = new Test01();
        ex.change(ex.str,ex.ch);
        System.out.println(ex.str+"and"); //helloand
        System.out.println(ex.ch);//hava

    }
}

class Test01{
    String str = new String("hello");
    final char[] ch = {'j','a','v','a'};
    public void change(String str,char[] ch){
        str = "java";
        System.out.println(str);//java
        ch[0]='h';
    }
}
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207151030970.png)



### String常用方法

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207151104227.png)



```JAVA
    public static void main(String[] args) {
        //1. equals  比较内容是否相同，区分大小写
        String str1 = "hello";
        String str2 = "Hello";
        System.out.println(str1.equals(str2));//

        // 2.equalsIgnoreCase 忽略大小写的判断内容是否相等
        String username = "johN";
        if ("john".equalsIgnoreCase(username)) {
            System.out.println("Success!");
        } else {
            System.out.println("Failure!");
        }
        // 3.length 获取字符的个数，字符串的长度
        System.out.println("早上好".length());
        // 4.indexOf 获取字符在字符串对象中第一次出现的索引，索引从0开始，如果找不到，返回-1
        String s1 = "wer@terwe@g";
        int index = s1.indexOf('@');
        System.out.println(index);// 3
        System.out.println("weIndex=" + s1.indexOf("we"));//0
        // 5.lastIndexOf 获取字符在字符串中最后一次出现的索引，索引从0开始，如果找不到，返回-1
        s1 = "wer@terwe@g@";
        index = s1.lastIndexOf('@');
        System.out.println(index);//11
        System.out.println("ter的位置=" + s1.lastIndexOf("ter"));//4
        // 6.substring 截取指定范围的子串
        String name = "hello,张三";
        //下面name.substring(6) 从索引6开始截取后面所有的内容
        System.out.println(name.substring(6));//截取后面的字符
        //name.substring(0,5)表示从索引0开始截取，截取到索引 5-1=4位置
        System.out.println(name.substring(2,5));//llo

    }
}
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207151110711.png)

> 由于replaceAll方法的第一个参数是一个正则表达式，而"."在正则表达式中表示任何字符，所以会把前面字符串的所有字符都替换成"/"。如果想替换的只是"."，要写成"\\.". 

```java
public class String03 {
    public static void main(String[] args) {
        // 1.toUpperCase转换成大写
        String s = "heLLo";
        System.out.println(s.toUpperCase());//HELLO
        // 2.toLowerCase
        System.out.println(s.toLowerCase());//hello
        // 3.concat拼接字符串
        String s1 = "宝玉";
        s1 = s1.concat("林黛玉").concat("薛宝钗").concat("together");
        System.out.println(s1);//宝玉林黛玉薛宝钗together
        // 4.replace 替换字符串中的字符
        s1 = "宝玉 and 林黛玉 林黛玉 林黛玉";
        //在s1中，将 所有的 林黛玉 替换成薛宝钗
        // s1.replace() 方法执行后，返回的结果才是替换过的.
        // 注意对 s1没有任何影响
        String s11 = s1.replace("宝玉", "jack");
        System.out.println(s1);//宝玉 and 林黛玉 林黛玉 林黛玉
        System.out.println(s11);//jack and 林黛玉 林黛玉 林黛玉
        // 5.split 分割字符串, 对于某些分割字符，我们需要 转义比如 | \\等
        String poem = "锄禾日当午,汗滴禾下土,谁知盘中餐,粒粒皆辛苦";
        // 1. 以 , 为标准对 poem 进行分割 , 返回一个数组
        // 2. 在对字符串进行分割时，如果有特殊字符，需要加入 转义符 \
        String[] split = poem.split(",");
        poem = "E:\\aaa\\bbb";
        split = poem.split("\\\\");
        System.out.println("==分割后内容===");
        for (int i = 0; i < split.length; i++) {
            System.out.println(split[i]);
        }
        // 6.toCharArray 转换成字符数组
        s = "happy";
        char[] chs = s.toCharArray();
        for (int i = 0; i < chs.length; i++) {
            System.out.println(chs[i]);
        }
        // 7.compareTo 比较两个字符串的大小，如果前者大，
        // 则返回正数，后者大，则返回负数，如果相等，返回0
        // (1) 如果长度相同，并且每个字符也相同，就返回 0
        // (2) 如果长度相同或者不相同，但是在进行比较时，可以区分大小
        //     就返回 if (c1 != c2) {
        //                return c1 - c2;
        //            }
        // (3) 如果前面的部分都相同，就返回 str1.len - str2.len
        String a = "jcck";// = "jac"  // len =3
        String b = "jack";// len = 4
        System.out.println(a.compareTo(b)); // 返回值是 'c' - 'a' = 2的值
        // 8.format 格式字符串
        /* 占位符有:
         * %s 字符串 %c 字符 %d 整型 %.2f 浮点型
         *
         */
        String name = "john";
        int age = 10;
        double score = 56.857;
        char gender = '男';
        //将所有的信息都拼接在一个字符串.
        String info =
                "我的姓名是" + name + "年龄是" + age + ",成绩是" + score + "性别是" + gender + "。希望大家喜欢我！";

        System.out.println(info);


        //1. %s , %d , %.2f %c 称为占位符
        //2. 这些占位符由后面变量来替换
        //3. %s 表示后面由 字符串来替换
        //4. %d 是整数来替换
        //5. %.2f 表示使用小数来替换，替换后，只会保留小数点两位, 并且进行四舍五入的处理
        //6. %c 使用char 类型来替换
        String formatStr = "我的姓名是%s 年龄是%d，成绩是%.2f 性别是%c.希望大家喜欢我！";

        String info2 = String.format(formatStr, name, age, score, gender);

        System.out.println("info2=" + info2);
    }
}
```

### StringBuffer类

1) String保存的是字符串常量，里面的值不能更改，每次String类的更新实际上就是更改地址，效率较低//private final char valuell;
2) StringBuffer保存的是字符串变量，里面的值可以更改，每次StringBuffer的更新实际上可以更新内容，不用每次更新地址，效率较高//char[] value;//这个放在堆.

 StringBuffer和String的转换

```java
public class StringAndStringBuffer {
    public static void main(String[] args) {
//看 String——>StringBuffer
        String str = "hello tom";
//方式 1 使用构造器
//注意： 返回的才是 StringBuffer 对象，对 str 本身没有影响
        StringBuffer stringBuffer = new StringBuffer(str);
//方式 2 使用的是 append 方法
        StringBuffer stringBuffer1 = new StringBuffer();
        stringBuffer1 = stringBuffer1.append(str);
        
//看看 StringBuffer ->String
        StringBuffer stringBuffer3 = new StringBuffer("Java学习");
//方式 1 使用 StringBuffer 提供的 toString 方法
        String s = stringBuffer3.toString();
//方式 2: 使用构造器来搞定
        String s1 = new String(stringBuffer3);
    }
}
```



### StringBuilder类

1)一个可变的字符序列。此类提供一个与StringBuffer 兼容的API，但不保证同步(StringBuilder不是线程安全)。该类被设计用作 StringBuffer的一个简易替换，用在字符串缓冲区被单个线程使用的时候。如果可能，建议优先采用该类，因为在大多数实现中，它比 StringBuffer要快。

2)在 StringBuilder 上的主要操作是append和 insert方法，可重载这些方法，以接受任意类型的数据。

### 比较

1) StringBuilder和 StringBuffer非常类似，均代表可变的字符序列，而且方法也一样
2) String:不可变字符序列,效率低,但是复用率高。
3) StringBuffer:可变字符序列、效率较高(增删)、线程安全,看源码
3) StringBuilder:可变字符序列、效率最高、线程不安全
5) String使用注意说明:
string s="a";1/创建了一个字符串
s += "b";//实际上原来的"a"字符串对象已经丢弃了，现在又产生了一个字符串s+"b”(也就是"ab")。如果多次执行这些改变串内容的操作，会导致大量副本字符串对象存留在内存中，降低效率。如果这样的操作放到循环中，会极大影响程序的性能=>结论:如果我们对String 做大量修改,不要使用String

```java
public class CompareSSS {
    public static void main(String[] args) {

        long startTime = 0L;
        long endTime = 0L;
        StringBuffer buffer = new StringBuffer("");

        startTime = System.currentTimeMillis();
        for (int i = 0; i < 80000; i++) {//StringBuffer 拼接 20000次
            buffer.append(String.valueOf(i));
        }
        endTime = System.currentTimeMillis();
        System.out.println("StringBuffer的执行时间：" + (endTime - startTime));


        StringBuilder builder = new StringBuilder("");
        startTime = System.currentTimeMillis();
        for (int i = 0; i < 80000; i++) {//StringBuilder 拼接 20000次
            builder.append(String.valueOf(i));
        }
        endTime = System.currentTimeMillis();
        System.out.println("StringBuilder的执行时间：" + (endTime - startTime));


        String text = "";
        startTime = System.currentTimeMillis();
        for (int i = 0; i < 80000; i++) {//String 拼接 20000
            text = text + i;
        }
        endTime = System.currentTimeMillis();
        System.out.println("String的执行时间：" + (endTime - startTime));

    }
}
```

使用的原则，结论:
1．如果字符串存在大量的修改操作，一般使用StringBuffer 或StringBuilder

2．如果字符串存在大量的修改操作，并在单线程的情况，使用 StringBuilder

3．如果字符串存在大量的修改操作，并在多线程的情况，使用 StringBuffer

4．如果我们字符串很少修改，被多个对象引用，使用String，比如配置信息等StringBuilder的方法使用和StringBuffer 一样，不再说.



## Math类

Math 类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数。

```java
public class MathMethod {
    public static void main(String[] args) {
//看看 Math 常用的方法(静态方法)
//1.abs 绝对值
        int abs = Math.abs(-9);
        System.out.println(abs);//9

//2.pow 求幂
        double pow = Math.pow(2, 4);//2 的 4 次方
        System.out.println(pow);//16
//3.ceil 向上取整,返回>=该参数的最小整数(转成 double);
        double ceil = Math.ceil(3.9);
        System.out.println(ceil);//4.0
//4.floor 向下取整，返回<=该参数的最大整数(转成 double)
        double floor = Math.floor(4.001);
        System.out.println(floor);//4.0
//5.round 四舍五入 Math.floor(该参数+0.5)
        long round = Math.round(5.51);
        System.out.println(round);//6
//6.sqrt 求开方
        double sqrt = Math.sqrt(9.0);
        System.out.println(sqrt);//3.0
//7.random 求随机数
// random 返回的是 0 <= x < 1 之间的一个随机小数
// 思考：请写出获取 a-b 之间的一个随机整数,a,b 均为整数 ，比如 a = 2, b=7
// 即返回一个数 x 2 <= x <= 7
        for (int i = 0; i < 10; i++) {
            System.out.println((int) (2 + Math.random() * (7 - 2 + 1)));
        }
//max , min 返回最大值和最小值
        int min = Math.min(1, 9);
        int max = Math.max(45, 90);
        System.out.println("min=" + min);
        System.out.println("max=" + max);
    }
}
```



## Arrays类

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Arrays01 {
    public static void main(String[] args) {
        //1.toString   遍历数组
        Integer[] integers = {1, 20, 90};
        System.out.println(Arrays.toString(integers));

        //2 sort排序方法的使用
        Integer arr[] = {1, -1, 7, 0, 89};
        //默认排序方法
        //从小到大
        Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));

        //定制排序方法
        //实现从大到小
        Arrays.sort(arr, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        System.out.println(Arrays.toString(arr));

        //3 binarySearch 通过二分搜索法查找，要求从小到大有序
        //如果数组中不存在该元素，就返回 return -(low +1);  //low 为该元素应该在的位置
        Integer[] arr2 = {1, 2, 90, 123, 567};
        int index = Arrays.binarySearch(arr2, 88);//
        System.out.println(index);

        //4 copyOf 数组元素的复制

        //1. 从 arr 数组中，拷贝 arr.length 个元素到 newArr 数组中
        //2. 如果拷贝的长度 > arr.length 就在新数组的后面 增加 null
        //3. 如果拷贝长度 < 0 就抛出异常 NegativeArraySizeException
        //4. 该方法的底层使用的是 System.arraycopy()
        Integer[] newArr = Arrays.copyOf(arr2, arr2.length);
        System.out.println("==拷贝执行完毕后==");
        System.out.println(Arrays.toString(newArr));

        //5 fill 数组元素的填充
        Integer[] num = new Integer[]{9, 3, 2};
        //使用 99 去填充 num 数组，可以理解成是替换原理的元素
        Arrays.fill(num, 99);
        System.out.println("==num 数组填充后==");
        System.out.println(Arrays.toString(num));

        //6 equals 比较两个数组元素内容是否完全一致
        Integer[] arr3 = {1, 2, 90, 123};
        //1. 如果 arr 和 arr2 数组的元素一样，则方法 true;
        //2. 如果不是完全一样，就返回 false
        boolean equals = Arrays.equals(arr, arr3);
        System.out.println("equals=" + equals);

        //7 asList 将一组值，转换成 list

        //1. asList 方法，会将 (2,3,4,5,6,1)数据转成一个 List 集合
        //2. 返回的 asList 编译类型 List(接口)
        //3. asList 运行类型 java.util.Arrays#ArrayList, 是 Arrays 类的
        // 静态内部类 private static class ArrayList<E> extends AbstractList<E>
        // implements RandomAccess, java.io.Serializable
        List asList = Arrays.asList(2, 3, 4, 5, 6, 1);

        System.out.println("asList=" + asList);
        System.out.println("asList 的运行类型" + asList.getClass());

    }
}
```

```java
package commonClasses.arrays_;

import java.util.Arrays;
import java.util.Comparator;

public class ArrayExercise {
    public static void main(String[] args) {
        Book[] books = new Book[4];
        books[0] = new Book("红楼梦", 100);
        books[1] = new Book("金瓶梅新", 90);
        books[2] = new Book("青年文摘 20 年", 5);
        books[3] = new Book("java 从入门到放弃~", 300);

        //按价格从低到高
//        Arrays.sort(books, new Comparator<Book>(){
//
//            @Override
//            public int compare(Book o1, Book o2) {
//                double priceCVal = o1.getPrice() -o2.getPrice();
//                if(priceCVal >0){
//                    return 1;
//                }else if(priceCVal < 0){
//                    return -1;
//                }else{
//                    return 0;
//                }
//            }
//        });
//        System.out.println(Arrays.toString(books));

        //按书名长度从长到短
        Arrays.sort(books, new Comparator<Book>() {
            @Override
            public int compare(Book o1, Book o2) {
                return o2.getName().length() - o1.getName().length();
            }
        });
        System.out.println(Arrays.toString(books));
    }
}

class Book {
    String name;
    double price;

    public Book() {
    }

    public Book(String name, double price) {
        this.name = name;
        this.price = price;
    }

    /**
     * 获取
     *
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     *
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     *
     * @return price
     */
    public double getPrice() {
        return price;
    }

    /**
     * 设置
     *
     * @param price
     */
    public void setPrice(double price) {
        this.price = price;
    }

    public String toString() {
        return "Book{name = " + name + ", price = " + price + "}";
    }
}
```



## System类

1)exit 退出当前程序

2)arraycopy:复制数组元素，比较适合底层调用，一般使用Arrays.copyOf完成复制数组.
		int[] src={1,2,33};
		int[] dest = new int[3];
		System.arraycopy(src,0, dest, 0.3);

3)currentTimeMillens:返回当前时间距离1970-1-1的毫秒数

4)gc:运行垃圾回收机制System.gco;



## 大数据类

### BigInteger类

Biglnteger适合保存比较大的整型

```java
import java.math.BigInteger;

public class BigInteger_ {
    public static void main(String[] args) {

        //当我们编程中，需要处理很大的整数，long 不够用
        //可以使用BigInteger的类来搞定
//        long l = 23788888899999999999999999999l;
//        System.out.println("l=" + l);

        BigInteger bigInteger = new BigInteger("23788888899999999999999999999");
        BigInteger bigInteger2 = new BigInteger("10099999999999999999999999999999999999999999999999999999999999999999999999999999999");
        System.out.println(bigInteger);
        //1. 在对 BigInteger 进行加减乘除的时候，需要使用对应的方法，不能直接进行 + - * /
        //2. 可以创建一个 要操作的 BigInteger 然后进行相应操作
        BigInteger add = bigInteger.add(bigInteger2);
        System.out.println(add);//加
        BigInteger subtract = bigInteger.subtract(bigInteger2);
        System.out.println(subtract);//减
        BigInteger multiply = bigInteger.multiply(bigInteger2);
        System.out.println(multiply);//乘
        BigInteger divide = bigInteger.divide(bigInteger2);
        System.out.println(divide);//除


    }
}
```



### BigDecimal

BigDecimal适合保存精度更高的浮点型(小数)

```java
import java.math.BigDecimal;

public class BigDecimal_ {
    public static void main(String[] args) {
        //当我们需要保存一个精度很高的数时，double 不够用
        //可以是 BigDecimal
        // double d = 1999.11111111111999999999999977788d;
        // System.out.println(d);
        BigDecimal bigDecimal = new BigDecimal("1999.11");
        BigDecimal bigDecimal2 = new BigDecimal("3");
        System.out.println(bigDecimal);
        //1. 如果对 BigDecimal 进行运算，比如加减乘除，需要使用对应的方法
        //2. 创建一个需要操作的 BigDecimal 然后调用相应的方法即可
        System.out.println(bigDecimal.add(bigDecimal2));
        System.out.println(bigDecimal.subtract(bigDecimal2));
        System.out.println(bigDecimal.multiply(bigDecimal2));
        //System.out.println(bigDecimal.divide(bigDecimal2));//可能抛出异常 ArithmeticException
        //在调用 divide 方法时，指定精度即可. BigDecimal.ROUND_CEILING
        //如果有无限循环小数，就会保留 分子 的精度
        System.out.println(bigDecimal.divide(bigDecimal2, BigDecimal.ROUND_CEILING));
    }
}
```



## 日期类

### 第一代日期类

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207151816780.png)

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Date01 {
    public static void main(String[] args) throws ParseException {
        //1. 获取当前系统时间
        //2. 这里的Date 类是在java.util包
        //3. 默认输出的日期格式是国外的方式, 因此通常需要对格式进行转换
        Date d1 = new Date(); //获取当前系统时间
        System.out.println("当前日期=" + d1);
        Date d2 = new Date(9234567); //通过指定毫秒数得到时间
        System.out.println("d2=" + d2); //获取某个时间对应的毫秒数

        //1. 创建 SimpleDateFormat对象，可以指定相应的格式
        //2. 这里的格式使用的字母是规定好，不能乱写
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 hh:mm:ss E");
        String format = sdf.format(d1); // format:将日期转换成指定格式的字符串
        System.out.println("当前日期=" + format);

        //1. 可以把一个格式化的String 转成对应的 Date
        //2. 得到Date 仍然在输出时，还是按照国外的形式，如果希望指定格式输出，需要转换
        //3. 在把String -> Date ， 使用的 sdf 格式需要和你给的String的格式一样，否则会抛出转换异常
        String s = "1996年01月01日 10:20:30 星期一";
        Date parse = sdf.parse(s);
        System.out.println("parse=" + sdf.format(parse));
    }
}
```

### 第二代日期类

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207151940303.png)



```java
import java.util.Calendar;

public class Calender_ {
    public static void main(String[] args) {
        //1. Calendar 是一个抽象类， 并且构造器是 private
        //2. 可以通过 getInstance() 来获取实例
        //3. 提供大量的方法和字段提供给程序员
        //4. Calendar 没有提供对应的格式化的类，因此需要程序员自己组合来输出(灵活)
        //5. 如果我们需要按照 24 小时进制来获取时间， Calendar.HOUR ==改成=> Calendar.HOUR_OF_DAY
        Calendar c = Calendar.getInstance(); //创建日历类对象//比较简单，自由
        System.out.println("c=" + c);
        //2.获取日历对象的某个日历字段
        System.out.println("年：" + c.get(Calendar.YEAR));
        // 这里为什么要 + 1, 因为 Calendar 返回月时候，是按照 0 开始编号
        System.out.println("月：" + (c.get(Calendar.MONTH) + 1));
        System.out.println("日：" + c.get(Calendar.DAY_OF_MONTH));
        System.out.println("小时：" + c.get(Calendar.HOUR));
        System.out.println("分钟：" + c.get(Calendar.MINUTE));
        System.out.println("秒：" + c.get(Calendar.SECOND));
        //Calender 没有专门的格式化方法，所以需要程序员自己来组合显示
        System.out.println(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-" +
                c.get(Calendar.DAY_OF_MONTH) +
                " " + c.get(Calendar.HOUR_OF_DAY) + ":" + c.get(Calendar.MINUTE) + ":" + c.get(Calendar.SECOND) );
    }
}
```

### 第三代日期类

```java
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

public class LocalDate_ {
    public static void main(String[] args) {
        //第三代日期类
        //1. 使用 now() 返回表示当前日期时间的 对象
        LocalDateTime ldt = LocalDateTime.now(); //LocalDate.now();//LocalTime.now()
        System.out.println(ldt);
        //2. 使用 DateTimeFormatter 对象来进行格式化
        // 创建 DateTimeFormatter 对象
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String format = dateTimeFormatter.format(ldt);
        System.out.println("格式化的日期=" + format);
        System.out.println("年=" + ldt.getYear());
        System.out.println("月=" + ldt.getMonth());
        System.out.println("月=" + ldt.getMonthValue());
        System.out.println("日=" + ldt.getDayOfMonth());
        System.out.println("时=" + ldt.getHour());
        System.out.println("分=" + ldt.getMinute());
        System.out.println("秒=" + ldt.getSecond());
        LocalDate now = LocalDate.now(); //可以获取年月日
        LocalTime now2 = LocalTime.now();//获取到时分秒
        //提供 plus 和 minus 方法可以对当前时间进行加或者减
        //看看 890 天后，是什么时候 把 年月日-时分秒
        LocalDateTime localDateTime = ldt.plusDays(890);
        System.out.println("890 天后=" + dateTimeFormatter.format(localDateTime));
        //看看在 3456 分钟前是什么时候，把 年月日-时分秒输出
        LocalDateTime localDateTime2 = ldt.minusMinutes(3456);
        System.out.println("3456 分钟前 日期=" + dateTimeFormatter.format(localDateTime2));
    }
}
```



## 练习题

```java
package commonClasses.homework;

public class Homework01 {
     /**
     * (1) 将字符串中指定部分进行反转。比如将"abcdef"反转为"aedcbf"
     * (2) 编写方法 public static String reverse(String  str, int start , int end)
     */
    public static void main(String[] args) {
        String str = "abcdef";
        System.out.println("===交换前===");
        System.out.println(str);
        try {
            str = reverse(str, 1, 4);
        } catch (Exception e) {
            System.out.println(e.getMessage());
            return;
        }
        System.out.println("===交换后===");
        System.out.println(str);
    }

    public static String reverse(String str, int start, int end) {
        //对输入的参数做一个验证
        //(1) 写出正确的情况
        //(2) 然后取反即可
        if (!(str != null && start >= 0 && end > start && end < str.length())) {
            throw new RuntimeException("参数不正确");
        }
        char[] ch = str.toCharArray();
        char temp;
        while (start < end) {
            temp = ch[start];
            ch[start] = ch[end];
            ch[end] = temp;
            start++;
            end--;
        }
        return new String(ch);
    }
}

```

```java
public class Homework02 {
    /**
     * 输入用户名、密码、邮箱，如果信息录入正确，则提示注册成功，否则生成异常对象
     * 要求：
     * (1) 用户名长度为2或3或4
     * (2) 密码的长度为6，要求全是数字  isDigital
     * (3) 邮箱中包含@和.   并且@在.的前面
     */
    public static void main(String[] args) {
        try {
            userRegister("fzy", "789565", "154@qq.com");
        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println("注册失败");
            return;
        }
        System.out.println("注册成功");
    }

    public static void userRegister(String name, String pwd, String email) {
        if (name.length() < 2 || name.length() > 4) {
            throw new RuntimeException("用户名长度必须为2-4");
        }
        if (!(pwd.length() == 6 && isDigital(pwd))) {
            throw new RuntimeException("密码要求长度为6,且全为数字");
        } else {
            //判断是否全是数字
        }
        if (!(email.indexOf('@') > 0 && email.indexOf('@') < email.indexOf('.'))) {
            throw new RuntimeException("邮箱格式不正确，");
        }
    }

    public static boolean isDigital(String str) {
        char[] ch = str.toCharArray();
        for (int i = 0; i < ch.length; i++) {
            if (ch[i] < '0' || ch[i] > '9') {
                return false;
            }
        }
        return true;
    }
}
```

```java
public class Homework03 {
    /**
     * 编写方法: 完成输出格式要求的字符串
     * 编写java程序，输入形式为： Feng zhui Yu的人名，以Yu,Feng .Z的形式打印
     * 出来 。其中.S是中间单词的首字母
     * 思路分析
     * (1) 对输入的字符串进行 分割split(" ")
     * (2) 对得到的String[] 进行格式化String.format（）
     * (3) 对输入的字符串进行校验即可
     */
    public static void main(String[] args) {
        printName("Feng zhui Yu");
    }

    public static void printName(String str) {
        if (str == null) {
            System.out.println("str is null");
            return;
        }
        String[] strArray = str.split(" ");
        if (strArray.length != 3) {
            System.out.println("格式不正确");
            return;
        }
        String infoStr = String.format("%s,%s .%c", strArray[2], strArray[0], strArray[1].toUpperCase().charAt(0));
        System.out.println(infoStr);
    }
}
```

```java
public class Homework04 {
    public static void main(String[] args) {
        String str = "abcHsp U 1234";
        countStr(str);
    }

    /**
     * 输入字符串，判断里面有多少个大写字母，多少个小写字母，多少个数字
     * 思路分析
     * (1) 遍历字符串，如果 char 在 '0'~'9' 就是一个数字
     * (2) 如果 char 在 'a'~'z' 就是一个小写字母
     * (3) 如果 char 在 'A'~'Z' 就是一个大写字母
     * (4) 使用三个变量来记录 统计结果
     */
    public static void countStr(String str) {
        if (str == null) {
            System.out.println("输入不能为 null");
            return;
        }
        int strLen = str.length();
        int numCount = 0;
        int lowerCount = 0;
        int upperCount = 0;
        int otherCount = 0;
        for (int i = 0; i < strLen; i++) {
            if(str.charAt(i) >= '0' && str.charAt(i) <= '9') {
                numCount++;
            } else if(str.charAt(i) >= 'a' && str.charAt(i) <= 'z') {
                lowerCount++;
            } else if(str.charAt(i) >= 'A' && str.charAt(i) <= 'Z') {
                upperCount++;
            } else {
                otherCount++;
            }
        }

        System.out.println("数字有 " + numCount);
        System.out.println("小写字母有 " + lowerCount);
        System.out.println("大写字母有 " + upperCount);
        System.out.println("其他字符有 " + otherCount);
    }
}
```



# 集合

单列集合

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207161333516.png)

双列集合

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207161334617.png)



## Collection接口

public interface Collection <E> extends lterable <E> 

1)collection实现子类可以存放多个元素，每个元素可以是Object

2)有些Collection的实现类，可以存放重复的元素，有些不可以

3)有些Collection的实现类，有些是有序的(List)，有些不是有序(Set)

4)Collection接口没有直接的实现子类,是通过它的子接口Set 和 List来实现的

- 常用方法

```java
import java.util.ArrayList;
import java.util.List;

public class CollectionMethod {
    public static void main(String[] args) {
        List list = new ArrayList();
//        add:添加单个元素
        list.add("jack");
        list.add(10);//list.add(new Integer(10))
        list.add(true);
        System.out.println("list=" + list);
//        remove:删除指定元素
        //list.remove(0);//删除第一个元素
        list.remove(true);//指定删除某个元素
        System.out.println("list=" + list);
//        contains:查找元素是否存在
        System.out.println(list.contains("jack"));//T
//        size:获取元素个数
        System.out.println(list.size());//2
//        isEmpty:判断是否为空
        System.out.println(list.isEmpty());//F
//        clear:清空
        list.clear();
        System.out.println("list=" + list);
//        addAll:添加多个元素
        ArrayList list2 = new ArrayList();
        list2.add("红楼梦");
        list2.add("三国演义");
        list.addAll(list2);
        System.out.println("list=" + list);
//        containsAll:查找多个元素是否都存在
        System.out.println(list.containsAll(list2));//T
//        removeAll：删除多个元素
        list.add("聊斋");
        list.removeAll(list2);
        System.out.println("list=" + list);//[聊斋]
//        说明：以ArrayList实现类来演示.

    }
}
```

- 遍历元素方式


1）Iterator迭代器

2）for循环增强

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class CollectionExercise {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add(new Dog("小黑", 3));
        list.add(new Dog("大黄", 100));
        list.add(new Dog("大壮", 8));
        System.out.println("使用Iterator");
        Iterator iterator = list.iterator();
        while (iterator.hasNext()) {
            Object next =  iterator.next();
            System.out.println(next);
        }
        System.out.println("使用增强for");
        for(Object dog : list) {
            System.out.println(dog);
        }

    }
}

/**
 * 创建 3 个 Dog {name, age} 对象，放入到 ArrayList 中，赋给 List 引用
 * 用迭代器和增强 for 循环两种方式来遍历
 * 重写 Dog 的 toString 方法， 输出 name 和 age
 */

class Dog {
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return "Dog{name = " + name + ", age = " + age + "}";
    }
}
```

## List接口

List 接口是 Collection接口的子接口
1) List集合类中元素有序(即添加顺序和取出顺序一致)、且可重复
2) List集合中的每个元素都有其对应的顺序索引，即支持索引。
1) List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根
据序号存取容器中的元素。

- 常用方法

  ```java
  import java.util.ArrayList;
  import java.util.List;
  
  public class ListMethod {
      public static void main(String[] args) {
          List list = new ArrayList();
          list.add("张三丰");
          list.add("贾宝玉");
          // void add(int index, Object ele):在 index 位置插入 ele 元素
          //在 index = 1 的位置插入一个对象
          list.add(1, "Java");
          System.out.println("list=" + list);
          // boolean addAll(int index, Collection eles):从 index 位置开始将 eles 中的所有元素添加进来
          List list2 = new ArrayList();
          list2.add("jack");
          list2.add("tom");
          list.addAll(1, list2);
          System.out.println("list=" + list);
          // Object get(int index):获取指定 index 位置的元素
          // int indexOf(Object obj):返回 obj 在集合中首次出现的位置
          System.out.println(list.indexOf("tom"));//2
          // int lastIndexOf(Object obj):返回 obj 在当前集合中末次出现的位置
          list.add("Java");
          System.out.println("list=" + list);
          System.out.println(list.lastIndexOf("Java"));
          // Object remove(int index):移除指定 index 位置的元素，并返回此元素
          list.remove(0);
          System.out.println("list=" + list);
          // Object set(int index, Object ele):设置指定 index 位置的元素为 ele , 相当于是替换. list.set(1, "玛丽");
          System.out.println("list=" + list);
          // List subList(int fromIndex, int toIndex):返回从 fromIndex 到 toIndex 位置的子集合
          // 注意返回的子集合 fromIndex <= subList < toIndex
          //subSet和subList都是返回元数据结构的一个视图.
          List returnlist = list.subList(0, 2);
          System.out.println("returnlist=" + returnlist);
      }
  }
  ```

### ArrayList

1)ArrayList中维护了一个Object类型的数组elementData
transient Object[ elementData;//transient表示瞬间，短暂的，表示该属性不会被序列化。

2)当创建ArrayList对象时，如果使用的是无参构造器，则初始elementData容量为0，第1次添加，则扩容elementData为10，如需要再次扩容，则扩容elementData为1.5倍。

3)如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如果需要扩容，
则直接扩容elementData为1.5倍。

### Vector

1) Vector类的定义说明
public class vector<E>extends AbstractList<E>
implements List<E>，RandomAccess,cloneable，Serializable
2) Vector底层也是一个对象数组，protected objectl[]elementData;
3) Vector是线程同步的，即线程安全，Vector类的操作方法带有synchronizedpublic synchronized E get(int index){
if (index >= elementCount)
throw new ArraylndexOutOfBoundsException(index);return elementData(index);
}
4) 在开发中,需要线程同步安全时,考虑使用Vector

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207161509127.png)



### LinkedList

1)LinkedList底层实现了双向链表和双端队列特点

2)可以添加任意元素(元素可以重复)，包括null

3)线程不安全,没有实现同步



- LinkedList的底层操作机制

1)LinkedList底层维护了一个双向链表.

2)LinkedList中维护了两个属性first和last分别指向首节点和尾节点

3)每个节点(Node对象)，里面又维护了prev、next、item三个属性，其中通过prev指向前一个,通过next指向后一个节点。最终实现双向链表

4)所以LinkedList的元素的添加和删除，不是通过数组完成的，相对来说效率较高。



## Set接口

同Collection的遍历方式一样，因为Set接口是Collection接口的子接口。

1.可以使用迭代器

2.增强for

3.不能使用索引的方式来获取

### HashSet

1）HashSet实现了Set接口

2）HashSet实际上是HashMap

3）可以存放null值，但是只能有一个null

4）HashSet不保证元素是有序的,取决于hash后，再确定索引的结果.(即，不保证存放元素的顺序和取出顺序一致)

5)不能有重复元泰/对象



- 分析HashSet的添加元素底层是如何实现(hash()+equals())

1. HashSet底层是 HashMap
2. 添加一个元素时，先得到hash值-会转成->索引值
1. 找到存储数据表table，看这个索引位置是否已经存放的有元素，如果没有，直接加入
。如果有，调用equals比较，如果相同，就放弃添加,如果不相同，则添加到最后
4. 在Java8中，如果一条链表的元素个数到达TREEIFY_THRESHOLD(就认是8)，并且table的大小>=MIN TREEIFY CAPACITY(默认64)。就会进行树化(红黑树)

- 扩容

1. HashSet底层是HashMap,第一次添加时,table数组扩容到16，临界值(threshold)是1加载因子(loadFactor)是0.75=12
2. 如果table数组使用到了临界值12,就会扩容到16*2=32,新的临界值就是32\*0.75 =24,依次类推
3. 在Java8中,如果一条链表的元素个数到达TREEIFY_THRESHOLD(默认是8).并且table的大小>=MIN TREEIFY CAPACITY(默认64).就会进行树化(红黑树)，否则仍然采用数组扩容机制

```java
import java.util.HashSet;
import java.util.Objects;

public class HashSet01 {
    public static void main(String[] args) {
        HashSet hs = new HashSet();
        hs.add(new Employee("tom", 12000, new MyDate(2000, 1, 1)));
        hs.add(new Employee("tom", 12000, new MyDate(2000, 1, 1)));//没有添加

        hs.add(new Employee("tom", 8000, new MyDate(2000, 1, 1)));//没有添加
        hs.add(new Employee("bob", 12000, new MyDate(2000, 1, 1)));
        hs.add(new Employee("tom", 12000, new MyDate(1999, 1, 1)));

        System.out.println(hs);

    }
}

/*
定义一个Employee类,该类包含:private成员属性name,sal,birthday(MyDate类型)，其中 birthday 为 MyDate类型(属性包括: year, month, day),要求;
1.创建Employee 放入HashSet中
2.当name和birthday的值相同时，认为是相同员工,不能添加到HashSet集合中
*/
class Employee {
    private String name;
    private double salary;
    MyDate birthday;

    public Employee() {
    }

    public Employee(String name, double salary, MyDate birthday) {
        this.name = name;
        this.salary = salary;
        this.birthday = birthday;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return Objects.equals(name, employee.name) && Objects.equals(birthday, employee.birthday);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, birthday);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public MyDate getBirthday() {
        return birthday;
    }

    public void setBirthday(MyDate birthday) {
        this.birthday = birthday;
    }

    public String toString() {
        return "\nEmployee{name = " + name + ", salary = " + salary + ", birthday = " + birthday + "}";
    }
}

class MyDate {
    private int year;
    private int month;
    private int day;

    public MyDate(int year, int month, int day) {
        this.year = year;
        this.month = month;
        this.day = day;
    }

    public int getYear() {
        return year;
    }

    public void setYear(int year) {
        this.year = year;
    }

    public int getMonth() {
        return month;
    }

    public void setMonth(int month) {
        this.month = month;
    }

    public int getDay() {
        return day;
    }

    public void setDay(int day) {
        this.day = day;
    }

    @Override
    public String toString() {
        return year + "-" + month + "-" + day;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        MyDate myDate = (MyDate) o;
        return year == myDate.year && month == myDate.month && day == myDate.day;
    }

    @Override
    public int hashCode() {
        return Objects.hash(year, month, day);
    }
}
```

### LinkedHashSet

1) LinkedHashSet是 HashSet的子类
2) LinkedHashSet底层是一个 LinkedHashMap，底层维护了一个数组+双向链表
3) LinkedHashSet根据元素的 hashCode值来决定元素的存储位置,同时使用链表维护元素的次序(图)，这使得元素看起来是以插入顺序保存的。
4) LinkedHashSet不允许添重复元素

### TreeSet

```java
import java.util.Comparator;
import java.util.TreeSet;

public class TreeSet_ {
    public static void main(String[] args) {
        //1. 当我们使用无参构造器，创建TreeSet时，仍然是无序的
        //2. 希望添加的元素，按照字符串大小来排序
        //3. 使用TreeSet 提供的一个构造器，可以传入一个比较器(匿名内部类)
        //   并指定排序规则
        TreeSet t = new TreeSet(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                //return ((String) o1).compareTo((String) o2);//(1)
                return ((String) o1).length() - ((String) o2).length();
            }
        });
        t.add("Jack");
        t.add("Bob");
        t.add("Hello");
        //t.add("Bob");//若内容相同，则不添加。//(1)
        t.add("Mary"); //若长度相同，则不添加。

        System.out.println(t);

    }
}
```



## Map接口

- JDK8的Map接口特点

1. Map与Collection并列存在。用于保存具有映射关系的数据:Key-Value
2. Map 中的key和value可以是任何引用类型的数据，会封装到HashMap$Node对象中
3. Map中的key不允许重复，当有相同的k时，会替换v
4. Map 中的value可以重复
5. Map 的key 可以为null, value也可以为null，注意key 为null,只能有一个，value为null ,可以多个
6. 常用String类作为Map的key
7. key 和value 之间存在单向一对一关系，即通过指定的key总能找到对应的value



- 常用方法

put:添加对象

remove:根据键删除映射关系

get：根据键获取值

size:获取元素个数

isEmpty:判断个数是否为 

clear:清除 k-v

containsKey:查找键是否存在



- 遍历方法

1)containsKey:查找键是否存在

2)keySet:获取所有的键

3)entrySet:获取所有关系k-v

4)values:获取所有的值

```java
import java.util.*;

public class MapExercise {
    public static void main(String[] args) {
        Person person1 = new Person(1, "John", 20000);
        Person person2 = new Person(2, "Bob", 15000);
        Person person3 = new Person(3, "Mary", 25000);
        HashMap hashMap = new HashMap();
        hashMap.put(person1.getId(), person1);
        hashMap.put(person2.getId(), person2);
        hashMap.put(person3.getId(), person3);
//      //遍历方法
        //第一组: 先取出 所有的 Key , 通过 Key 取出对应的 Value
        System.out.println("第一组");
        Set keySet = hashMap.keySet();
        //增强for
        for(Object key : keySet) {
            if(((Person)hashMap.get(key)).getSalary()>18000){
                System.out.println(hashMap.get(key));
            }
        }
        //迭代器
        Iterator iterator1 = keySet.iterator();
        while (iterator1.hasNext()) {
            Object next = iterator1.next();
            if(((Person)hashMap.get(next)).getSalary()>18000){
                System.out.println(hashMap.get(next));
            }
        }

        //第二组: 把所有的 values 取出
        System.out.println("第二组");
        Collection values = hashMap.values();
        //for
        for (Object value : values){
            if(((Person)value).getSalary()>18000){
                System.out.println(value);
            }
        }
        Iterator iterator2 = values.iterator();
        while (iterator2.hasNext()) {
            Object next =  iterator2.next();
            if(((Person)next).getSalary()>18000){
                System.out.println(next);
            }
        }

        //第三组: 通过 EntrySet 来获取 k-v
        System.out.println("第三组");
        Set entrySet = hashMap.entrySet();
        //for
        for(Object entry : entrySet){
            //将 entry 转成 Map.Entry
            Map.Entry m = (Map.Entry) entry;
            if(((Person)m.getValue()).getSalary()>18000){
                System.out.println(m.getValue());
            }
        }
        //迭代器
        Iterator iterator3 = entrySet.iterator();
        while (iterator3.hasNext()) {
            Map.Entry m = (Map.Entry) iterator3.next();
            if(((Person)m.getValue()).getSalary()>18000){
                System.out.println(m.getValue());
            }
        }

    }
}
class Person{
    private int id;
    private String name;
    private double salary;

    public Person() {
    }

    public Person(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public String toString() {
        return "Person{id = " + id + ", name = " + name + ", salary = " + salary + "}";
    }
}
```

### HashMap

1) Map接口的常用实现类:HashMap、Hashtable和Properties.
2) HashMap是 Map接口使用频率最高的实现类。
3) HashMap是以 key-val对的方式来存储数据(HashMap$Node类型)
4)  key不能重复，但是值可以重复,允许使用null键和null值。
5) 如果添加相同的key，则会覆盖原来的key-val ,等同于修改.(key不会替换，val会替换)
6) 与HashSet一样，不保证映射的顺序，因为底层是以hash表的方式来存储的.(jdk8的hashMap底层数组+链表+红黑树)
7) HashMap没有实现同步，因此是线程不安全的,方法没有做同步互斥的操作,没有
synchronized
8) 扩容机制与HashSet一致

### HashTable

- HashTable的基本介绍

1. 存放的元素是键值对:即K-V
2. hashtable的键和值都不能为null，否则会抛出NullPointerException
3. hashTable使用方法基本上和HashMap一样
4. hashTable是线程安全的(synchronized). hashMap是线程不安全的

```java
import java.util.Properties;

public class Properties_ {
    public static void main(String[] args) {

        //1. Properties 继承  Hashtable
        //2. 可以通过 k-v 存放数据，当然key 和 value 不能为 null
        //增加
        Properties properties = new Properties();
        //properties.put(null, "abc");//抛出 空指针异常
        //properties.put("abc", null); //抛出 空指针异常
        properties.put("john", 100);//k-v
        properties.put("lucy", 100);
        properties.put("lic", 100);
        properties.put("lic", 88);//如果有相同的key ， value被替换

        System.out.println("properties=" + properties);

        //通过k 获取对应值
        System.out.println(properties.get("lic"));//88

        //删除
        properties.remove("lic");
        System.out.println("properties=" + properties);

        //修改
        properties.put("john", "约翰");
        System.out.println("properties=" + properties);

    }
}
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207182054271.png)



### Properties

1. Properties类继承自Hashtable类并且实现了Map接口，也是使用一种键值对的形
式来保存数据。
2. 使用特点和Hashtable类似
1. Properties还可以用于从xxx.properties 文件中，加载数据到Properties类对象,
并进行读取和修改
4.  xxx.properties文件通常作为配置文件



### TreeMap

```java
import java.util.Comparator;
import java.util.TreeMap;

public class TreeMap_ {
    public static void main(String[] args) {
        TreeMap treeMap = new TreeMap(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                //按照传入的 k(String) 的大小进行排序
                //按照K(String) 的长度大小排序
                //return ((String) o2).compareTo((String) o1);
                return ((String) o2).length() - ((String) o1).length();
            }
        });
        treeMap.put("jack", "杰克");
        treeMap.put("tom", "汤姆");
        treeMap.put("kristina", "克瑞斯提诺");
        treeMap.put("smith", "斯密斯");
        treeMap.put("bob", "鲍勃");//加入不了，但"鲍勃"替换"汤姆"

        System.out.println("treemap=" + treeMap);
    }
}
```



## Collections工具类

1) Collections是一个操作 Set、List 和 Map等集合的工具类
2) Collections中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作

### 排序

1) reverse(List):反转List中元素的顺序
2) shuffle(List):对List集合元素进行随机排序
3) sort(List):根据元素的自然顺序对指定List集合元素按升序排序
4) sort(List,Comparator):根据指定的Comparator产生的顺序对 List集合元素进行排序
5) swap(List,int i，int j):将指定 list集合中的i处元素和j处元素进行交换

### 查找、替换

1) Object max(Collection):根据元素的自然顺序，返回给定集合中的最大元素
2) object max(Collection,Comparator):根据Comparator指定的顺序，返回给定集合中的最大元素
3) Object min(Collection)
4) Object min(Collection, Comparator)
5) int frequency(Collection，object):返回指定集合中指定元素的出现次数
5) void copy(List dest,List src):将src中的内容复制到dest中
7) boolean replaceAll(List list,Object oldval,Object newVal):使用新值替换List 对象的所有旧值

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Collections_ {
    public static void main(String[] args) {

        //创建ArrayList 集合，用于测试.
        List list = new ArrayList();
        list.add("tom");
        list.add("smith");
        list.add("king");
        list.add("milan");
        list.add("tom");


//        reverse(List)：反转 List 中元素的顺序
        Collections.reverse(list);
        System.out.println("list=" + list);
//        shuffle(List)：对 List 集合元素进行随机排序
//        for (int i = 0; i < 5; i++) {
//            Collections.shuffle(list);
//            System.out.println("list=" + list);
//        }

//        sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
        Collections.sort(list);
        System.out.println("自然排序后");
        System.out.println("list=" + list);
//        sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
        //我们希望按照 字符串的长度大小排序
        Collections.sort(list, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                //可以加入校验代码.
                return ((String) o2).length() - ((String) o1).length();
            }
        });
        System.out.println("字符串长度大小排序=" + list);
//        swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换

        //比如
        Collections.swap(list, 0, 1);
        System.out.println("交换后的情况");
        System.out.println("list=" + list);

        //Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
        System.out.println("自然顺序最大元素=" + Collections.max(list));
        //Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
        //比如，我们要返回长度最大的元素
        Object maxObject = Collections.max(list, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                return ((String) o1).length() - ((String) o2).length();
            }
        });
        System.out.println("长度最大的元素=" + maxObject);


        //Object min(Collection)
        //Object min(Collection，Comparator)
        //上面的两个方法，参考max即可

        //int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
        System.out.println("tom出现的次数=" + Collections.frequency(list, "tom"));

        //void copy(List dest,List src)：将src中的内容复制到dest中

        ArrayList dest = new ArrayList();
        //为了完成一个完整拷贝，我们需要先给dest 赋值，大小和list.size()一样
        for (int i = 0; i < list.size(); i++) {
            dest.add("");
        }
        //拷贝
        Collections.copy(dest, list);
        System.out.println("dest=" + dest);

        //boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值
        //如果list中，有tom 就替换成 汤姆
        Collections.replaceAll(list, "tom", "汤姆");
        System.out.println("list替换后=" + list);
        
    }
}
```

### 

## 小结

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207191116718.png)



## 练习题

```java
import java.util.ArrayList;

public class Homework01 {
    public static void main(String[] args) {
        ArrayList arrayList = new ArrayList();
        arrayList.add(new News("新冠确诊病例超千万，数百万印度教信徒赴恒河“圣浴”引民众担忧"));
        arrayList.add(new News("男子突然想起2个月前钓的鱼还在网兜里，捞起一看赶紧放生"));
        arrayList.add(new News("樱花树下你和我"));
        for (int i = arrayList.size() - 1; i >= 0; i--) {
            String title = ((News) (arrayList.get(i))).getTitle();
            if (title.length() <= 15) {
                System.out.println(title);
            }
            else{
                title = title.substring(0, 15);
                title += "...";
                System.out.println(title);
            }
        }
    }
}

/**
 * 按要求实现：
 * (1) 封装一个新闻类，包含标题和内容属性，提供get、set方法，重写toString方法，打印对象时只打印标题；
 * (2) 只提供一个带参数的构造器，实例化对象时，只初始化标题；并且实例化两个对象：
 * 新闻一：新冠确诊病例超千万，数百万印度教信徒赴恒河“圣浴”引民众担忧
 * 新闻二：男子突然想起2个月前钓的鱼还在网兜里，捞起一看赶紧放生
 * (3) 将新闻对象添加到ArrayList集合中，并且进行倒序遍历；
 * (4) 在遍历集合过程中，对新闻标题进行处理，超过15字的只保留前15个，然后在后边加“…”
 * (5) 在控制台打印遍历出经过处理的新闻标题；
 */

class News {
    private String title;
    private String description;

    public News(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    @Override
    public String toString() {
        return "News{" +
                "title='" + title + '\'' +
                '}';
    }
}
```

```java
import java.util.*;

public class Homework03 {
    public static void main(String[] args) {
        HashMap hashMap = new HashMap();
        hashMap.put("jack", 650);
        hashMap.put("tom", 1200);
        hashMap.put("smith", 2900);
        System.out.println(hashMap);

        hashMap.put("jack", 2600);
        System.out.println(hashMap);

        for (Object key : hashMap.keySet()) {
            hashMap.put(key, (Integer) hashMap.get(key) + 100);
        }

        System.out.println("=============遍历=============");
        Set entrySet = hashMap.entrySet();
        Iterator iterator = entrySet.iterator();
        while (iterator.hasNext()) {
            Map.Entry entry = (Map.Entry) iterator.next();
            System.out.println(entry.getKey() + "-" + entry.getValue());
        }

        System.out.println("====遍历所有的工资====");
        Collection values = hashMap.values();
        for (Object value : values) {
            System.out.println(value);
        }

    }
}
/**
 * 按要求完成下列任务
 * 1）使用HashMap类实例化一个Map类型的对象m，键（String）和值（int）分别用于存储员工的姓名和工资，
 * 存入数据如下： jack—650元；tom—1200元；smith——2900元；
 * 2）将jack的工资更改为2600元
 * 3）为所有员工工资加薪100元；
 * 4）遍历集合中所有的员工
 * 5）遍历集合中所有的工资
 */
```

```java
import java.util.HashSet;
import java.util.Objects;

public class Homework06 {
    public static void main(String[] args) {
        HashSet set = new HashSet();//ok
        Person p1 = new Person(1001,"AA");//ok
        Person p2 = new Person(1002,"BB");//ok
        set.add(p1);//ok
        set.add(p2);//ok
        p1.name = "CC";
        set.remove(p1);
        System.out.println(set);//2
        set.add(new Person(1001,"CC"));
        System.out.println(set);//3
        set.add(new Person(1001,"AA"));
        System.out.println(set);//4

    }
}

class Person {
    public String name;
    public int id;

    public Person(int id, String name) {
        this.name = name;
        this.id = id;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return id == person.id &&
                Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, id);
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", id=" + id +
                '}';
    }
}
```



# 泛型

1)泛型又称参数化类型，是Jdk5.0出现的新特性,解决数据类型的安全性问题

2)在类声明或实例化时只要指定好需要的具体的类型即可。

3)Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生ClassCastException异常。同时，代码更加简洁、健壮

4)泛型的作用是:可以在类声明时通过一个标识表示类中某个属性的类型，或者是某个方法的返回值的类型，或者是参数类型。

```java
import java.util.*;

public class GenericExercise {
    public static void main(String[] args) {
        //使用泛型方式给 HashSet 放入 3 个学生对象
        HashSet<Student> students = new HashSet<Student>();
        students.add(new Student("jack", 18));
        students.add(new Student("tom", 28));
        students.add(new Student("mary", 19));
        for (Student student : students) {
            System.out.println(student);
        }

        //使用泛型方式给 HashMap 放入 3 个学生对象
        //K -> String V->Student
        HashMap<String, Student> hm = new HashMap<String, Student>();
        hm.put("milan", new Student("milan", 38));
        hm.put("smith", new Student("smith", 48));
        hm.put("bob", new Student("bob", 28));

        Set<Map.Entry<String, Student>> entries = hm.entrySet();
        Iterator<Map.Entry<String, Student>> iterator = entries.iterator();
        while (iterator.hasNext()) {
            Map.Entry<String, Student> next = iterator.next();
            System.out.println(next.getKey() + "-" + next.getValue());
        }
    }
}

class Student {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return "Student{name = " + name + ", age = " + age + "}";
    }
}
```

1. interface List\<T>{},public class HashSet\<E>{}..等等
说明:T,E只能是引用类型

2. 在给泛型指定具体类型后，可以传入该类型或者其子类类型

1. .泛型使用形式
List\<lnteger> list1 = new ArrayList\<lnteger>{}:

List\<lnteger> list2 = new ArrayList<>{};//推荐

如果我们这样写List list3 = new ArrayList();默认给它的泛型是[\<E> E就是Object ]

```java
import java.util.ArrayList;
import java.util.Comparator;

public class GenericExercise02 {
    public static void main(String[] args) {
        ArrayList<Employee> employees = new ArrayList<>();
        employees.add(new Employee("tom", 20000, new MyDate(1980, 12, 11)));
        employees.add(new Employee("jack", 12000, new MyDate(2001, 12, 12)));
        employees.add(new Employee("tom", 50000, new MyDate(1980, 12, 10)));

        System.out.println(employees);

        employees.sort(new Comparator<Employee>() {
            @Override
            public int compare(Employee emp1, Employee emp2) {
                if (!(emp1 instanceof Employee && emp2 instanceof Employee)) {
                    System.out.println("类型不正确");
                    return 0;
                }
                //比较name
                int i = emp1.getName().compareTo(emp2.getName());
                if (i != 0) {
                    return i;
                }
                //比较birthday
                return emp1.getBirthday().compareTo(emp2.getBirthday());
            }
        });

        System.out.println(employees);

    }
}

/**
 * 定义 Employee 类
 * 1) 该类包含：private 成员变量 name,sal,birthday，其中 birthday 为 MyDate 类的对象；
 * 2) 为每一个属性定义 getter, setter 方法；
 * 3) 重写 toString 方法输出 name, sal, birthday
 * 4) MyDate 类包含: private 成员变量 month,day,year；并为每一个属性定义 getter, setter 方法；
 * 5) 创建该类的 3 个对象，并把这些对象放入 ArrayList 集合中（ArrayList 需使用泛型来定义），对集合中的元素进
 * 行排序，并遍历输出：
 * <p>
 * 排序方式： 调用 ArrayList 的 sort 方法 , * 传入 Comparator 对象[使用泛型]，
 * 先按照 name 排序，如果 name 相同，则按生日日期的先后排序。【即：定制排序】
 */
class MyDate {
    private int year;
    private int month;
    private int day;

    public MyDate(int year, int month, int day) {
        this.year = year;
        this.month = month;
        this.day = day;
    }

    public int getYear() {
        return year;
    }

    public void setYear(int year) {
        this.year = year;
    }

    public int getMonth() {
        return month;
    }

    public void setMonth(int month) {
        this.month = month;
    }

    public int getDay() {
        return day;
    }

    public void setDay(int day) {
        this.day = day;
    }

    public String toString() {
        return "MyDate{year = " + year + ", month = " + month + ", day = " + day + "}";
    }

    public int compareTo(MyDate o) {
        if (year != o.year) {
            return year - o.year;
        }
        if (month != o.month) {
            return month - o.month;
        }
        if (day != o.day) {
            return day - o.day;
        }
        return 0;
    }
}

class Employee {
    private String name;
    private double salary;
    private MyDate birthday;

    public Employee(String name, double salary, MyDate birthday) {
        this.name = name;
        this.salary = salary;
        this.birthday = birthday;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public MyDate getBirthday() {
        return birthday;
    }

    public void setBirthday(MyDate birthday) {
        this.birthday = birthday;
    }

    @Override
    public String toString() {
        return "\nEmployee{" +
                "name='" + name + '\'' +
                ", salary=" + salary +
                ", birthday=" + birthday +
                '}';
    }
}
```



## 自定义泛型

### 自定义泛型类

- 基本语法

class类名<T, R.>{//...表示可以有多个泛型

成员

}

- 注意细节

1)普通成员可以使用泛型(属性、方法)

2)使用泛型的数组，不能初始化

3)静态方法中不能使用类的泛型

4)泛型类的类型，是在创建对象时确定的(因为创建对象时，需要指定确定类型)

5)如果在创建对象时，没有指定类型，默认为Object



### 自定义泛型接口

- 基本语法

interface接口名<T,R...>{}

- 注意细节

1)接口中，静态成员也不能使用泛型(这个和泛型类规定一样)

2)泛型接口的类型,在继承接口或者实现接口时确定

3)没有指定类型，默认为Object



### 自定义泛型方法

- 基本语法

修饰符<T,R.>返回类型方法名(参数列表){

}

- 注意细节

1.泛型方法,可以定义在普通类中,也可以定义在泛型类中

2.当泛型方法被调用时，类型会确定

3.public void eat(E e){},修饰符后没有<T,R..> eat方法不是泛型方法，而是使用了泛型



## 泛型的继承和通配符

1. 泛型不具备继承性
2. <?>:支持任意泛型类型
3. <? extends A>:支持A类以及A类的子类,规定了泛型的上限
4. <? super A>:支持A类以及A类的父类，不限于直接父类，规定了泛型的下限

```java
import java.util.ArrayList;
import java.util.List;


public class GenericExtends {
    public static void main(String[] args) {

        Object o = new String("xx");

        //泛型没有继承性
        //List<Object> list = new ArrayList<String>();

        //举例说明下面三个方法的使用
        List<Object> list1 = new ArrayList<>();
        List<String> list2 = new ArrayList<>();
        List<AA> list3 = new ArrayList<>();
        List<BB> list4 = new ArrayList<>();
        List<CC> list5 = new ArrayList<>();

        //如果是 List<?> c ，可以接受任意的泛型类型
        printCollection1(list1);
        printCollection1(list2);
        printCollection1(list3);
        printCollection1(list4);
        printCollection1(list5);

        //List<? extends AA> c： 表示 上限，可以接受 AA或者AA子类
//        printCollection2(list1);//×
//        printCollection2(list2);//×
        printCollection2(list3);//√
        printCollection2(list4);//√
        printCollection2(list5);//√

        //List<? super AA> c: 支持AA类以及AA类的父类，不限于直接父类
        printCollection3(list1);//√
        //printCollection3(list2);//×
        printCollection3(list3);//√
        //printCollection3(list4);//×
        //printCollection3(list5);//×


        //冒泡排序

        //插入排序

        //....


    }

    // ? extends AA 表示 上限，可以接受 AA或者AA子类
    public static void printCollection2(List<? extends AA> c) {
        for (Object object : c) {
            System.out.println(object);
        }
    }

    //说明: List<?> 表示 任意的泛型类型都可以接受
    public static void printCollection1(List<?> c) {
        for (Object object : c) { // 通配符，取出时，就是Object
            System.out.println(object);
        }
    }


    // ? super 子类类名AA:支持AA类以及AA类的父类，不限于直接父类，
    //规定了泛型的下限
    public static void printCollection3(List<? super AA> c) {
        for (Object object : c) {
            System.out.println(object);
        }
    }

}

class AA {
}

class BB extends AA {
}

class CC extends BB {
}
```



## JUnit

1.一个类有很多功能代码需要测试，为了测试，就需要写入到main方法中

2.如果有多个功能代码测试，就需要来回注销，切换很麻烦

3.如果可以直接运行一个方法，就方便很多，并且可以给出相关信息，就好了。

```java
import org.junit.jupiter.api.Test;

import java.util.*;

public class Homework01 {
    public static void main(String[] args) {

    }

    @Test
    public void testList() {
        //说明
        //这里我们给T 指定类型是User
        DAO<User> dao = new DAO<>();
        dao.save("001", new User(1, 10, "jack"));
        dao.save("002", new User(2, 18, "king"));
        dao.save("003", new User(3, 38, "smith"));

        List<User> list = dao.list();

        System.out.println("list=" + list);

        dao.update("003", new User(3, 58, "milan"));
        dao.delete("001");//删除
        System.out.println("===修改后====");
        list = dao.list();
        System.out.println("list=" + list);

        System.out.println("id=003 " + dao.get("003"));//milan
    }
}

/**
 * 定义个泛型类 DAO<T>，在其中定义一个Map 成员变量，Map 的键为 String 类型，值为 T 类型。
 * <p>
 * 分别创建以下方法：
 * (1) public void save(String id,T entity)： 保存 T 类型的对象到 Map 成员变量中
 * (2) public T get(String id)：从 map 中获取 id 对应的对象
 * (3) public void update(String id,T entity)：替换 map 中key为id的内容,改为 entity 对象
 * (4) public List<T> list()：返回 map 中存放的所有 T 对象
 * (5) public void delete(String id)：删除指定 id 对象
 * <p>
 * 定义一个 User 类：
 * 该类包含：private成员变量（int类型） id，age；（String 类型）name。
 * <p>
 * 创建 DAO 类的对象， 分别调用其 save、get、update、list、delete 方法来操作 User 对象，
 * 使用 Junit 单元测试类进行测试。
 * <p>
 * 思路分析
 * 1. 定义User类
 * 2. 定义Dao<T>泛型类
 */

class User {
    private int id;
    private int age;
    private String name;

    public User(int id, int age, String name) {
        this.id = id;
        this.age = age;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String toString() {
        return "User{id = " + id + ", age = " + age + ", name = " + name + "}";
    }
}

class DAO<T> {
    private Map<String, T> map = new HashMap<>();

    public void save(String id, T entity) {
        map.put(id, entity);
    }

    public T get(String id) {
        return map.get(id);
    }

    public void update(String id, T entity) {
        map.put(id, entity);
    }

    public List<T> list() {
        List<T> list = new ArrayList<T>();
        Set<String> keySet = map.keySet();
        for (String key : keySet) {
            list.add(map.get(key));
        }
        return list;
    }

    public void delete(String id) {
        map.remove(id);
    }
}
```



# Java绘图技术

- Component类提供了两个和绘图相关最重要的方法:

1. paint(Graphics g)绘制组件的外观
1. repaint()刷新组件的外观。

- 当组件第一次在屏幕显示的时候,程序会自动的调用paint(方法来绘制组件。
- 在以下情况paint()将会被调用:

1. 窗口最小化.再最大化
2. 窗口的大小发生变化
3. repaint方法被调用

- Graphics类、可以理解就是画笔,为我们提供了各种绘制图形的方法

1. 画直线drawLine(int x1,int y1,int x2,int y2)
2. 画矩形边框drawRect(int x, int y, int width, int height)
3. 画椭圆边框drawOval(int x, int y, int width, int height)
4. 填充矩形fillRect(int x, int y. int width, int height)
5. 填充椭圆fillOval(int x, int y. int width, int height)
6. 画图片drawlmage(Image img, int x, int y, ..)
7. 画字符串drawString(String str. int x, int y)
8. 设置画笔的字体setFont(Font font)
9. 设置画笔的颜色setColor(Color c)

```java
import javax.swing.*;
import java.awt.*;
 

public class DrawCircle extends JFrame { //JFrame对应窗口,可以理解成是一个画框

    //定义一个面板
    private MyPanel mp = null;

    public static void main(String[] args) {
        new DrawCircle();
        System.out.println("退出程序~");
    }

    public DrawCircle() {//构造器
        //初始化面板
        mp = new MyPanel();
        //把面板放入到窗口(画框)
        this.add(mp);
        //设置窗口的大小
        this.setSize(400, 300);
        //当点击窗口的小×，程序完全退出.
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);//可以显示
    }
}

//1.先定义一个MyPanel, 继承JPanel类， 画图形，就在面板上画
class MyPanel extends JPanel {


    //说明:
    //1. MyPanel 对象就是一个画板
    //2. Graphics g 把 g 理解成一支画笔
    //3. Graphics 提供了很多绘图的方法
    //Graphics g
    @Override
    public void paint(Graphics g) {//绘图方法
        super.paint(g);//调用父类的方法完成初始化.
        System.out.println("paint 方法被调用了~");
        //画出一个圆形.
        //g.drawOval(10, 10, 100, 100);


        //演示绘制不同的图形..
        //画直线 drawLine(int x1,int y1,int x2,int y2)
        //g.drawLine(10, 10, 100, 100);
        //画矩形边框 drawRect(int x, int y, int width, int height)
        //g.drawRect(10, 10, 100, 100);
        //画椭圆边框 drawOval(int x, int y, int width, int height)
        //填充矩形 fillRect(int x, int y, int width, int height)
        //设置画笔的颜色
//        g.setColor(Color.blue);
//        g.fillRect(10, 10, 100, 100);

        //填充椭圆 fillOval(int x, int y, int width, int height)
//        g.setColor(Color.red);
//        g.fillOval(10, 10, 100, 100);

        //画图片 drawImage(Image img, int x, int y, ..)
        //1. 获取图片资源, /bg.png 表示在该项目的根目录去获取 bg.png 图片资源
//        Image image = Toolkit.getDefaultToolkit().getImage(Panel.class.getResource("/bg.png"));
//        g.drawImage(image, 10, 10, 175, 221, this);
        //画字符串 drawString(String str, int x, int y)//写字
        //给画笔设置颜色和字体
        g.setColor(Color.red);
        g.setFont(new Font("隶书", Font.BOLD, 50));
        //这里设置的 100， 100， 是 "北京你好"左下角
        g.drawString("北京你好", 100, 100);
        //设置画笔的字体 setFont(Font font)
        //设置画笔的颜色 setColor(Color c)


    }
}
```



# 事件处理机制

java事件处理是采取"委派事件模型"。当事件发生时,产生事件的对象，会把此"信息”传递给"事件的监听者" 处理，这里所说的“信息"实际上就是java.awt.event事件类库里某个类所创建的对象,把它称为"事件的对象""。

事件源:事件源是一个产生事件的对象，比如按钮，窗口等。

事件:事件就是承载事件源状态改变时的对象，比如当键盘事件、鼠标事件、窗口事件等等，会生成一个事件对象，该对象保存着当前事件很多信息，比如KeyEvent对象有含有被按下键的Code值。java.awt.event包和javax.swing.event包中定义了各种事件类型

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207201537332.png)

事件监听器接口:

（1）当事件源产生一个事件，可以传送给事件监听者处理

（2）事件监听者实际上就是一个类,该类实现了某个事件监听器接口比如前面我们案例申的MyPanle就是一个类,它实现了KeyListener接口，它就可以作为一个事件监听者，对接受到的事件进行处理

（3）事件监听器接口有多种，不同的事件监听器接口可以监听不同的事件,一个类可以实现多个监听接口

（4）这些接口在java.awt.event包和javax.swing.event包中定义.

```java
package tankGame01.event_;

import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

/**
 * 演示小球通过键盘控制上下左右的移动-> 讲解Java的事件控制
 */
public class BallMove extends JFrame { //窗口
    MyPanel mp = null;

    public static void main(String[] args) {
        BallMove ballMove = new BallMove();
    }

    //构造器
    public BallMove() {
        mp = new MyPanel();
        this.add(mp);
        this.setSize(400, 300);
        //窗口JFrame 对象可以监听键盘事件, 即可以监听到面板发生的键盘事件
        this.addKeyListener(mp);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);
    }
}

//面板, 可以画出小球
//KeyListener 是监听器, 可以监听键盘事件
class MyPanel extends JPanel implements KeyListener {

    //为了让小球可以移动, 把他的左上角的坐标(x,y)设置变量
    int x = 10;
    int y = 10;

    @Override
    public void paint(Graphics g) {
        super.paint(g);
        g.fillOval(x, y, 20, 20); //默认黑色
    }

    //有字符输出时，该方法就会触发
    @Override
    public void keyTyped(KeyEvent e) {
    }

    //当某个键按下，该方法会触发
    @Override
    public void keyPressed(KeyEvent e) {

        //System.out.println((char)e.getKeyCode() + "被按下..");

        //根据用户按下的不同键，来处理小球的移动 (上下左右的键)
        //在java中，会给每一个键，分配一个值(int)
        if (e.getKeyCode() == KeyEvent.VK_DOWN) {//KeyEvent.VK_DOWN就是向下的箭头对应的code
            y++;
        } else if (e.getKeyCode() == KeyEvent.VK_UP) {
            y--;
        } else if (e.getKeyCode() == KeyEvent.VK_LEFT) {
            x--;
        } else if (e.getKeyCode() == KeyEvent.VK_RIGHT) {
            x++;
        }

        //让面板重绘
        this.repaint();
    }

    //当某个键释放(松开)，该方法会触发
    @Override
    public void keyReleased(KeyEvent e) {

    }
}

```



# 多线程基础

1.单线程:同一个时刻,只允许执行一个线程

2.多线程:同一个时刻，可以执行多个线程，比如:一个qq进程，可以同时打开多个聊天窗口，一个迅雷进程，可以同时下载多个文件

3.并发:同一个时刻，多个任务交替执行，造成一种“貌似同时”的错觉，简单的说，单核cpu实现的多任务就是并发。

4.并行:同一个时刻，多个任务同时执行。多核cpu可以实现并行。



## 线程基本使用

### 继承 Thread 类

```java
public class Thread01 {
    public static void main(String[] args) throws InterruptedException {

        //创建Cat对象，可以当做线程使用
        Cat cat = new Cat();

        /*
            (1)
            public synchronized void start() {
                start0();
            }
            (2)
            //start0() 是本地方法，是JVM调用, 底层是c/c++实现
            //真正实现多线程的效果， 是start0(), 而不是 run
            private native void start0();
         */

        cat.start();//启动线程-> 最终会执行cat的run方法


        //cat.run();//run方法就是一个普通的方法, 没有真正的启动一个线程，就会把run方法执行完毕，才向下执行
        //说明: 当main线程启动一个子线程 Thread-0, 主线程不会阻塞, 会继续执行
        //这时 主线程和子线程是交替执行..
        System.out.println("主线程继续执行" + Thread.currentThread().getName());//名字main
        for (int i = 0; i < 60; i++) {
            System.out.println("主线程 i=" + i);
            //让主线程休眠
            Thread.sleep(1000);
        }

    }
}


//1. 当一个类继承了 Thread 类， 该类就可以当做线程使用
//2. 我们会重写 run方法，写上自己的业务代码
//3. run Thread 类 实现了 Runnable 接口的run方法
/*
    @Override
    public void run() {
        if (target != null) {
            target.run();
        }
    }
 */


class Cat extends Thread {

    int times = 0;

    @Override
    public void run() {//重写run方法，写上自己的业务逻辑

        while (true) {
            //该线程每隔1秒。在控制台输出 “喵喵, 我是小猫咪”
            System.out.println("喵喵, 我是小猫咪" + (++times) + " 线程名=" + Thread.currentThread().getName());
            //让该线程休眠1秒 ctrl+alt+t
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            if (times == 80) {
                break;//当times 到80, 退出while, 这时线程也就退出..
            }
        }
    }
}

```



### 实现 Runnable 接口

```java
public class Thread02 {
    public static void main(String[] args) {
        Dog dog = new Dog();
        //dog.start(); 这里不能调用start
        //创建了Thread对象，把 dog对象(实现Runnable),放入Thread
        Thread thread = new Thread(dog);
        thread.start();

//        Tiger tiger = new Tiger();//实现了 Runnable
//        ThreadProxy threadProxy = new ThreadProxy(tiger);
//        threadProxy.start();
    }
}

class Animal {
}

class Tiger extends Animal implements Runnable {

    @Override
    public void run() {
        System.out.println("老虎嗷嗷叫....");
    }
}

//线程代理类 , 模拟了一个极简的Thread类
class ThreadProxy implements Runnable {//你可以把Proxy类当做 ThreadProxy

    private Runnable target = null;//属性，类型是 Runnable

    @Override
    public void run() {
        if (target != null) {
            target.run();//动态绑定（运行类型Tiger）
        }
    }

    public ThreadProxy(Runnable target) {
        this.target = target;
    }

    public void start() {
        start0();//这个方法时真正实现多线程方法
    }

    public void start0() {
        run();
    }
}


class Dog implements Runnable { //通过实现Runnable接口，开发线程

    int count = 0;

    @Override
    public void run() { //普通方法
        while (true) {
            System.out.println("小狗汪汪叫..hi" + (++count) + Thread.currentThread().getName());

            //休眠1秒
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            if (count == 10) {
                break;
            }
        }
    }
}

```

### 多线程执行

```java
public class Thread03 {
    public static void main(String[] args) {

        T1 t1 = new T1();
        T2 t2 = new T2();
        Thread thread1 = new Thread(t1);
        Thread thread2 = new Thread(t2);
        thread1.start();//启动第1个线程
        thread2.start();//启动第2个线程
        //...

    }
}

class T1 implements Runnable {

    int count = 0;

    @Override
    public void run() {
        while (true) {
            //每隔1秒输出 “hello,world”,输出10次
            System.out.println("hello,world " + (++count));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            if (count == 60) {
                break;
            }
        }
    }
}

class T2 implements Runnable {

    int count = 0;

    @Override
    public void run() {
        //每隔1秒输出 “hi”,输出5次
        while (true) {
            System.out.println("hi " + (++count));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            if (count == 50) {
                break;
            }
        }
    }
}
```



### 线程终止

1.当线程完成任务后，会自动退出。

2.还可以通过使用变量来控制run方法退出的方式停止线程，即通知方式

```java
public class ThreadExit_ {
    public static void main(String[] args) throws InterruptedException {
        T t1 = new T();
        t1.start();

        //如果希望main线程去控制t1 线程的终止, 必须可以修改 loop
        //让t1 退出run方法，从而终止 t1线程 -> 通知方式

        //让主线程休眠 10 秒，再通知 t1线程退出
        System.out.println("main线程休眠10s...");
        Thread.sleep(10 * 1000);
        t1.setLoop(false);
    }
}

class T extends Thread {
    private int count = 0;
    //设置一个控制变量
    private boolean loop = true;
    @Override
    public void run() {
        while (loop) {

            try {
                Thread.sleep(50);// 让当前线程休眠50ms
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("T 运行中...." + (++count));
        }

    }

    public void setLoop(boolean loop) {
        this.loop = loop;
    }
}
```



### 线程常用方法

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207202201999.png)

注意事项和细节

1.start底层会创建新的线程，调用run,run就是一个简单的方法调用，不会启动新线程

2.线程优先级的范围

3.interrupt，中断线程，但并没有真正的结束线程。所以一般用于中断正在休眠线

4.sleep:线程的静态方法，使当前线程休眠

```java
public class ThreadMethod01 {
    public static void main(String[] args) throws InterruptedException {
        //测试相关的方法
        T t = new T();
        t.setName("老王");
        t.setPriority(Thread.MIN_PRIORITY);//1
        t.start();//启动子线程


        //主线程打印5 hi ,然后我就中断 子线程的休眠
        for(int i = 0; i < 5; i++) {
            Thread.sleep(1000);
            System.out.println("hi " + i);
        }

        System.out.println(t.getName() + " 线程的优先级 =" + t.getPriority());//1

        t.interrupt();//当执行到这里，就会中断 t线程的休眠.



    }
}

class T extends Thread { //自定义的线程类
    @Override
    public void run() {
        while (true) {
            for (int i = 0; i < 100; i++) {
                //Thread.currentThread().getName() 获取当前线程的名称
                System.out.println(Thread.currentThread().getName() + "  吃包子~~~~" + i);
            }
            try {
                System.out.println(Thread.currentThread().getName() + " 休眠中~~~");
                Thread.sleep(20000);//20秒
            } catch (InterruptedException e) {
                //当该线程执行到一个interrupt 方法时，就会catch 一个 异常, 可以加入自己的业务代码
                //InterruptedException 是捕获到一个中断异常.
                System.out.println(Thread.currentThread().getName() + "被 interrupt了");
            }
        }
    }
}
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207202213258.png)

```java
public class ThreadMethod02 {
    public static void main(String[] args) throws InterruptedException {
        Thread t3 = new Thread(new T3());//创建子线程
        for (int i = 1; i <= 10; i++) {
            System.out.println("hi " + i);
            if(i == 5) {//说明主线程输出了5次 hi
                t3.start();//启动子线程 输出 hello...
                t3.join();//立即将t3子线程，插入到main线程，让t3先执行
            }
            Thread.sleep(1000);//输出一次 hi, 让main线程也休眠1s
        }
    }
}

class T3 implements Runnable {
    private int count = 0;

    @Override
    public void run() {
        while (true) {
            System.out.println("hello " + (++count));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            if (count == 10) {
                break;
            }
        }
    }
}
```



### 用户线程和守护线程

1.用户线程:也叫工作线程，当线程的任务执行完或通知方式结束

2.守护线程:一般是为工作线程服务的，当所有的用户线程结束，守护线程自动结束

3.常见的守护线程:垃圾回收机制

```java
public class ThreadMethod03 {
    public static void main(String[] args) throws InterruptedException {
        MyDaemonThread myDaemonThread = new MyDaemonThread();
        //如果我们希望当main线程结束后，子线程自动结束
        //,只需将子线程设为守护线程即可
        myDaemonThread.setDaemon(true);
        myDaemonThread.start();

        for( int i = 1; i <= 10; i++) {//main线程
            System.out.println("A在辛苦的工作...");
            Thread.sleep(1000);
        }
    }
}

class MyDaemonThread extends Thread {
    public void run() {
        for (; ; ) {//无限循环
            try {
                Thread.sleep(1000);//休眠1000毫秒
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("B和C快乐聊天，哈哈哈~~~");
        }
    }
}
```

## 线程的生命周期

NEW:尚未启动的线程处于此状态。

RUNNABLE:在Java虚拟机中执行的线程处于此状态。

BLOCKED:;被阻塞等待监视器锁定的线程处于此状态。

WAITING:正在等待另一个线程执行特定动作的线程处于此状态。

TIMED_WAITING:正在等待另一个线程执行动作达到指定等待时间的线程处于此状态。

TERMINATED:已退出的线程处于此状态。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207252034559.png)

```java
public class ThreadState_ {
    public static void main(String[] args) throws InterruptedException {
        T t = new T();
        System.out.println(t.getName() + " 状态 " + t.getState());
        t.start();

        while (Thread.State.TERMINATED != t.getState()) {
            System.out.println(t.getName() + " 状态 " + t.getState());
            Thread.sleep(500);
        }
        System.out.println(t.getName() + " 状态 " + t.getState());
    }
}
class T extends Thread {
    @Override
    public void run() {
        while (true) {
            for (int i = 0; i < 10; i++) {
                System.out.println("hi " + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            break;
        }
    }
}
```



## 线程的同步

### Synchronized

1．在多线程编程，一些敏感数据不允许被多个线程同时访问，此时就使用同步访问技
术，保证数据在任何同一时刻，最多有一个线程访问，以保证数据的完整性。

2．也可以这里理解:线程同步，即当有一个线程在对内存进行操作时，其他线程都不可以对这个内存地址进行操作，直到该线程完成操作，其他线程才能对该内存地址进行操作.

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207211006770.png)



### 互斥锁

1.Java语言中，引入了对象互斥锁的概念，来保证共享数据操作的完整性。

2.每个对象都对应于一个可称为“互斥锁”的标记，这个标记用来保证在任一时刻，只能有一个线程访问该对象。

3.关键字synchronized来与对象的互斥锁联系。当某个对象用synchronized修饰时,表明该对象在任一时刻只能由一个线程访问

4.同步的局限性:导致程序的执行效率要降低

5.同步方法(非静态的)的锁可以是this,也可以是其他对象(要求是同一个对象)

6.同步方法(静态的)的锁为当前类本身。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207211044920.png)



### 线程的死锁

多个线程都占用了对方的锁资源，但不肯相让，导致了死锁，在编程是一定要避免死锁的发生。

```java
public class DeadLock_ {
    public static void main(String[] args) {
        //模拟死锁现象
        DeadLockDemo A = new DeadLockDemo(true);
        A.setName("A线程");
        DeadLockDemo B = new DeadLockDemo(false);
        B.setName("B线程");
        A.start();
        B.start();
    }
}


//线程
class DeadLockDemo extends Thread {
    static Object o1 = new Object();// 保证多线程，共享一个对象,这里使用static
    static Object o2 = new Object();
    boolean flag;

    public DeadLockDemo(boolean flag) {//构造器
        this.flag = flag;
    }

    @Override
    public void run() {

        //下面业务逻辑的分析
        //1. 如果flag 为 T, 线程A 就会先得到/持有 o1 对象锁, 然后尝试去获取 o2 对象锁
        //2. 如果线程A 得不到 o2 对象锁，就会Blocked
        //3. 如果flag 为 F, 线程B 就会先得到/持有 o2 对象锁, 然后尝试去获取 o1 对象锁
        //4. 如果线程B 得不到 o1 对象锁，就会Blocked
        if (flag) {
            synchronized (o1) {//对象互斥锁, 下面就是同步代码
                System.out.println(Thread.currentThread().getName() + " 进入1");
                synchronized (o2) { // 这里获得li对象的监视权
                    System.out.println(Thread.currentThread().getName() + " 进入2");
                }

            }
        } else {
            synchronized (o2) {
                System.out.println(Thread.currentThread().getName() + " 进入3");
                synchronized (o1) { // 这里获得li对象的监视权
                    System.out.println(Thread.currentThread().getName() + " 进入4");
                }
            }
        }
    }
}
```



### 释放锁

- 以下操作会释放锁

1.当前线程的同步方法、同步代码块执行结束案例

2.当前线程在同步代码块、同步方法中遇到break、return.

3.当前线程在同步代码块、同步方法中出现了未处理的Error或Exception，导致异常结束

4.当前线程在同步代码块、同步方法中执行了线程对象的wait()方法，当前线程暂停，并释放锁。

- 以下操作不会释放锁

1．线程执行同步代码块或同步方法时，程序调用Thread.sleep0、Thread.yield()方法暂停当前线程的执行,不会释放锁

2．线程执行同步代码块时，其他线程调用了该线程的suspend()方法将该线程挂起，该线程不会释放锁。提示：应尽量避免使用suspend()和resume()来控制线程，方法不再推荐使用



## 练习题

```java
import java.util.Scanner;

public class Homework01 {
    public static void main(String[] args) {
        A a = new A();
        B b = new B(a);//一定要注意.
        a.start();
        b.start();
    }
}

//创建A线程类
class A extends Thread {
    private boolean loop = true;

    @Override
    public void run() {
        //输出1-100数字
        while (loop) {
            System.out.println((int)(Math.random() * 100 + 1));
            //休眠
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("a线程退出...");

    }

    public void setLoop(boolean loop) {//可以修改loop变量
        this.loop = loop;
    }
}

//直到第2个线程从键盘读取了“Q”命令
class B extends Thread {
    private A a;
    private Scanner scanner = new Scanner(System.in);

    public B(A a) {//构造器中，直接传入A类对象
        this.a = a;
    }

    @Override
    public void run() {
        while (true) {
            //接收到用户的输入
            System.out.println("请输入你指令(Q)表示退出:");
            char key = scanner.next().toUpperCase().charAt(0);
            if(key == 'Q') {
                //以通知的方式结束a线程
                a.setLoop(false);
                System.out.println("b线程退出.");
                break;
            }
        }
    }
}
```

```java
public class Homework02 {
    public static void main(String[] args) {
        Money money = new Money();
        new Thread(money).start();
        new Thread(money).start();

    }
}

class Money implements Runnable {
    private int balance = 11000;

    @Override
    public void run() {
        while (true) {
            synchronized (this) {
                if (balance < 1000) {
                    System.out.println("余额不足");
                    break;
                }
                balance -= 1000;
                System.out.print(Thread.currentThread().getName());
                System.out.println("  balance=" + balance);
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```



# IO流

## 文件

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207212129556.png)

###  常用的文件操作

- **创建文件对象相关构造器和方法**

new File(String pathname)//根据路径构建一个File对象

new File(File parent,String child)//根据父目录文件+子路径构建

new File(String parent,String child)//根据父目录+子路径构建

```java
import org.junit.jupiter.api.Test;

import java.io.File;
import java.io.IOException;


public class FileCreate {
    public static void main(String[] args) {

    }

    //(1) new File(String pathname)//根据路径构建一个File对象
    @Test
    public void create01() {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\file_\\new1.txt";
        File file = new File(filePath);

        try {
            file.createNewFile();
            System.out.println("文件创建成功");
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    //(2) new File(File parent,String child)//根据父目录文件+子路径构建
    @Test
    public void create02() {
        File parentFile = new File("D:\\Java_Project\\basicOfJava\\io_\\file_\\");
        String fileName = "new2.txt";
        File file = new File(parentFile, fileName);
        try {
            file.createNewFile();
            System.out.println("文件创建成功");
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    //(3) new File(String parent,String child) //根据父目录+子路径构建
    @Test
    public void create03() {
        String parentPath = "D:\\Java_Project\\basicOfJava\\io_\\file_\\";
        String fileName = "news3.txt";
        File file = new File(parentPath, fileName);

        try {
            file.createNewFile();
            System.out.println("创建成功~");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
import org.junit.jupiter.api.Test;

import java.io.File;

public class FileInformation {
    public static void main(String[] args) {

    }
    @Test
    //获取文件信息
    public void info(){
        // 先创建文件
        File file = new File("D:\\Java_Project\\basicOfJava\\io_\\file_\\news1.txt");

        //调用方法得到信息
        System.out.println("文件名字="+file.getName());
        System.out.println("文件绝对路径="+file.getAbsolutePath());
        System.out.println("文件父级目录="+file.getParent());
        System.out.println("文件大小(字节)="+file.length());
        System.out.println("文件是否存在="+file.exists());
        System.out.println("是不是一文件="+file.isFile());
        System.out.println("是不是一个目录"+file.isDirectory());

    }
}
```

- **目录的操作和文件删除**

```java
public class Directory_ {
    public static void main(String[] args) {

        //
    }

    //判断 d:\\news1.txt 是否存在，如果存在就删除
    @Test
    public void m1() {

        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\file_\\news2.txt";
        File file = new File(filePath);
        if (file.exists()) {
            if (file.delete()) {
                System.out.println(filePath + "删除成功");
            } else {
                System.out.println(filePath + "删除失败");
            }
        } else {
            System.out.println("该文件不存在...");
        }

    }

    //判断 D:\\demo02 是否存在，存在就删除，否则提示不存在
    //在java编程中，目录也被当做文件
    //目录下有文件会删除失败
    @Test
    public void m2() {

        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\file_\\demo02";
        File file = new File(filePath);
        if (file.exists()) {
            if (file.delete()) {
                System.out.println(filePath + "删除成功");
            } else {
                System.out.println(filePath + "删除失败");
            }
        } else {
            System.out.println("该目录不存在...");
        }

    }

    //判断 D:\\demo\\a\\b\\c 目录是否存在，如果存在就提示已经存在，否则就创建
    @Test
    public void m3() {

        String directoryPath = "D:\\Java_Project\\basicOfJava\\io_\\file_\\demo01\\a\\b\\c";
        File file = new File(directoryPath);
        if (file.exists()) {
            System.out.println(directoryPath + "存在..");
        } else {
            if (file.mkdirs()) { //创建一级目录使用mkdir() ，创建多级目录使用mkdirs()
                System.out.println(directoryPath + "创建成功..");
            } else {
                System.out.println(directoryPath + "创建失败...");
            }
        }
        
    }
}
```



## IO流分类

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207221318752.png)



### InputStream

- InputStream：字节输入流

- InputStream抽象类是所有类字节输入流的超类

- InputStream常用的子类

  1.FilelnputStream:文件输入流

  2.BufferedInputStream:缓冲字节输入流

  3.ObjectInputStream:对象字节输入流

```java
import org.junit.Test;

import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStream_ {
    public static void main(String[] args) {

    }

    /**
     * 演示读取文件...
     * 单个字节的读取，效率比较低
     * -> 使用 read(byte[] b)
     */
    @Test
    public void readFile01() {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\inputstream_\\hello.txt";
        int readData = 0;
        FileInputStream fileInputStream = null;
        try {
            //创建 FileInputStream 对象，用于读取 文件
            fileInputStream = new FileInputStream(filePath);
            //从该输入流读取一个字节的数据。 如果没有输入可用，此方法将阻止。
            //如果返回-1 , 表示读取完毕
            while ((readData = fileInputStream.read()) != -1) {
                System.out.print((char) readData);//转成char显示
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭文件流，释放资源.
            try {
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

    /**
     * 使用 read(byte[] b) 读取文件，提高效率
     */
    @Test
    public void readFile02() {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\inputstream_\\hello.txt";
        //字节数组
        byte[] buf = new byte[8]; //一次读取8个字节.
        int readLen = 0;
        FileInputStream fileInputStream = null;
        try {
            //创建 FileInputStream 对象，用于读取 文件
            fileInputStream = new FileInputStream(filePath);
            //从该输入流读取最多b.length字节的数据到字节数组。 此方法将阻塞，直到某些输入可用。
            //如果返回-1 , 表示读取完毕
            //如果读取正常, 返回实际读取的字节数
            while ((readLen = fileInputStream.read(buf)) != -1) {
                System.out.print(new String(buf, 0, readLen));//显示
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭文件流，释放资源.
            try {
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

### OutputStream

```java
import org.junit.jupiter.api.Test;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class FileOutputStream01 {
    public static void main(String[] args) {

    }

    @Test
    public void writeFile() throws IOException {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\outputstream_\\a.txt";
        File file = new File(filePath);
        if (!file.exists()) {
            file.createNewFile();
        }
        OutputStream out = null;
        //1. new FileOutputStream(filePath) 创建方式，当写入内容是，会覆盖原来的内容
        //2. new FileOutputStream(filePath, true) 创建方式，当写入内容是，是追加到文件后面
        out = new FileOutputStream(filePath);
        //write(byte[] b, int off, int len) 将 len字节从位于偏移量 off的指定字节数组写入此文件输出流
        out.write("hello,world!".getBytes());

    }
}
```

```java
import org.junit.jupiter.api.Test;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileCopy {
    public static void main(String[] args) {

    }

    @Test
    public void copyFile() {
        String path1 = "D:\\Java_Project\\basicOfJava\\io_\\inputstream_\\hello.txt";
        String path2 = "D:\\Java_Project\\basicOfJava\\io_\\outputstream_\\hello.txt";
        FileInputStream fileInput = null;
        FileOutputStream fileOutput = null;

        try {
            fileInput = new FileInputStream(path1);
            fileOutput = new FileOutputStream(path2);
            //定义一个字节数组,提高读取效果
            byte[] buf = new byte[1024];
            int readLen = 0;
            while ((readLen = fileInput.read(buf)) != -1) {
                fileOutput.write(buf, 0, readLen);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        finally {
            try {
                if (fileInput != null) {
                    fileInput.close();
                }
                if (fileOutput != null) {
                    fileOutput.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```



### FileReader

**FileReader 相关方法：**

1. new FileReader(File/String)
2. read:每次读取单个字符，返回该字符，如果到文件末尾返回-1
3. read(char[):批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回-1

*相关API:*

1. new String(char):将char[]转换成String
2. new String(char[,off,len):将char的指定部分转换成String

```java
package io_.reader_;

import java.io.FileReader;
import java.io.IOException;

public class FileReader_ {
    public static void main(String[] args) {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\reader_\\story.txt";
        FileReader fileReader = null;
        try {
            //按单个字符读取
            fileReader = new FileReader(filePath);
            System.out.println("按单个字符读取");
            int data = 0;
            while ((data = fileReader.read()) != -1) {
                System.out.print((char) data);
            }
            System.out.println();
            //按字符数组读取
            fileReader = new FileReader(filePath);
            System.out.println("按字符数组读取");
            char[] chars = new char[8];
            int readLen = 0;
            while ((readLen = fileReader.read(chars)) != -1) {
                System.out.print(new String(chars, 0, readLen));
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fileReader != null) {
                try {
                    fileReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```



### FileWriter

**FileWriter相关方法**

1. new FileWriter(File/String):覆盖模式，相当于流的指针在首端
2. new FileWriter(File/String,true):追加模式，相当于流的指针在尾端
3. write(int):写入单个字符
4. write(char[]):写入指定数组
5. write(char[],off,len):写入指定数组的指定部分
6. write (string):写入整个字符串
7. write(string,off,len):写入字符串的指定部分

*相关API:* String类:toCharArray:将String转换成char

*注意*:FileWriter使用后，必须要关闭(close)或刷新(flush)，否则写入不到指定的文件!

```java
import java.io.FileWriter;
import java.io.IOException;

public class FileWriter_ {
    public static void main(String[] args) {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\writer_\\note.txt";
        FileWriter fileWriter = null;
        char[] chars = {'a', 'b', 'c'};
        try {
            fileWriter = new FileWriter(filePath);
//          3) write(int):写入单个字符
            fileWriter.write('H');
//            4) write(char[]):写入指定数组
            fileWriter.write(chars);
//            5) write(char[],off,len):写入指定数组的指定部分
            fileWriter.write("学习学习".toCharArray(), 0, 3);
//            6) write（string）：写入整个字符串
            fileWriter.write(" 你好北京~");
            fileWriter.write("风雨之后，定见彩虹");
//            7) write(string,off,len):写入字符串的指定部分
            fileWriter.write("上海天津", 0, 2);
            //在数据量大的情况下，可以使用循环操作.
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fileWriter.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```



## 节点流和处理流

**节点流**可以从一个特定的数据源读写数据，如FIleReader、FileWriter

**处理流**(也叫包装流)是“连接”在已存在的流（节点流或处理流)之上，为程序提供更为强大的读写功能，也更加灵活,如BufferedReader、BufferedWriter

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207231727352.png)

**节点流和处理流的区别和联系**

1. 节点流是底层流/低级流,直接跟数据源相接。
2. 处理流(包装流)包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出。
3. 处理流(也叫包装流)对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连



### BufferedReader

```java
import java.io.*;

public class BufferedReader_ {
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader("D:\\Java_Project\\basicOfJava\\io_\\reader_\\story.txt"));
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
        reader.close();
    }
}
```



### BufferedWriter

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriter_ {
    public static void main(String[] args) throws IOException {
        BufferedWriter bw = new BufferedWriter(new FileWriter("D:\\Java_Project\\basicOfJava\\io_\\writer_\\note.txt",true));
        bw.newLine();
        bw.write("Hello World");
        bw.newLine();
        bw.write("goodbye world");
        bw.close();
    }
}
```



### BufferedInputStream

BufferedInputStream是字节流在创建BufferedInputStream时，会创建一个内部缓冲区数组.

### BufferedOutputStream

BufferedOutputStream是字节流,实现缓冲的输出流,可以将多个字节写入底层输出流中,而不必对每次字节写入调用底层系统.

```java
import java.io.*;

public class BufferedCopy02 {
    public static void main(String[] args) throws IOException {
        String srcFilePath = "D:\\Java_Project\\basicOfJava\\io_\\inputstream_\\高山流水.mp3";
        String dstFilePath = "D:\\Java_Project\\basicOfJava\\io_\\outputstream_\\高山流水.mp3";

        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcFilePath));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(dstFilePath));

        byte[] bytes = new byte[1024];
        int readLen = 0;
        while ((readLen = bis.read(bytes)) != -1) {
            bos.write(bytes, 0, readLen);
        }

        bis.close();
        bos.close();


    }
}
```



### ObjectOutputStream

**序列化和反序列化**

1.序列化就是在保存数据时，保存数据的值和数据类型

2.反序列化就是在恢复数据时,恢复数据的值和数据类型

3.需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一:
	Serializable //这是一个标记接口,没有方法
	Externalizable //该接口有方法需要实现，因此我们一般实现上面的Serializable接口



ObjectOutputStream 提供 序列化功能

```java
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;


public class ObjectOutStream_ {
    public static void main(String[] args) throws Exception {
//序列化后，保存的文件格式，不是存文本，而是按照他的格式来保存
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\outputstream_\\data.dat";
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));
//序列化数据到 \data.dat
        oos.writeInt(100);// int -> Integer (实现了 Serializable)
        oos.writeBoolean(true);// boolean -> Boolean (实现了 Serializable)
        oos.writeChar('a');// char -> Character (实现了 Serializable)
        oos.writeDouble(9.5);// double -> Double (实现了 Serializable)
        oos.writeUTF("学习学习");//String
//保存一个 dog 对象
        oos.writeObject(new Dog(10));
        oos.close();
        System.out.println("数据保存完毕(序列化形式)");
    }
}
class Dog implements Serializable {
    int age;

    public Dog(int age) {
        this.age = age;
    }
}
```



### ObjectInputStream

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ObjectInputStream_ {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\outputstream_\\data.dat";
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filePath));
        // 2.读取， 注意顺序
        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());
        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());
        System.out.println(ois.readObject());
//        System.out.println(ois.readObject());
//        System.out.println(ois.readObject());

        ois.close();
        System.out.println("以反序列化的方式读取(恢复)ok~");
    }
}
```



1)读写顺序要一致

2)要求序列化或反序列化对象,需要实现 Serializable

3)序列化的类中建议添加SerialVersionUID.为了提高版本的兼容性

4)序列化对象时，默认将里面所有属性都进行序列化，但**除了static或transient修饰的成员**

5)序列化对象时，要求里面属性的类型也需要实现序列化接口

6)序列化具备可继承性,也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化



## 标准输入输出流

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207232041319.png)



## 转换流

1.InputStreamReader:Reader的子类，可以将InputStream(字节流)包装成(换)Reader(字符流)

2.OutputStreamWriter:Writer的子类，实现将OutputStream(字节流)包装成Writer(宁符流)

3.当处理纯文本数据时，如果使用字符流效率更高，并且可以有效解决中文问题，所以建议将字节流转换成字符流

4.可以在使用时指定编码格式(比如utf-8, gbk , gb2312, IS08859-1等)

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;

public class InputStreamReader_ {
    public static void main(String[] args) throws IOException {

        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\transformation_\\a.txt";
        //解读
        //1. 把 FileInputStream 转成 InputStreamReader
        //2. 指定编码 gbk
        //InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath), "gbk");
        //3. 把 InputStreamReader 传入 BufferedReader
        //BufferedReader br = new BufferedReader(isr);

        //将2 和 3 合在一起
        BufferedReader br = new BufferedReader(new InputStreamReader(
                new FileInputStream(filePath), "gbk"));

        //4. 读取
        String s = br.readLine();
        System.out.println("读取内容=" + s);
        //5. 关闭外层流
        br.close();
    }
}
```

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

public class OutputStreamWriter_ {
    public static void main(String[] args) throws IOException {
        String filePath = "D:\\Java_Project\\basicOfJava\\io_\\transformation_\\b.txt";

        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(filePath), "gbk");

        osw.write("god,天");
        osw.close();

    }
}
```



## 打印流

### PrintStream 

PrintStream （字节打印流/输出流）

```java
import java.io.IOException;
import java.io.PrintStream;

public class PrintStream_ {
    public static void main(String[] args) throws IOException {
        //在默认情况下，PrintStream 输出数据的位置是 标准输出，即显示器
        PrintStream out = System.out;
        out.print("Hello, world!\n");
        out.write("Hello, 世界!".getBytes());
        //可以修改打印流输出的位置、设备
        System.setOut(new PrintStream("D:\\Java_Project\\basicOfJava\\io_\\printstream_\\p1.txt"));
        System.out.println("goodbye, world!");

        out.close();

    }
}
```

### PrintWriter

```java
public class PrintWriter_ {
    public static void main(String[] args) throws IOException {

        //PrintWriter printWriter = new PrintWriter(System.out);
        PrintWriter printWriter = new PrintWriter(new FileWriter("D:\\Java_Project\\basicOfJava\\io_\\printstream_\\p2.txt"));
        printWriter.print("hi, 北京你好~~~~");
        printWriter.close();//flush + 关闭流, 才会将数据写入到文件..

    }
}
```

## Properties类

1)专门用于读写配置文件的集合类

配置文件的格式:

键=值

键=值

2)注意:键值对不需要有空格，值不需要用引号一起来。默认类型是String

3)Properties的常见方法

- load:加载配置文件的键值对到Properties对象

- list:将数据显示到指定设备

- getProperty(key):根据键获取值

- setProperty(key,value):设置键值对到Properties对象

- store:将Properties中的键值对存储到配置文件,在idea中，保存信息到配置文件，如果含

  有中文，会存储为unicode码

```java
public class Properties02 {
    public static void main(String[] args) throws IOException {
        //使用 Properties 类来读取 mysql.properties 文件

        //1. 创建 Properties 对象
        Properties properties = new Properties();
        //2. 加载指定配置文件
        properties.load(new FileReader("src\\mysql.properties"));
        //3. 把 k-v 显示控制台
        properties.list(System.out);
        //4. 根据 key 获取对应的值
        String user = properties.getProperty("user");
        String pwd = properties.getProperty("pwd");
        System.out.println("用户名=" + user);
        System.out.println("密码是=" + pwd);

        //使用 Properties 类来创建 配置文件，修改配置文件内容
        Properties properties2 = new Properties();
        //创建
        //1.如果该文件没有 key 就是创建
        //2.如果该文件有 key ,就是修
        properties2.setProperty("charset", "utf8");
        properties2.setProperty("user", "汤姆");//注意保存时，是中文的 unicode 码值
        properties2.setProperty("pwd","888888");
        //将 k-v 存储文件中即可
        properties2.store(new FileOutputStream("src\\mysql2.properties"), "hello world");
        System.out.println("保存配置文件成功~");

    }
}
```

```java
package reflection_;

public class Cat {
    private String name = "招财猫";
    public int age = 10; //public的

    public Cat() {} //无参构造器

    public Cat(String name) {
        this.name = name;
    }

    public void hi() { //常用方法
        //System.out.println("hi " + name);
    }
    public void cry() { //常用方法
        System.out.println(name + " 喵喵叫..");
    }
}
```
