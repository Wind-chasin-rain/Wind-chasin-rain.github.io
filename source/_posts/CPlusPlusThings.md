---
title: CPlusPlusThings-practical_exercises
date: 2022-05-08 19:57:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206101440373.jpeg
tags: C++
categories: 学习笔记
description: CPlusPlusThings-practical_exercises
updated:
mathjax:
highlight_shrink:
---

## day1

#### 联合体union

结构体和共用体的`区别`在于：结构体的各个成员会占用不同的内存，互相之间没有影响；而共用体的所有成员占用同一段内存，修改一个成员会影响其余所有成员。

#### 一种条件编译指令注释

```c++
int main(int argc, char* argv[])
{

#if 1
    cout << "hello" << endl;
#endif

#if 0
    cout << "world" << endl; // 该行代码被注释掉
#endif

    cout << "hello world" << endl;

    return 0;
}
```

## day2

#### enum枚举

```c++
enum Roster {Tom, Sharon, Bill, Teresa, John};//该语句将创建一个名为 Roster 的数据类型。
```

默认情况下，编译器设置第一个枚举量为 0，下一个为 1，以此类推。在上述示例中，Tom 的值将是 0，Sharon 的值为 1，等等。最后一个枚举量 John 的值为 4。

```c++
Roster student;
student = Sharon;
if (student == Sharon)  //Sharon 周围没有引号。因为它是一个命名常量，而不是字符串常数。
```

即使枚举数据类型中的值实际存储为整数，也不能总是将整数值替换为符号名称。例如，不能使用下面的语句将 Sharon 赋值给 student：

```c++
student = 1; //错误
```

但是，可以使用整数值而不是符号名称来测试枚举变量。例如，以下两个 if 语句是等效的：

```c++
if (student == Bill)
if (student == 2)
```

【示例 1】

```c++
enum Colors { red, orange, yellow = 9, green, blue };
```

在该示例中，命名常量 red 将被赋值为 0，orange 将为 1，yellow 将为 9，green 将为 10，blue 将为 11。

【示例 2】

```c++
enum Rooms { livingroom = 1, den, bedroom, kitchen };
```

在该示例中，livingroom 被赋值为 1，den 将为 2，bedroom 将为 3，kitchen 将为 4。

## day3

#### inline内联函数

关键字inline 必须与函数定义体放在一起才能使函数成为内联，仅将inline 放在函数声明前面不起任何作用。

**建议把inline函数的定义放到头文件中。在每个调用该inline函数的文件中包含该头文件。**



#### 前向引用声明

```c++
/*
使用前向引用声明虽然可以解决一些问题，但它并不是万能的。需要注意的是，
尽管使用了前向引用声明，但是在提供一个完整的类声明之前，不能声明该类的对象，
也不能在内联成员函数中使用该类的对象。请看下面的程序段：
*/

//第一种
#include<iostream>
class Fred;	//前向引用声明
class Barney {
   Fred x;	//错误：类Fred的声明尚不完善
};
class Fred {
   Barney y;
};


//第二种
class Fred;	//前向引用声明

class Barney {
 public:
   void method()
   {
     x->yabbaDabbaDo();	//错误：Fred类的对象在定义之前被使用
   }
 private:
   Fred* x;   //正确，经过前向引用声明，可以声明Fred类的对象指针
 };

class Fred {
 public:
   void yabbaDabbaDo();
 private:
   Barney* y;
}; 
```

#### 静态数据成员

```c++
/*
学习知识：
静态数据成员
用关键字static声明
该类的所有对象维护该成员的同一个拷贝
必须在类外定义和初始化，用(::)来指明所属的类。
*/
#include <iostream>
using namespace std;
class Point	
{
public:	
	Point(int xx=0, int yy=0) {X=xx; Y=yy; countP++; } 
    Point(Point &p);	
	int GetX() {return X;}
	int GetY() {return Y;}
	void GetC() {cout<<" Object id="<<countP<<endl;}
private:	
	int X,Y;
    //静态数据成员，必须在外部定义和初始化，内部不能直接初始化！
	static int countP;
};
Point::Point(Point &p)
{	X=p.X;
	Y=p.Y;
	countP++;
}
//必须在类外定义和初始化，用(::)来指明所属的类。
int Point::countP=0; 
int main()	
{	Point A(4,5);	
	cout<<"Point A,"<<A.GetX()<<","<<A.GetY();
	A.GetC();	
	Point B(A);	
	cout<<"Point B,"<<B.GetX()<<","<<B.GetY();
	B.GetC();	
    system("pause");
    return 0;
}
```

