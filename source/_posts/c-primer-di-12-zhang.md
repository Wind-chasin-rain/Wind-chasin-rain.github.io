---
title: C++ Primer(第12章 动态内存)
date: 2022-03-15 14:27:37
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071355690.jpg
tags: C++
categories: 学习笔记
description: 第12章 动态内存
updated:
mathjax:
highlight_shrink:
---

##### 12.2

```c++
#include<iostream>
#include<memory>
#include <vector>
#include <string>
#include <initializer_list>
#include <exception>

using namespace std;

class StrBlob
{
public:
	using size_type = vector<string>::size_type;

	StrBlob() :data(make_shared<vector<string>>()) {}
	StrBlob(initializer_list<string> il) : data(make_shared<vector<string>>(il)) {}

	size_type size() const
	{
		return data->size();
	}

	bool empty() const
	{
		return data->empty();
	}

	void push_back(const string& s) const
	{
		data->push_back(s);
	}

	void pop_back() const
	{
		check(0, "pop_back on empty StrBlob");
		data->pop_back();
	}

	string& front()
	{
		check(0, "front on empty StrBlob");
		return data->front();
	}

	string& back()
	{
		check(0, "back on empty StrBlob");
		return data->back();
	}

	const string& front() const
	{
		check(0, "front on empty StrBlob");
		return data->front();
	}

	const string& back() const
	{
		check(0, "back on empty StrBlob");
		return data->back();
	}

private:
	void check(size_type i, const string& msg) const
	{
		if (i >= data->size())
			throw out_of_range(msg);
	}
	shared_ptr<vector<string>> data;
};


int main() {

	system("pause");
	return 0;
}
```

##### 12.6

```c++
vector<int>* func1() {
	return new vector<int>;
}
vector<int>* func2(vector<int>* v_ptr) {
	int a;
	while (cin >> a) {
		v_ptr->push_back(a);
	}
	return v_ptr;
}
void func3(vector<int>* v_ptr) {
	for (auto i : (*v_ptr)) {
		cout << i << " ";
	}
	cout << endl;
}
int main() {
	auto p = func1();
	p = func2(p);
	func3(p);
	delete(p);
	system("pause");
	return 0;
}
```

##### 12.7

```c++
shared_ptr<vector<int>> func1() {
	return make_shared<vector<int>>();
}
shared_ptr<vector<int>> func2(shared_ptr<vector<int>> p) {
	int a;
	while (cin >> a) {
		p->push_back(a);
	}
	return p;
}
void func3(shared_ptr<vector<int>> p) {
	for (auto i : (*p)) {
		cout << i << " ";
	}
	cout << endl;
}
int main() {
	auto p = func1();
	p = func2(p);
	func3(p);

	system("pause");
	return 0;
}
```

##### 智能指针规范

```
1：不使用相同的内置指针值初始化（或reset）多个智能指针

2：不delete get()返回的指针

3：不使用get()初始化或reset另一个只能指针

4：当你使用的智能指针管理的资源不是new分配的内存，记住传递一个删除器
```

##### 12.15

```c++
struct destination;
struct connection;
connection connect(destination*);
void disconnect(connection);
void end_connection(destination* p) {
	disconnect(*p);
}
void f(destination& d) {
	connection c = connect(&d);
	shared_ptr<connection>p(&c, [](connection* p){disconnect(*p)});
}
```

##### 12.16

```c++
    unique_ptr<string> p1(new string("pezy"));
    // unique_ptr<string> p2(p1); // copy
    //                      ^
    // Error: Call to implicitly-deleted copy constructor of 'unique_ptr<string>'
    //
    // unique_ptr<string> p3 = p1; // assign
    //                      ^
    // Error: Call to implicitly-deleted copy constructor of 'unique_ptr<string>'
    std::cout << *p1 << std::endl;
    p1.reset(nullptr);
```

##### 12.17

```
(a) 不合法。在定义一个 unique_ptr 时，需要将其绑定到一个new 返回的指针上。
(b) 合法。但是可能会有后续的程序错误。当 p1 被释放时，p1 所指向的对象也被释放，所以导致 pi 成为一个空悬指针。
(c) 合法。但是也可能会使得 pi2 成为空悬指针。
(d) 不合法。当 p3 被销毁时，它试图释放一个栈空间的对象。
(e) 合法。
(f) 不合法。p5 和 p2 指向同一个对象，当 p5 和 p2 被销毁时，会使得同一个指针被释放两次。
```

