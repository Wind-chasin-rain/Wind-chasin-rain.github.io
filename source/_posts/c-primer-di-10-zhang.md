---
title: C++ Primer(第10章 泛型容器)
date: 2022-03-13 14:45:47
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071352936.png
tags: C++
categories: 学习笔记
description: 第10章 泛型容器
updated:
mathjax:
highlight_shrink:
---

##### 10.1

1：知识点：泛型算法：算法是因为其实现了一些经典算法的公共接口，排序和搜索。泛型是因为他们可以作用于不同类型的元素和多种容器类型甚至是内置数组。故称泛型算法

知识点2：基本上都定义在algorithm和numeric两个头文件中

知识点3：这些算法一般情况下只作用于迭代器之上，不对容器进行直接操作

```c++
int main() {
	int i;
	vector<int>v;
	while (cin >> i) {
		v.push_back(i);
	}
	int val = 5;
	int res = 0;
	res = count(v.begin(), v.end(), val);
	cout << "val: " << val << " 出现" << res << "次" << endl;
	system("pause");
	return 0;
}
```

##### 10.2

```c++
int main() {
	string s;
	list<string>li;
	while (cin >> s) {
		li.push_back(s);
	}
	string val = "hi";
	int res = 0;
	res = count(li.begin(), li.end(), val);
	cout << "val: " << val << " 出现" << res << "次" << endl;
	system("pause");
	return 0;
}
```

##### 10.6

```c++
	vector<int>v = { 1,2,3,4,5,6,7,8,9 };
	fill_n(v.begin(),v.size(),0);
	for (auto i : v) {
		cout << i << " ";
	}
```

##### 10.9

```c++
void printV(vector<string>v) {
	for (auto i : v) {
		cout << i << " ";
	}
	cout << endl;
}

void elimDups(vector<string>& words) {
	sort(words.begin(), words.end());
	printV(words);
	auto end_unique = unique(words.begin(), words.end());
	printV(words);
	words.erase(end_unique,words.end());
	printV(words);
}
```

##### 10.12

```c++
class Sales_data {
public:
	string bn;
	Sales_data(string s) :bn(s) {
	}
	string isbn() {
		return bn;
	}
};
bool compareIsbn(Sales_data s1, Sales_data s2) {
	return s1.isbn() < s2.isbn();
}
void sort_Sa(vector<Sales_data>v) {
	stable_sort(v.begin(), v.end(), compareIsbn);
	for (auto i : v) {
		cout << i.isbn() << " ";
	}
}
int main() {
	vector<Sales_data>v;
	string s;
	while (cin >> s) {
		v.push_back(Sales_data(s));
	}
	sort_Sa(v);
	system("pause");
	return 0;
}
```

##### 10.13

```c++
bool func5(const string& s) {
	if (s.size() < 5)	return false;
	return true;
}
int main() {
	vector<string>words;
	string s;
	while (cin >> s) {
		words.push_back(s);
	}
	auto it = partition(words.begin(), words.end(), func5);
	for (auto i = words.begin(); i != it; ++i) {
		cout << *i << " ";
	}
	system("pause");
	return 0;
}
```

##### lambda表达式

知识点1：我们希望对算法进行更多参数的操作，衍生出lambda表达式，一个lambda表达式表示一个可调用代码单元，它可以定义在函数的内部。表达式的形式：f = [捕获列表](参数列表){函数体}，参数列表为空时，()可省略。
知识点2：如果未指定返回内容，则lambda返回void。

知识点3：lambda只有在捕获列表中捕获一个它所在函数的局部变量才能在函数体中使用该变量，lambda可以直接使用定义在函数之外的名字或者局部static变量

##### 10.14

```c++
int main() {

	auto f = [](int& a, int& b) {return  a + b; };
	int a, b;
	cin >> a >> b;
	cout << f(a, b) << endl;

	system("pause");
	return 0;
}
```

##### 10.15

```c++
int main() {
	int a = 1;
	auto f = [a](int b) {return a + b; };
	cout << f(5) << endl;

	system("pause");
	return 0;
}
```

##### 10.17

```c++
stable_sort(v.begin(), v.end(), [](Sales_data s1, Sales_data s2) {return s1.isbn() < s2.isbn(); });
```



partition()返回的是最后一个使谓词为true的元素的后一个位置的迭代器

find_if()返回的是第一个使谓词返回非0值的元素，若不存在这样的元素，则返回尾迭代器



##### 10.20

```c++
int main() {

	vector<string>v;
	string s;
	string::size_type sz=6;
	while (cin >> s) {
		v.push_back(s);
	}
	auto wc = count_if(v.begin(), v.end(), [=](const string& a) {return a.size() > sz; });
	cout << wc << endl;
	system("pause");
	return 0;
}
```

##### 10.21

