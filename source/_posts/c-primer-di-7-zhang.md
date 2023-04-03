---
title: C++ Primer(第7章 类)
date: 2022-03-08 20:02:08
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071351002.jpg
tags: C++
categories: 学习笔记
description: 第7章 类
updated:
mathjax:
highlight_shrink:
---


##### 7.2

```c++
class Sales_data {
public:
	string bookNo;
	unsigned units_sold;
	double revenue;
private:
	string isbn()const {
		return bookNo;
	}
	Sales_data& combine(const Sales_data& rhs) {
		units_sold += rhs.units_sold;
		revenue += rhs.revenue;
		return *this;
	}
	double avg_price() const {
		if (units_sold) {
			return revenue / units_sold;
		}
		return 0;
	}
};
```

##### 7.4

```c++
class Person {
public:
	string m_Name;
	string m_Address;

	Person(string name,string address) {
		this->m_Name = name;
		this->m_Address = address;
	}
	void Show_info()
	{
		cout << this->m_Name << "'s" << " address is " <<  this->m_Address << endl;
	}//类内定义函数

	string& const getName() {
		return this->m_Name;
	}
	string& const getAddress() {
		return this->m_Address;
	}

};
```

##### 7.24

```c++
class Screen {
	using pos = string::size_type;
public:
	Screen() = default;
	Screen(pos hei, pos wid) :height(hei),width(wid) {};
	Screen(pos hei, pos wid, char c) :height(hei), width(wid), contents(hei*wid,c) {};

private:
	pos  cursor = 0;
	pos height = 0, width = 0;
	string contents;
};
```

##### 7.27

```c++
class Screen {
public:
	using pos = string::size_type;
	Screen() = default;
	Screen(pos hei, pos wid) :height(hei),width(wid) {};
	Screen(pos hei, pos wid, char c) :height(hei), width(wid), contents(hei*wid,c) {};
	Screen &move(pos r, pos c);
	Screen &set(char);
	Screen &set(pos, pos, char);
	Screen &display(ostream& os) {
		do_dispaly(os);
		return *this;
	}
	const Screen &display(ostream& os) const {
		do_dispaly(os);
		return *this;
	}


private:
	pos  cursor = 0;
	pos height = 0, width = 0;
	string contents;
	void do_dispaly(ostream& os)const {
		os << contents;
	}
};

inline Screen &Screen::move(pos r, pos c) {
	pos row = r * width;
	cursor = row + c;
	return *this;
}
inline Screen &Screen::set(char c) {
	contents[cursor] = c;
	return *this;
}

inline Screen &Screen::set(pos r, pos col, char ch)
{
	contents[r * width + col] = ch;
	return *this;
}
```

##### 7.31

```c++
class Y;
class X {
	Y* p;
};
class Y {
	X x;
};
```

##### 7.32

```c++
class Screen;
class Window_mgr {
public:
	using ScreenIndex = std::vector<Screen>::size_type;
	void clear(ScreenIndex);
private:
	std::vector<Screen> screens;
};

class Screen {

	friend void Window_mgr::clear(ScreenIndex);

public:
	typedef std::string::size_type pos;
	Screen() = default;
	Screen(pos ht, pos wd) :height(ht), width(wd) {}
	Screen(pos ht, pos wd, char c) :height(ht), width(wd), contents(ht* wd, c) {}

	Screen& set(char);
	Screen& set(pos, pos, char);
	Screen& move(pos, pos);

	char get() const { return contents[cursor]; }
	char get(pos r, pos c) const { return contents[r * width + c]; }

	Screen& display(std::ostream& os)
	{
		do_display(os);
		return *this;
	}

	const Screen& display(std::ostream& os) const
	{
		do_display(os);
		return *this;
	}

	void do_display(std::ostream& os) const { os << contents; }

private:
	mutable size_t access_ctr;
	pos height, width, cursor;
	std::string contents;
};

void Window_mgr::clear(ScreenIndex i) {
	Screen& s = screens[i];
	s.contents = string(s.height * s.width, ' ');
}
```

##### 7.35

```c++
typedef string Type;
Type initVal(); // string
class Exercise {
public:
    typedef double Type;
    Type setVal(Type); // double
    Type initVal(); // double
private:
    int val;
};

Type Exercise::setVal(Type parm) {  // first is `string`, second is `double`
    val = parm + initVal();    
    return val;
}
```

##### 7.41

```c++
struct Sales_data
{
	friend std::istream &read(std::istream &is, Sales_data &item);
	friend std::ostream &print(std::ostream &os, const Sales_data &item);
	friend Sales_data add(const Sales_data &lhs, const Sales_data &rhs);
public:
	Sales_data(const std::string &s, unsigned n, double p) : bookNo(s), units_sold(n), revenue(p*n) 
	{ 
		std::cout << "Sales_data(const std::string &s, unsigned n, double p)" << std::endl; 
	}
	Sales_data() : Sales_data("", 0, 0) 
	{ 
		std::cout << "Sales_data() : Sales_data(\"\", 0, 0)" << std::endl; 
	}
	Sales_data(const std::string &s) : Sales_data(s, 0, 0) 
	{ 
		std::cout << "Sales_data(const std::string &s) : Sales_data" << std::endl; 
	}
	Sales_data(std::istream &is) : Sales_data() 
	{ 
		read(is, *this);
		std::cout << "Sales_data(std::istream &is) : Sales_data()" << std::endl; 
	}
	std::string isbn() const { return bookNo; }
	Sales_data& combine(const Sales_data&);
private:
	inline double avg_price() const;

	std::string bookNo;
	unsigned units_sold = 0;
	double revenue = 0.0;
};

Sales_data& Sales_data::combine(const Sales_data &rhs)
{
	units_sold += rhs.units_sold;
	revenue += rhs.revenue;

	return *this;
}

inline double Sales_data::avg_price() const
{
	if (units_sold)
		return revenue / units_sold;
	else
		return 0;
}

std::istream &read(std::istream &is, Sales_data &item)
{
	double price = 0;

	is >> item.bookNo >> item.units_sold >> price;
	item.revenue = price * item.units_sold;

	return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
	os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();

	return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
	Sales_data sum = lhs;
	sum.combine(rhs);

	return sum;
}
int main()
{
	Sales_data a("0-1-999-9", 2, 10);
	cout << endl;

	Sales_data b;
	cout << endl;

	Sales_data c("0-1-999-9");
	cout << endl;

	Sales_data d(cin);
	system("pause");
	return 0;
}
```

##### 7.43

```c++
class NoDefault {
public:
	NoDefault(int ){}
};
class C {
public:
	NoDefault nd;
	C():nd(5){}
};
```