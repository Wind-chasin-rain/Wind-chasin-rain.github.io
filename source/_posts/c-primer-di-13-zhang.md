---
title: C++ Primer(第13章 拷贝控制)
date: 2022-03-24 16:02:13
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071356674.jpg
tags: C++
categories: 学习笔记
description: 第13章 拷贝控制
updated:
mathjax:
highlight_shrink:
---
##### 13.5

```c++
class HasPtr {
public:
	HasPtr(const string &s=string()):ps(new string()),i(0){}
	HasPtr(const HasPtr& hp): ps(new string(*hp.ps)),i(hp.i){}
private:
	string* ps;
	int i;
};
```

##### 13.8

```c++
	HasPtr& operator=(const HasPtr& hp) {
		string* temp_ps = new string(*hp.ps);
		delete ps;
		ps = temp_ps;
		i = hp.i;
		return *this;
	}
```

##### 13.17

```c++
class numbered
{
public:
	numbered()
	{
		cout << "numbered()" << endl;
		mysn = unique++;
	}
	numbered(const numbered& n)
	{
		cout << "numbered(const numbered& n)" << endl;
		mysn = unique++;
	}

	int mysn;
	static int unique;
};

int numbered::unique = 10;

void f(const numbered& s)
{
	cout << s.mysn <<endl;
}


int main() {
	numbered a, b = a, c = b;
	f(a);
	f(b);
	f(c);

	system("pause");
	return 0;
}
```

##### 13.18

```c++
class Employee {
public:
	Employee() {
		m_Id = num++;
	}
	Employee(string name) {
		m_Name = name;
		m_Id = num++;
	}
	Employee(const Employee&) = delete;
	Employee operator=(const Employee&) = delete;
	int id() { return m_Id; }
private:
	static int num;
	string m_Name;
	int m_Id;
};
int Employee::num = 10;
```

##### 13.22

```c++
class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
    HasPtr& operator=(const HasPtr &hp) {
        auto new_p = new std::string(*hp.ps);
        delete ps;
        ps = new_p;
        i = hp.i;
        return *this;
    }
    ~HasPtr() {
        delete ps;
    } 
private:
    std::string *ps;
    int i;
};
```

##### 13.27

```c++
class HasPtr {
public:
	HasPtr(const string &s=string()):ps(new string()),i(0),use(new size_t(1)) {}
	HasPtr(const HasPtr& hp) : ps(new string(*hp.ps)), i(hp.i), use(new size_t(*hp.use)) { ++* use; }
	HasPtr& operator=(const HasPtr& hp) {
		++*hp.use;
		if (-- * use == 0) {
			delete ps;
			delete use;
		}
		ps = hp.ps;
		i = hp.i;
		use = hp.use;
		return *this;
	}
	~HasPtr() {
		if (-- * use == 0) {
			delete ps;
			delete use;
		}
	}
private:
	string* ps;
	int i;
	size_t* use;
};
```

##### 13.28

```c++
class TreeNode {
public:
	TreeNode():value(string()),count(new int(1)),left(nullptr),right(nullptr) {}
	TreeNode(const TreeNode& rhs) :value(rhs.value), count(new int(*rhs.count)),left(rhs.left),right(rhs.right) { ++* count; }
	TreeNode& operator=(const TreeNode& rhs);
	~TreeNode() {
		if (-- * count == 0) {
			delete left;
			delete right;
			delete count;
		}
	}
private:
	string value;
	int* count;
	TreeNode* left;
	TreeNode* right;
};
TreeNode& TreeNode::operator=(const TreeNode& rhs) {
	++* rhs.count;
	if (-- * count == 0) {
		delete left;
		delete right;
		delete count;
	}
	value = rhs.value;
	count = rhs.count;
	left = rhs.left;
	right = rhs.right;
	return *this;
}
class BinStrTree {
public:
	BinStrTree():root(new TreeNode()){}
	BinStrTree(const BinStrTree&bst):root(new TreeNode(*bst.root)){}
	BinStrTree& operator=(const BinStrTree& bst);
	~BinStrTree() {
		delete root;
	}
private:
	TreeNode* root;
};

BinStrTree& BinStrTree::operator=(const BinStrTree& bst) {
	TreeNode* temp_r = bst.root;
	delete root;
	root = temp_r;
	return *this;
}
```

##### 13.30

```c++
class HasPtr {
	friend void swap(HasPtr&, HasPtr&);
public:
	HasPtr(const string &s=string()):ps(new string(s)),i(0),use(new size_t(1)) {}
	HasPtr(const HasPtr& hp) : ps(new string(*hp.ps)), i(hp.i), use(new size_t(*hp.use)) { ++* use; }
	HasPtr& operator=(const HasPtr& hp) {
		++*hp.use;
		if (-- * use == 0) {
			delete ps;
			delete use;
		}
		ps = hp.ps;
		i = hp.i;
		use = hp.use;
		return *this;
	}
	~HasPtr() {
		if (-- * use == 0) {
			delete ps;
			delete use;
		}
	}
	void show() { cout << *ps << endl; }
private:
	string* ps;
	int i;
	size_t* use;
};
inline void swap(HasPtr&lhs, HasPtr&rhs) {
	using std::swap;
	swap(lhs.ps, rhs.ps);
	swap(lhs.i, lhs.i);
	cout << "call swap(HasPtr& lhs, HasPtr& rhs)" << endl;
}
int main() {

	HasPtr a("hello");
	HasPtr b("world");
	a.show();
	b.show();
	swap(a, b);
	a.show();
	b.show();
	system("pause");
	return 0;
}
```

