---
title: C++ Primer(第15章 面向对象程序设计)
date: 2022-04-01 13:57:30
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071359496.jpg
tags: C++
categories: 学习笔记
description: 第15章 面向对象程序设计
updated:
mathjax:
highlight_shrink:
---


##### 15.1

**什么是虚成员？**

对于某些函数，基类希望它的派生类各自定义适合自身的版本，此时基类就将这些函数声明成虚函数。

##### 15.2

**protected 访问说明符与 private 有何区别？**

- protected ： 基类和和其派生类还有友元可以访问。
- private ： 只有基类本身和友元可以访问。

##### 15.3

```c++
#include<string>

class Quote {
public:
	Quote() = default;
	Quote(const std::string &book,double sales_price):bookNo(book),price(sales_price){}
	std::string isbn() const { return bookNo; }
	virtual double net_price(std::size_t n) const {
		return n * price;
	}
	virtual ~Quote() = default;
    
    virtual void debug()const;//15.11

private:
	std::string bookNo;
protected:
	double price = 0.0;
};
```

```c++
#include"Quote.h"
double print_total(std::ostream& os, const Quote& item, std::size_t n) {
    double ret = item.net_price(n);

    os << "ISBN: " << item.isbn()
        << " # sold: " << n << " total due: " << ret << std::endl;
    return ret;
}
```

##### 15.5

```c++
class Bulk_quote :public Quote {
public:
	Bulk_quote() = default;
	Bulk_quote(const std::string& book, double sales_price,std::size_t qty,double disc)
		:Quote(book,sales_price),min_qty(qty),discount(disc){}
	double net_price(std::size_t)const override;
    
	void debug() const override;//15.11
    
private:
	std::size_t min_qty = 0;
	double discount = 0.0;
};

double Bulk_quote::net_price(std::size_t cnt) const
{
    if (cnt >= min_qty)    return cnt * (1 - discount) * price;
    else return cnt * price;
}
```

##### 15.6

```c++
int main()
{
    Quote q("123", 10);
    Bulk_quote bq("123", 10, 5, 0.2);
    auto t1 = print_total(std::cout, q, 4);
    auto t2 = print_total(std::cout, bq, 10);


    return 0;
}

//ISBN: 123 # sold: 4 total due: 40
//ISBN: 123 # sold: 8 total due: 80
```

##### 15.7

```c++
class Limit_quote :public Quote {
public:
	Limit_quote() = default;
	Limit_quote(const std::string& book, double sales_price, std::size_t max, double disc)
		:Quote(book,sales_price),max_qty(max),discount(disc) {}
	double net_price(std::size_t)const override;

    void debug() const override;//15.11
    
private:
	std::size_t max_qty = 0;
	double discount = 0.0;
};

double Limit_quote::net_price(std::size_t cnt) const
{
    if (cnt <= max_qty)    return cnt * (1 - discount) * price;
    else return max_qty * (1 - discount) * price + (cnt - max_qty) * price;
}


int main()
{
    Limit_quote lq("159", 10, 5, 0.2);
    //ISBN: 159 # sold: 4 total due: 32 
    print_total(std::cout, lq, 4); 
    //ISBN: 159 # sold: 10 total due: 90
    print_total(std::cout, lq, 10);
    
    return 0;
}
```

##### 15.11

```c++
void Quote::debug()const {
	std::cout << "This is Quote Class" << std::endl;
	std::cout << "ISBN: " << bookNo << std::endl;
	std::cout << "Price: " << price << std::endl;
}
void Bulk_quote::debug()const {
	std::cout << "This is Bulk_quote Class" << std::endl;
	std::cout << "min_qty: " << min_qty << std::endl;
	std::cout << "discount: " << discount << std::endl;
	std::cout << "Price: " << price << std::endl;
}

void Limit_quote::debug() const
{
	std::cout << "This is Bulk_quote Class" << std::endl;
	std::cout << "max_qty: " << max_qty << std::endl;
	std::cout << "discount: " << discount << std::endl;
	std::cout << "Price: " << price << std::endl;
}
```

