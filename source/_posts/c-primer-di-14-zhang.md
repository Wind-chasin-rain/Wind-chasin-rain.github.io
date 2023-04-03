---
title: C++ Primer(第14章 重载运算与类型转换)
date: 2022-03-28 19:57:55
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071356431.jpg
tags: C++
categories: 学习笔记
description: 第14章 重载运算与类型转换
updated:
mathjax:
highlight_shrink:
---


##### Sales_data

```c++
////****************h****************
#include <string>
#include <iostream>

class Sales_data {
    friend std::istream& operator>>(std::istream&, Sales_data&);
    friend std::ostream& operator<<(std::ostream&, const Sales_data&);
    friend Sales_data operator+(const Sales_data&, const Sales_data&);

public:
    Sales_data(const std::string& s, unsigned n, double p) :bookNo(s), units_sold(n), revenue(n* p) { }
    Sales_data() : Sales_data("", 0, 0.0f) { }
    Sales_data(const std::string& s) : Sales_data(s, 0, 0.0f) { }
    Sales_data(std::istream& is);

    Sales_data& operator=(const std::string&);
    Sales_data& operator+=(const Sales_data&);
    explicit operator std::string() const { return bookNo; }
    explicit operator double() const { return avg_price(); }

    std::string isbn() const { return bookNo; }

private:
    inline double avg_price() const;

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

std::istream& operator>>(std::istream&, Sales_data&);
std::ostream& operator<<(std::ostream&, const Sales_data&);
Sales_data operator+(const Sales_data&, const Sales_data&);

inline double Sales_data::avg_price() const
{
    return units_sold ? revenue / units_sold : 0;
}

////****************cpp****************
#include"Sales_data.h"

Sales_data::Sales_data(std::istream& is) : Sales_data()
{
    is >> *this;
}

Sales_data& Sales_data::operator+=(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

std::istream& operator>>(std::istream& is, Sales_data& item)
{
    double price = 0.0;
    is >> item.bookNo >> item.units_sold >> price;
    if (is)
        item.revenue = price * item.units_sold;
    else
        item = Sales_data();
    return is;
}

std::ostream& operator<<(std::ostream& os, const Sales_data& item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
    return os;
}

Sales_data operator+(const Sales_data& lhs, const Sales_data& rhs)
{
    Sales_data sum = lhs;
    sum += rhs;
    return sum;
}

Sales_data& Sales_data::operator=(const std::string& isbn)
{
    *this = Sales_data(isbn);
    return *this;
}
```

##### String