##### 10.31

```c++
bool operator < (HasPtr&lhs, HasPtr&rhs) {
	cout << "call operator < (HasPtr&lhs, HasPtr&rhs) " << endl;
	return lhs.ps < rhs.ps;
}
int main() {
	
	HasPtr s{ "s" }, a{ "a" }, c{ "c" };
	vector<HasPtr> vec{ s, a, c };
	sort(vec.begin(), vec.end());
	for (auto v : vec) {
		v.show();
	}
	system("pause");
	return 0;
}
```

##### 13.37

```c++
//.h
class Folder;

class Message
{
	friend void swap(Message&, Message&);
	friend void swap(Folder&, Folder&);
	friend class Folder;
public:
	explicit Message(const std::string& s = "") :contents(s) {}
	Message(const Message&);
	Message& operator=(const Message&);
	~Message();
	void save(Folder&);
	void remove(Folder&);
	void print_debug();

private:
	std::string contents;
	std::set<Folder*> folders;

	void add_to_Folders(const Message&);
	void remove_from_Folders();

	void addFldr(Folder* f) { folders.insert(f); }
	void remFlddr(Folder* f) { folders.erase(f); }
};

void swap(Message&, Message&);

class Folder
{
	friend void swap(Message&, Message&);
	friend void swap(Folder&, Folder&);
	friend class Message;
public:
	Folder() = default;
	Folder(const Folder&);
	Folder& operator=(const Folder&);
	~Folder();
	void print_debug();

private:
	std::set<Message*> msgs;

	void add_to_Message(const Folder&);
	void remove_from_Message();

	void addMsg(Message* m) { msgs.insert(m); }
	void remMsg(Message* m) { msgs.erase(m); }
};

void swap(Folder&, Folder&);


//.cpp
void swap(Message& lhs, Message& rhs)
{
	using std::swap;
	for (auto f : lhs.folders)
		f->remMsg(&lhs);
	for (auto f : rhs.folders)
		f->remMsg(&rhs);

	swap(lhs.folders, rhs.folders);
	swap(lhs.contents, rhs.contents);

	for (auto f : lhs.folders)
		f->addMsg(&lhs);
	for (auto f : rhs.folders)
		f->addMsg(&rhs);
}

void Message::save(Folder& f)
{
	folders.insert(&f);
	f.addMsg(this);
}

void Message::remove(Folder& f)
{
	folders.erase(&f);
	f.remMsg(this);
}

void Message::add_to_Folders(const Message& m)
{
	for (auto f : m.folders)
		f->addMsg(this);
}

Message::Message(const Message& m) :contents(m.contents), folders(m.folders)
{
	add_to_Folders(m);
}

void Message::remove_from_Folders()
{
	for (auto f : folders)
		f->remMsg(this);
	folders.clear();
}

Message::~Message()
{
	remove_from_Folders();
}

Message& Message::operator=(const Message& rhs)
{
	remove_from_Folders();
	contents = rhs.contents;
	folders = rhs.folders;
	add_to_Folders(rhs);
	return *this;
}

void Message::print_debug()
{
	std::cout << contents << std::endl;
}

void swap(Folder& lhs, Folder& rhs)
{
	using std::swap;
	for (auto m : lhs.msgs)
		m->remFlddr(&lhs);
	for (auto m : rhs.msgs)
		m->remFlddr(&rhs);

	swap(lhs.msgs, rhs.msgs);

	for (auto m : lhs.msgs)
		m->addFldr(&lhs);
	for (auto m : rhs.msgs)
		m->addFldr(&rhs);
}

void Folder::add_to_Message(const Folder& f)
{
	for (auto m : f.msgs)
		m->addFldr(this);
}

Folder::Folder(const Folder& f) :msgs(f.msgs)
{
	add_to_Message(f);
}

void Folder::remove_from_Message()
{
	for (auto m : msgs)
		m->remFlddr(this);
	msgs.clear();
}

Folder::~Folder()
{
	remove_from_Message();
}

Folder& Folder::operator=(const Folder& rhs)
{
	remove_from_Message();
	msgs = rhs.msgs;
	add_to_Message(rhs);
	return *this;
}

void Folder::print_debug()
{
	for (auto m : msgs)
		std::cout << m->contents << " ";
	std::cout << std::endl;
}
```

##### 10.39

