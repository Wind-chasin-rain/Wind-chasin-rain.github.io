---
title: Java学习笔记01
date: 2022-07-15 19:57:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207150103990.jpg
tags: Java
categories: 学习笔记
description: 变量、运算符、程序控制结构、数组、面向对象编程
updated:
mathjax:
highlight_shrink:
---

# java概述

## 转义字符

```java
//转义字符
public class ChangeChar {
	public static void main(String[] args){

		// \t :一个制表位，实现对齐功能
		System.out.println("北京\t天津\t上海");
		// \n :换行符
		System.out.println("ab\ncd\nef");
		// \\ : 一个\
		System.out.println("北京\\天津\\\\上海");
		// \" : 一个"
		System.out.println("北京\"天津\"上海");
		// \' : 一个'
		System.out.println("北京\'天津\'上海");
		// \r : 一个回车
		// 输出为  上海天津
		System.out.println("北京天津\r上海");
		// 输出为  北京天津
		// 		  上海
		System.out.println("北京天津\r\n上海");

		//test
		System.out.println("书名\t作者\t价格\t销量\n三国\t罗贯中\t120\t1000");
	}
}

/* 运行结果
北京    天津    上海
ab
cd
ef
北京\天津\\上海
北京"天津"上海
北京'天津'上海
上海天津
北京天津
上海
书名    作者    价格    销量
三国    罗贯中  120     1000
*/

```

## 文档注释

```java
/**
 * @author 麦子落
 * @version 1.0
 */

//  类
public class Comment02 {

	// 编写一个main方法
	public static void main(String[] args){
		
	}
}
```

在cmd中输入`javadoc -d d:\\temp -author -version Comment02.java`生成文档注释

## DOS

`md d:\\test`创建目录/文件夹

`rd d:\\test` 删除目录/文件夹

dir ：查看当前目录有什么内容

cd  : 切换到其他盘下  例：cd \D c:

..    : 切换到上一级目录 例： cd ..

cd \ :  切换到根目录

tree ：查看指定的目录下的所有的子级目录  例: tree d:

cls  : 清屏

exit ：退出DOS

echo ：输入内容到文件  例：echo hello >  hello.txt

copy:  拷贝

del : 删除文件

move ：剪切



# 变量

## 加法

```java
public class Plus {
	
	public static void main(String[] args){
		System.out.println(100+98);  //198
		System.out.println("100"+98); //10098

		System.out.println(100+3+"hello"); //103hello
		System.out.println("hello"+100+3); //hello1003
	}
}
```

## 数据类型

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206291445661.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206291446955.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206291455904.png)

boolean类型不可以0或非0的整数替代false和true，这点和C语言不同。

```java
public class StringToBasic { 

	//编写一个main方法
	public static void main(String[] args) {
		
		//基本数据类型->String
		
		int n1 = 100;
		float f1 = 1.1F;
		double d1 = 4.5;
		boolean b1 = true;
		String s1 = n1 + "";
		String s2 = f1 + "";
		String s3 = d1 + "";
		String s4 = b1 + "";
		System.out.println(s1 + " " + s2 + " " + s3 + " " + s4);

		//String->对应的基本数据类型
		String s5 = "123";
		//会在OOP 讲对象和方法的时候回详细
		//解读 使用 基本数据类型对应的包装类，的相应方法，得到基本数据类型
		int num1 = Integer.parseInt(s5);
		double num2 = Double.parseDouble(s5);
		float num3 = Float.parseFloat(s5);
		long num4 = Long.parseLong(s5);
		byte num5 = Byte.parseByte(s5);
		boolean b = Boolean.parseBoolean("true");
		short num6 = Short.parseShort(s5);

		System.out.println("===================");
		System.out.println(num1);//123
		System.out.println(num2);//123.0
		System.out.println(num3);//123.0
		System.out.println(num4);//123
		System.out.println(num5);//123
		System.out.println(num6);//123
		System.out.println(b);//true

		//怎么把字符串转成字符char -> 含义是指 把字符串的第一个字符得到
		//解读  s5.charAt(0) 得到 s5字符串的第一个字符 '1'
		System.out.println(s5.charAt(0));
		
	}
}
```



# 运算符

## 算术运算符

```java
public class ArithmeticOperatorExercise{
	public static void main(String[] args){
		//1.需求:
		//假如还有 59 天放假，问：合 xx 个星期零 xx 天
		//2.思路分析
		//(1) 使用 int 变量 days 保存 天数
		//(2) 一个星期是 7 天 星期数 weeks： days / 7 零 xx 天 leftDays days % 7
		//(3) 输出

		//3.走代码
		int days = 505;
		int weeks = days / 7;
		int leftDays = days % 7;
		System.out.println("还有"+days+"天放假，合"+weeks+"个星期零"+leftDays+"天");


		//定义一个变量保存华氏温度，华氏温度转换摄氏温度的公式为
		//：5/9*(华氏温度-100),请求出华氏温度对应的摄氏温度
		//
		//2 思路分析
		//(1) 先定义一个 double huaShi 变量保存 华氏温度
		//(2) 根据给出的公式，进行计算即可 5/9*(华氏温度-100)
		// 考虑数学公式和 java 语言的特性
		//(3) 将得到的结果保存到 double sheShi

		//3 走代码	
		double huaShi = 234.5;
		double sheShi = 5.0 / 9 * (huaShi - 100);
		System.out.println("华氏温度" + huaShi+ " 对应的摄氏温度=" + sheShi);
	}
}
```

## 关系运算符

也称比较运算符，结果都是boolean型。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206292003148.png)

## 逻辑运算符

结果都是boolean型

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206292004527.png)

&& 和 & 使用区别 

1) &&短路与：如果第一个条件为 false，则第二个条件不会判断，最终结果为 false，效率高
2) & 逻辑与：不管第一个条件是否为 false，第二个条件都要判断，效率

|| 和 | 使用区别 

1. ||短路或：如果第一个条件为 true，则第二个条件不会判断，最终结果为 true，效率高
2. | 逻辑或：不管第一个条件是否为 true，第二个条件都要判断，效率低

```java
public class LogicOperator{
	public static void main(String[] args){
		int x = 5; 
		int y = 5;
		if(x++==6 & ++y==6){
			x = 11;
		}
		System.out.println("x=" + x + "y=" + y);
		// 6 6 

		boolean i = true;
		boolean j = false;
		short z = 46;
		if((z++==46)&&(j=true)) z++;
		if((i=false)||(++z==49)) z++;
		System.out.println("z=" + z);
		// 50
	}
}
```

## 赋值运算符

```java
public class AssignOperator { 

	//编写一个main方法
	public static void main(String[] args) {

		int n1 = 10;
		n1 += 4;// n1 = n1 + 4;
		System.out.println(n1); // 14
		n1 /= 3;// n1 = n1 / 3;//4
		System.out.println(n1); // 4

		//！！！复合赋值运算符会进行类型转换！！！
		byte b = 3;
		b += 2; // 等价 b = (byte)(b + 2);
		b++; // b = (byte)(b+1);

	}
}
```

## 三元运算符

```java
public class TernaryOperator { 

	//编写一个main方法
	public static void main(String[] args) {

		int a = 10;
		int b = 99;
		// 解读
		// 1. a > b 为 false
		// 2. 返回 b--, 先返回 b的值,然后在 b-1
		// 3. 返回的结果是99 
		int result = a > b ? a++ : b--;
		System.out.println("result=" + result);
		System.out.println("a=" + a);
		System.out.println("b=" + b);

	}
}
```

## 运算符优先级

上一行运算符总优先先下一行

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206292037981.png)

## 键盘输入语句

```java
import java.util.Scanner;//表示把java.util下的Scanner类导入 
public class Input { 

	//编写一个main方法
	public static void main(String[] args) {
		//演示接受用户的输入
		//步骤
		//Scanner类 表示 简单文本扫描器，在java.util 包
		//1. 引入/导入 Scanner类所在的包
		//2. 创建 Scanner 对象 , new 创建一个对象,体会
		//   myScanner 就是 Scanner类的对象
		Scanner myScanner = new Scanner(System.in);
		//3. 接收用户输入了， 使用 相关的方法
		System.out.println("请输入名字");

		//当程序执行到 next 方法时，会等待用户输入~~~
		String name = myScanner.next(); //接收用户输入字符串
		System.out.println("请输入年龄");
		int age = myScanner.nextInt(); //接收用户输入int
		System.out.println("请输入薪水");
		double sal = myScanner.nextDouble(); //接收用户输入double
		System.out.println("人的信息如下:");
		System.out.println("名字=" + name 
			+ " 年龄=" + age + " 薪水=" + sal);

	}
}
```

## 进制

二进制转换成八进制 

规则：从低位开始,将二进制数每三位一组，转成对应的八进制数即可。 

案例：请将 ob11010101 转成八进制 ob11(3)010(2)101(5) => 0325 

 二进制转换成十六进制 

规则：从低位开始，将二进制数每四位一组，转成对应的十六进制数即可。 

案例：请将 ob11010101 转成十六进制 ob1101(D)0101(5) = 0xD5

八进制转换成二进制 

规则：将八进制数每 1 位，转成对应的一个 3 位的二进制数即可。 案例：请将 0237 转成二进制 02(010)3(011)7(111) = 0b10011111 

十六进制转换成二进制 

规则：将十六进制数每 1 位，转成对应的 4 位的一个二进制数即可。 

案例：请将 0x23B 转成二进制 0x2(0010)3(0011)B(1011) = 0b001000111011

## 原码、反码、补码

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206292115544.png)



# 程序控制结构

## 顺序控制

### 分支

```java
import java.util.Scanner;
public class MyTest{
	public static void main(String[] args){
		/* 根据淡旺季的月份和年龄 打印票价
			4-10旺季：
					成人（18-60）：60
					儿童（<18) :30
					老人（>60) :20
			淡季：
					成人：40
					其他：20
		*/

		Scanner myScanner = new Scanner(System.in);
		System.out.println("请输入月份");
		int month = myScanner.nextInt();
		System.out.println("请输入年龄");
		int age = myScanner.nextInt();		

		if(month >= 4 && month <= 10){
			if(age >= 18 && age <= 60) {
				System.out.println("票价为60元");
			}
			else if(age < 18){
				System.out.println("票价为30元");
			}
			else {
				System.out.println("票价为20元");
			}
		}
		else {
			if(age >= 18 && age <= 60) {
				System.out.println("票价为40元");
			}
			else {
				System.out.println("票价为20元");
			}
		}
	}
}
	
```

### switch语句

```java
import java.util.Scanner;
public class Switch01 {
    public static void main(String[] args){
        //编写一个 main 方法
        /*
        案例：Switch01.java
        请编写一个程序，该程序可以接收一个字符，比如:a,b,c,d,e,f,g
        a 表示星期一，b 表示星期二 …
        根据用户的输入显示相应的信息.要求使用 switch 语句完成
        思路分析
        1. 接收一个字符 , 创建 Scanner 对象
        2. 使用 switch 来完成匹配,并输出对应信息
        代码
        */

        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入a-g");
        char c1 = scanner.next().charAt(0);
        switch (c1) {
            case 'a': System.out.println("星期一"); break;
            case 'b': System.out.println("星期二"); break;
            case 'c': System.out.println("星期三"); break;
            case 'd': System.out.println("星期四"); break;
            case 'e': System.out.println("星期五"); break;
            case 'f': System.out.println("星期六"); break;
            case 'g': System.out.println("星期七"); break;
            default : System.out.println("错误"); break;
        }
    }
}
```