```c++
//****************h****************
#include<iostream>
#include<string>
#include<algorithm>
#include<memory>
#include<vector>

#ifndef _MSC_VER
#define NOEXCEPT noexcept
#else
#define NOEXCEPT
#endif

//===================================================================================
//
//		|s|t|r|i|n|g|\0|-------------------|
//		 ^			 ^  ^ first_free       ^
//		elements	 last_elem			   cap
//
//===================================================================================

class String
{
    friend std::ostream& operator<<(std::ostream&, const String&);
    friend std::istream& operator>>(std::istream&, String&);
    friend bool operator==(const String&, const String&);
    friend bool operator!=(const String&, const String&);
    friend bool operator< (const String&, const String&);
    friend bool operator> (const String&, const String&);
    friend bool operator<=(const String&, const String&);
    friend bool operator>=(const String&, const String&);

public:
    String() : String("") { }
    String(const char*);
    String(const String&);
    String& operator=(const String&);
    String(String&&) NOEXCEPT;
    String& operator=(String&&)NOEXCEPT;
    ~String();

    void push_back(const char);

    char* begin() const { return elements; }
    char* end() const { return last_elem; }

    const char* c_str() const { return elements; }
    size_t size() const { return last_elem - elements; }
    size_t length() const { return size(); }
    size_t capacity() const { return cap - elements; }

    void reserve(size_t);
    void resize(size_t);
    void resize(size_t, char);

    char& operator[](std::size_t n) { return elements[n]; }
    const char& operator[](std::size_t n) const { return elements[n]; }

private:
    std::pair<char*, char*> alloc_n_copy(const char*, const char*);
    void range_initializer(const char*, const char*);
    void free();
    void reallocate();
    void alloc_n_move(size_t new_cap);
    void chk_n_alloc() { if (first_free == cap) reallocate(); }

private:
    char* elements;
    char* last_elem;
    char* first_free;
    char* cap;
    std::allocator<char> alloc;
};

std::ostream& operator<<(std::ostream&, const String&);
std::istream& operator>>(std::istream&, String&);
bool operator==(const String&, const String&);
bool operator!=(const String&, const String&);
bool operator< (const String&, const String&);
bool operator> (const String&, const String&);
bool operator<=(const String&, const String&);
bool operator>=(const String&, const String&);

//****************cpp****************
#include"String.h"


//===========================================================================
//
//		operator - friend
//
//===========================================================================

std::ostream& operator<<(std::ostream& os, const String& lhs)
{
    os << lhs.c_str();
    return os;
}

std::istream& operator>>(std::istream& is, String& rhs)
{
    for (char c; (c = is.get()) != '\n';) {
        rhs.push_back(c);
    }
    return is;
}

bool operator==(const String& lhs, const String& rhs)
{
    return (lhs.size() == rhs.size() && std::equal(lhs.begin(), lhs.end(), rhs.begin()));
}

bool operator!=(const String& lhs, const String& rhs)
{
    return !(lhs == rhs);
}

bool operator<(const String& lhs, const String& rhs)
{
    return std::lexicographical_compare(lhs.begin(), lhs.end(), rhs.begin(), rhs.end());
}

bool operator>(const String& lhs, const String& rhs)
{
    return rhs < lhs;
}

bool operator<=(const String& lhs, const String& rhs)
{
    return !(rhs < lhs);
}

bool operator>=(const String& lhs, const String& rhs)
{
    return !(lhs < rhs);
}

//===========================================================================
//
//		Constructors
//
//===========================================================================

String::String(const char* s)
{
    char* sl = const_cast<char*>(s);
    while (*sl)
        ++sl;
    range_initializer(s, ++sl);
}

//===========================================================================
//
//		Big 5
//
//===========================================================================

String::String(const String& rhs)
{
    range_initializer(rhs.elements, rhs.first_free);
}

String& String::operator = (const String& rhs)
{
    auto newstr = alloc_n_copy(rhs.elements, rhs.first_free);
    free();
    elements = newstr.first;
    first_free = cap = newstr.second;
    last_elem = first_free - 1;
    return *this;
}

String::String(String&& s) NOEXCEPT : elements(s.elements), last_elem(s.last_elem), first_free(s.first_free), cap(s.cap)
{
    s.elements = s.last_elem = s.first_free = s.cap = nullptr;
}

String& String::operator = (String&& rhs) NOEXCEPT
{
    if (this != &rhs) {
        free();
        elements = rhs.elements;
        last_elem = rhs.last_elem;
        first_free = rhs.first_free;
        cap = rhs.cap;
        rhs.elements = rhs.last_elem = rhs.first_free = rhs.cap = nullptr;
    }
    return *this;
}

String::~String()
{
    free();
}

//===========================================================================
//
//		members
//
//===========================================================================

void String::push_back(const char c)
{
    chk_n_alloc();
    *last_elem = c;
    last_elem = first_free;
    alloc.construct(first_free++, '\0');
}

void String::reallocate()
{
    //	\0    |    -
    //  ^          ^
    // elements    first_free
    // last_elem   cap

    auto newcapacity = size() ? 2 * (size() + 1) : 2;
    alloc_n_move(newcapacity);
}

void String::alloc_n_move(size_t new_cap)
{
    auto newdata = alloc.allocate(new_cap);
    auto dest = newdata;
    auto elem = elements;
    for (size_t i = 0; i != size() + 1; ++i)
        alloc.construct(dest++, std::move(*elem++));
    free();
    elements = newdata;
    last_elem = dest - 1;
    first_free = dest;
    cap = elements + new_cap;
}

void String::free()
{
    if (elements) {
        std::for_each(elements, first_free, [this](char& c) { alloc.destroy(&c); });
        alloc.deallocate(elements, cap - elements);
    }
}

std::pair<char*, char*>
String::alloc_n_copy(const char* b, const char* e)
{
    auto str = alloc.allocate(e - b);
    return{ str, std::uninitialized_copy(b, e, str) };
}

void String::range_initializer(const char* first, const char* last)
{
    auto newstr = alloc_n_copy(first, last);
    elements = newstr.first;
    first_free = cap = newstr.second;
    last_elem = first_free - 1;
}

void String::reserve(size_t new_cap)
{
    if (new_cap <= capacity()) return;
    alloc_n_move(new_cap);
}

void String::resize(size_t count, char c)
{
    if (count > size()) {
        if (count > capacity()) reserve(count * 2);
        for (size_t i = size(); i != count; ++i) {
            *last_elem++ = c;
            alloc.construct(first_free++, '\0');
        }

    }
    else if (count < size()) {
        while (last_elem != elements + count) {
            --last_elem;
            alloc.destroy(--first_free);
        }
        *last_elem = '\0';
    }
}

void String::resize(size_t count)
{
    resize(count, ' ');
}
```