##### 15.12

> 有必要将一个成员函数同时声明成 override 和 final 吗？为什么？

有必要。override 的含义是重写基类中相同名称的虚函数，final 是阻止它的派生类重写当前虚函数。

##### 15.15-15.16

```c++
class Quote
{
    friend bool operator !=(const Quote& lhs, const Quote& rhs);
public:
    Quote() { std::cout << "default constructing Quote\n"; }
    Quote(const std::string& b, double p) :
        bookNo(b), price(p) {
        std::cout << "Quote : constructor taking 2 parameters\n";
    }

    // copy constructor
    Quote(const Quote& q) : bookNo(q.bookNo), price(q.price)
    {
        std::cout << "Quote: copy constructing\n";
    }

    // move constructor
    Quote(Quote&& q) noexcept : bookNo(std::move(q.bookNo)), price(std::move(q.price))
    {
        std::cout << "Quote: move constructing\n";
    }

    // copy =
    Quote& operator =(const Quote& rhs)
    {
        if (*this != rhs)
        {
            bookNo = rhs.bookNo;
            price = rhs.price;
        }
        std::cout << "Quote: copy =() \n";

        return *this;
    }

    // move =
    Quote& operator =(Quote&& rhs)  noexcept
    {
        if (*this != rhs)
        {
            bookNo = std::move(rhs.bookNo);
            price = std::move(rhs.price);
        }
        std::cout << "Quote: move =!!!!!!!!! \n";

        return *this;
    }

    std::string     isbn() const { return bookNo; }
    virtual double  net_price(std::size_t n) const { return n * price; }
    virtual void    debug() const;

    virtual ~Quote()
    {
        std::cout << "destructing Quote\n";
    }

private:
    std::string bookNo;

protected:
    double  price = 10.0;
};

bool inline
operator !=(const Quote& lhs, const Quote& rhs)
{
    return lhs.bookNo != rhs.bookNo
        &&
        lhs.price != rhs.price;
}

class Disc_quote:public Quote {
public:
	Disc_quote() = default;
	Disc_quote(const std::string& book, double sales_price, std::size_t qty, double disc)
		:Quote(book,sales_price),quantity(qty),discount(disc) {}
	double net_price(std::size_t) const = 0;
protected:
	std::size_t quantity = 0;
	double discount = 0.0;
};

class Bulk_quote : public Disc_quote
{

public:
    Bulk_quote() { std::cout << "default constructing Bulk_quote\n"; }
    //Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :
    //    Disc_quote(b, p, q, disc) {
    //    std::cout << "Bulk_quote : constructor taking 4 parameters\n";
    //}

    using Disc_quote::Disc_quote;//15.27

    // copy constructor
    Bulk_quote(const Bulk_quote& bq) : Disc_quote(bq)
    {
        std::cout << "Bulk_quote : copy constructor\n";
    }

    // move constructor
    //page 535, " In a constructor, noexcept appears between the parameter list and the : that begins the constructor initializer list"
    Bulk_quote(Bulk_quote&& bq) noexcept : Disc_quote(std::move(bq))
    {
        std::cout << "Bulk_quote : move constructor\n";
    }

    // copy =()
    Bulk_quote& operator =(const Bulk_quote& rhs)
    {
        Disc_quote::operator =(rhs);
        std::cout << "Bulk_quote : copy =()\n";

        return *this;
    }


    // move =()
    Bulk_quote& operator =(Bulk_quote&& rhs) noexcept
    {
        Disc_quote::operator =(std::move(rhs));
        std::cout << "Bulk_quote : move =()\n";

        return *this;
    }

    double net_price(std::size_t n) const override;
    void  debug() const override;

    ~Bulk_quote() override
    {
        std::cout << "destructing Bulk_quote\n";
    }
};



class Limit_quote :public Disc_quote {
public:
	Limit_quote() = default;
	Limit_quote(const std::string& book, double sales_price, std::size_t qty, double disc)
		:Disc_quote(book, sales_price, qty, disc) {}
	double net_price(std::size_t)const override;
	void debug() const override;

};

double Bulk_quote::net_price(std::size_t cnt) const
{
    if (cnt >= quantity)    return cnt * (1 - discount) * price;
    else return cnt * price;
}

double Limit_quote::net_price(std::size_t cnt) const
{
    if (cnt <= quantity)    return cnt * (1 - discount) * price;
    else return quantity * (1 - discount) * price + (cnt - quantity) * price;
}


void Quote::debug()const {
	std::cout << "This is Quote Class" << std::endl;
	std::cout << "ISBN: " << bookNo << std::endl;
	std::cout << "Price: " << price << std::endl;
}
void Bulk_quote::debug()const {
	std::cout << "This is Bulk_quote Class" << std::endl;
	std::cout << "min_qty: " << quantity << std::endl;
	std::cout << "discount: " << discount << std::endl;
	std::cout << "Price: " << price << std::endl;
}

void Limit_quote::debug() const
{
	std::cout << "This is Bulk_quote Class" << std::endl;
	std::cout << "max_qty: " << quantity << std::endl;
	std::cout << "discount: " << discount << std::endl;
	std::cout << "Price: " << price << std::endl;
}
```