```java
import java.util.Scanner;

public class SwitchExercise {
    public static void main(String[] args) {
        //1) 使用 switch 把小写类型的 char 型转为大写(键盘输入)。
        // 只转换 a, b, c, d, e. 其它的输出 "other"。
        Scanner scanner = new Scanner(System.in);
        char c = scanner.next().charAt(0);
        switch (c) {
            case 'a':
                System.out.println('A');
                break;
            case 'b':
                System.out.println('B');
                break;
            case 'c':
                System.out.println('C');
                break;
            case 'd':
                System.out.println('D');
                break;
            case 'e':
                System.out.println('E');
                break;
            default:
                System.out.println("other");
                break;
        }

        //2) 对学生成绩大于 60 分的，输出"合格"。
        // 低于 60 分的，输出"不合格"。(注：输入的成绩不能大于 100)
        int score = scanner.nextInt();
        if (score >= 0 && score <= 100) {
            int s = score / 60;
            switch (s) {
                case 1:
                    System.out.println("合格");
                    break;
                case 0:
                    System.out.println("不合格");
                    break;
                default:
                    System.out.println("分数错误");
                    break;
            }
        }

        //3) 根据用于指定月份，打印该月份所属的季节。
        // 3,4,5 春季 6,7,8 夏季 9,10,11 秋季 12, 1, 2 冬季
        int month = scanner.nextInt();
        switch (month) {
            case 3:
            case 4:
            case 5:
                System.out.println("春季");
                break;
            case 6:
            case 7:
            case 8:
                System.out.println("夏季");
                break;
            case 9:
            case 10:
            case 11:
                System.out.println("秋季");
                break;
            case 12:
            case 1:
            case 2:
                System.out.println("冬季");
                break;
        }
    }
}

```

switch 和 if 的比较 

1) 如果判断的具体数值不多，而且符合 byte、 short 、int、 char、enum[枚举]、String 这 6 种类型。虽然两个语句都可以使用，建议使用 swtich 语句。
2) 其他情况：对区间判断，对结果为 boolean 类型判断，使用 if，if 的使用范围更广。

## 循环控制

### for

```java
public class ForExercise {
    public static void main(String[] args) {
        //打印 1~100 之间所有是 9 的倍数的整数，统计个数及总和
        int sum = 0;
        int count = 0;
        int start = 1;
        int end = 100;
        for (int i = start; i < end; i++) {
            if (i % 9 == 0) {
                System.out.println(i);
                count++;
                sum += i;
            }
        }
        System.out.println("count: " + count);
        System.out.println("sum: " + sum);

        // 打印算式
        for (int i = 0, j = 5; i <= 5 && j >= 0; i++, j--) {
            System.out.println(i + " + " + j + " = " + (i + j));
        }
        int n=9;
        for (int i = 0; i < n; i++) {
            System.out.println(i + " + " + (n-i) + " = " + n);
        }
    }
}

```

### while

```java
public class WhileExercise {
    public static void main(String[] args){
        // 打印 1—100 之间所有能被 3 整除的数
        int start = 1;
        int end = 100;
        int step = 3;
        while (start <= end){
            if(start % step == 0){
                System.out.println(start);
            }
            start++;
        }
        // 打印 40—200 之间所有的偶数
        start = 40;
        end = 200;
        step = 2;
        while (start <= end){
            if(start % step == 0){
                System.out.println(start);
            }
            start++;
        }
    }
}

```

### do-while

```java
import java.util.Scanner;

public class DoWhileExercise {
    public static void main(String[] args) {
        //打印 1—100 [学生做]
        //计算 1—100 的和 [学生做]
        int start = 1;
        int end = 100;
        int sum = 0;
        do {
            System.out.println(start);
            sum += start;
            start++;
        } while (start <= end);
        System.out.println(sum);

        //统计 1---200 之间能被 5 整除但不能被 3 整除的个数
        start = 1;
        end = 200;
        int count = 0;
        do {
            if (start % 5 == 0 && start % 3 != 0) {
                System.out.println(start);
                count++;
            }
            start++;
        } while (start <= end);
        System.out.println(count);

        //如果李三不还钱，则老韩将一直使出五连鞭，直到李三说还钱
        //[System.out.println("老韩问：还钱吗？y/n")
        Scanner scanner = new Scanner(System.in);
        char c;
        do {
            System.out.println("老韩问：还钱吗？y/n");
            c = scanner.next().charAt(0);
        } while (c != 'y');
    }
}

```

## 多重循环控制

```java
import java.util.Scanner;

public class MulForExercise {
    public static void main(String[] args) {
        //统计 3 个班成绩情况，每个班有 5 名同学，
        //求出各个班的平均分和所有班级的平均分[学生的成绩从键盘输入]。
        //统计三个班及格人数，每个班有 5 名同学。
//        Scanner scanner = new Scanner(System.in);
//        double score ;
//        double avgScore=0.0 ;
//        double allAvgScore=0.0 ;
//        int count=0 ;
//        for(int i=0; i<3 ;i++){
//            for(int j=0; j<5; j++){
//                score = scanner.nextDouble();
//                avgScore+=score/5;
//                if(score>=60)   count++;
//            }
//            System.out.println("班级"+(i+1)+"的平均分为： "+ avgScore);
//            allAvgScore+=avgScore/3;
//            avgScore=0.0;
//        }
//        System.out.println("所有班级的平均分为： "+ allAvgScore);
//        System.out.println("总及格人数为： " + count );

        //九九乘法表
        for (int i = 1; i <= 9; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print(j + " * " + i + " = " + (i * j) + "\t");

            }
            System.out.println();
        }

    }
}

```

```java
public class Stars {
    public static void main(String[] args) {
        // 打印金字塔
//            *
//           ***
//          *****
//         *******
//        *********

        //i表示层数
        for (int i = 1; i <= 5; i++) {
            //输出空格
            for (int k = 1; k <= 5 - i; k++) {
                System.out.print(" ");
            }
            //输出*号
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }

        //打印空心金字塔
//            *
//           * *
//          *   *
//         *     *
//        *********
        int level = 5;
        for (int i = 1; i <= level; i++) {
            //打印两边空格
            for (int k = 1; k <= level - i; k++) {
                System.out.print(" ");
            }
            //输出*号和中间空格
            for (int j = 1; j <= 2 * i - 1; j++) {
                if (j == 1 || j == 2 * i - 1 || i == level) {
                    System.out.print("*");
                } else {
                    System.out.print(" ");
                }
            }
            System.out.println("");
        }
    }
}
```

## break

```java
import java.util.Scanner;

public class BreakExercise {
    public static void main(String[] args) {
        int chances = 3;
        for (int i = 1; i <= chances; i++) {
            Scanner scanner = new Scanner(System.in);
            System.out.println("输入用户名");
            String username = scanner.next();
            System.out.println("输入密码");
            String password = scanner.next();
            //第二种比较写法更推荐，可以避免空指针
            if (username.equals("麦子落") && "666".equals(password)) {
                System.out.println("登录成功");
                break;
            }
            System.out.println("你还有" + (chances - i) + "次机会");
        }

    }
}

```

## 本章小结

```java
public class ControlTest {
    public static void main(String[] args) {
        //homework1
        //某人有100000元，每经过一次路口，需要交费
        // 1)当现金>50000,每次交5%
        // 2)当<=50000,每次交1000
        // 使用while break方式完成
        double cash = 100000.0;
        //可通过次数
        int count = 0;
        while (true) {
            if (cash > 50000) {
                cash = 0.95 * cash;
                count++;
            } else if (cash >= 1000) {
                cash -= 1000;
                count++;
            } else {
                break;
            }
        }
        System.out.println("count:" + count);

        //homework2
        //判断一个整数是否是水仙花数  153 = 1*1*1 + 3*3*3 + 5*5*5
        for (int i = 100; i < 1000; i++) {
            int i1 = i / 100;
            int i2 = i % 100 / 10;
            int i3 = i % 10;
            if (i1 * i1 * i1 + i2 * i2 * i2 + i3 * i3 * i3 == i) {
                System.out.println(i + "是水仙花数");
            }
        }

        //homework3
        //输出1-100之间不能被5整除的数，每5个一行
        count = 0;
        for (int i = 1; i <= 100; i++) {
            if (i % 5 != 0) {
                count++;
                System.out.print(i + "\t");

                //判断一行5个是否完成
                //取余操作 不用每次让count从0开始计数
                if (count % 5 == 0) {
                    System.out.println("");
                }
            }
        }

        //homework4
        //输出小写的a-z和大写的Z-A
        for (char c1 = 'a'; c1 <= 'z'; c1++) {
            System.out.print(c1 + " ");
        }
        System.out.println("");
        for (char c2 = 'Z'; c2 >= 'A'; c2--) {
            System.out.print(c2 + " ");
        }
        System.out.println("");

        //homework5
        // 输出1-1/2+1/3-1/4....1/100
        double sum = 0;
        for (int i = 1; i <= 100; i++) {
            //分子要写出1.0
            if (i % 2 == 0) {
                sum -= 1.0 / i;
            } else {
                sum += 1.0 / i;
            }
        }
        System.out.println("sum = " + sum);

        //homework6
        //求1+(1+2)+(1+2+3)+...+(1+2+3+...+100)
        int sum6 = 0;
        for (int i = 1; i <= 100; i++) {
            for (int j = 1; j <= i; j++) {
                sum6 += j;
            }
        }
        System.out.println("sum6 = " + sum6);

    }
}

```



# 数组、排序和查找

## 数组

### 数组使用注意事项和细节 

1) 数组是多个相同类型数据的组合，实现对这些数据的统一管理 
2)  数组中的元素可以是任何数据类型，包括基本类型和引用类型，但是不能混用。 
3) 数组创建后，如果没有赋值，有默认值 int 0，short 0, byte 0, long 0, float 0.0,double 0.0，char \u0000，boolean false，String null 
4) 使用数组的步骤 1. 声明数组并开辟空间 2 给数组各个元素赋值 3 使用数组 
5) 数组的下标是从 0 开始的。
6) 数组下标必须在指定范围内使用，否则报：下标越界异常，比如 韩顺平循序渐进学 Java 零基础 第 148页 int [] arr=new int[5]; 则有效下标为 0-4 
7) 数组属引用类型，数组型数据是对象(object)

```java
public class ArrayExercise {
    public static void main(String[] args) {
        //创建一个 char 类型的 26 个元素的数组，分别放置'A'-'Z'。
        // 使用 for 循环访问所有元素并打印出来
        char[] array = new char[26];
        //放置
        for (int i = 0; i < array.length; i++) {
            array[i] = (char) ('A' + i);
        }
        //打印
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i] + " ");
        }
        System.out.println("");

        //求一个int[]的最大值，并得到对应的下标
        //求和和平均值
        int[] array2 = {4, -1, 8, 10, 23};
        int max = array2[0];
        int index = 0;
        int sum=0;
        double avg=0.0;
        for (int i = 0; i < array2.length; i++) {
            if (array2[i] > max) {
                max = array2[i];
                index = i;
            }
            sum+=array2[i];
        }
        avg=(double)sum/array2.length;
        System.out.println("最大值为: " + max + " 下标为: " + index);
        System.out.println("和为: " + sum + " 平均值为: " + avg);
    }
}

```

###  数组赋值机制 

1) 基本数据类型赋值，这个值就是具体的数据，而且相互不影响。 int n1 = 2; int n2 = n1; 
2) 数组在默认情况下是引用传递，赋的值是地址。

```java
public class ArrayAssign { 

	//编写一个main方法
	public static void main(String[] args) {

		//基本数据类型赋值, 赋值方式为值拷贝
		//n2的变化，不会影响到n1的值
		int n1 = 10;
		int n2 = n1;

		n2 = 80;
		System.out.println("n1=" + n1);//10
		System.out.println("n2=" + n2);//80

		//数组在默认情况下是引用传递，赋的值是地址，赋值方式为引用赋值
		//是一个地址 , arr2变化会影响到 arr1
		int[] arr1 = {1, 2, 3};
		int[] arr2 = arr1;//把 arr1赋给 arr2
		arr2[0] = 10;

		//看看arr1的值
		System.out.println("====arr1的元素====");
		for(int i = 0; i < arr1.length; i++) {
			System.out.println(arr1[i]);//10, 2, 3
		}

		System.out.println("====arr2的元素====");
		for(int i = 0; i < arr2.length; i++) {
			System.out.println(arr2[i]);//10, 2, 3
		}

	}
}
```