##### StrBlob  StrBlodPtr

```c++
//****************h****************
#include <vector>
using std::vector;

#include <string>
using std::string;

#include <initializer_list>
using std::initializer_list;

#include <memory>
using std::make_shared; using std::shared_ptr;

#include <exception>
#include <stdexcept>

#ifndef _MSC_VER
#define NOEXCEPT noexcept
#else
#define NOEXCEPT
#endif

class StrBlobPtr;
class ConstStrBlobPtr;
//=================================================================================
//
//		StrBlob - custom vector<string>
//
//=================================================================================

class StrBlob {
    using size_type = vector<string>::size_type;
    friend class ConstStrBlobPtr;
    friend class StrBlobPtr;
    friend bool operator==(const StrBlob&, const StrBlob&);
    friend bool operator!=(const StrBlob&, const StrBlob&);
    friend bool operator< (const StrBlob&, const StrBlob&);
    friend bool operator> (const StrBlob&, const StrBlob&);
    friend bool operator<=(const StrBlob&, const StrBlob&);
    friend bool operator>=(const StrBlob&, const StrBlob&);

public:
    StrBlob() : data(make_shared<vector<string>>()) { }
    StrBlob(initializer_list<string> il) : data(make_shared<vector<string>>(il)) { }

    StrBlob(const StrBlob& sb) : data(make_shared<vector<string>>(*sb.data)) { }
    StrBlob& operator=(const StrBlob&);

    StrBlob(StrBlob&& rhs) NOEXCEPT : data(std::move(rhs.data)) { }
    StrBlob& operator=(StrBlob&&)NOEXCEPT;

    StrBlobPtr begin();
    StrBlobPtr end();

    ConstStrBlobPtr cbegin() const;
    ConstStrBlobPtr cend() const;

    string& operator[](size_t n);
    const string& operator[](size_t n) const;

    size_type size() const { return data->size(); }
    bool empty() const { return data->empty(); }

    void push_back(const string& t) { data->push_back(t); }
    void push_back(string&& s) { data->push_back(std::move(s)); }

    void pop_back();
    string& front();
    string& back();
    const string& front() const;
    const string& back() const;

private:
    void check(size_type, const string&) const;

    shared_ptr<vector<string>> data;
};

bool operator==(const StrBlob&, const StrBlob&);
bool operator!=(const StrBlob&, const StrBlob&);
bool operator< (const StrBlob&, const StrBlob&);
bool operator> (const StrBlob&, const StrBlob&);
bool operator<=(const StrBlob&, const StrBlob&);
bool operator>=(const StrBlob&, const StrBlob&);

inline void StrBlob::pop_back()
{
    check(0, "pop_back on empty StrBlob");
    data->pop_back();
}

inline string& StrBlob::front()
{
    check(0, "front on empty StrBlob");
    return data->front();
}

inline string& StrBlob::back()
{
    check(0, "back on empty StrBlob");
    return data->back();
}

inline const string& StrBlob::front() const
{
    check(0, "front on empty StrBlob");
    return data->front();
}

inline const string& StrBlob::back() const
{
    check(0, "back on empty StrBlob");
    return data->back();
}

inline void StrBlob::check(size_type i, const string& msg) const
{
    if (i >= data->size()) throw std::out_of_range(msg);
}

inline string& StrBlob::operator[](size_t n)
{
    check(n, "out of range");
    return data->at(n);
}

inline const string& StrBlob::operator[](size_t n) const
{
    check(n, "out of range");
    return data->at(n);
}

//=================================================================================
//
//		StrBlobPtr - custom iterator of StrBlob
//
//=================================================================================

class StrBlobPtr {
    friend bool operator==(const StrBlobPtr&, const StrBlobPtr&);
    friend bool operator!=(const StrBlobPtr&, const StrBlobPtr&);
    friend bool operator< (const StrBlobPtr&, const StrBlobPtr&);
    friend bool operator> (const StrBlobPtr&, const StrBlobPtr&);
    friend bool operator<=(const StrBlobPtr&, const StrBlobPtr&);
    friend bool operator>=(const StrBlobPtr&, const StrBlobPtr&);

public:
    StrBlobPtr() : curr(0) { }
    StrBlobPtr(StrBlob& s, size_t sz = 0) : wptr(s.data), curr(sz) { }

    string& deref() const;
    StrBlobPtr& incr();

    string& operator[](size_t n);
    const string& operator[](size_t n) const;

private:
    shared_ptr<vector<string>> check(size_t, const string&) const;

    std::weak_ptr<vector<string>> wptr;
    size_t curr;
};

bool operator==(const StrBlobPtr&, const StrBlobPtr&);
bool operator!=(const StrBlobPtr&, const StrBlobPtr&);
bool operator< (const StrBlobPtr&, const StrBlobPtr&);
bool operator> (const StrBlobPtr&, const StrBlobPtr&);
bool operator<=(const StrBlobPtr&, const StrBlobPtr&);
bool operator>=(const StrBlobPtr&, const StrBlobPtr&);

inline string& StrBlobPtr::deref() const
{
    auto p = check(curr, "dereference past end");
    return (*p)[curr];
}

inline StrBlobPtr& StrBlobPtr::incr()
{
    check(curr, "increment past end of StrBlobPtr");
    ++curr;
    return *this;
}

inline shared_ptr<vector<string>> StrBlobPtr::check(size_t i, const string& msg) const
{
    auto ret = wptr.lock();
    if (!ret) throw std::runtime_error("unbound StrBlobPtr");
    if (i >= ret->size()) throw std::out_of_range(msg);
    return ret;
}

inline string& StrBlobPtr::operator[](size_t n)
{
    auto p = check(n, "dereference out of range.");
    return (*p)[n];
}

inline const string& StrBlobPtr::operator[](size_t n) const
{
    auto p = check(n, "dereference out of range.");
    return (*p)[n];
}

//=================================================================================
//
//		ConstStrBlobPtr - custom const_iterator of StrBlob
//
//=================================================================================

class ConstStrBlobPtr {
    friend bool operator==(const ConstStrBlobPtr&, const ConstStrBlobPtr&);
    friend bool operator!=(const ConstStrBlobPtr&, const ConstStrBlobPtr&);
    friend bool operator< (const ConstStrBlobPtr&, const ConstStrBlobPtr&);
    friend bool operator> (const ConstStrBlobPtr&, const ConstStrBlobPtr&);
    friend bool operator<=(const ConstStrBlobPtr&, const ConstStrBlobPtr&);
    friend bool operator>=(const ConstStrBlobPtr&, const ConstStrBlobPtr&);

public:
    ConstStrBlobPtr() : curr(0) { }
    ConstStrBlobPtr(const StrBlob& s, size_t sz = 0) : wptr(s.data), curr(sz) { }

    const string& deref() const;
    ConstStrBlobPtr& incr();

    const string& operator[](size_t n) const;

private:
    std::shared_ptr<vector<string>> check(size_t, const string&) const;

    std::weak_ptr<vector<string>> wptr;
    size_t curr;
};

bool operator==(const ConstStrBlobPtr&, const ConstStrBlobPtr&);
bool operator!=(const ConstStrBlobPtr&, const ConstStrBlobPtr&);
bool operator< (const ConstStrBlobPtr&, const ConstStrBlobPtr&);
bool operator> (const ConstStrBlobPtr&, const ConstStrBlobPtr&);
bool operator<=(const ConstStrBlobPtr&, const ConstStrBlobPtr&);
bool operator>=(const ConstStrBlobPtr&, const ConstStrBlobPtr&);

inline const string& ConstStrBlobPtr::deref() const
{
    auto p = check(curr, "dereference past end");
    return (*p)[curr];
}

inline ConstStrBlobPtr& ConstStrBlobPtr::incr()
{
    check(curr, "increment past end of StrBlobPtr");
    ++curr;
    return *this;
}

inline std::shared_ptr<vector<string>> ConstStrBlobPtr::check(size_t i, const string& msg) const
{
    auto ret = wptr.lock();
    if (!ret) throw std::runtime_error("unbound StrBlobPtr");
    if (i >= ret->size()) throw std::out_of_range(msg);
    return ret;
}

inline const string& ConstStrBlobPtr::operator[](size_t n) const
{
    auto p = check(n, "dereference out of range.");
    return (*p)[n];
}

//****************cpp****************
#include"StrBlob StrBlodPtr.h"

//==================================================================
//
//		StrBlob - operators
//
//==================================================================

bool operator==(const StrBlob& lhs, const StrBlob& rhs)
{
    return *lhs.data == *rhs.data;
}

bool operator!=(const StrBlob& lhs, const StrBlob& rhs)
{
    return !(lhs == rhs);
}

bool operator< (const StrBlob& lhs, const StrBlob& rhs)
{
    return std::lexicographical_compare(lhs.data->begin(), lhs.data->end(), rhs.data->begin(), rhs.data->end());
}

bool operator> (const StrBlob& lhs, const StrBlob& rhs)
{
    return rhs < lhs;
}

bool operator<=(const StrBlob& lhs, const StrBlob& rhs)
{
    return !(rhs < lhs);
}

bool operator>=(const StrBlob& lhs, const StrBlob& rhs)
{
    return !(lhs < rhs);
}

//================================================================
//
//		StrBlobPtr - operators
//
//================================================================

bool operator==(const StrBlobPtr& lhs, const StrBlobPtr& rhs)
{
    return lhs.curr == rhs.curr;
}

bool operator!=(const StrBlobPtr& lhs, const StrBlobPtr& rhs)
{
    return !(lhs == rhs);
}

bool operator< (const StrBlobPtr& x, const StrBlobPtr& y)
{
    return x.curr < y.curr;
}

bool operator>(const StrBlobPtr& x, const StrBlobPtr& y)
{
    return x.curr > y.curr;
}

bool operator<=(const StrBlobPtr& x, const StrBlobPtr& y)
{
    return x.curr <= y.curr;
}

bool operator>=(const StrBlobPtr& x, const StrBlobPtr& y)
{
    return x.curr >= y.curr;
}

//================================================================
//
//		ConstStrBlobPtr - operators
//
//================================================================

bool operator==(const ConstStrBlobPtr& lhs, const ConstStrBlobPtr& rhs)
{
    return lhs.curr == rhs.curr;
}

bool operator!=(const ConstStrBlobPtr& lhs, const ConstStrBlobPtr& rhs)
{
    return !(lhs == rhs);
}

bool operator< (const ConstStrBlobPtr& lhs, const ConstStrBlobPtr& rhs)
{
    return lhs.curr < rhs.curr;
}

bool operator>(const ConstStrBlobPtr& lhs, const ConstStrBlobPtr& rhs)
{
    return lhs.curr > rhs.curr;
}

bool operator<=(const ConstStrBlobPtr& lhs, const ConstStrBlobPtr& rhs)
{
    return lhs.curr <= rhs.curr;
}

bool operator>=(const ConstStrBlobPtr& lhs, const ConstStrBlobPtr& rhs)
{
    return lhs.curr >= rhs.curr;
}

//==================================================================
//
//		copy assignment operator and move assignment operator.
//
//==================================================================

StrBlob& StrBlob::operator=(const StrBlob& lhs)
{
    data = make_shared<vector<string>>(*lhs.data);
    return *this;
}

StrBlob& StrBlob::operator=(StrBlob&& rhs) NOEXCEPT
{
    if (this != &rhs) {
        data = std::move(rhs.data);
        rhs.data = nullptr;
    }

    return *this;
}

//==================================================================
//
//		members
//
//==================================================================

StrBlobPtr StrBlob::begin()
{
    return StrBlobPtr(*this);
}

StrBlobPtr StrBlob::end()
{
    return StrBlobPtr(*this, data->size());
}

ConstStrBlobPtr StrBlob::cbegin() const
{
    return ConstStrBlobPtr(*this);
}

ConstStrBlobPtr StrBlob::cend() const
{
    return ConstStrBlobPtr(*this, data->size());
}
```