#### 静态成员函数

```c++
/*
知识点：
静态成员函数
类外代码可以使用类名和作用域操作符来调用静态成员函数。
静态成员函数只能引用属于该类的静态数据成员或静态成员函数。
*/
#include<iostream>
using namespace std;
class A
{
public:
    static void f(A a);
    static void g();
private:
    int x;
    static int global;
};
//静态数据成员必须在类外定义和初始化，用(::)来指明所属的类。
int A::global = 0;

void A::f(A a)
{

    //静态成员函数只能引用属于该类的静态数据成员或静态成员函数。
    //cout<<x; //对x的引用是错误的
    cout << a.x;  //正确
}
void A::g() {
    global = 5;
    cout << global << endl;
}

int main(int argc, char const* argv[])
{
    A a;
    a.g();//5
    a.f(A());//0
    system("pause");
    return 0;
}

```

#### 函数综合练习题

```c++
/*
一圆型游泳池如图所示，现在需在其周围建一圆型过道，并在其四周围上栅栏。栅栏价格为35元/米，过道造价为20元/平方米。
过道宽度为3米，游泳池半径由键盘输入。要求编程计算并输出过道和栅栏的造价。

图形描述：大圆嵌套小圆：
小圆在大圆中间，小圆为游泳池，大圆与小圆间隔为过道。
*/
#include<iostream>
using namespace std;

const float PI = 3.14159;
const float FencePrice = 35;
const float ConcretePrice = 20;

class Price {
public:
    Price(float bj);
    float zhouchang()const;
    float mianji()const;
private:
    float r;//半径
};
Price::Price(float bj) {
    r = bj;
}
float Price::zhouchang()const {
    return 2 * PI * r;
}
float Price::mianji()const {
    return PI * r * r;
}

int main(int argc, char const* argv[])
{
    float bj;
    cout << "输入游泳池半径： " << endl;
    cin >> bj;
    Price p1(bj);
    Price p2(bj + 3);
    cout << "过道造价为： " << (ConcretePrice * (p2.mianji() - p1.mianji())) << endl;
    cout << "栅栏造价为： " << (FencePrice * p2.zhouchang()) << endl;

    system("pause");
    return 0;
}
```

## day4

#### const用法

常类型的对象必须进行初始化，而且不能被更新。
常引用：被引用的对象不能被更新。
const  类型说明符  &引用名
常对象：必须进行初始化,不能被更新。
类名  const  对象名
常数组：数组元素不能被更新。
类型说明符  const  数组名[大小]...
常指针：指向常量的指针。



```c++
#include<iostream>
using namespace std;
class R
{
public:
    R(int r1, int r2) { R1 = r1; R2 = r2; }
    //const区分成员重载函数
    void print();
    void print() const;
private:
    int R1, R2;
};
/*
常成员函数说明格式：类型说明符  函数名（参数表）const;
这里，const是函数类型的一个组成部分，因此在实现部分也要带const关键字。
const关键字可以被用于参与对重载函数的区分
！！！通过常对象只能调用它的常成员函数！！！
*/

void R::print()
{
    cout << "普通调用" << endl;
    cout << R1 << ":" << R2 << endl;
}
//实例化也需要带上
void R::print() const
{
    cout << "常对象调用" << endl;
    cout << R1 << ";" << R2 << endl;
}
int main()
{
    R a(5, 4);
    a.print();  //调用void print()
    //!!!通过常对象只能调用它的常成员函数!!!
    const R b(20, 52);
    b.print();  //调用void print() const
    system("pause");
    return 0;
}
```

#### ifndef

```c++
/*
#ifndef   标识符
       程序段1
#else
       程序段2
#endif
如果“标识符”未被定义过，则编译程序段1，否则编译程序段2。

*/
```

## day5

#### 构造函数和析构函数的构造规则