### 数组翻转

```java
public class ArrayReverse {
    public static void main(String[] args){

        int arr[] = {11, 22, 33, 44, 55, 66};
        int temp = 0;
        int len = arr.length;
        for (int i = 0; i < len / 2; i++) {
            temp = arr[i];
            arr[i] = arr[len - i - 1];
            arr[len - 1 - i] = temp;
        }
        for (int i = 0; i < len; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
        int []arr2 = new int[len];
        for(int i = len-1;i >= 0;i--) {
            arr2[len - 1 - i] = arr[i];
        }
        for (int i = 0; i < len; i++) {
            System.out.print(arr2[i] + " ");
        }

    }
}
```

### 数组缩减

```java
import java.util.Scanner;

public class ArrayReduce {
    public static void main(String[] args) {
        Scanner myScanner = new Scanner(System.in);
        //初始化数组
        int[] arr = {1, 2, 3, 4, 5};
        do {
            System.out.println("是否进行缩减 y/n");
            char key = myScanner.next().charAt(0);
            //如果输入 n ,就结束
            if (key == 'n') {
                break;
            }
            if (arr.length == 1) {
                System.out.println("只有一个元素，不再缩减");
                break;
            }
            int[] arrNew = new int[arr.length - 1];
            //遍历 arr 数组，依次将 arr 的元素拷贝到 arrNew 数组
            for (int i = 0; i < arr.length - 1; i++) {
                arrNew[i] = arr[i];
            }
            //让 arr 指向 arrNew,
            arr = arrNew;
            //输出 arr 看看效果
            System.out.println("====arr 缩减后元素情况====");
            for (int i = 0; i < arr.length; i++) {
                System.out.print(arr[i] + "\t");
            }
        } while (true);
        System.out.println("结束...");

    }
}

```

## 排序

```java
public class BubbleSort {
    public static void main(String[] args) {
        //冒泡排序
        int arr[] = {24, 69, 80, 57, 13, 84, 10, 5};
        int len = arr.length;
        int temp = 0;
        for (int j = len; j > 1; j--) {
            for (int i = 0; i < j - 1; i++) {
                if (arr[i] > arr[i + 1]) {
                    temp = arr[i];
                    arr[i] = arr[i + 1];
                    arr[i + 1] = temp;
                }
            }
        }
        for (int i = 0; i < len; i++) {
            System.out.print(arr[i] + " ");
        }

    }
}

```

## 查找

```java
import java.util.Scanner;

public class SeqSearch {
    public static void main(String[] args) {
        //顺序查找
        String arr[] = {"白毛", "金毛", "紫毛", "青毛", "黑毛"};
        Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        int index = -1;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i].equals(str)) {
                System.out.println(str + "找到了,下标为：" + i);
                index = i;
                break;
            }
        }
        if (index == -1) {
            System.out.println(str + "未找到");
        }

    }
}

```

## 二维数组

```java
public class TwoDimensionalArray {
    public static void main(String[] args) {
        //遍历二维数组，得到和
        int arr[][] = {{4, 6}, {1, 4, 5, 7}, {-2}};
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
            System.out.print("第" + (i + 1) + "个一维数组为：");
            for (int j = 0; j < arr[i].length; j++) {
                System.out.print(arr[i][j] + " ");
                sum += arr[i][j];
            }
            System.out.println();
        }
        System.out.println("和为：" + sum);

        //杨辉三角
//        1
//        1 1
//        1 2 1
//        1 3 3 1
//        1 4 6 4 1
//        1 5 10 10 5 1
        //设置行数n
        int n = 10;
        int yanghui[][] = new int[n][];
        for (int i = 0; i < n; i++) {
            yanghui[i] = new int[i + 1];
            for (int j = 0; j < yanghui[i].length; j++) {
                if (j == 0 || j == yanghui[i].length - 1) {
                    yanghui[i][j] = 1;
                } else {
                    yanghui[i][j] = yanghui[i - 1][j - 1] + yanghui[i - 1][j];
                }
            }
        }
        for (int i = 0; i < yanghui.length; i++) {
            for (int j = 0; j < yanghui[i].length; j++) {
                System.out.print(yanghui[i][j] + "\t");
            }
            System.out.println("");
        }
    }
}

```

## 本章小结

```java
public class ASFHomework {
    public static void main(String[] args) {
        //homework1
        // 将一个元素插入有序数组，要求插入后依然有序
        int arr[] = {10, 12, 45, 90};
        //寻找插入位置
        int index = arr.length;
        //要插入的元素
        int num = 23;
        for (int i = 0; i < arr.length; i++) {
            if (num < arr[i]) {
                index = i;
                break;
            }
        }
        //根据下标插入元素
        int[] newArr = new int[arr.length + 1];
        for (int i = 0, j = 0; i < newArr.length; i++) {
            if (i != index) {
                newArr[i] = arr[j];
                j++;
            } else {
                newArr[i] = num;
            }
        }
        //打印数组
        for (int i = 0; i < newArr.length; i++) {
            System.out.print(newArr[i] + " ");
        }
        System.out.println("");

        //homework2
        //随机生成10个整数（1-100）保存到数组，并倒序打印，
        //求平均值，最大值和最大值的下标，并查找是否有8
        int[] arr2 = new int[10];
        int sum2 = 0;
        int max2 = 0;
        int index2 = 0;
        for (int i = 0; i < arr2.length; i++) {
            arr2[i] = (int) (Math.random() * 100) + 1;
            sum2 += arr2[i];
            if (arr2[i] > max2) {
                max2 = arr2[i];
                index2 = i;
            }
        }
        //倒序打印
        //查找是否有8
        boolean ifHave8 = false;
        for (int i = arr2.length - 1; i >= 0; i--) {
            System.out.print(arr2[i] + " ");
            if (arr2[i] == 8) {
                ifHave8 = true;
            }
        }
        System.out.println();
        if (ifHave8 == true) {
            System.out.println("有8");
        } else {
            System.out.println("没有8");
        }

        //打印平均值，最大值和最大值的下标
        System.out.println("平均值为：" + (double) sum2 / arr2.length);
        System.out.println("最大值为: " + max2 + " 下标为： " + index2);
    }
}

```



# 面向对象编程（基础部分）

## 类与对象

### Java 内存的结构分析 

1) 栈：一般存放基本数据类型(局部变量) 
2) 堆：存放对象(Cat cat , 数组等) 
3) 方法区：常量池(常量，比如字符串)， 类加载信息

### Java 创建对象的流程简单分析 

```java
Person p = new Person(); 

p.name = “jack”; 

p.age = 10 
```

1) 先加载 Person 类信息(属性和方法信息, 只会加载一次) 
2)  在堆中分配空间, 进行默认初始化(看规则) 
3)  把地址赋给 p , p 就指向对象 
4) 进行指定初始化， 比如 p.name =”jack” p.age = 1

### 成员方法

```java
public class Method {

    public static void main(String[] args) {
        //编写类 AA ，有一个方法：
        // 判断一个数是奇数 odd 还是偶数, 返回 boolean
        AA a = new AA();
        int num = 9;
        boolean index = a.isOdd(num);
        if (index) {
            System.out.println(num + "是奇数");
        } else {
            System.out.println(num + "是偶数");
        }

        //根据行，列，字符打印
        a.print(4, 5, '$');
    }
}

class AA {
    public boolean isOdd(int num) {
        return num % 2 != 0;
    }

    public void print(int row, int col, char c) {
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                System.out.print(c);
            }
            System.out.println();
        }
    }
}
```

### 方法传参机制

1.基本数据类型，传递的是值（值拷贝），形参的任何改变不影响实参。

```java
public class MethodParameter01 {
    //编写一个 main 方法
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
//创建 AA 对象 名字 obj
        AA obj = new AA();
        obj.swap(a, b); //调用 swap
        System.out.println("main 方法 a=" + a + " b=" + b);//a=10 b=20
    }
}

class AA {
    public void swap(int a, int b) {
        System.out.println("\na 和 b 交换前的值\na=" + a + "\tb=" + b);//a=10 b=20
//完成了 a 和 b 的交换
        int tmp = a;
        a = b;
        b = tmp;
        System.out.println("\na 和 b 交换后的值\na=" + a + "\tb=" + b);//a=20 b=10
    }
}
```

2.引用类型传递的是地址（传递也是值，但是值是地址），可以通过形参影响实参！

3.成员方法返回类型是引用类型应用实例

```java
public class MethodParameter01 {
    //编写一个 main 方法
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.name = "John";
        p1.age = 18;
        Person p2 = new Person();
        p2=p2.copyPerson(p1);
        System.out.println("p1  "+p1.name+" "+p1.age);
        System.out.println("p2  "+p2.name+" "+p2.age);
        //比较看看是否为同一个对象
        //falas
        System.out.println(p1 == p2);
    }
}
class Person {
    String name;
    int age;
    public Person copyPerson(Person p1) {
        Person p2= new Person();
        p2.name = p1.name;
        p2.age = p1.age;
        return p2;
    }
}

```



### 方法递归调用

```java
public class recursion {
    public static void main(String[] args) {
        T t1 = new T();
        t1.test(4);

        System.out.println(t1.factorial(5));
    }
}

class T {
    public void test(int n) {
        if (n > 2) {
            test(n - 1);
        }
        System.out.println("n = " + n);
    }

    //阶乘
    public int factorial(int n) {
        if (n == 1) {
            return 1;
        } else {
            return factorial(n - 1) * n;
        }
    }
}
```

```java
package ClassAndObject;

public class RecursionExercise {
    public static void main(String[] args) {
        //使用递归的方式求出斐波那契数 1,1,2,3,5,8,13...
        // 给你一个整数 n，求出它的值是多少
        TT t1 = new TT();
        System.out.println(t1.fibonacci(5));

        /*
        猴子吃桃子问题：有一堆桃子，猴子第一天吃了其中的一半，并再多吃了一个！
        以后每天猴子都吃其中的一半，然后再多吃一个。当到第 10 天时，
        想再吃时（即还没吃），发现只有 1 个桃子了。问题：最初共多少个桃子？
        */
        System.out.println(t1.peach(1));

    }
}

class TT {
    public int fibonacci(int n) {
        if (n <= 2) {
            return 1;
        } else {
            return fibonacci(n - 1) + fibonacci(n - 2);
        }
    }

    public int peach(int day) {
        if (day == 10) {
            return 1;
        } else if (day >= 1 && day <= 9) {
            return peach(day + 1) * 2 + 2;
        } else {
            System.out.println("day 在 1-10");
            return -1;
        }
    }
}
```

#### 迷宫问题