##### StrVec

```c++
//****************h****************
#include <memory>
#include <string>
#include <initializer_list>

#ifndef _MSC_VER
#define NOEXCEPT noexcept
#else
#define NOEXCEPT
#endif

class StrVec
{
    friend bool operator==(const StrVec&, const StrVec&);
    friend bool operator!=(const StrVec&, const StrVec&);
    friend bool operator< (const StrVec&, const StrVec&);
    friend bool operator> (const StrVec&, const StrVec&);
    friend bool operator<=(const StrVec&, const StrVec&);
    friend bool operator>=(const StrVec&, const StrVec&);

public:
    StrVec() : elements(nullptr), first_free(nullptr), cap(nullptr) { }
    StrVec(std::initializer_list<std::string>);
    StrVec(const StrVec&);
    StrVec& operator=(const StrVec&);
    StrVec(StrVec&&) NOEXCEPT;
    StrVec& operator=(StrVec&&)NOEXCEPT;
    ~StrVec();

    void push_back(const std::string&);
    size_t size() const { return first_free - elements; }
    size_t capacity() const { return cap - elements; }
    std::string* begin() const { return elements; }
    std::string* end() const { return first_free; }

    std::string& at(size_t pos) { return *(elements + pos); }
    const std::string& at(size_t pos) const { return *(elements + pos); }

    void reserve(size_t new_cap);
    void resize(size_t count);
    void resize(size_t count, const std::string&);

    StrVec& operator=(std::initializer_list<std::string>);
    std::string& operator[](std::size_t n) { return elements[n]; }
    const std::string& operator[](std::size_t n) const { return elements[n]; }

private:
    std::pair<std::string*, std::string*> alloc_n_copy(const std::string*, const std::string*);
    void free();
    void chk_n_alloc() { if (size() == capacity()) reallocate(); }
    void reallocate();
    void alloc_n_move(size_t new_cap);
    void range_initialize(const std::string*, const std::string*);

private:
    std::string* elements;
    std::string* first_free;
    std::string* cap;
    std::allocator<std::string> alloc;
};

bool operator==(const StrVec&, const StrVec&);
bool operator!=(const StrVec&, const StrVec&);
bool operator< (const StrVec&, const StrVec&);
bool operator> (const StrVec&, const StrVec&);
bool operator<=(const StrVec&, const StrVec&);
bool operator>=(const StrVec&, const StrVec&);

//****************cpp****************
#include"StrVec.h"

void StrVec::push_back(const std::string& s)
{
    chk_n_alloc();
    alloc.construct(first_free++, s);
}

std::pair<std::string*, std::string*>
StrVec::alloc_n_copy(const std::string* b, const std::string* e)
{
    auto data = alloc.allocate(e - b);
    return{ data, std::uninitialized_copy(b, e, data) };
}

void StrVec::free()
{
    if (elements) {
        for (auto p = first_free; p != elements;)
            alloc.destroy(--p);
        alloc.deallocate(elements, cap - elements);
    }
}

StrVec::StrVec(const StrVec& rhs)
{
    auto newdata = alloc_n_copy(rhs.begin(), rhs.end());
    elements = newdata.first;
    first_free = cap = newdata.second;
}

StrVec::~StrVec()
{
    free();
}

StrVec& StrVec::operator = (const StrVec& rhs)
{
    auto data = alloc_n_copy(rhs.begin(), rhs.end());
    free();
    elements = data.first;
    first_free = cap = data.second;
    return *this;
}

void StrVec::alloc_n_move(size_t new_cap)
{
    auto newdata = alloc.allocate(new_cap);
    auto dest = newdata;
    auto elem = elements;
    for (size_t i = 0; i != size(); ++i)
        alloc.construct(dest++, std::move(*elem++));
    free();
    elements = newdata;
    first_free = dest;
    cap = elements + new_cap;
}

void StrVec::reallocate()
{
    auto newcapacity = size() ? 2 * size() : 1;
    alloc_n_move(newcapacity);
}

void StrVec::reserve(size_t new_cap)
{
    if (new_cap <= capacity()) return;
    alloc_n_move(new_cap);
}

void StrVec::resize(size_t count)
{
    resize(count, std::string());
}

void StrVec::resize(size_t count, const std::string& s)
{
    if (count > size()) {
        if (count > capacity()) reserve(count * 2);
        for (size_t i = size(); i != count; ++i)
            alloc.construct(first_free++, s);
    }
    else if (count < size()) {
        while (first_free != elements + count)
            alloc.destroy(--first_free);
    }
}

StrVec& StrVec::operator=(std::initializer_list<std::string>il)
{
    auto data = alloc_n_copy(il.begin(), il.end());
    free();
    elements = data.first;
    first_free = cap = data.second;
    return *this;
}

bool operator==(const StrVec&lhs, const StrVec&rhs) {
    return (lhs.size() == rhs.size()) && (std::equal(lhs.begin(), lhs.end(), rhs.begin()));
}

bool operator!=(const StrVec& lhs, const StrVec& rhs) {
    return !(lhs == rhs);
}

bool operator<(const StrVec& lhs, const StrVec& rhs)
{
    return std::lexicographical_compare(lhs.begin(), lhs.end(), rhs.begin(), rhs.end());
}

bool operator>(const StrVec& lhs, const StrVec& rhs)
{
    return rhs < lhs;
}

bool operator<=(const StrVec& lhs, const StrVec& rhs)
{
    return !(rhs < lhs);
}

bool operator>=(const StrVec& lhs, const StrVec& rhs)
{
    return !(lhs < rhs);
}
```