1、派生类可以不定义构造函数的情况 
当具有下述情况之一时，派生类可以不定义构造函数。
基类没有定义任何构造函数。
基类具有缺省参数的构造函数。
基类具有无参构造函数。
2、派生类必须定义构造函数的情况 
当基类或成员对象所属类只含有带参数的构造函数时，即使派生类本身没有数据成员要初始化，它也必须定义构造函数，并以构造函数初始化列表的方式向基类和成员对象的构造函数传递参数，以实现基类子对象和成员对象的初始化。 
3、派生类的构造函数只负责直接基类的初始化 

#### 派生类对象的构造

- 先构造基类
- 再构造成员
- 最后构造自身（调用构造函数）

基类构造顺序由派生层次决定：**最远的基类最先构造**
成员构造顺序和定义顺序符合
析构函数的析构顺序与构造相反

#### 基类与派生类对象的关系 

基类对象与派生类对象之间存在赋值相容性。包括以下几种情况：
把派生类对象赋值给基类对象。即用派生类对象中从基类继承来的数据成员逐个赋值给基类对象的数据成员。

把派生类对象的地址赋值给基类指针。

用派生类对象初始化基类对象的引用。

如果函数的形参是基类对象或基类对象的引用，在调用函数时可以用派生类对象作为实参。

反之则不行，即不能把基类对象赋值给派生类对象；不能把基类对象的地址赋值给派生类对象的指针；也不能把基类对象作为派生对象的引用。

```c++
#include <iostream>
using namespace std;
class A {
    int a;
public:
    void setA(int x) { a = x; }
    int getA() { return a; }
};
class B :public A {
    int b;
public:
    void setB(int x) { b = x; }
    int getB() { return b; }
};
void f1(A a, int x)
{
    a.setA(x);
}
void f2(A* pA, int x)
{
    pA->setA(x);
}
void f3(A& rA, int x)
{
    rA.setA(x);
}

int main() {
    A a1, * pA;
    B b1, * pB;
    a1.setA(1);
    b1.setA(2);
    //把派生类对象赋值给基类对象。
    a1 = b1;
    cout << a1.getA() << endl;  //2
    cout << b1.getA() << endl;  //2
    a1.setA(10);
    cout << a1.getA() << endl;  //10
    cout << b1.getA() << endl;  //2
    //把派生类对象的地址赋值给基类指针。	 	
    pA = &b1;
    pA->setA(20);
    cout << pA->getA() << endl; //20
    cout << b1.getA() << endl;  //20
    //用派生类对象初始化基类对象的引用。
    A& ra = b1;
    ra.setA(30);
    cout << pA->getA() << endl; //30
    cout << b1.getA() << endl;  //30
    b1.setA(7);
    cout << b1.getA() << endl;  //7
    f1(b1, 100);
    cout << "1111111111" << endl;
    cout << b1.getA() << endl;	//7	    
    f2(&b1, 200);
    cout << b1.getA() << endl;  //200
    f3(b1, 300);
    cout << b1.getA() << endl;  //300
    system("pause");
    return 0;
}
```

#### 继承访问权限

一、公有继承
1.基类中protected的成员
类内部：可以访问
类的使用者：不能访问
类的派生类成员：可以访问
2.派生类不可访问基类的private成员
3.派生类可访问基类的protected成员
4.派生类可访问基类的public成员

二、私有继承
派生类不可访问基类的任何成员与函数

三、保护继承
派生方式为protected的继承称为保护继承，在这种继承方式下，
基类的public成员在派生类中会变成protected成员，
基类的protected和private成员在派生类中保持原来的访问权限
*注意点：当采用保护继承的时候，由于public成员变为protected成员，因此类的使用者不可访问！而派生类可访问！*

四、派生类对基类成员的访问形式
1.通过派生类对象直接访问基类成员 
2.在派生类成员函数中直接访问基类成员 
3.通过基类名字限定访问被重载的基类成员名  

#### 虚基类调用次序

```c++
#include <iostream>
using namespace std;
class A {
    int a;
public:
    A() { cout << "Constructing A" << endl; }
};
class B {
public:
    B() { cout << "Constructing B" << endl; }
};
class C {
public:
    C() { cout << "Constructing C" << endl; }
};
class B1 : public C ,virtual public B, virtual public A {
public:
    B1(int i) { cout << "Constructing B1" << endl; }
};
class B2 :virtual public C ,public A, virtual public B  {
public:
    B2(int j) { cout << "Constructing B2" << endl; }
};
class D : public B1, public B2 {
public:
    D(int m, int n) : B1(m), B2(n) { cout << "Constructing D" << endl; }
    A a;
};

int main() {
    D d(1, 2);
    system("pause");
    return 0;
}
/*
Constructing B
Constructing A
Constructing C
Constructing C
Constructing B1
Constructing A
Constructing B2
Constructing A
Constructing D
*/
//被虚拟继承的基类,在其所有的派生类中，仅出现一次//
```