```java
package ClassAndObject;

public class MiGong {
    public static void main(String[] args) {
        //创建迷宫
        int map[][] = new int[8][7];
        // 0表示可以走，1表示障碍物
        //将上下两行设置为1
        for (int i = 0; i < 7; i++) {
            map[0][i] = 1;
            map[7][i] = 1;
        }
        //将左右两列设置为1
        for (int i = 0; i < 8; i++) {
            map[i][0] = 1;
            map[i][6] = 1;
        }
        map[3][1] = 1;
        map[3][2] = 1;
        map[2][2] = 1;
        //打印当前迷宫地图
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 7; j++) {
                System.out.print(map[i][j] + " ");
            }
            System.out.println("");
        }
        //找路
        TMiGong t1 = new TMiGong();
        t1.findWay(map, 1, 1);
        System.out.println("找路后");
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 7; j++) {
                System.out.print(map[i][j] + " ");
            }
            System.out.println("");
        }
    }
}

class TMiGong {
    //findWay方法来找路劲
    //若找到，返回true
    //map是迷宫地图，i，j是当前位置
    //2表示可以走 3表示走过，但是走不通
    //当map[6][5]=2时，找到出路
    //找路策略：下 右 上 左
    public boolean findWay(int[][] map, int i, int j) {
        if (map[6][5] == 2) {
            return true;
        } else {
            if (map[i][j] == 0) {
                map[i][j] = 2;
                if (findWay(map, i + 1, j)) {
                    return true;
                } else if (findWay(map, i, j + 1)) {
                    return true;
                } else if (findWay(map, i - 1, j)) {
                    return true;
                } else if (findWay(map, i, j - 1)) {
                    return true;
                } else {
                    map[i][j] = 3;
                    return false;
                }
            } else {
                return false;
            }
        }
    }
}
```

#### 汉诺塔问题

```Java
package ClassAndObject;

public class HanoiTower {
    public static void main(String[] args) {
        //汉诺塔问题
        Tower tower = new Tower();
        tower.move(4, 'A', 'B', 'C');
    }

}

class Tower {
    //num 表示要移动的个数, a, b, c 分别表示 A 塔，B 塔,C塔
    public void move(int num, char a, char b, char c) {
        if (num == 1) {
            System.out.println(a + "->" + c);
        } else {
            //先移动上面所有的盘到 b, 借助 c
            move(num - 1, a, c, b);
            //(2)把最下面的这个盘，移动到 c
            System.out.println(a + "->" + c);
            //(3)再把 b 塔的所有盘，移动到 c ,借助a
            move(num - 1, b, a, c);
        }
    }
}
```

#### 八皇后问题

```java
package ClassAndObject;

public class Queen8 {
    public static void main(String[] args){
        Queen8 queen8 = new Queen8();
        queen8.check(0);
        System.out.printf("一共有%d解法", count);
        System.out.println();
        System.out.printf("一共判断冲突的次数%d次", judgeCount);
    }
    int max = 8;// 8个皇后摆放在8*8棋盘
    // 定义数组array, 保存皇后放置位置的结果,比如 arr = {0 , 4, 7, 5, 2, 6, 1, 3}
    int[] array = new int[max];
    static int count = 0;// 记录共有多少中摆放方式
    static int judgeCount = 0;// 记录判断了多少次

    /**
     * 编写一个方法，放置第n个皇后 特别注意: check 是 每一次递归时， 进入到check中都有 for(int i = 0; i < max;i++)
     * 因此会有回溯
     *
     * @param n
     */
    private void check(int n) {
        if (n == max) {// n = 8: 第n+1=9个皇后, 说明前8个皇后就已经放好
            print(); // 输出该情况的摆法
            return;// 结束
        }

        // 依次放入皇后，并判断是否冲突
        for (int i = 0; i < max; i++) {
            // 先把当前这个皇后 n , 放到该行的第(i+1)列
            array[n] = i;
            // 判断当放置第n个皇后到i列时，是否冲突
            if (judge(n)) { // 不冲突
                // 若不冲突，就接着放n+1个皇后,即开始递归
                check(n + 1); //
            }
            // 如果冲突，就继续执行 array[n] = i; 即将第n个皇后，放置在本行得 后移的一个位置
        }
    }

    /**
     * 判断当我们放置第n个皇后, 就去检测该皇后是否和前面已经摆放的皇后冲突
     *
     * @param n 表示第n个皇后
     * @return
     */
    private boolean judge(int n) {
        judgeCount++;
        for (int i = 0; i < n; i++) {
            // 说明
            // 1. array[i] == array[n] 表示判断 第n个皇后是否和前面的n-1个皇后在同一列
            // 2. Math.abs(n-i) == Math.abs(array[n] - array[i]) 表示判断第n个皇后是否和第i皇后是否在同一斜线
            // n = 1 放置第 2列:1 <---> n = 1 array[1] = 1
            // Math.abs(1-0) == 1 同理 Math.abs(array[n] - array[i]) = Math.abs(1-0) = 1
            // 3. 判断是否在同一行, 没有必要，因为 n 每次都在递增
            if (array[i] == array[n] || Math.abs(n - i) == Math.abs(array[n] - array[i])) {
                return false;
            }
        }
        return true;
    }

    // 写一个方法，可以将皇后摆放的位置输出
    private void print() {
        count++;
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i] + " ");
        }
        System.out.println();
    }
}

```



## 重载

1) 方法名：必须相同
2) 形参类型：必须不同（形参类型或个数或顺序，至少有一个不同，参数名无要求）
3) 返回类型：无要求

```java
public class OverLoad00 {
    public static void main(String[] args) {
        OverLoadExercise ole = new OverLoadExercise();
        ole.m(5);
        ole.m(4, 6);
        ole.m("hello world");

        System.out.println(ole.max(4, 10));
        System.out.println(ole.max(4.9, -10.8));
        System.out.println(ole.max(2.5, 5.2, 1.7));
    }
}

class OverLoadExercise {
    /*
    编写程序，类 OverLoadExercise 中定义三个重载方法并调用。方法名为 m。
    三个方法分别接收一个 int 参数、两个 int 参数、一个字符串参数。
    分别执行平方运算并输出结果，相乘并输出结果，输出字符串信息。
    在主类的 main ()方法中分别用参数区别调用三个方法。
    定义三个重载方法 max()，第一个方法，返回两个 int 值中的最大值，
    第二个方法，返回两个 double 值中的最大值，
    第三个方法，返回三个 double 值中的最大值，
    并分别调用三个方法
    */

    public void m(int n1) {
        System.out.println(n1 + "的平方 = " + n1 * n1);
    }

    public void m(int n1, int n2) {
        System.out.println(n1 + " * " + n2 + " = " + n1 * n2);
    }

    public void m(String s1) {
        System.out.println(s1);
    }

    public int max(int n1, int n2) {
        return n1 > n2 ? n1 : n2;
    }

    public double max(double n1, double n2) {
        return n1 > n2 ? n1 : n2;
    }

    public double max(double n1, double n2, double n3) {
        if (n1 > n2) {
            return n1 > n3 ? n1 : n3;
        } else {
            return n2 > n3 ? n2 : n3;
        }
    }
}
```

### 可变参数

java 允许将同一个类中多个同名同功能但参数个数不同的方法，封装成一个方法。

1. 可变参数的实参可以为0个或任意多个。
2. 可变参数的实参可以是数组。
3. 可变参数的本质就是数组。
4. 可变参数可以和普通类型的参数一起放在形参列表，但必须保证可变参数在最后。
5. 一个形参列表中只能出现一个可变参数。

```java
public class VarParameter {
    public static void main(String[] args) {
        VarParameterEXercise vpe = new VarParameterEXercise();
        //可变参数的实参可以为0个或任意多个。
        System.out.println(vpe.sum(5, 4, 3, 8, 1));
        //可变参数的实参可以是数组。
        int[] arr = {8, 1, 2, 3};
        System.out.println(vpe.sum(arr));

        System.out.println(vpe.showScore("Bob",60.2,87.1,88.8));
    }
}

class VarParameterEXercise {
    //计算 2 个数的和，3 个数的和 ， 4. 5， 。。
    //1. int... 表示接受的是可变参数，类型是 int ,即可以接收多个 int(0-多)
    //2. 使用可变参数时，可以当做数组来使用 即 nums 可以当做数组
    //3. 遍历 nums 求和即可

    public int sum(int... nums) {
        System.out.println("接收的参数个数=" + nums.length);
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            res += nums[i];
        }
        return res;
    }

    /*
    有三个方法，分别实现返回姓名和两门课成绩(总分)，
    返回姓名和三门课成绩(总分)，返回姓名和五门课成绩（总分）。
    封装成一个可变参数的方法
    */

    public String showScore(String name, double... scores) {
        double totalScore = 0.0;
        for (int i = 0; i < scores.length; i++) {
            totalScore += scores[i];
        }
        return name + " 有 " + scores.length + "门课的成绩总分为=" + totalScore;
    }
}
```

### 作用域

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207071110347.png)



## 构造器

基本语法：

 [修饰符] 方法名(形参列表){ 方法体; } 



1) 构造器的修饰符可以默认， 也可以是 public protected private 
2) 构造器没有返回值 
3) 方法名和类名字必须一样 
4) 参数列表和成员方法一样的规则 
5) 构造器的调用，由系统完成

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207071122294.png)

```java
public class Constructor {
    public static void main(String[] args) {
        ConstructorExercise ce = new ConstructorExercise("Jack", 22);
        ConstructorExercise ce2 = new ConstructorExercise();
        System.out.println(ce.name + ce.age);
        System.out.println(ce2.name + ce2.age);
    }
}

class ConstructorExercise {
    String name;
    int age;

    //构造器
    public ConstructorExercise() {
        System.out.println("无参构造器被调用~~");
        age = 18;
    }

    public ConstructorExercise(String pName, int pAge) {
        System.out.println("构造器被调用~~");
        name = pName;
        age = pAge;
    }
}

class ConstructorExercise01 {

}
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207071424624.png)

### this

1) this 关键字可以用来访问本类的属性、方法、构造器 
2) this 用于区分当前类的属性和局部变量
3) 访问成员方法的语法：this.方法名(参数列表);
4) 访问构造器语法：this(参数列表); 注意只能在构造器中使用(即只能在构造器中访问另外一个构造器, 必须放在第一 条语句) 
5) this 不能在类定义的外部使用，只能在类定义的方法中使用。

```java
public class This {
    public static void main(String[] args) {
        ThisPerson p1 = new ThisPerson("Jack", 20);
        ThisPerson p2 = new ThisPerson("Jack", 20);
        System.out.println(p1.compareTo(p2));
    }
}

class ThisPerson {
    String name;
    int age;

    public ThisPerson(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public boolean compareTo(ThisPerson other) {
        return this.name.equals(other.name) && other.age == this.age;
    }
}

```

## 本章小结

```java
package ClassAndObject;

import java.util.Random;
import java.util.Scanner;

public class Homework {
    public static void main(String[] args) {
        //homework01
        double[] dArr = {2.0, 1.8, 5.4, 1.7, 0.2};
        A01 a01 = new A01();
        System.out.println(a01.max(a01.max(dArr)));
        //homework02
        String[] sArr = {"draw", "front", "back", "raise"};
        System.out.println(a01.find("front", sArr));
        //homework03
        Book01 book = new Book01("笑傲江湖", 300);
        book.info();
        book.updatePrice();//更新价格
        book.info();
        //homework04
        int[] iArr = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        int[] newArr = a01.copyArr(iArr);
        for (int i = 0; i < newArr.length; i++) {
            System.out.print(newArr[i] + " ");
        }
        System.out.println();
        //homework05
        Circle01 circle = new Circle01(3.5);
        System.out.println("半径为 " + circle.r + " 周长为： "
                + circle.getGirth() + " 面积为： " + circle.getArea());
        //homework06
        Cale cale = new Cale(15, 3);
        System.out.println("和" + cale.add() + "\t差" + cale.sub()
                + "\t积" + cale.mul() + "\t商" + cale.div());
        //homework07
        PassObject po = new PassObject();
        Circle01 c = new Circle01(1);
        po.printAreas(c, 5);
        //homework08
        Game01 game = new Game01();
        for (int i = 0; i < 5; i++) {
            System.out.println("第 " + (i + 1) + " 局");
            System.out.println("请输入你的选择：0-石头 1-剪刀 2-布");
            Scanner scanner = new Scanner(System.in);
            int pSelect = scanner.nextInt();
            game.person(pSelect);
            game.print();
        }

    }
}

//homework01 编写类A01，定义方法max，实现求double数组的最大值，并返回。
//homework02 定义方法find，实现查找某字符串数组中的元素查找，并返回索引，如果找不到，返回-1。
//homework04 实现数组的复制功能

class A01 {
    public double max(double... values) {
        if (values.length == 0) {
            System.out.println("数组为空");
            return 0;
        } else {
            double max = values[0];
            for (int i = 1; i < values.length; i++) {
                if (values[i] > max) {
                    max = values[i];
                }
            }
            return max;
        }
    }