##### 14.34

```c++
class ITE {
public:
	int operator()(bool a, int b, int c) {
		return a ? b : c;
	}
};
```

##### 13.36

```c++
class GetInput {
public:
	GetInput(std::istream& i = std::cin) : is(i) { }
	std::string operator()() const {
		std::string str;
		std::getline(is, str);
		return is ? str : std::string();
	}

private:
	std::istream& is;
};

int main() {
	
	GetInput getInput;
	std::vector<std::string> vec;
	for (std::string tmp; !(tmp = getInput()).empty(); ) vec.push_back(tmp);
	for (const auto& str : vec) std::cout << str << " ";
	std::cout << std::endl;

	system("pause");
	return 0;
}
```

##### 14.37

```c++
class IsEqual {
public:
	IsEqual(int num):value(num){}
	bool operator()(int elem) {
		return value == elem;
	}
private:
	int value;
};

int main() {
	
	vector<int>vec{ 0,1,2,3,4,5,6,5,5,5,7,7 };
	IsEqual ie(5);
	std::replace_if(vec.begin(), vec.end(), IsEqual(5), -1);
	for (auto i : vec)
		cout << i << " ";
	cout << endl;

	system("pause");
	return 0;
}
```

##### 14.39

```c++
class IsInRange {
public:
	IsInRange(int lower,int upper):_lower(lower),_upper(upper){}
	bool operator()(const string str)const  {
		return str.size() >= _lower && str.size() <= _upper;
	}

private:
	int _lower;
	int _upper;
};

int main() {
	
	ifstream ifs("14.39.txt");
	string word;
	int count = 0;
	int count10 = 0;
	vector<int>vec{0,1,2,3,4,5,6,7,8,9,10 };
	while (ifs >> word) {
		if (IsInRange(1, 9)(word))
			++count;
		else ++count10;
	}
	cout << count << endl;
	cout << count10 << endl;

	system("pause");
	return 0;
}
```