##### 12.23

```c++
int main() {
	char* p = new char[strlen("hello""world") + 1]();
	strcat(p, "hello ");
	strcat(p, "world");
	cout << p << endl;
	string str1{ "hello " }, str2{ "world" };
	cout << str1 + str2 << endl;

	system("pause");
	return 0;
}
```

##### 12.24

```c++
int main() {
	
	string s;
	cin >> s;
	char* input = new char[s.size() + 1];
	strcpy(input, s.c_str());
	cout << input << endl;
	delete[]input;

	system("pause");
	return 0;
}
```

##### 12.26

```c++
int main() {
	
	size_t n=100;
	allocator<string>alloc;
	auto p = alloc.allocate(n);
	auto q = p;
	string s;
	while (cin >> s && q != p + n) {
		alloc.construct(q++, s);
	}
	const size_t size = q - p;
	cout << size << endl;
	while (q != p) {
		cout << *--q << " ";
		alloc.destroy(q);
	}
	alloc.deallocate(p, n);
	system("pause");
	return 0;
}
```

##### 12.28

```c++
int main() {
	using line_no = vector<string>::size_type;
	ifstream ifs("12.30.txt");
	vector<string> file;
	map<string, set<line_no>>wm;
	string text;
	while (getline(ifs, text)) {
		file.push_back(text);
		istringstream line(text);
		string word;
		int n = file.size() - 1;
		while (line >> word) {
			wm[word].insert(n);
		}
	}
	while (true) {
		cout << "enter word to look for,or q to quit: ";
		string s;
		if (!(cin >> s) || s == "q")	break;
		else {
			auto loc = wm.find(s);
			if (loc == wm.end())
				cout << "don't find" << endl;
			else {
				cout << s << " occur " << wm[s].size()  << endl;
				for (auto num : wm[s]) {
					cout << "\t(line " << num + 1 << ")"
						<< file[num] << endl;
				}
			}
		}
	}

	system("pause");
	return 0;
}
```



##### 12.30

```c++
class QueryResult;
class TextQuery {
public:
	friend QueryResult;
	using line_no = vector<string>::size_type;
	TextQuery(ifstream&);
	QueryResult query(const string&) const;
private:
	shared_ptr<vector<string>> file;
	map<string, shared_ptr<set<line_no>>>wm;
};
TextQuery::TextQuery(ifstream& is):file(new vector<string>) {
	string text;
	while (getline(is, text)) {
		file->push_back(text);
		int n = file->size() - 1;
		istringstream line(text);
		string word;
		while (line >> word) {
			auto& lines = wm[word];
			if (!lines)
				lines.reset(new set<line_no>);
			lines->insert(n);
		}
	}
}
class QueryResult {
	friend ostream& print(ostream&, const QueryResult&);
public:
	QueryResult(string s,shared_ptr<set<TextQuery::line_no>>p,shared_ptr<vector<string>> f):sought(s),lines(p),file(f){}

private:
	string sought;
	shared_ptr<set<TextQuery::line_no>>lines;
	shared_ptr<vector<string>> file;
};
QueryResult TextQuery::query(const string& sought) const {
	static shared_ptr<set<line_no>> nodata(new set<line_no >);
	auto loc = wm.find(sought);
	if (loc == wm.end())
		return QueryResult(sought, nodata, file);
	else
		return QueryResult(sought, loc->second, file);
}
ostream& print(ostream& os, const QueryResult& qr) {
	os << qr.sought << " occurs " << qr.lines->size() << " " << endl;
	for (auto num : *qr.lines) {
		os << "\y(line " << num + 1 << ")"
			<< *(qr.file->begin() + num) << endl;
	}
	return os;
}
void runQueries(ifstream& infile) {
	TextQuery tq(infile);
	while (true) {
		cout << "enter word to look for,or q to quit: ";
		string s;
		if (!(cin >> s) || s == "q")	break;
		print(cout, tq.query(s)) << endl;
	}
}
int main() {
	ifstream ifs("12.30.txt");
	runQueries(ifs);

	system("pause");
	return 0;
}
```