    public int find(String s1, String... values) {
        for (int i = 0; i < values.length; i++) {
            if (values[i].equals(s1)) {
                return i;
            }
        }
        return -1;
    }

    public int[] copyArr(int[] iArr) {
        int[] newArr = new int[iArr.length];
        for (int i = 0; i < iArr.length; i++) {
            newArr[i] = iArr[i];
        }
        return newArr;
    }
}

//homework03
//编写类Book01，定义方法updatePrice，实现更改某本书的价格。
//若价格>150，则改为150，>100改为100，否则不变。

class Book01 {
    String name;
    double price;

    public Book01(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public void updatePrice() {
        //如果方法中，没有 price 局部变量, this.price 等价 price
        if (price > 150) {
            price = 150;
        } else if (price > 100) {
            price = 100;
        }
    }

    //显示书籍情况
    public void info() {
        System.out.println("书名=" + this.name + " 价格=" + this.price);
    }
}

//homework05
//定义一个圆类，定义属性：半径，求周长方法，求面积方法

class Circle01 {
    double r;

    public Circle01(double r) {
        this.r = r;
    }

    public double getGirth() {
        return Math.PI * 2 * r;
    }

    public double getArea() {
        return Math.PI * r * r;
    }

}

//homework06
//定义一个Cale类，实现加减乘除

class Cale {
    double num1;
    double num2;

    public Cale(double num1, double num2) {
        this.num1 = num1;
        this.num2 = num2;
    }

    public double add() {
        return num1 + num2;
    }

    public double sub() {
        return num1 - num2;
    }

    public double mul() {
        return num1 * num2;
    }

    public Double div() {
        if (num2 == 0) {
            System.out.println("num2 不能为0");
            return null;
        } else {
            return num1 / num2;
        }
    }
}

//homework07
/*
题目要求：
(1) 定义一个Circle类，包含一个double型的radius属性代表圆的半径，findArea()方法返回圆的面积。
(2) 定义一个类PassObject，在类中定义一个方法printAreas()，该方法的定义如下：
     public void printAreas(Circle c, int times) 	//方法签名/声明
(3) 在printAreas方法中打印输出1到times之间的每个整数半径值，以及对应的面积。
     例如，times为5，则输出半径1，2，3，4，5，以及对应的圆面积。
(4) 在main方法中调用printAreas()方法，调用完毕后输出当前半径值。

 */

class PassObject {
    public void printAreas(Circle01 c, int times) {
        for (int i = 1; i <= times; i++) {
            c.r = i;
            System.out.println("半径为： " + (double) i + " 面积为：" + c.getArea());
        }
        System.out.println(c.r);
    }
}

//homework08
//猜拳 0石头，1剪刀，2布
class Game01 {
    //玩家胜利次数
    int win = 0;
    //玩家失败次数
    int lose = 0;
    //平局次数
    int draw = 0;
    //选择出什么
    int pSelect = 0;
    int cSelect = 0;

    public void person(int pSelect) {
        this.pSelect = pSelect;
    }

    //电脑出什么
    public void computer() {
        Random r = new Random();
        this.cSelect = r.nextInt(3);
    }

    //判断本局结果
    public void judge() {
        if ((pSelect == 0 && cSelect == 1) || (pSelect == 1 && cSelect == 2) || (pSelect == 2 && cSelect == 0)) {
            System.out.println("你赢了");
            this.win++;
        } else if (pSelect == cSelect) {
            System.out.println("平局");
            this.draw++;
        } else {
            System.out.println("你输了");
            this.lose++;
        }
    }

    //打印本局情况和当前战绩
    public void print() {
        computer();
        System.out.println("玩家出: " + pSelect + " 电脑出：" + cSelect);
        judge();
        System.out.println("玩家赢: " + win + " 局\t玩家输: " + lose + " 局\t平: " + draw + " 局");
    }
}
```



# 面向对象编程（中级部分）

## 包

作用：

1. 区分相同名字的类
2. 当类很多时，可以很好的管理类
3. 控制访问范围

```java
//package的作用是声明当前类所在的包，需要放在类(或者文件)的最上面，
// 一个类中最多只有一句package

package com.pkg;

//import指令 位置放在package的下面，在类定义前面,可以有多句且没有顺序要求
import java.util.Scanner;
import java.util.Arrays;

//...

//类定义
public class PkgDetail {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int[] arr = {0, -1, 1};
        Arrays.sort(args);
    }
}

```



## 访问修饰符

java 提供四种访问控制修饰符号，用于控制方法和属性(成员变量)的访问权限（范围）:

1) 公开级别:用 public 修饰,对外公开 。
2) 受保护级别:用 protected 修饰,对子类和同一个包中的类公开。
3) 默认级别:没有修饰符号,向同一个包的类公开。
4) 私有级别:用 private 修饰,只有类本身可以访问,不对外公开。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207072019663.png)



## 封装

封装（encapsulation）就是把抽象的数据【属性】和对数据的操作【方法】封装在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作【方法】，才能对数据进行操作。

### 封装的实现步骤



![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207072031099.png)

```java
package intermediate.encap;

public class Encap01 {
    public static void main(String[] args) {
        Person person = new Person();
        person.setName("John");
        person.setAge(300);
        person.setSalary(30000);
        System.out.println(person.info());

        Person person2 = new Person("Smith11111",40,20000);
        System.out.println(person2.info());

    }

}

/*
不能随便查看人的年龄,工资等隐私，并对设置的年龄进行合理的验证。年龄合理就设置，否则给默认年龄,
必须在 1-120, 年龄， 工资不能直接查看 ， name 的长度在 2-6 字符 之间
 */

class Person {
    public String name;//姓名
    private int age;//年龄
    private double salary;//工资

    //alt+insert生成set和get

    public Person() {
    }
    public Person(String name, int age, double salary) {
        setName(name);
        setAge(age);
        setSalary(salary);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if (name.length() >= 2&&name.length()<=6) {
            this.name = name;
        }else{
            System.out.println("name length must be between 2 and 6");
        }

    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        //判断
        if (age >= 1 && age <= 120) {
            this.age = age;
        } else {
            System.out.println("error:Age must in 1 to 120");
            this.age = 0;
        }
    }

    public double getSalary() {
        //对当前对象的权限判断
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    //返回属性信息
    public String info() {
        return "name=" + name + " age=" + age + " salary=" + salary;
    }
}
```

```java
package intermediate.encap;

public class AccountTest {
    public static void main(String[] args) {
        //创建Account
        Account account = new Account();
        account.setName("jack");
        account.setBalance(60);
        account.setPassword("123456");

        account.showInfo();
    }
}

/*
 * 创建程序,在其中定义两个类：Account 和 AccountTest 类体会 Java 的封装性。
 * Account 类要求具有属性：姓名（长度为 2 位 3 位或 4 位）、余额(必须>20)、
 * 密码（必须是六位）, 如果不满足，则给出提示信息，并给默认值(程序员自己定)
 * 通过 setXxx 的方法给 Account 的属性赋值。
 * 在 AccountTest中测试
 */

class Account{
    private String name;
    private double balance;
    private String password;

    public Account() {
    }
    public Account(String name, double balance, String password) {
        setName(name);
        setBalance(balance);
        setPassword(password);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if(name.length()>=2&&name.length()<=4){
            this.name = name;
        }
        else {
            System.out.println("name must be between 2 and 4 characters");
            this.name ="";
        }
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        if(balance>=20){
            this.balance = balance;
        }
        else{
            System.out.println("balance must >= 20");
            this.balance=0;
        }
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        if(password.length()==6){
            this.password = password;
        }
        else{
            System.out.println("password must be 6 characters,Default password is 000000");
            this.password= String.valueOf(000000);
        }
    }

    public void showInfo(){
        System.out.println("account information: \nname:" + name +
                " balance:" + balance + " password:" + password);
    }
}
```











## 继承

继承可以解决代码复用,让我们的编程更加靠近人类思维.当多个类存在相同的属性(变量)和方法时,可以从这些类中抽象出父类,在父类中定义这些相同的属性和方法，所有的子类不需要重新定义这些属性和方法，只需要通过 extends 来 声明继承父类即可。

基本语法：

class 子类 extends 父类{

}

（1）子类会自动拥有父类定义的属性和方法

（2）父类又叫超类，基类

（3）子类又叫派生类

细节：

1) 子类继承了所有的属性和方法，非私有的属性和方法可以在子类直接访问, 但是私有属性和方法不能在子类直接访问，要通过父类提供公共的方法去访问 
2) 子类必须调用父类的构造器， 完成父类的初始化 
3) 当创建子类对象时，不管使用子类的哪个构造器，默认情况下总会去调用父类的无参构造器，如果父类没有提供无参构造器，则必须在子类的构造器中用 super 去指定使用父类的哪个构造器完成对父类的初始化工作，否则，编译不会通过。
4) 如果希望指定去调用父类的某个构造器，则显式的调用一下 : super(参数列表) 
5) super 在使用时，必须放在构造器第一行(super 只能在构造器中使用) 
6) super() 和 this() 都只能放在构造器第一行，因此这两个方法不能共存在一个构造器 
7) java 所有类都是 Object 类的子类, Object 是所有类的基类. 
8) 父类构造器的调用不限于直接父类！将一直往上追溯直到 Object 类(顶级父类) 
9) 子类最多只能继承一个父类(指直接继承)，即 java 中是单继承机制。 思考：如何让 A 类继承 B 类和 C 类？ 【A 继承 B， B 继承 C】 
10) 不能滥用继承，子类和父类之间必须满足 is-a 的逻辑关系

```java
public class Extends01 {
    public static void main(String[] args) {
        C c = new C();
    }

}

class A {//A类

    public A() {
        System.out.println("我是A类");
    }
}

class B extends A { //B类,继承A类		//main方法中： C c =new C(); 输出么内容? 3min
    public B() {
        System.out.println("我是B类的无参构造");
    }

    public B(String name) {
        System.out.println(name + "我是B类的有参构造");
    }
}

class C extends B {   //C类，继承 B类
    public C() {
        this("hello");
        System.out.println("我是c类的无参构造");
    }

    public C(String name) {
        super("hahah");
        System.out.println("我是c类的有参构造");
    }
}

//run
我是A类
hahah我是B类的有参构造
我是c类的有参构造
我是c类的无参构造
```

```java
public class Extends02 {
    public static void main(String[] args) {
        /*
        编写 Computer 类，包含 CPU、内存、硬盘等属性，getDetails 方法用于返回 Computer 的详细信息
        编写 PC 子类，继承 Computer 类，添加特有属性【品牌 brand】
        编写 NotePad 子类，继承 Computer 类，添加特有属性【color】
        编写 Test 类，在 main 方法中创建 PC 和 NotePad 对象，分别给对象中特有的属性赋值，以及从 Computer 类继承的
        属性赋值，并使用方法并打印输出信息
         */
        PC pc = new PC("intel", 16, 500, "IBM");
        pc.printInfo();
        NotePad notePad = new NotePad("intel", 8, 256, "RED");
        notePad.printInfo();

    }
}
//----------------------
public class Computer {
    private String cpu;
    private int memory;
    private int disk;

    public Computer() {
    }

    public Computer(String cpu, int memory, int disk) {
        this.cpu = cpu;
        this.memory = memory;
        this.disk = disk;
    }