##### 14.42

```c++
std::count_if(ivec.cbegin(), ivec.cend(), std::bind(std::greater<int>(), _1, 1024));
std::find_if(svec.cbegin(), svec.cend(), std::bind(std::not_equal_to<std::string>(), _1, "pooh"));
std::transform(ivec.begin(), ivec.end(), ivec.begin(), std::bind(std::multiplies<int>(), _1, 2));
```

##### 14.43

```c++
#include <iostream>
#include <string>
#include <functional>
#include <algorithm>

int main()
{
    auto data = { 2, 3, 4, 5 };
    int input;
    std::cin >> input;
    std::modulus<int> mod;
    auto predicator = [&](int i){ return 0 == mod(input, i); };
    auto is_divisible = std::any_of(data.begin(), data.end(), predicator);
    std::cout << (is_divisible ? "Yes!" : "No!") << std::endl;

    return 0;
}
```

##### 14.44

```c++
int add(int i, int j) { return i + j; }
auto mod = [](int i, int j) { return i % j; };
struct Div { int operator ()(int i, int j) const { return i / j; } };

auto binops = std::map<std::string, std::function<int(int, int)>>
{
    { "+", add },                               // function pointer 
    { "-", std::minus<int>() },                 // library functor 
    { "/", Div() },                             // user-defined functor 
    { "*", [](int i, int j) { return i * j; } },  // unnamed lambda 
    { "%", mod }                                // named lambda object 
};
```