##### 15.17

![image-20220327173806133](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071358199.png)

##### 15.23

```c++
class Base
{
public:
    void pub_mem();   // public member
protected:
    int prot_mem;     // protected member
private:
    char priv_mem;    // private member
};

struct Pub_Derv : public    Base
{
    void memfcn(Base& b) { b = *this; }
};
struct Priv_Derv : private   Base
{
    void memfcn(Base& b) { b = *this; }
};
struct Prot_Derv : protected Base
{
    void memfcn(Base& b) { b = *this; }
};

struct Derived_from_Public : public Pub_Derv
{
    void memfcn(Base& b) { b = *this; }
};
struct Derived_from_Private : public Priv_Derv
{
    //void memfcn(Base &b) { b = *this; }
};
struct Derived_from_Protected : public Prot_Derv
{
    void memfcn(Base& b) { b = *this; }
};

int main()
{
    Pub_Derv d1;
    Base* p = &d1;

    Priv_Derv d2;
    //p = &d2;

    Prot_Derv d3;
    //p = &d3;

    Derived_from_Public dd1;
    p = &dd1;

    Derived_from_Private dd2;
    //p =& dd2;

    Derived_from_Protected dd3;
    //p = &dd3;


    return 0;
}
```

##### 12.28

```c++
    vector<Quote> vec;
    vec.push_back(Bulk_quote("!23", 10, 5, 0.2));
    cout << vec.back().net_price(10) << endl;

//100
```



##### 15.29

```c++
int main()
{
    std::vector<shared_ptr<Quote>> basket;

    basket.push_back(make_shared<Quote>("hello", 10));//100
    basket.push_back(make_shared<Bulk_quote>("world", 20, 5, 0.2));//160
    basket.push_back(make_shared<Limit_quote>("good", 15, 5, 0.2));//135

    double all = 0.0;
    for (auto const& i : basket) {
        cout << i->net_price(10) << endl;
        all += i->net_price(10);
    }
    cout << all << endl;//395

    return 0;
}
```

##### 15.30

