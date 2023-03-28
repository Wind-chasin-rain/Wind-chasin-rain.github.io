---
title: C++ Primer(第9章 顺序容器)
date: 2022-03-11 16:48:00
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071352284.png
tags: C++
categories: 学习笔记
description: 第9章 顺序容器
updated:
mathjax:
highlight_shrink:
---


##### 9.2

```c++
list<deque<int>>;
```

##### 9.4

```c++
bool func1(vector<int>::iterator begin, vector<int>::iterator end, int n) {
	while (begin != end) {
		if (*begin == n)	return true;
		++begin;
	}
	return false;
}
```

##### 9.7-9.8

```c++
vector<int>::size_type //size_type指的是无符号整数类型
list<string>::iterator || list<string>::const_iterator //读操作
list<string>::iterator//写操作
```

##### 9.13

```c++
	list<int>li;
	vector<int>vec;
	vector<double>vec1(li.begin(), li.end());
	vector<double>vec2(vec.begin(), vec.end());
//容器之间的拷贝，容器的类型和其中元素的类型都必须相同

//利用迭代器进行拷贝，只需要其元素的范围，利用的是迭代器范围的对应元素进行初始化
```

##### 9.15

```c++
bool func3(vector<int>&v1, vector<int>&v2) {
	if (v1 == v2)	return true;
	else return false;
}
```

##### 9.16

```c++
bool func3(list<int>&v1, vector<int>&v2) {
	vector<int>v3(v1.begin(), v1.end());
	if (v3 == v2)	return true;
	else return false;
}
```

##### 9.18

```c++
	deque<string>dq;//9.19 将deque改成list
	string s;
	while (cin >> s) {
		dq.push_back(s);
	}
	for (auto it = dq.begin(); it != dq.end(); ++it) {
		cout << *it << endl;
	}
```

##### 9.20

```c++
	list<int>li{0,1,2,3,4,5,6,7,8,9};
	deque<int>odd;
	deque<int>even;
	for (auto i : li) {
		if (i % 2) {
			odd.push_back(i);
		}
		else {
			even.push_back(i);
		}
	}
```

##### 9.22

```c++
	vector<int>iv{0,10,10,30,10};
	int some_val = 10;

	vector<int>::iterator iter = iv.begin(), mid = iv.begin() + iv.size() / 2;
	while (iter != mid) {
		if (*iter == some_val)
		{
			iter = iv.insert(iter, 2 * some_val);
			mid = iv.begin() + iv.size() / 2;
			iter++;
		}
		iter++;
	}

	for (auto i : iv) {
		cout << i << " ";
	}//0 20 10 10 30 10
```

##### 9.26

```c++
	//erase()操作返回的是最后一个被删元素的后一个位置
	int ia[] = { 0,1,1,2,3,5,8,13,21,55,89 };
	vector<int>vec(ia, end(ia));
	list<int>li(ia, end(ia));
	for (auto it = vec.begin(); it != vec.end(); ) {
		if (*it % 2==0) {
			it = vec.erase(it);
		}
		else {
			++it;
		}
	}
	for (auto it = li.begin(); it != li.end();) {
		if (*it % 2)
			it = li.erase(it);
		else
			++it;
	}
```

##### 9.27

```c++
	int ia[] = { 0,1,1,2,3,5,8,13,21,55,89 };	
	forward_list<int>fl(ia, end(ia));
	auto prev = fl.before_begin();
	auto curr = fl.begin();
	while (curr != fl.end()) {
		if (*curr % 2) {
			curr = fl.erase_after(prev);
		}
		else {
			prev = curr;
			++curr;
		}
	}
```

##### 9.28

```c++
forward_list<string> func(forward_list<string>fl, string s1, string s2) {
	auto it2 = fl.begin();
	auto it1 = fl.before_begin();
	while (it2 != fl.end()) {
		if (*it2 == s1) {
			fl.insert_after(it2, s2);
			return fl;
		}
		else {
			it1 = it2;
			++it2;
		}
	}
	fl.insert_after(it1,s2);
	return fl;
}
```

##### 9.31

```c++
	forward_list<int>fl{0,1,2,3,4,5,6,7,8,9};
	auto it2 = fl.begin();
	auto it1 = fl.before_begin();
	while (it2 != fl.end()) {
		if (*it2 % 2) {
			it2 = fl.insert_after(it2, *it2);
			it1 = it2;
			++it2;
		}
		else {
			it2 = fl.erase_after(it1);
		}
	}
```

##### 9.41

```c++
	vector<char> vc;
	for (int i = 0; i < 10; ++i) {
		vc.push_back('a' + i);
	}
	string s(vc.begin(),vc.end());
	cout << s << endl;
```

##### 9.43

```c++
void func(string &s, string &oldVal, string &newVal) {
	auto iter = s.begin();
	while (iter + oldVal.size() != s.end()) {
		if (oldVal == string(iter, iter+oldVal.size())) {
			iter = s.erase(iter, iter + oldVal.size());
			iter = s.insert(iter, newVal.begin(), newVal.end());
			iter += newVal.size();
		}
		else {
			++iter;
		}
	}
}

int main() {

	string s("though,you don't love me");
	string oldVal("though");
	string newVal("tho");
	func(s, oldVal, newVal);
	cout << s;
	system("pause");
	return 0;
}
```

##### 9.44

```c++
void func(string &s, string &oldVal, string &newVal) {
	
	for (int i = 0; i + oldVal.size() < s.size();++i) {
		if (oldVal == s.substr(i,oldVal.size())) {
			s.replace(i, oldVal.size(), newVal);
			i += newVal.size()-1;
		}
	}
}
```

