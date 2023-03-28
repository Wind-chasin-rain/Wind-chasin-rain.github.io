---
title: C++ Primer(第8章 IO库)
date: 2022-03-08 20:03:28
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071351719.jpg
tags: C++
categories: 学习笔记
description: 第8章 IO库
updated:
mathjax:
highlight_shrink:
---


##### 8.1

```c++
istream& iofunc(istream& is) {
	string s;
	while (is >> s) {
		cout << s << endl;
	}
	is.clear();
	return is;
}


int main() {

	iofunc(cin);
	system("pause");
	return 0;
}
```

##### 8.4

```c++
	vector<string>v;
	ifstream ifs;
	ifs.open("ifile.txt", ios::in);
	if (ifs.is_open()) //if(ifs>>s)逐个单词
    {
		string s;
		while (getline(ifs,s)) {
			v.push_back(s);
			cout << s << endl;
		}
		ifs.close();
	}
```

##### 8.10

```c++
	string line, word;
	vector<string> vec;
	while (getline(cin,line))
	{	
		vec.push_back(line);
		cout << vec.back() << endl;
		istringstream record(vec.back());
		while (record >> word) {
			cout << word << endl;
		}
	}

```

##### 8.11

```c++
struct PersonInfo {
	string name;
	vector<string> phones;
};

int main() {
	string line, word;
	vector<PersonInfo> people;
	istringstream record;

	while (getline(cin, line)) {
		record.str(line);
		PersonInfo info;
		record >> info.name;
		while (record >> word) {
			info.phones.push_back(word);
		}
		record.clear();//复位
		people.push_back(info);
	}

	for (const auto &entry : people) {
		cout << entry.name << " ";
		for (const auto &ph : entry.phones) {
			cout << ph << " ";
		}
		cout << endl;
	}

	return 0;
}
```