```c++
//***********h***********
class Basket {
public:
    //clone是一个虚函数
    void add_item(const Quote& sale) {
        items.insert(shared_ptr<Quote>(sale.clone()));
    }
    void add_item(Quote&& sale) {
        items.insert(shared_ptr<Quote>(move(sale).clone()));
    }
    double total_receipt(ostream&)const;
private:
    static bool compare(const shared_ptr <Quote>& lhs, const shared_ptr <Quote>& rhs) {
        return lhs->isbn() < rhs->isbn();
    }
    multiset<shared_ptr<Quote>, decltype(compare)*> items{ compare };
};
//***********cpp***********
double Basket::total_receipt(ostream& os) const
{
	double sum = 0.0;
	for (auto iter = items.cbegin(); iter != items.cend(); iter = items.upper_bound(*iter)) {
		sum += print_total(os, **iter, items.count(*iter));
	}
	os << "Total Sale: " << sum << endl;
	return sum;
}
//***********main***********
int main()
{
    Basket basket;

    for (unsigned i = 0; i != 10; ++i)
        basket.add_item(Bulk_quote("Bible", 20.6, 20, 0.3));

    for (unsigned i = 0; i != 10; ++i)
        basket.add_item(Bulk_quote("C++Primer", 30.9, 5, 0.4));

    for (unsigned i = 0; i != 10; ++i)
        basket.add_item(Quote("CLRS", 40.1));

    ofstream log("log.txt", std::ios_base::app | std::ios_base::out);

    cout << basket.total_receipt(log) << endl;

    return 0;
}
```

##### 15.32

> 当一个 Query 类型的对象被拷贝、移动、赋值或销毁时，将分别发生什么？

- **拷贝：**当被拷贝时，合成的拷贝构造函数被调用。它将拷贝两个数据成员至新的对象。而在这种情况下，数据成员是一个智能指针，当拷贝时，相应的智能指针指向相同的地址，计数器增加1.
- **移动：**当移动时，合成的移动构造函数被调用。它将移动数据成员至新的对象。这时新对象的智能指针将会指向原对象的地址，而原对象的智能指针为 nullptr，新对象的智能指针的引用计数为 1.
- **赋值：**合成的赋值运算符被调用，结果和拷贝的相同的。
- **销毁：**合成的析构函数被调用。对象的智能指针的引用计数递减，当引用计数为 0 时，对象被销毁。

##### 15.36

```c++
Query q = Query("fiery") & Query("bird") | Query("wind");

WordQuery::WordQuery(wind)
Query::Query(const std::string& s) where s=wind
WordQuery::WordQuery(bird)
Query::Query(const std::string& s) where s=bird
WordQuery::WordQuery(fiery)
Query::Query(const std::string& s) where s=fiery
BinaryQuery::BinaryQuery()  where s=&
AndQuery::AndQuery()
Query::Query(std::shared_ptr<Query_base> query)
BinaryQuery::BinaryQuery()  where s=|
OrQuery::OrQuery
Query::Query(std::shared_ptr<Query_base> query)
```

```c++
std::cout << q <<std::endl;

Query::rep()
BinaryQuery::rep()
Query::rep()
WodQuery::rep()
Query::rep()
BinaryQuery::rep()
Query::rep()
WodQuery::rep()
Query::rep()
WodQuery::rep()
((fiery & bird) | wind)
```

##### Query