调用顺序的规定：

先调用虚基类的构造函数，再调用非虚基类的构造函数
若同一层次中包含多个虚基类,这些虚基类的构造函数按它们的说明的次序调用
若虚基类由非基类派生而来,则仍然先调用基类构造函数,再调用派生类构造函数

## day6

#### C++对抽象类具有以下限定

- 抽象类中含有纯虚函数，由于纯虚函数没有实现代码，所以不能建立抽象类的对象。
- 抽象类只能作为其他类的基类，可以通过抽象类对象的指针或引用访问到它的派生类对象，实现运行时的多态性。
- 如果派生类只是简单地继承了抽象类的纯虚函数，而没有重新定义基类的纯虚函数，则派生类也是一个抽象类。

## day7

#### 重载二元运算符

（1）非静态成员运算符重载

以类成员形式重载的运算符参数比实际参数少一个，第1个参数是以this指针隐式传递的。 

```c++
class Complex{
		double real,image;
public:
		Complex operator+(Complex b){……}
......
};
```

（2） 友元运算符重载

如果将运算符函数作为类的友元重载，它需要的参数个数就与运算符实际需要的参数个数相同。比如，若用友元函数重载Complex类的加法运算符，则形式如下：

```c++
class Complex{
……
		friend Complex operator+(Complex a,Complex b);		//声明
//......
};

Complex  operator+(Complex a,Complex b){……}     		//定义
```

*对于不要求左值且可以交换参数次序的运算符（如+、-、*、/ 等运算符），最好用非成员形式（包括友元和普通函数）的重载运算符函数实现。*

#### 重载一元运算符 

一元运算符只需要一个运算参数，如取地址运算符（&）、负数（?）、自增加（++）等。

前自增(减)与后自增(减)：C++编译器可以通过在运算符函数参数表中是否插入关键字int 来区分这两种方式。

```c++
//前缀
operator -- ();
operator -- (X & x);
//后缀
operator -- (int);
operator -- (X & x, int);
```

#### 重载赋值运算符=

1、赋值运算符“=”的重载特殊性

赋值运算进行时将调用此运算符

只能用成员函数重载

如果需要而没有定义时，编译器自动生成，该版本进行bit-by-bit拷贝

#### 重载赋值运算符[]

1、[ ]是一个二元运算符，其重载形式如下：

```c++
class X{
……
		X& operator[](int n);
};
```

2、重载[]需要注意的问题

- []是一个二元运算符，其第1个参数是通过对象的this指针传递的，第2个参数代表数组的下标
- 由于[]既可以出现在赋值符“=”的左边，也可以出现在赋值符“=”的右边，所以重载运算符[]时常返回引用。
- **[]只能被重载为类的非静态成员函数，不能被重载为友元和普通函数**。

#### 重载( ) 

1、运算符( )是函数调用运算符，也能被重载。且只能被重载为类的成员函数。

2、运算符( )的重载形式如下：

```c++
class X{
……
		X& operator( )(参数表);
}；
```

其中的参数表可以包括任意多个参数。

3、运算符( )的调用形式如下：

X Obj;              		//对象定义

Obj()(参数表);  		//调用形式1

Obj(参数表);       		//调用形式2

```c++
#include<iostream>
using namespace std;
class X
{
public:
	int operator() (int i = 0)
	{
		cout << "X::operator(" << i << ")" << endl; return i;
	};
	int operator() (int i, int j)
	{
		cout << "X::operator(" << i << "," << j << ")" << endl;
		return i;
	};
	int operator[] (int i)
	{
		cout << "X::operator[" << i << "]" << endl; return i;
	};
	int operator[] (char* cp)
	{
		cout << "X::operator[" << cp << "]" << endl; return 0;
	};
};
int main(void)
{
	X obj;	int i = obj(obj(1), 2);
	char* c=const_cast<char*>("abcd");
	int a = obj[i];	int b = obj[c];
	cout << "a=" << a << endl;
	cout << "b=" << b << endl;
	system("pause");
}
```