    //返回Computer信息
    public String getDetails() {
        return "cpu=" + cpu + ", memory=" + memory + ", disk=" + disk;
    }

    public String getCpu() {
        return cpu;
    }

    public void setCpu(String cpu) {
        this.cpu = cpu;
    }

    public int getMemory() {
        return memory;
    }

    public void setMemory(int memory) {
        this.memory = memory;
    }

    public int getDisk() {
        return disk;
    }

    public void setDisk(int disk) {
        this.disk = disk;
    }
}
//----------------------
public class PC extends Computer {
    private String brand;

    public PC(String cpu, int memory, int disk, String brand) {
        super(cpu, memory, disk);
        this.brand = brand;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public void printInfo() {
        System.out.println("PC信息: ");
        System.out.println(getDetails() + ", brand=" + brand);
    }
}
//----------------------
public class NotePad extends Computer {
    private String color;

    public NotePad(String cpu, int memory, int disk, String color) {
        super(cpu, memory, disk);
        this.color = color;
    }

    public void printInfo() {
        System.out.println("NotePad信息: ");
        System.out.println(getDetails() + ", color=" + color);
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }
}
```

## super关键字

super 代表父类的引用，用于访问父类的属性、方法、构造器。

1.访问父类的属性，但不能访问父类的private属性

​         super.属性名；

2.访问父类的方法，不能访问父类的private方法

​         super.方法名（参数列表）;

3.访问父类的构造器

​         super（参数列表）；

### super 和 this 的比较

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207091601893.png)

## 方法重写/覆盖（override）

方法覆盖（重写）就是子类有一个方法，和父类的某个方法的名称、返回类型、参数一样，那么我们就说子类的这个方法覆盖了父类的方法。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207091610481.png)

```java
public class TestOverride {
    public static void main(String[] args) {
        /*
        1) 编写一个 Person 类，包括属性/private（name、age），构造器、方法 say(返回自我介绍的字符串）。
        2) 编写一个 Student 类，继承 Person 类，增加 id、score 属性/private，以及构造器，定义 say 方法(返回自我介绍的信息)。
        3) 在 main 中,分别创建 Person 和 Student 对象，调用 say 方法输出自我介绍
         */
        Person p1 = new Person("fzy",16);
        Student p2 = new Student("mzl",23,"0335",99.9);
        System.out.println(p1.say());
        System.out.println(p2.say());
    }
}
//---------------
public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
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

    public String say() {
        return "name: " + name + " age: " + age;
    }
}
//---------------
public class Student extends Person {
    private String ID;
    double score;

    public Student() {
    }

    public Student(String name, int age, String ID, double score) {
        super(name, age);
        this.ID = ID;
        this.score = score;
    }

    public String getID() {
        return ID;
    }

    public void setID(String ID) {
        this.ID = ID;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }

    public String say() {
        return super.say() + " ID: " + ID + " score: " + score;
    }
}

```



## 多态

多态（polymorphic):方法和对象具有多种形态。

1.方法的多态

重写和重载就体现了多态

2.对象的多态

（1）一个对象的编译类型和运行类型可以不一致

（2）编译类型在定义对象时，就确定了，不能改变

（3）运行类型是可以变化的．

（4）编译类型看定义时＝号的左边，运行类型看＝号的右边

```java
Animal animal＝new Dog（）：//animal 编译类型是Animal，运行类型Dog 

animal ＝ new Cat（）；//animal的运行类型变成了Cat，编译类型仍然是 Animal
```

要点：

- 多态的前提是：两个对象(类)存在继承关系 

- 多态的向上转型：

1）本质：父类的引用指向了子类的对象

2）语法：父类类型 引用名 ＝ new 子类类型（）；

3）特点：编译类型看左边，运行类型看右边。

可以调用父类中的所有成员（需遵守访问权限），不能调用子类中特有成员；最终运行效果看子类的具体实现！

- 多态的向下转型：

1）语法：子类类型 引用名＝（子类类型）父类引用；

2）只能强转父类的引用，不能强转父类的对象

3）要求父类的引用必须指向的是当前目标类型的对象

4）当向下转型后，可以调用子类类型中所有的成员

- 属性没有重写之说：属性的值看`编译类型`

```java
public class PolyDetail02 {
    public static void main(String[] args) {
        //属性没有重写之说！属性的值看编译类型
        Base base = new Sub();//向上转型
        System.out.println(base.count);// ？ 看编译类型 10
        Sub sub = new Sub();
        System.out.println(sub.count);//?  20
    }
}

class Base { //父类
    int count = 10;//属性
}
class Sub extends Base {//子类
    int count = 20;//属性
}

```

- instanceOf 比较操作符，用于判断对象的`运行类型`是否为 XX 类型或 XX 类型的子类型

```java
public class PolyDetail03 {
    public static void main(String[] args) {
        BB bb = new BB();
        System.out.println(bb instanceof  BB);// true
        System.out.println(bb instanceof  AA);// true

        //aa 编译类型 AA, 运行类型是BB
        //BB是AA子类
        AA aa = new BB();
        System.out.println(aa instanceof AA);// true
        System.out.println(aa instanceof BB);// true

        Object obj = new Object();
        System.out.println(obj instanceof AA);//false
        String str = "hello";
        //System.out.println(str instanceof AA);
        System.out.println(str instanceof Object);//true
    }
}

class AA {} //父类
class BB extends AA {}//子类
```

```java
package intermediate.poly;

public class PolyExercise {
    public static void main(String[] args) {
        Sub s = new Sub();
        System.out.println(s.count);//20
        s.display();//20
        Base b = s;
        System.out.println(b == s);//T
        System.out.println(b.count);//10
        b.display();//20
    }

}

class Base {//父类
    int count = 10;

    public void display() {
        System.out.println(this.count);
    }
}

class Sub extends Base {//子类
    int count = 20;

    public void display() {
        System.out.println(this.count);
    }
}


```

### 动态绑定

1.当调用对象方法时，该方法会和该对象的内存地址/运行类型绑定

2.当调用对象属性时，没有动态绑定机制，哪里声明，哪里使用

```java
public class DynamicBinding {
    public static void main(String[] args) {
        //a 的编译类型 A, 运行类型 B
        A a = new B();//向上转型
        System.out.println(a.sum());//?40 -> 30
        System.out.println(a.sum1());//?30-> 20
    }
}

class A {//父类
    public int i = 10;
    //动态绑定机制:

    public int sum() {//父类sum()
        return getI() + 10;//20 + 10
    }

    public int sum1() {//父类sum1()
        return i + 10;//10 + 10
    }

    public int getI() {//父类getI
        return i;
    }
}

class B extends A {//子类
    public int i = 20;

//    public int sum() {
//        return i + 20;
//    }

    public int getI() {//子类getI()
        return i;
    }

//    public int sum1() {
//        return i + 10;
//    }
}
```

### 多态数组

```java
public class PloyArray {
    public static void main(String[] args) {
        /*
        现有一个继承结构如下：要求创建 1 个 Person 对象、2 个 Student 对象和
        2 个 Teacher 对象, 统一放在数组中，并调用每个对象say 方法
        */
        Person[] persons = new Person[5];
        persons[0] = new Person("jack", 20);
        persons[1] = new Student("mary", 18, 100);
        persons[2] = new Student("smith", 19, 30.1);
        persons[3] = new Teacher("scott", 30, 20000);
        persons[4] = new Teacher("king", 50, 25000);
        for (int i = 0; i < persons.length; i++) {
            // person[i]的编译类型为Person，运行类型为元素类型
            System.out.println(persons[i].say());
            if(persons[i] instanceof Student){
                ((Student)persons[i]).study();
            }
            else if(persons[i] instanceof Teacher){
                ((Teacher) persons[i]).teach();
            }
            else{
                System.out.println("不是学生或者老师");
            }
        }
    }
}
//----------------
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
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

    public String say() {
        return "name: " + name + " age: " + age;
    }
}
//----------------
public class Student extends Person {
    double score;

    public Student(String name, int age, double score) {
        super(name, age);
        this.score = score;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }

    public String say() {
        return super.say() + " score: " + score;
    }

    public void study(){
        System.out.println("Student " + getName() + " is studying...");
    }
}
//----------------
public class Teacher extends Person {
    double salary;

    public Teacher(String name, int age, double salary) {
        super(name, age);
        this.salary = salary;
    }

    public double getScore() {
        return salary;
    }

    public void setScore(double score) {
        this.salary = score;
    }

    public String say() {
        return super.say() + " salary: " + salary;
    }

    public void teach(){
        System.out.println("Teacher " + getName() + " is teaching...");
    }
}
```

### 多态参数

方法定义的形参类型为父类类型，实参类型允许为子类类型。

```java
package intermediate.poly.polyparameter_;

public class PloyParameter {
    public static void main(String[] args) {
        /*
        定义员工类Employee，包含姓名和月工资（private），以及计算年工资getAnnual
        的方法。普通员工和经理继承了员工，经理类多了奖金bonus属性和管理manage方法，
        普通员工多了work方法，普通员工和经理类要求分别重写getAnnual方法

        测试类中添加一个方法showEmpAnnual(Employee e),实现获取任何对象的
        年工资，并在main方法中调用该方法。

        测试类中添加一个方法，testWork,普通员工调用work方法，经理调用manage方法

         */
        Worker tom = new Worker("tom", 5000);
        Manager smith = new Manager("smith", 10000, 50000);
        PloyParameter ploy = new PloyParameter();
        ploy.showEmpAnnual(tom);
        ploy.showEmpAnnual(smith);
        ploy.testWork(tom);
        ploy.testWork(smith);

    }

    public void showEmpAnnual(Employee e) {
        System.out.println(e.getAnnual());
    }

    public void testWork(Employee e) {
        if (e instanceof Worker) {
            ((Worker) e).work();
        } else if (e instanceof Manager) {
            ((Manager) e).manage();
        }
    }
}

class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
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

    public double getAnnual() {
        return salary * 12;
    }
}

class Worker extends Employee {
    public Worker(String name, double salary) {
        super(name, salary);
    }

    public void work() {
        System.out.println("Worker" + getName() + "正在工作");
    }

    public double getAnnual() {
        //普通员工没有额外收入
        return super.getAnnual();
    }

}

class Manager extends Employee {
    private double bonus;

    public Manager(String name, double salary, double bonus) {
        super(name, salary);
        this.bonus = bonus;
    }

    public double getAnnual() {
        return bonus + super.getAnnual();
    }

    public void manage() {
        System.out.println("Manager" + getName() + "正在管理工作");
    }
}
```



## Object类详解

### equals方法

==与equals的对比

1.==：既可以判断基本类型，又可以判断引用类型

2.==：如果判断基本类型，判断的是值是否相等。示例：int i＝10；double d＝10.0；

3.==：如果判断引用类型，判断的是地址是否相等，即判定是不是同一个对象

4.equals：是Object类中的方法，只能判断引用类型

5.默认判断的是地址是否相等，子类中往往重写该方法，用于判断内容是否相等。比如Integer，String

```java
public class Equals01 {
    public static void main(String[] args) {
        Integer x = new Integer(1000);
        Integer y = new Integer(1000);
        System.out.println(x==y); //false
        System.out.println(x.equals(y)); //true

        String s1 = new String("hello");
        String s2 = new String("hello");
        System.out.println(s1==s2); //false
        System.out.println(s1.equals(s2)); //true
    }
}
```

### hashCode

1) 提高具有哈希结构的容器的效率！ 
2) 两个引用，如果指向的是同一个对象，则哈希值肯定是一样的！
3) 两个引用，如果指向的是不同对象，则哈希值是不一样的 
4)  哈希值主要根据地址号来的！但不能完全将哈希值等价于地址。 
5)  在集合中 hashCode 如果需要的话，也会重写,。

```java
package intermediate.Object_;