```c++
class StrVec
{
public:
	StrVec() : elements(nullptr), first_free(nullptr), cap(nullptr) { }
	StrVec(const StrVec&);
	StrVec& operator=(const StrVec&);
	~StrVec();

	void push_back(const std::string&);
	size_t size() const { return first_free - elements; }
	size_t capacity() const { return cap - elements; }
	std::string* begin() const { return elements; }
	std::string* end() const { return first_free; }

	void reserve(size_t new_cap);
	void resize(size_t count);
	void resize(size_t count, const std::string&);

private:
	std::pair<std::string*, std::string*> alloc_n_copy(const std::string*, const std::string*);
	void free();
	void chk_n_alloc() { if (size() == capacity()) reallocate(); }
	void reallocate();
	void alloc_n_move(size_t new_cap);

private:
	std::string* elements;
	std::string* first_free;
	std::string* cap;
	std::allocator<std::string> alloc;
};

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
```

##### 13.40

```c++
void StrVec::range_initialize(const std::string* first, const std::string* last)
{
	auto newdata = alloc_n_copy(first, last);
	elements = newdata.first;
	first_free = cap = newdata.second;
}
StrVec::StrVec(std::initializer_list<std::string> il)
{
	range_initialize(il.begin(), il.end());
}
```

##### 13.43

```c++
for_each(elements, first_free, [this](string& rhs) {alloc.destroy(&rhs); });
alloc.deallocate(elements,cap-elements);
```

##### 13.44

```c++
class String {
public:
	String()=default;
	String(const char*);
	String(const String&);
	String& operator=(const String&);
	~String() { free(); }

private:
	allocator<char>alloc;
	char* elements;
	char* end;

	pair<char*, char*> alloc_n_copy(const char* b, const char* e);
	void range_initializer(const char* b, const char* e);
	void free();

};

pair<char*, char*> String::alloc_n_copy(const char* b, const char* e) {
	auto data = alloc.allocate(e - b);
	return { data,uninitialized_copy(b,e,data) };
}

void String::range_initializer(const char* b, const char* e) {
	auto newdata = alloc_n_copy(b, e);
	elements = newdata.first;
	end = newdata.second;
}

//接受c风格字符串参数的构造函数，s为指向字符串的指针(首位置)
String::String(const char* s) {
	auto s1 = const_cast<char*>(s);//转化为非常量的指针
	while (*s1) {
		++s1;//使其指向最后一个位置的尾部
	}
	alloc_n_copy(s, s1);
}

String::String(const String& rhs) {
	auto newdata = alloc_n_copy(rhs.elements, rhs.end);
	elements = newdata.first;
	end = newdata.second;
}

void String::free() {
	if (elements) {
		for (auto p = end; p != elements;) {
			alloc.destroy(--p);
		}
		alloc.deallocate(elements, end - elements);
	}
}

String& String::operator=(const String&rhs) {
	auto data = alloc_n_copy(rhs.elements, rhs.end);
	free();
	elements = data.first;
	end = data.second;
	return *this;
}
```

##### 13.48

```c++
int main() {
	
    char text[] = "world";

    String s0;
    String s1("hello");
    String s2(s0);//1
    String s3 = s1;//2 拷贝构造
    String s4(text);
    s2 = s1;//3 拷贝赋值运算符


    std::vector<String> svec;
    svec.reserve(5);
    svec.push_back(s0);//4  拷贝构造
    svec.push_back(s1);//5  
    svec.push_back(s2);//6
    svec.push_back(s3);//7
    svec.push_back(s4);//8


	system("pause");
	return 0;
}
```

##### 13.49

```c++
	StrVec(StrVec&& s) noexcept :elements(s.elements), first_free(s.first_free), cap(s.cap) {
		s.elements = s.first_free = s.cap = nullptr;
	}
	StrVec& operator=(StrVec&& rhs)noexcept {
		if (this != &rhs) {
			free();
			elements = rhs.elements;
			first_free = rhs.first_free;
			cap = rhs.cap;
			rhs.elements = rhs.first_free = rhs.cap = nullptr;
		}
		return *this;
	}

	String(String&& s)noexcept :elements(s.elements), end(s.end) { s.elements = s.end = nullptr; }
	String& operator=(String&& rhs) noexcept{
		if (this != &rhs) {
			free();
			elements = rhs.elements;
			end = rhs.end;
			rhs.elements = rhs.end = nullptr;
		}
		return *this;
	}
```

##### 13.58

```c++
class Foo {
public:
    Foo sorted()&&;
    Foo sorted() const&;
private:
    vector<int> data;
};
Foo Foo::sorted()&& {
    sort(data.begin(), data.end());
    std::cout << "&&" << std::endl; // debug
    return *this;
}

Foo Foo::sorted() const& {
    //    Foo ret(*this);
    //    sort(ret.data.begin(), ret.data.end());
    //    return ret;

    std::cout << "const &" << std::endl; // debug

//    Foo ret(*this);
//    ret.sorted();     // Exercise 13.56
//    return ret;

    return Foo(*this).sorted(); // Exercise 13.57
}
int main() {

    Foo().sorted(); // call "&&"
    Foo f;
    f.sorted(); // call "const &"

	system("pause");
	return 0;
}
```