#### 案例

```c++
#define _CRT_SECURE_NO_WARNINGS
//设计一个字符串类String，通过运算符重载实现字符串的输入、输出以及+=、==、!=、<、>、>=、[ ]等运算。

#include <iostream>
#include <cstring>
using namespace std;
class String
{
private:
    int length; //字符串长度
    char* sPtr; //存放字符串的指针
    void setString(const char* s2);
    friend ostream& operator<<(ostream& os, const String& s)
    {
        return os << s.sPtr;
    };
    friend istream& operator>>(istream& is, String& s)
    {
        return is >> s.sPtr;
    }; //重载输入运算符
public:
    String(const char* = "");
    String& operator=(const String& R)
    {
        length = R.length;
        strcpy(sPtr, R.sPtr);
        return *this;
    };                                         //重载赋值运算符 =
    String& operator+=(const String& R);       //字符串的连接 +=
    bool operator==(const String& R);          //字符串的相等比较 ==
    bool operator!=(const String& R);          //字符串的不等比较 !=
    bool operator!();                          //判定字符串是否为空
    bool operator<(const String& R) const;     //字符串的小于比较 <
    bool operator>(const String& R);           //字符串的大于比较 >
    bool operator>=(const String& R);          //字符串的大于等于比较
    char& operator[](int);                     //字符串的下标运算
    ~String() {};
};
String& String::operator+=(const String& R)
{
    char* temp = sPtr;
    length += R.length;
    sPtr = new char[length + 2];
    strcpy(sPtr, temp);
    strcat(sPtr, " ");
    strcat(sPtr, R.sPtr);
    delete[] temp;
    return *this;
}
String::String(const char* str)
{
    sPtr = new char[strlen(str) + 1];
    strcpy(sPtr, str);
    length = strlen(str);
};
bool String::operator==(const String& R) { return strcmp(sPtr, R.sPtr) == 0; }
bool String::operator!=(const String& R) { return !(*this == R); }
bool String::operator!() { return length == 0; }
bool String::operator<(const String& R) const { return strcmp(sPtr, R.sPtr) < 0; }
bool String::operator>(const String& R) { return R < *this; }
bool String::operator>=(const String& R) { return !(*this < R); }
char& String::operator[](int subscript) { return sPtr[subscript]; }
int main()
{
    String s1("happy"), s2("new year"), s3;
    cout << "s1 is " << s1 << "\ns2 is " << s2 << "\ns3 is " << s3
        << "\n比较s2和s1:"
        << "\ns2 ==s1结果是 " << (s2 == s1 ? "true" : "false")
        << "\ns2 != s1结果是 " << (s2 != s1 ? "true" : "false")
        << "\ns2 >  s1结果是 " << (s2 > s1 ? "true" : "false")
        << "\ns2 <  s1结果是 " << (s2 < s1 ? "true" : "false")
        << "\ns2 >= s1结果是 " << (s2 >= s1 ? "true" : "false");
    cout << "\n\n测试s3是否为空: ";
    if (!s3)
    {
        cout << "s3是空串" << endl; //L3
        cout << "把s1赋给s3的结果是：";
        s3 = s1;
        cout << "s3=" << s3 << "\n"; //L5
    }
    cout << "s1 += s2 的结果是：s1="; //L6
    s1 += s2;
    cout << s1; //L7

    cout << "\ns1 += to you 的结果是："; //L8
    s1 += "to you";
    cout << "s1 = " << s1 << endl; //L9
    s1[0] = 'H';
    s1[6] = 'N';
    s1[10] = 'Y';
    cout << "s1 = " << s1 << "\n"; //L10
    system("pause");
    return 0;
}

```

```c++
/*运行结果
s1 is happy
s2 is new year
s3 is
比较s2和s1:
s2 ==s1结果是 false
s2 != s1结果是 true
s2 >  s1结果是 true
s2 <  s1结果是 false
s2 >= s1结果是 true

测试s3是否为空: s3是空串
把s1赋给s3的结果是：s3=happy
s1 += s2 的结果是：s1=happy new year
s1 += to you 的结果是：s1 = happy new year to you
s1 = Happy New Year to you
*/
```

## day8

#### 函数模板的特化