```c++
#include<iostream>
using namespace std;
#include<vector>
#include<algorithm>
#include<map>
#include<set>
#include<memory>
#include<string>
#include<fstream>
#include<sstream>

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

class QueryResult {
	friend ostream& print(ostream&, const QueryResult&);
public:
	QueryResult(string s, shared_ptr<set<TextQuery::line_no>>p, shared_ptr<vector<string>> f) :sought(s), lines(p), file(f) {}
	using line_no = TextQuery::line_no;
	using Iter = std::set<line_no>::iterator;
	Iter begin() const { return lines->begin(); }
	Iter end() const { return lines->end(); }
	shared_ptr<std::vector<std::string>> get_file() const
	{
		return std::make_shared<std::vector<std::string>>(file);
	}
private:
	string sought;
	shared_ptr<set<TextQuery::line_no>>lines;
	shared_ptr<vector<string>> file;
};


class Query;
class Query_base {
	friend class Query;
protected:
	using line_no = TextQuery::line_no;
	virtual ~Query_base() = default;
private:
	//eval返回与当前Query匹配的QueryResult
	virtual QueryResult eval(const TextQuery&)const = 0;
	//rep 是表示查询的一个string
	virtual string rep()const = 0;
};

class Query {
	friend Query operator~(const Query&);
	friend Query operator|(const Query&, const Query&);
	friend Query operator&(const Query&, const Query&);
public:
	Query(const string&);
	QueryResult eval(const TextQuery& t) const{ return q->eval(t); }
	string rep()const { 
		std::cout << "Query::rep() \n";
		return q->rep(); 
	}
private:
	Query(shared_ptr<Query_base>query):q(query){
		std::cout << "Query::Query(std::shared_ptr<Query_base> query)\n";
	}
	shared_ptr<Query_base>q;
};

class WordQuery :public Query_base {
	friend class Query;
	WordQuery(const string& s) :query_word(s){}
	//定义继承来的纯虚函数
	QueryResult eval(const TextQuery& t) const {return t.query(query_word);}
	string rep()const {return query_word;}

	string query_word;        //要查找的单词
};

class NotQuery :public Query_base {
	friend Query operator~(const Query&);
	NotQuery(const Query&q):query(q) {}

	string rep()const { return "~(" + query.rep() + ")"; }
	QueryResult eval(const TextQuery&)const;

	Query query;
};


class BinaryQuery :public Query_base {
protected:

	BinaryQuery(const Query&l,const Query&r,string s):lhs(l),rhs(r),opSym(s){}
	// 不定义 eval
	string rep()const { return "(" + lhs.rep() + " " + opSym + " " + rhs.rep() + ")"; }

	Query lhs, rhs;	  //运算对象
	string opSym;     //运算符的名字
};

class AndQuery :public BinaryQuery {
	friend Query operator&(const Query&, const Query&);
	AndQuery(const Query&left,const Query& right):BinaryQuery(left,right,"&") {}

	QueryResult eval(const TextQuery&) const;
};

class OrQuery :public BinaryQuery {
	friend Query operator|(const Query&, const Query&);
	OrQuery(const Query& left, const Query& right) :BinaryQuery(left, right, "|") {}

	QueryResult eval(const TextQuery&) const;
};
```

```c++
#include"TextQuery.h"

TextQuery::TextQuery(ifstream& is) :file(new vector<string>) {
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
		os << "\t(line " << num + 1 << ")"
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

inline Query::Query(const string&s):q(new WordQuery(s)) {
	std::cout << "Query::Query(const std::string& s) where s=" + s + "\n";
}

Query operator~(const Query& operand)
{
	return  shared_ptr<Query_base>(new NotQuery(operand));
}

Query operator&(const Query&lhs, const Query&rhs)
{
	return shared_ptr<Query_base>(new AndQuery(lhs, rhs));
}

Query operator|(const Query&lhs, const Query&rhs)
{
	return shared_ptr<Query_base>(new OrQuery(lhs, rhs));
}

inline std::ostream&
operator << (std::ostream& os, const Query& query)
{
	// make a virtual call through its Query_base pointer to rep();
	return os << query.rep();
}

QueryResult OrQuery::eval(const TextQuery& text)const {
	auto right = rhs.eval(text), left = lhs.eval(text);
	auto ret_lines = make_shared<set<line_no>>(left.begin(), left.end());

	ret_lines->insert(right.begin(), right.end());
	return QueryResult(rep(), ret_lines, left.get_file());
}

QueryResult AndQuery::eval(const TextQuery&text) const
{
	auto right = rhs.eval(text), left = lhs.eval(text);
	auto ret_lines = make_shared<set<line_no>>();
	set_intersection(left.begin(), left.end(), right.begin(), right.end(), 
		inserter(*ret_lines, ret_lines->begin()));
	return QueryResult(rep(), ret_lines, left.get_file());
}

QueryResult NotQuery::eval(const TextQuery& text) const
{
	auto result = query.eval(text);

	auto ret_lines= make_shared<set<line_no>>();
	auto beg = result.begin(), end = result.end();
	auto sz = result.get_file()->size();
	for (size_t n = 0; n != sz; ++n) {
		if (beg == end || *beg != n) {
			ret_lines->insert(n);
		}
		else if (beg != end)
			++beg;
	}
	return QueryResult(rep(), ret_lines, result.get_file());

}

```

