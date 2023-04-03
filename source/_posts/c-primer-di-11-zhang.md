---
title: C++ Primer(第11章 关联容器)
date: 2022-03-14 18:17:43
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202206071354041.jpg
tags: C++
categories: 学习笔记
description: 第11章 关联容器
updated:
mathjax:
highlight_shrink:
---
##### 11.3

```c++
int main() {
		
	map<string, size_t>word_count;
	string word;
	while (cin >> word) {
		++word_count[word];
	}
	for (const auto& m : word_count) {
		cout << m.first << " 出现了 " << m.second << " 次" << endl;
	}

	system("pause");
	return 0;
}
```

##### 11.4

```c++
int main() {
		
	map<string, size_t>word_count;
	string word;
	while (cin >> word) {
		for (auto &c : word) {	
			c = tolower(c);
		}
		word.erase(find_if(word.begin(), word.end(), ispunct),word.end());
		cout << word << endl;
		++word_count[word];
	}
	for (const auto& m : word_count) {
		cout << m.first << " 出现了 " << m.second << " 次" << endl;
	}

	system("pause");
	return 0;
}
```

##### 11.7

```c+
int main() {
		
	map<string, vector<string>>family;
	string s1;
	while (cin >> s1) {
		if (family.count(s1)) {
			cout << " 已存在家庭,添加孩子 " << endl;
			string s2;
			cin >> s2;
			family[s1].push_back(s2);
		}
		family[s1];
	}
	for (auto& f : family) {
		cout << f.first << endl;
		for (auto i : f.second) {
			cout << i << "  ";
		}
		cout << endl;
	}
	system("pause");
	return 0;
}
```

##### 11.8

```c++
int main() {
		
	vector<string>vec;
	string word;
	while (cin >> word) {
		if (find(vec.begin(), vec.end(), word) == vec.end()) {
			vec.push_back(word);
			cout << "添加成功：" << word << endl;
		}
		else {
			cout << "已存在该单词" << endl;
		}
	}
	for (auto i : vec) {
		cout << i << " ";
	}
	system("pause");
	return 0;
}
```

##### 11.12-11.13

```c++
	vector<pair<string, int>>vec;
	string s;
	int i;
	while(cin >> s >> i) {
		//vec.push_back(make_pair(s, i));
		//vec.push_back({ s,i });
		vec.emplace_back(s, i);
	}
```

##### 11.14

```c++
	map<string, vector<pair<string,string>>>family;
	string s1;
	while (cin >> s1) {
		if (family.count(s1)) {
			cout << " 已存在家庭,添加孩子 " << endl;
			string s2, s3;
			cin >> s2 >> s3;
			family[s1].emplace_back(s2,s3);
		}
		family[s1];
	}
```

##### 11.16

```c++
	map<int, string>m{ {5,"good"} };
	auto vt = m.begin();
	vt->second = "hello";
```

##### 11.22

```c++
std::pair<std::string, std::vector<int>>    // argument
std::pair<std::map<std::string, std::vector<int>>::iterator, bool> // return
```

##### 11.31

```c++
int main() {

	multimap<string, string>authors{
		{ "alan", "DMA" },
		{ "pezy", "LeetCode" },
		{ "alan", "CLRS" },
		{ "wang", "FTP" },
		{ "pezy", "CP5" },
		{ "wang", "CPP-Concurrency" }
	};
	auto it = authors.find("wang");
	auto num = authors.count("wang");
	string work = "CP5";
	while (num) {
		if (it->second == work) {
			authors.erase(it);
			break;
		}
		++it;
		--num;
	}
	for (const auto& author : authors)
		std::cout << author.first << " " << author.second << std::endl;


	system("pause");
	return 0;
}
```

##### 11.32

```c++
int main() {

	multimap<string, string>authors{
		{ "alan", "DMA" },
		{ "pezy", "LeetCode" },
		{ "alan", "CLRS" },
		{ "wang", "FTP" },
		{ "pezy", "CP5" },
		{ "wang", "CPP-Concurrency" }
	};

	map<string, multiset<string>> order_authors;
	for (const auto& author : authors)
		order_authors[author.first].insert(author.second);
	for (const auto& author : order_authors)
	{
		cout << author.first << ": ";
		for (const auto& work : author.second)
			cout << work << " ";
		cout << std::endl;
	}
	system("pause");
	return 0;
}
```

##### 11.33

```c++
map<string, string> buildMap(ifstream& map_file) {
	map<string, string>trans_map;
	string key;			//转换前
	string value;		//替换后
	// 第一个单词存入 key， 剩余存入value
	while (map_file >> key && getline(map_file, value)) {
		if (value.size() > 1)
			trans_map[key] = value.substr(1); //getline 读取了空格 要跳过前导空格
		else
			throw runtime_error("no rule for" + key);
	}
	return trans_map;
}

const string& transform(const string& s, const map<string, string>& m) {
	auto map_it = m.find(s);
	if (map_it != m.end())
		return map_it->second;
	else
		return s;
}
void word_trans(ifstream& map_file, ifstream& input) {
	auto trans_map = buildMap(map_file);		// 保存转换规则
	string text;
	while (getline(input, text)) {
		istringstream stream(text);
		string word;
		bool firstword = true;		//控制是否打印空格
		while (stream >> word) {
			if (firstword)
				firstword = false;
			else
				cout << " ";
			cout << transform(word, trans_map);
		}
		cout << endl;
	}
}
int main() {

	ifstream ifs1("11.33-1.txt");
	ifstream ifs2("11.33-2.txt");
	word_trans(ifs1, ifs2);
	system("pause");
	return 0;
}
```