- 特化的原因
  但在某些情况下，模板描述的通用算法不适合特定的场合（数据类型等）
  比如：如max函数

```c++
char * cp = max (“abcd”, “1234”);
实例化为：char * max (char * a, char * b){return a > b ? a : b;}
```

这肯定是有问题的，因为字符串的比较为：

```c++
char * max (char * a, char * b)
{	return strcmp(a, b)>0 ? a : b;   }
```

- 特化
  所谓特化，就是针对模板不能处理的特殊数据类型，编写与模板同名的特殊函数专门处理这些数据类型。
  模板特化的定义形式：
  template <> 返回类型 函数名<特化的数据类型>(参数表) {
  	…… 							
  }
  说明：
  ① template < >是模板特化的关键字，< >中不需要任何内容；
  ② 函数名后的< >中是需要特化处理的数据类型。

说明
① 当程序中同时存在模板和它的特化时，**特化将被优先调用**；
② 在同一个程序中，除了函数模板和它的特化外，还可以有同名的普通函数。其区别在于C++会对普通函数的调用实参进行隐式的类型转换，但不会对模板函数及特化函数的参数进行任何形式的类型转换。
调用顺序
当同一程序中具有模板与普通函数时，其匹配顺序如下：
完全匹配的非模板函数
完全匹配的模板函数
类型相容的非模板函数

## day9

#### 异常处理

1.catch捕获异常时，不会进行数据类型的默认转换。
2.限制异常的方法

- 当一个函数声明中不带任何异常描述时，它可以抛出任何异常。例如：

```c++
int f(int,char);                 //函数f可以抛出任何异常
```

- 在函数声明的后面添加一个throw参数表，在其中指定函数可以抛出的异常类型。例如：

```c++
int g(int,char)  throw(int,char);  //只允许抛出int和char异常。
```

- 指定throw限制表为不包括任何类型的空表，不允许函数抛出任何异常。如：

```c++
int h(int,char) throw();//不允许抛出任何异常
```

3.捕获所有异常
在多数情况下，catch都只用于捕获某种特定类型的异常，但它也具有捕获全部异常的能力。其形式如下：

```c++
catch(…) {
    ……                        //异常处理代码
}
```

4.再次抛出异常
如是catch块无法处理捕获的异常，它可以将该异常再次抛出，使异常能够在恰当的地方被处理。再次抛出的异常不会再被同一个catch块所捕获，它将被传递给外部的catch块处理。要在catch块中再次抛出同一异常，只需在该catch块中添加不带任何参数的throw语句即可。
5.异常的嵌套调用
try块可以嵌套，即一个try块中可以包括另一个try块，这种嵌套可能形成一个异常处理的调用链。

#### 异常类的捕获

派生异常类无法捕获基类异常，基类异常类可捕获派生类异常。

```c++
#include<iostream>
using namespace std;
class BasicException{
    public:
        string Where(){return "BasicException...";}
};
class FileSysException:public BasicException{
    public:
        string Where(){return "FileSysException...";}
};
class FileNotFound:public FileSysException{
    public:
        string Where(){return "FileNotFound...";}
};
class DiskNotFound:public FileSysException{
    public:
        string Where(){return "DiskNotFound...";}
};
int main(){
    try{
//       //派生异常类无法捕获基类异常
         throw FileSysException();
    }
    catch(DiskNotFound p){cout<<p.Where()<<endl;}
    catch(FileNotFound p){cout<<p.Where()<<endl;}
    catch(FileSysException p){cout<<p.Where()<<endl;}
    catch(BasicException p){cout<<p.Where()<<endl;}
    try{
//        .....  //基类异常类可捕获派生类异常
         throw DiskNotFound();
    }
    catch(BasicException p){cout<<p.Where()<<endl;}
    catch(FileSysException p){cout<<p.Where()<<endl;}
    catch(DiskNotFound p){cout<<p.Where()<<endl;}
    catch(FileNotFound p){cout<<p.Where()<<endl;}
}

FileSysException...
BasicException...
```

#### 异常类多态