##### 7.45

```c++
void func(string& name, string& qz, string& hz) {
	name.insert(name.begin(), qz.begin(), qz.end());
	name.append(hz);
}
```

##### 7.46

```c++
void func(string& name, string& qz, string& hz) {
	name.insert(0,qz);
	name.insert(name.size(), hz);
}
```

##### 7.47

```c++
	string s("ab2c3d7R4E6");
	string numbers{ "123456789" };
	string alphabet{ "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ" };
	string::size_type pos = 0;
	while ((pos = s.find_first_of(numbers, pos)) != string::npos) {
		cout << s[pos] << " ";
		++pos;
	}
	cout << endl;
	pos = 0;
	while((pos = s.find_first_of(alphabet, pos)) != string::npos) {
		cout << s[pos] << " ";
		++pos;
	}

//find_first_not_of
	string s("ab2c3d7R4E6");
	string numbers{ "123456789" };
	string alphabet{ "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ" };
	string::size_type pos = 0;
	while ((pos = s.find_first_not_of(alphabet, pos)) != string::npos) {
		cout << s[pos] << " ";
		++pos;
	}
	cout << endl;
	pos = 0;
	while ((pos = s.find_first_not_of(numbers, pos)) != string::npos) {
		cout << s[pos] << " ";
		++pos;
	}
```

##### 9.49

```c++
	string str("abcdefghijklmnopqrstuvwxyz");
	string cender("bdfhkltgjpqy");
	string::size_type pos = 0, prepos = 0;
	string::size_type len = 0;
	string res;
	while ((pos = str.find_first_of(cender, pos)) != string::npos) {
		if (pos - prepos > len) {
			len = pos - prepos;
			res = str.substr(prepos, len);
		}
		prepos = pos+1;
		++pos;
	}
	cout << len << " " << res;
```

##### 9.50

```c++
	vector<string>vec{ "3.14","1.23","4.89","4.56" };
	int i = 0;
	float f = 0;
	for (auto s : vec) {
		i += stoi(s);
		f += stof(s);
	}
	cout << i << " " << f << endl;
```

##### 9.51

```c++
#include<iostream>
#include<string>

using namespace std;

const string mm[12] = { "Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sept","Oct","Nov","Dec" };

int findmonth(const string& mon) {
	int pos;
	for (int i = 0; i < 12; ++i) {
		if ((pos = mon.find(mm[i])) != string::npos) {
			return i + 1;
		}
	}
}
<!-- more -->


class Date {
public:
	Date(const string& str) {
		string data_str = str;
		string::size_type index1 = 0;
		string::size_type index2 = 0;
		if (str.find(',') != string::npos) {
			index1 = str.find(' ');
			index2 = str.find(',', index1 + 1);
			string mon = str.substr(0, index1);
			month = findmonth(mon);
			day = stoi(str.substr(index1 + 1, index2-index1-1));
			year = stoi(str.substr(index2 + 1));
		}
		else if (str.find('/') != string::npos) {
			index1 = str.find_first_of('/');
			index2 = str.find_first_of('/', index1 + 1);
			year = stoi(str.substr(index2 + 1));
			month = stoi(str.substr(index1 + 1, index2 - 1 - index1));
			day = stoi(str.substr(0, index1));
		}
		else {
			index1 = str.find_first_of(' ');
			index2 = str.find_first_of(' ', index1 + 1);
			string mon = str.substr(0, index1);
			month = findmonth(mon);
			day = stoi(str.substr(index1 + 1, index2 - 1 - index1));
			year = stoi(str.substr(index2 + 1));
		}
	}
	void getdate() {
		cout << "Year:" << year << " " << "Month:" << month << " " << "Day:" << day << endl;
	}
private:
	unsigned year, month, day;
};


int main() {
	string d1 = "May 5,2016", d2 = "9/9/1999", d3 = "Mar 11 2022";
	Date a(d1), b(d2), c(d3);
	a.getdate();
	b.getdate();
	c.getdate();

	system("pause");
	return 0;
}
```

##### 9.52

```c++
string func(string &s) {
	string suanshu("+-*/%");
	while (s.find_first_of(suanshu) != string::npos) {
		auto pos1 = s.find_first_of(suanshu);
		int a = stoi(s.substr(0, pos1));
		int b = stoi(s.substr(pos1 + 1));
		if (s[pos1] == '+')	s = to_string(a + b);
		else if(s[pos1]=='-')	s = to_string(a - b);
		else if (s[pos1] == '*')	s = to_string(a * b);
		else if (s[pos1] == '/')	s = to_string(a / b);
		else	s = to_string(a % b);
	}
	return s;
}
int main() {
	string str("5+(1+2)+(4-3)*6/(11%3)");
	stack<char>st;
	string::size_type pos1 = 0;
	string::size_type pos2 = 0;
	int len = str.size();
	for (int i = 0; i < len; ++i) {
		if (str[i] == ')') {
			string temp;
			while (!st.empty()&&st.top() != '(') {
				temp += st.top();
				//cout << st.top() << endl;
				st.pop();
			}
			st.pop();
			reverse(temp.begin(),temp.end());
			temp = func(temp);
			for (auto c : temp) {
				st.push(c);
			}
		}
		else
			st.push(str[i]);
		//cout << st.top() << endl;
		len = str.size();
	}
	str = "";
	while(!st.empty()) {
		str += st.top();
		st.pop();
	}
	reverse(str.begin(),str.end());
	cout << str << endl;

	system("pause");
	return 0;
}
```