public class HashCode_ {
    public static void main(String[] args) {
        HC hc = new HC();
        HC hc2 = new HC();
        HC hc3 = hc;
        System.out.println(hc.hashCode());
        System.out.println(hc2.hashCode());
        System.out.println(hc3.hashCode());
    }
}

class HC {
}
//run
1324119927
990368553
1324119927
```

### toString

1) 基本介绍 默认返回：全类名+@+哈希值的十六进制，【查看 Object 的 toString 方法】 子类往往重写 toString 方法，用于返回对象的属性信息
2) 重写 toString 方法，打印对象或拼接对象时，都会自动调用该对象的 toString 形式.
3)  当直接输出一个对象时，toString 方法会被默认的调用, 比如 System.out.println(monster)； 就会默认调用 monster.toString()

```java
public class ToString_ {
    public static void main(String[] args) {
        Monster monster = new Monster("小妖怪", "巡山的", 1000);
        System.out.println(monster.toString() + " hashcode=" + monster.hashCode());

        System.out.println("==当直接输出一个对象时，toString 方法会被默认的调用==");
        System.out.println(monster); //等价 monster.toString()

    }
}

class Monster {
    private String name;
    private String job;
    private double sal;

    public Monster(String name, String job, double sal) {
        this.name = name;
        this.job = job;
        this.sal = sal;
    }

    @Override
    public String toString() {
        return "Monster{" +
                "name='" + name + '\t' +
                ", job='" + job + '\t' +
                ", sal=" + sal +
                '}';
    }
}
```

### finalize

1) 当对象被回收时，系统自动调用该对象的 finalize 方法。子类可以重写该方法，做一些释放资源的操作。
2) 什么时候被回收：当某个对象没有任何引用时，则 jvm 就认为这个对象是一个垃圾对象，就会使用垃圾回收机制来销毁该对象，在销毁该对象前，会先调用 finalize 方法。
3) 垃圾回收机制的调用，是由系统来决定(即有自己的 GC 算法), 也可以通过 System.gc() 主动触发垃圾回收机制。



## 断点调试

```java
//数组越界异常
public class Debug01 {
    public static void main(String[] args) {
        int[] arr = {1, 10, -1};
        for (int i = 0; i <= arr.length; i++) {
            System.out.println(arr[i]);
        }
        System.out.println("--------------------------------");
    }
}
//run
1
10
-1
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
	at intermediate.debug_.Debug01.main(Debug01.java:7)
    
//debug对象创建的过程
//debug动态绑定
```



## 零钱通项目

1.项目需求说明

使用 Java 开发 零钱通项目 , 可以完成收益入账，消费，查看明细，退出系统等功能。

2.项目的界面

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207111205079.png)

3.项目代码实现

```java
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

public class OOP_ {
    //1. 先完成显示菜单，并可以选择菜单，给出对应提示
    //2. 完成零钱通明细
    //3. 完成收益入账
    //4. 消费
    //5. 退出
    //6. 用户输入4退出时，给出提示"你确定要退出吗? y/n"，必须输入正确的y/n ，否则循环输入指令，直到输入y 或者 n
    //7. 在收益入账和消费时，判断金额是否合理，并给出相应的提示

    boolean loop = true;
    Scanner scanner = new Scanner(System.in);
    String key = "";

    //2. 完成零钱通明细
    //(1) 可以把收益入账和消费，保存到数组 (2) 可以使用对象 (3) 简单的话可以使用String拼接
    String details = "-----------------零钱通明细------------------";

    //3. 完成收益入账
    double money = 0;
    double balance = 0;
    // date 是 java.util.Date 类型，表示日期
    Date date = null;
    //日期格式化
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");

    //4. 消费
    //定义新变量，保存消费的原因
    String note = "";

    public void showMenu() {
        do {
            System.out.println("\n================零钱通菜单===============");
            System.out.println("\t\t\t1 零钱通明细");
            System.out.println("\t\t\t2 收益入账");
            System.out.println("\t\t\t3 消费");
            System.out.println("\t\t\t4 退     出");

            System.out.print("请选择(1-4): ");
            key = scanner.next();

            switch (key) {
                case "1":
                    this.detail();
                    break;
                case "2":
                    this.income();
                    break;
                case "3":
                    this.pay();
                    break;
                case "4":
                    //用户输入4退出时，给出提示"你确定要退出吗? y/n"，必须输入正确的y/n
                    this.exit();
                    break;
                default:
                    System.out.println("选择有误，请重新选择");
            }

        } while (loop);

        System.out.println("-----退出了零钱通项目-----");
    }

    //完成零钱通明细
    public void detail() {
        System.out.println(details);
    }

    //完成收益入账
    public void income() {
        System.out.print("收益入账金额:");
        money = scanner.nextDouble();
        //找出不正确的金额条件，然后给出提示, 就直接return
        if (money <= 0) {
            System.out.println("收益入账金额 需要 大于 0");
            return; //退出方法，不在执行后面的代码。
        }
        //找出正确金额的条件
        balance += money;
        //拼接收益入账信息到 details
        date = new Date(); //获取当前日期
        details += "\n收益入账\t+" + money + "\t" + sdf.format(date) + "\t" + balance;

    }

    //消费
    public void pay() {
        System.out.print("消费金额:");
        money = scanner.nextDouble();
        //找出金额不正确的情况
        if (money <= 0 || money > balance) {
            System.out.println("你的消费金额 应该在 0-" + balance);
            return;
        }
        System.out.print("消费说明:");
        note = scanner.next();
        balance -= money;
        //拼接消费信息到 details
        date = new Date(); //获取当前日期
        details += "\n" + note + "\t-" + money + "\t" + sdf.format(date) + "\t" + balance;
    }

    //退出
    public void exit() {
        //用户输入4退出时，给出提示"你确定要退出吗? y/n"，必须输入正确的y/n ，
        // 否则循环输入指令，直到输入y 或者 n
        String choice = "";
        while (true) { //要求用户必须输入y/n ,否则就一直循环
            System.out.println("你确定要退出吗? y/n");
            choice = scanner.next();
            if ("y".equals(choice) || "n".equals(choice)) {
                break;
            }
        }
        //当用户退出while ,进行判断
        if (choice.equals("y")) {
            loop = false;
        }
    }
}

```

```java
public class SmallChangeSys {

    public static void main(String[] args) {
        System.out.print("\t\t\t\t欢迎使用");
        new OOP_().showMenu();
    }
}
```

## 本章小结

```java
public class Homework07 {
    /*
    （1）做一个Student类，Student类有名称（name），性别（sex），年龄 （age），学号（stu＿id），做合理封装，通过构造器在创建对象时将4个属性赋值。
    （2）写一个Teacher类，Teacher类有名称（name），性别（sex），年龄 （age），工龄（work age），做合理封装，通过构造器在创建对象时将4个属性赋值。
    （3）抽取一个父类Person类，将共同属性和方法放到Person类
    （4）学生需要有学习的方法（study），在方法里写生“我承诺，我会好好学习。”
    （5） 教师需要有教学的方法（teach），在方法里写上“我承诺，我会认真教学。”
    （6）学生和教师都有玩的方法（play），学生玩的是足球，老师玩的是象棋，此方法是返回字符串的，分别返回“xx爱玩足球”和“xx爱玩象棋”（其中xx分别代表学生和老师的姓名）。
    （7）定义多态数组，里面保存2个学生和2个教师，要求按年龄从高到低排序
    （8）定义方法，形参为Person类型，功能：调用学生的study或教师的
     */

    public static void main(String[] args) {
        Person[] persons = new Person[4];
        persons[0] = new Student("张三", '男', 19, "00220711");
        persons[1] = new Student("李四", '女', 18, "00220712");
        persons[2] = new Teacher("王五", '男', 40, 12);
        persons[3] = new Teacher("刘六", '女', 35, 10);

        //按年龄从高到低排序
        Person tmp = new Person();
        for (int i = 0; i < persons.length - 1; i++) {
            for (int j = 0; j < persons.length - 1 - i; j++) {
                if (persons[j].getAge() < persons[j + 1].getAge()) {
                    tmp = persons[j];
                    persons[j] = persons[j + 1];
                    persons[j + 1] = tmp;
                }
            }
        }
        Homework07 h = new Homework07();
        for (int i = 0; i < persons.length - 1; i++) {
            h.test(persons[i]);
            System.out.println("-------------------");
        }
    }

    //打印信息
    public void test(Person p) {
        if (p instanceof Student) {
            p.printInfo();
            ((Student) p).study();
        } else if (p instanceof Teacher) {
            p.printInfo();
            ((Teacher) p).teach();
        }
    }
}

```

Person类

```java
public class Person {
    private String name;
    private char sex;
    private int age;

    public Person() {
    }

    public Person(String name, char sex, int age) {
        this.name = name;
        this.sex = sex;
        this.age = age;
    }

    public String play() {
        return this.name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void printInfo() {
        System.out.print("姓名：" + this.name + "\n年龄：" + this.age + "\n性别：" + this.sex + "\n");
    }
}

```

Student类

```java
public class Student extends Person {
    private String stu_id;

    public Student(String name, char sex, int age, String stu_id) {
        super(name, sex, age);
        this.stu_id = stu_id;
    }

    public String play() {
        return super.play() + "爱玩足球";
    }

    public void study() {
        System.out.println("我会好好学习");
    }

    public String getStu_id() {
        return stu_id;
    }

    public void setStu_id(String stu_id) {
        this.stu_id = stu_id;
    }

    public void printInfo() {
        System.out.println("学生的信息");
        super.printInfo();
        System.out.println("学号：" + this.stu_id);
        //this.study();
        System.out.println(this.play());
    }
}

```

Teacher类

```java
public class Teacher extends Person {
    private int work_age;

    public Teacher(String name, char sex, int age, int work_age) {
        super(name, sex, age);
        this.work_age = work_age;
    }

    public String play() {
        return super.play() + "爱玩象棋";
    }

    public void teach() {
        System.out.println("我会认真教课");
    }

    public int getWork_age() {
        return work_age;
    }

    public void setWork_age(int work_age) {
        this.work_age = work_age;
    }

    public void printInfo() {
        System.out.println("老师的信息");
        super.printInfo();
        System.out.println("工龄：" + this.work_age);
        //this.teach();
        System.out.println(this.play());
    }
}

```

# 房屋出租系统

## 界面

1.主菜单

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121029078.png)

2.新增房源

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121038224.png)

3.查找房源

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121039702.png)

4.删除房源

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121041354.png)

5.修改房源

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121039792.png)

6.房源列表

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121037263.png)

7.退出

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207121041083.png)

## 设计

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202207111751492.png)

## 实现

```java
public class HouseRentApp {
    public static void main(String[] args) {
        //创建HouseView对象,并且显示主菜单，是整个程序的入口
        new HouseView().mainMenu();
        System.out.println("===你退出房屋出租系统==");
    }
}

```



```java
//class House
public class House {
    private int id;
    private String name;
    private String phone;
    private String address;
    private int rent;
    private String state;

    public House(int id, String name, String phone, String address, int rent, String state) {
        this.id = id;
        this.name = name;
        this.phone = phone;
        this.address = address;
        this.rent = rent;
        this.state = state;
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

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public int getRent() {
        return rent;
    }

    public void setRent(int rent) {
        this.rent = rent;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }

    @Override
    public String toString() {
        return id +
                "\t" + name +
                "\t" + phone +
                "\t" + address +
                "\t" + rent +
                "\t" + state;
    }
}
```

```java
//class HouseView
/**
 * 1. 显示界面
 * 2. 接收用户的输入
 * 3. 调用HouseService完成对房屋信息的各种操作
 */
public class HouseView {