```c++
#include <iostream>
using namespace std;
class BasicException
{
public:
    virtual string Where() { return "BasicException..."; }
};
class FileSysException : public BasicException
{
public:
    virtual string Where() { return "FileSysException..."; }
};
class FileNotFound : public FileSysException
{
public:
    virtual string Where() { return "FileNotFound..."; }
};
class DiskNotFound : public FileSysException
{
public:
    virtual string Where() { return "DiskNotFound..."; }
};
int main()
{
    try
    {
        DiskNotFound err;
        throw &err;
    }
    catch (BasicException *p)           //可捕获派生类异常
    {
        cout << p->Where() << endl;     //多态调用异常类方法
    }
}

DiskNotFound...
```

#### 调用异常类成员函数

```c++
//Eg10-11.cpp
#include <iostream>
using namespace std;
const int MAX = 3;
class Full {
    int a;
public:
    Full(int i) :a(i) {}
    int getValue() { return a; }
};
class Empty {};
class Stack {
private:
    int s[MAX];
    int top;
public:
    Stack() { top = -1; }
    void push(int a) {
        if (top >= MAX - 1)
            throw Full(a);
        s[++top] = a;
    }
    int pop() {
        if (top < 0)
            throw Empty();
        return s[top--];
    }
};
int main() {
    Stack s;
    try {
        s.push(10);
        s.push(20);
        s.push(30);
        s.push(40);
    }
    catch (Full e) {
        cout << "Exception: Stack Full..." << endl;
        cout << "The value not push in stack:" << e.getValue() << endl;
    }
    system("pause");
}


Exception: Stack Full...
The value not push in stack:40
```

## day10

#### get读取数据

```c++
#include <iostream>
using namespace std;
int main()
{
	char a, b, c, d;
	cin.get(a);
	cin.get(b);
	c = cin.get();
	d = cin.get();
	cout << int(a) << ',' << int(b) << ',' << int(c) << ',' << int(d) << endl;
	system("pause");
	return 0;
}

/*
用法：a = cin.get() ?或者 ?cin.get(a)
结束条件：输入字符足够后回车
说明：这个是单字符的输入，用途是输入一个字符，把它的ASCALL码存入到a中
处理方法：与cin不同，cin.get()在缓冲区遇到[enter]，[space]，[tab]不会作为舍弃，而是继续留在缓冲区中
*/
```

```c++
#include <iostream>
using namespace std;
//cin.get(arrayname,size)  把字符输入到arrayname中，长度不超过size
int main()
{
    //get()两个参数

    //1.输入串长<size，输入串长>arraylength，会自动扩张arrayname大小，使能保存所有数据
	// char a[10];
	// cin.get(a,20);
	// cout<<a<<endl;
    // cout<<sizeof(a)<<endl;
    //输入：123456789012[enter]
	//输出a数组：123456789012 可以发现，输入12个字符到a[10]中，系统自动扩充a[10]，此时实际数组长	  为13（‘123456789012’\0’’）。但当计算sizeof(a)时，还是现实为10
    //2.输入串长<size，输入串长<arraylength，把串全部输入，后面补‘\0’
    // char b[10];
	// cin.get(b,20);
	// cout<<b<<endl;//12345，此时数组内数据为‘12345'\0’
    // cout<<sizeof(b)<<endl;
    //3.输入串长>size，先截取size个字符，若还是大于arraylength，则输入前arraylength-1个字符，最后补充‘\0’
    // char c[5];
	// cin.get(c,10);
	// cout<<c<<endl;
    // cout<<sizeof(c)<<endl;
    //4.输入串长>size，先截取size个字符，若小于arraylength，则把截取串放入数组中，最后补充‘\0’
    // char d[10];
	// cin.get(d,5);
	// cout<<d<<endl;
    // cout<<sizeof(d)<<endl;
    
    //get()三个参数
    /*
        用法：cin.get(arrayname,size,s) ?把数据输入到arrayname字符数组中，当到达长度size时结束或者遇到字符s时结束
        注释：a必须是字符数组，即char a[]l类型，不可为string类型；size为最大的输入长度；

    */
    int i;
    char e[10];
    cin.get(e,8,',');
    cout<<e;
    system("pause");
    return 0;
}
```

#### getline

