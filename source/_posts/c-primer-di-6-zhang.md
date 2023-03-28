---
title: C++ Primer(第6章 函数)
date: 2022-03-08 19:57:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071350337.jpg
tags: C++
categories: 学习笔记
description: 第6章 函数
updated:
mathjax:
highlight_shrink:
---

##### 6.10-6.12

```c++
void reset1(int* p, int* q) {
	int temp = *p;
	*p = *q;
	*q = temp;
}
void reset2(int& p, int& q) {
	int temp = p;
	p = q;
	q = temp;
}
int main() 
{	
	int i = 42, j = 22;
	reset1(&i, &j);
	cout << i << "+" << j << endl;//22+42
	reset2(i, j);
	cout << i << "+" << j << endl;//42+22
	system("pause");
	return 0;
}
```

##### 6.17

```c++
bool test01(const string& s) {
	for (char c : s) {
		if (isupper(c))	return true;
	}
	return false;
}
void test02(string& s) {
	for (char& c : s) {
		c = tolower(c);
	}
}
int main() 
{
	string s{ "hello WorlD" };
	cout << test01(s);
	test02(s);
	cout << s;
	system("pause");
	return 0;
}
```

##### 6.18

```c++
class matrix{};
bool compare(matrix& i, matrix& j);
vector<int>::iterator change_val(int,vector<int>::iterator);
```

##### 6.21

```c++
int test01(const int& i, const int* j) {
	if (i > *j)	return i;
	else return *j;
}
int main() 
{
	int i, t;
	cin >> i >> t;
	int* j = &t;
	cout << test01(i, j);
	system("pause");
	return 0;
}
```

##### 6.22

```c++
int test01(int* p, int* q) {
	int* temp = p;
	p = q;
	q = temp;
}
```

##### 6.27

```c++
int func(initializer_list<int>il) {
	int res = 0;
	for (const auto& elem : il) {
		res += elem;
	}
	return res;
}
int main() 
{
	initializer_list<int>il{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
	cout << func(il);
	system("pause");
	return 0;
}

```

##### P 201 函数返回引用

```c++
float temp;
float fn1(float r) {
	temp = r * r * 3.14;
	return temp;
}
float& fn2(float r) { //&说明返回的是temp的引用，换句话说就是返回temp本身
    temp = r * r * 3.14;
    return temp;
}
int main() 
{

    float a = fn1(5.0); //case 1：返回值
    //float &b=fn1(5.0); //case 2:用函数的返回值作为引用的初始化值 [Error] invalid initialization of non-const reference of type 'float&' from an rvalue of type 'float'
                           //（有些编译器可以成功编译该语句，但会给出一个warning） 
    float c = fn2(5.0);//case 3：返回引用
    float& d = fn2(5.0);//case 4：用函数返回的引用作为新引用的初始化值
    cout << a << endl;//78.5
    //cout << b << endl;//78.5
    cout << c << endl;//78.5
    cout << d << endl;//78.5
    temp = 10;
    cout << d << endl;//10
	system("pause");
	return 0;
}
```

##### 6.36-6.37

```c++
string(&func(string (& arrStr)[10]))[10];

using arrT = string[10];
arrT& func1(arrT& arr);//使用类型别名

auto func2(arrT& arr)->string(&)[10];// 尾置返回

string arrS[10];
decltype (arrS)& func3(arrT& arr);//decltype
```

##### 6.38

```c++
decltype(odd) &arrPtr(int i) {
	return (i & 2) ? odd : even;
}
```

##### 6.42

```c++
string make_plural(size_t ctr, const string& word, const string& ending ="s") {
	return (ctr > 1) ? word + ending : word;
}


int main() 
{	
	cout << make_plural(1, "success", "es") << "+" << make_plural(2, "success", "es") << endl;
	cout << make_plural(1, "failure") << "+" << make_plural(2, "failure") << endl;
	system("pause");
	return 0;
}
```

##### 6.47

```c++
void printVec(vector<int>& vec)
{
#ifndef NDEBUG
	cout << "vector size: " << vec.size() << endl;
#endif
	if (!vec.empty())
	{
		auto tmp = vec.back();
		vec.pop_back();
		printVec(vec);
		cout << tmp << " ";
	}
}


int main() 
{	
	vector<int> vec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };//c++11特性
	printVec(vec);

	system("pause");
	return 0;
}
```

##### 6.55

```c++
int add(int a, int b) {
	return  a + b;
}
int sub(int a, int b) {
	return a - b;
}
int multi(int a, int b) {
	return a * b;
}
int divide(int a, int b) {
	return a / b;
}

int main()
{
	vector<int (*)(int, int)>v{ add,sub,multi,divide };
	for (const auto c : v)	cout << c(4, 2) << endl;
	system("pause");
	return 0;
}
```