```c++
int main() {
	int n = 10;
	auto f = [&n]()->bool {
		if (n > 0) { --n; return true; }
		else return false;
	};
	while (f()) {
		cout << n << endl;
	}
	system("pause");
	return 0;
}
```

##### 10.22

```c++
bool func6(const string& s, string::size_type sz) {
	return s.size() <= sz;
}
int main() {

	vector<string>v;
	string s;
	string::size_type sz=0 ;
	while (cin >> s) {
		v.push_back(s);
	}
	auto wc = count_if(v.begin(), v.end(), bind(func6, _1, 6));
	cout << wc << endl;
	system("pause");
	return 0;
}
```

##### 10.24

```c++
bool check_size(int sz, string s) {
	return sz > s.size();
}

int main() {

	vector<int>v{1,2,3,4,5,6,7,8,9};
	string s{"hello"};
	auto wc = find_if(v.begin(), v.end(), bind(check_size, _1, s));
	cout << *wc << endl;
	system("pause");
	return 0;
}
```

##### 10.27

```c++
	vector<int>v{1,2,2,4,5,5,7,7,7};
	list<int>lst;
	unique_copy(v.begin(), v.end(), back_inserter(lst));
```

##### 10.28

```c++
	vector<int>v{1,2,3,4,5,6,7,8,9};
	vector<int>v1;
	vector<int>v2;
	vector<int>v3;
	copy(v.begin(), v.end(), inserter(v1,v1.begin()));
	copy(v.begin(), v.end(), back_inserter(v2));
	copy(v.begin(), v.end(), front_inserter(v3));//不支持
```

##### 10.29

```c++
int main() {
	
	ifstream in("10_29.txt");
	vector<string>v;
	istream_iterator<string> it(in), end;
	//while (it != end) {
	//	v.push_back(*it++);
	//}
	copy(it, end, back_inserter(v));
	for (auto i : v) {
		cout << i << " ";
	}
	
	system("pause");
	return 0;
}
```



##### 10.30

```c++
int main() {

	vector<int>v;
	istream_iterator<int>str(cin), end;
	copy(str, end, back_inserter(v));
	sort(v.begin(), v.end());
	for (auto i : v) {
		cout << i << " ";
	}
	system("pause");
	return 0;
}
```

##### 10.31

```c++
int main() {

	vector<int>v;
	istream_iterator<int>str(cin), end;
	copy(str, end, back_inserter(v));
	sort(v.begin(), v.end());
	vector<int>v1;
	unique_copy(v.begin(), v.end(), back_inserter(v1));
	for (auto i : v1) {
		cout << i << " ";
	}
	system("pause");
	return 0;
}
```

##### 10.33

```c++
int main() {
	ifstream in("1.txt");//导入第一个参数，作为输入文件
	istream_iterator<int> it1(in), end;//定义流迭代器，输入流，和输入流的尾迭代器

	vector<int> vec1;//存储用vector
	/*	copy(it1,end,back_inserter(vec1));//将流中数据存入vector*/
	while (it1 != end)
	{
		vec1.push_back(*it1);
		++it1;
	}

	ofstream out("even.txt");
	ofstream out2("odd.txt");//目标写文件
	ostream_iterator<int> it2(out, "\n");//定义流迭代器，输出流，每行结尾换行
	ostream_iterator<int> it3(out2, " ");//定义流迭代器，输出流
	for (int i = 0; i < vec1.size(); ++i)
	{
		if (vec1[i] % 2 == 0)//偶数
		{
			*it2++ = vec1[i];//偶数放在even.txt中
		}
		else
		{
			*it3++ = vec1[i];//奇数放在odd.txt中
		}
	}
	
	system("pause");
	return 0;
}
```

##### 10.34

```c++
int main() {
	
	vector<int>vec = { 0,1,2,3,4,5,6,7,8,9 };
	for (auto r_it = vec.rbegin(); r_it != vec.rend(); ++r_it) {
		cout << *r_it << endl;
	}
	system("pause");
	return 0;
}
```

##### 10.35

```c++
int main() {
	
	vector<int>vec = { 0,1,2,3,4,5,6,7,8,9 };
	for (auto it = vec.end(); it != vec.begin(); --it) {
		cout << *(it - 1) << endl;
	}
	system("pause");
	return 0;
}
```

##### 10.36

```
find(lst.begin(), lst.end(), 0);
```

##### 10.37

```c++
int main() {
	
	vector<int>vec = { 1,2,3,4,5,6,7,8,9,10 };
	list<int>lst;
	copy( vec.rbegin()+3, vec.rend() - 2, back_inserter(lst));
	for (auto i : lst) {
		cout << i << " ";
	}

	system("pause");
	return 0;
}
```

##### 10.42

```c++
list1.sort();//使用其成员函数版本的算法，排序
list1.unique();//删除相同元素
```