    private boolean loop = true;//控制显示菜单
    private char key = ' ';//接收用户选择
    HouseService houseService = new HouseService(10);

    //根据id修改房屋信息
    public void update() {
        System.out.println("=============修改房屋信息============");
        System.out.println("请选择待修改房屋编号(-1表示退出)");
        int updateId = Utility.readInt();
        if (updateId == -1) {
            System.out.println("=============你放弃修改房屋信息============");
            return;
        }

        //根据输入得到updateId，查找对象
        //返回的是引用类型[即:就是数组的元素]
        House house = houseService.findById(updateId);
        if (house == null) {
            System.out.println("=============修改房屋信息编号不存在..============");
            return;
        }

        System.out.print("姓名(" + house.getName() + "): ");
        String name = Utility.readString(8, "");//这里如果用户直接回车表示不修改该信息,默认""
        if (!"".equals(name)) {//修改
            house.setName(name);
        }

        System.out.print("电话(" + house.getPhone() + "):");
        String phone = Utility.readString(12, "");
        if (!"".equals(phone)) {
            house.setPhone(phone);
        }
        System.out.print("地址(" + house.getAddress() + "): ");
        String address = Utility.readString(18, "");
        if (!"".equals(address)) {
            house.setAddress(address);
        }
        System.out.print("租金(" + house.getRent() + "):");
        int rent = Utility.readInt(-1);
        if (rent != -1) {
            house.setRent(rent);
        }
        System.out.print("状态(" + house.getState() + "):");
        String state = Utility.readString(3, "");
        if (!"".equals(state)) {
            house.setState(state);
        }
        System.out.println("=============修改房屋信息成功============");

    }

    //根据id查找房屋信息
    public void findHouse() {
        System.out.println("=============查找房屋信息============");
        System.out.print("请输入要查找的id: ");
        int findId = Utility.readInt();
        //调用方法
        House house = houseService.findById(findId);
        if (house != null) {
            System.out.println(house);
        } else {
            System.out.println("=============查找房屋信息id不存在============");
        }
    }

    //完成退出确认
    public void exit() {
        //这里我们使用Utility提供方法，完成确认
        char c = Utility.readConfirmSelection();
        if (c == 'Y') {
            loop = false;
        }
    }

    //编写delHouse() 接收输入的id,调用Service 的del方法
    public void delHouse() {
        System.out.println("=============删除房屋信息============");
        System.out.print("请输入待删除房屋的编号(-1退出):");
        int delId = Utility.readInt();
        if (delId == -1) {
            System.out.println("=============放弃删除房屋信息============");
            return;
        }
        //注意该方法本身就有循环判断的逻辑,必须输出Y/N
        char choice = Utility.readConfirmSelection();
        if (choice == 'Y') {//真的删除
            if (houseService.del(delId)) {
                System.out.println("=============删除房屋信息成功============");
            } else {
                System.out.println("=============房屋编号不存在，删除失败============");
            }
        } else {
            System.out.println("=============放弃删除房屋信息============");
        }

    }

    //编写addHouse() 接收输入，创建House对象，调用add方法
    public void addHouse() {
        System.out.println("=============添加房屋============");
        System.out.print("姓名: ");
        String name = Utility.readString(8);
        System.out.print("电话: ");
        String phone = Utility.readString(12);
        System.out.print("地址: ");
        String address = Utility.readString(16);
        System.out.print("月租: ");
        int rent = Utility.readInt();
        System.out.print("状态: ");
        String state = Utility.readString(3);
        //创建一个新的House对象, 注意id 是系统分配的，
        House newHouse = new House(0, name, phone, address, rent, state);
        if (houseService.add(newHouse)) {
            System.out.println("=============添加房屋成功============");
        } else {
            System.out.println("=============添加房屋失败============");
        }
    }

    //编写listHouses()显示房屋列表
    public void listHouses() {
        System.out.println("=============房屋列表============");
        System.out.println("编号\t\t房主\t\t电话\t\t地址\t\t月租\t\t状态(未出租/已出租)");
        House[] houses = houseService.list();//得到所有房屋信息
        for (int i = 0; i < houses.length; i++) {
            if (houses[i] == null) {//如果为null,就不用再显示了
                break;
            }
            System.out.println(houses[i]);
        }
        System.out.println("=============房屋列表显示完毕============");

    }


    //显示主菜单
    public void mainMenu() {
        do {
            System.out.println("\n=============房屋出租系统菜单============");
            System.out.println("\t\t\t1 新 增 房 源");
            System.out.println("\t\t\t2 查 找 房 屋");
            System.out.println("\t\t\t3 删 除 房 屋 信 息");
            System.out.println("\t\t\t4 修 改 房 屋 信 息");
            System.out.println("\t\t\t5 房 屋 列 表");
            System.out.println("\t\t\t6 退      出");
            System.out.print("请输入你的选择(1-6): ");
            key = Utility.readChar();
            switch (key) {
                case '1':
                    addHouse();
                    break;
                case '2':
                    findHouse();
                    break;
                case '3':
                    delHouse();
                    break;
                case '4':
                    update();
                    break;
                case '5':
                    listHouses();
                    break;
                case '6':
                    exit();
                    break;
            }
        } while (loop);
    }


}

```

```java
//class HouseService
/**
 * HouseService.java<=>类 [业务层]
 * //定义House[] ,保存House对象
 * 1. 响应HouseView的调用
 * 2. 完成对房屋信息的各种操作(增删改查c[create]r[read]u[update]d[delete])
 */

public class HouseService {
    private House[] houses; //保存House对象
    private int houseNums = 1; //记录当前有多少个房屋信息
    private int idCounter = 1; //记录当前的id增长到哪个值

    //构造器
    public HouseService(int size) {
        houses = new House[size];//当创建HouseService对象，指定数组大小
        houses[0] = new House(1, "jack", "112", "海淀区", 2000, "未出租");
    }


    //findById方法,返回House对象或者null
    public House findById(int findId) {
        for (int i = 0; i < houseNums; i++) {
            if (findId == houses[i].getId()) {//要删除的房屋(id),是数组下标为i的元素
                return houses[i];
            }
        }
        return null;
    }

    public boolean del(int delId) {

        //应当先找到要删除的房屋信息对应的下标
        int index = -1;
        for (int i = 0; i < houseNums; i++) {
            if (delId == houses[i].getId()) {//要删除的房屋(id),是数组下标为i的元素
                index = i;//就使用index记录i
            }
        }

        if (index == -1) {//说明delId在数组中不存在
        }
        for (int i = index; i < houseNums - 1; i++) {
            houses[i] = houses[i + 1];
        }
        //把当有存在的房屋信息的最后一个 设置null
        houses[--houseNums] = null;
        return true;

    }

    //add方法，添加新对象,返回boolean
    public boolean add(House newHouse) {
        //判断是否还可以继续添加(我们暂时不考虑数组扩容的问题)
        if (houseNums == houses.length) {//不能再添加
            System.out.println("数组已满，不能再添加了...");
            return false;
        }
        //把newHouse对象加入到，新增加了一个房屋
        houses[houseNums++] = newHouse;
        //需要设计一个id自增长的机制, 然后更新newHouse的id
        newHouse.setId(++idCounter);
        return true;
    }

    //list方法，返回houses
    public House[] list() {
        return houses;
    }


}
```

```java
//class Utility

/**
 * 工具类的作用:
 * 处理各种情况的用户输入，并且能够按照程序员的需求，得到用户的控制台输入。
 */

import java.util.*;

/**
 *
 */
public class Utility {
    //静态属性。。。
    private static Scanner scanner = new Scanner(System.in);


    /**
     * 功能：读取键盘输入的一个菜单选项，值：1——5的范围
     *
     * @return 1——5
     */
    public static char readMenuSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1, false);//包含一个字符的字符串
            c = str.charAt(0);//将字符串转换成字符char类型
            if (c != '1' && c != '2' &&
                    c != '3' && c != '4' && c != '5') {
                System.out.print("选择错误，请重新输入：");
            } else {
                break;
            }
        }
        return c;
    }

    /**
     * 功能：读取键盘输入的一个字符
     *
     * @return 一个字符
     */
    public static char readChar() {
        String str = readKeyBoard(1, false);//就是一个字符
        return str.charAt(0);
    }

    /**
     * 功能：读取键盘输入的一个字符，如果直接按回车，则返回指定的默认值；否则返回输入的那个字符
     *
     * @param defaultValue 指定的默认值
     * @return 默认值或输入的字符
     */

    public static char readChar(char defaultValue) {
        String str = readKeyBoard(1, true);//要么是空字符串，要么是一个字符
        return (str.length() == 0) ? defaultValue : str.charAt(0);
    }

    /**
     * 功能：读取键盘输入的整型，长度小于2位
     *
     * @return 整数
     */
    public static int readInt() {
        int n;
        for (; ; ) {
            String str = readKeyBoard(10, false);//一个整数，长度<=10位
            try {
                n = Integer.parseInt(str);//将字符串转换成整数
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }

    /**
     * 功能：读取键盘输入的 整数或默认值，如果直接回车，则返回默认值，否则返回输入的整数
     *
     * @param defaultValue 指定的默认值
     * @return 整数或默认值
     */
    public static int readInt(int defaultValue) {
        int n;
        for (; ; ) {
            String str = readKeyBoard(10, true);
            if (str.equals("")) {
                return defaultValue;
            }

            //异常处理...
            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }

    /**
     * 功能：读取键盘输入的指定长度的字符串
     *
     * @param limit 限制的长度
     * @return 指定长度的字符串
     */

    public static String readString(int limit) {
        return readKeyBoard(limit, false);
    }

    /**
     * 功能：读取键盘输入的指定长度的字符串或默认值，如果直接回车，返回默认值，否则返回字符串
     *
     * @param limit        限制的长度
     * @param defaultValue 指定的默认值
     * @return 指定长度的字符串
     */

    public static String readString(int limit, String defaultValue) {
        String str = readKeyBoard(limit, true);
        return str.equals("") ? defaultValue : str;
    }


    /**
     * 功能：读取键盘输入的确认选项，Y或N
     * 将小的功能，封装到一个方法中.
     *
     * @return Y或N
     */
    public static char readConfirmSelection() {
        System.out.println("请输入你的选择(Y/N): 请小心选择");
        char c;
        for (; ; ) {//无限循环
            //在这里，将接受到字符，转成了大写字母
            //y => Y n=>N
            String str = readKeyBoard(1, false).toUpperCase();
            c = str.charAt(0);
            if (c == 'Y' || c == 'N') {
                break;
            } else {
                System.out.print("选择错误，请重新输入：");
            }
        }
        return c;
    }

    /**
     * 功能： 读取一个字符串
     *
     * @param limit       读取的长度
     * @param blankReturn 如果为true ,表示 可以读空字符串。
     *                    如果为false表示 不能读空字符串。
     *                    <p>
     *                    如果输入为空，或者输入大于limit的长度，就会提示重新输入。
     * @return
     */
    private static String readKeyBoard(int limit, boolean blankReturn) {

        //定义了字符串
        String line = "";

        //scanner.hasNextLine() 判断有没有下一行
        while (scanner.hasNextLine()) {
            line = scanner.nextLine();//读取这一行

            //如果line.length=0, 即用户没有输入任何内容，直接回车
            if (line.length() == 0) {
                if (blankReturn) {
                    return line;//如果blankReturn=true,可以返回空串
                } else {
                    continue; //如果blankReturn=false,不接受空串，必须输入内容
                }
            }

            //如果用户输入的内容大于了 limit，就提示重写输入
            //如果用户如的内容 >0 <= limit ,我就接受
            if (line.length() < 1 || line.length() > limit) {
                System.out.print("输入长度（不能大于" + limit + "）错误，请重新输入：");
                continue;
            }
            break;
        }

        return line;
    }
}

```