```c++
#include<iostream>
using namespace std;
/*
（1）cin.getline(arrayname,size)与cin.get(arrayname,size)的区别
cin.get(arrayname,size)当遇到[enter]时会结束目前输入，他不会删除缓冲区中的[enter]
cin.getline(arrayname,size)当遇到[enter]时会结束当前输入，但是会删除缓冲区中的[enter]
*/
int main()
{
    /*
    char a[10];
    char b;
    cin.get(a,10);
    cin.get(b);
    cout<<a<<endl<<int(b);//输入：12345[enter] 输出：12345 【换行】 10*/
    /*char c[10];
    char d;
    cin.getline(c,10);
    cin.get(d);
    cout<<c<<endl<<int(d);//输入：12345[enter]a[enter] 输出：12345【换行】97*/
    
    //cin.getline(arrayname,size,s)与cin.gei(arrayname,size,s)的区别
    /*
    cin.getline(arrayname,size,s)当遇到s时会结束输入，并把s从缓冲区中删除
    cin.get（arrayname,size,s）当遇到s时会结束输入，但不会删除缓冲区中的s
    */

    char e[10];
    char f;
    cin.get(e, 10, ',');
    cin.get(f);
    cout << e << endl << f;//输入：12345,[enter] 输出：12345【换行】，说明：cin,get不会删除缓冲区的，
    char e1[10];
    char f1;
    cin.getline(e1, 10, ',');
    cin.get(f1);
    cout << e1 << endl << f1;//输入：asd,wqe 输出：asd【换行】w
    system("pause");
}

```

#### put write

```c++
#include<iostream>
using namespace std;
//函数原型
//put(char c)
//write(const char*c, int n)
int main() {
    char c;
    char a[50] = "this is a string...";
    cout << "use get() input char:";
    while ((c = cin.get()) != '\n') {
        cout.put(c);
        cout.put('\n');
        cout.put('t').put('h').put('i').put('s').put('\n');
        cout.write(a, 12).put('\n');
        cout << "look" << "\t here!" << endl;
    }
    system("pause");
}
/*
use get() input char:good
g
this
this is a st
look     here!
o
this
this is a st
look     here!
o
this
this is a st
look     here!
d
this
this is a st
look     here!
*/
```

#### 输出格式

**setf()的第一原型：**

![image-20220505194212664](D:\Typora\图片\image-20220505194212664.png)

**setf()的第二原型：**
第二原型包含两个参数，第一个参数和第一原型里的参数一样，第二个参数指出要清除第一参数中的哪些位，也就是说，在第二原型中，第一个参数指出要设置哪些位，第二个参数指出要清除哪些位。

![image-20220505194144432](D:\Typora\图片\image-20220505194144432.png)

```c++
#include<iostream>
using namespace std;

int main(int argc, char const* argv[])
{
    char c[30] = "this is string";
    double d = -1231.232;
    cout.width(30);
    cout.fill('*');
    cout.setf(ios::left);
    cout << c << "----L1" << endl;
    cout.width(30);
    cout.fill('-');
    cout.setf(ios::right);
    cout << c << "----L2" << endl;
    cout.setf(ios::dec | ios::showbase | ios::showpoint);
    cout.width(30);
    cout << d << "----L3" << "\n";
    cout.setf(ios::showpoint);
    cout.precision(10);//有效数字？
    cout.width(30);
    cout << d << "----L4" << "\n";
    cout.width(30);
    cout.setf(ios::oct, ios::basefield);
    cout << 100 << "----L5" << "\n";
    system("pause");
    return 0;
}

/*
this is string****************----L1
----------------this is string----L2
-----------------------1231.23----L3
-------------------1231.232000----L4
--------------------------0144----L5
*/

```

![image-20220505194539668](D:\Typora\图片\image-20220505194539668.png)

```c++
//Eg12-5.cpp
#include<iostream>
#include<iomanip>
using namespace std;
int main() {
    char c[30] = "this is string";
    double d = -1234.8976;
    cout << setw(30) << left << setfill('*') << c << "----L1" << endl;
    cout << setw(30) << right << setfill('*') << c << "----L2" << endl;
    //showbase显示数值的基数前缀
    cout << dec << showbase << showpoint << setw(30) << d << "----L3" << "\n";
    //showpoint显示小数点
    cout << setw(30) << showpoint << setprecision(10) << d << "----L4" << "\n";
    //setbase(8)设置八进制
    cout << setw(30) << setbase(16) << 100 << "----L5" << "\n";
    system("pause");
}

/*
this is string****************----L1
****************this is string----L2
**********************-1234.90----L3
******************-1234.897600----L4
**************************0x64----L5
*/
```



