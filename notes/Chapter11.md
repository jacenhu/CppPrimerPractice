## 第十一章 关联容器

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P374-P397

关联容器支持高效的关键字查找和访问。

| 类型               | 备注                              |
| ------------------ | --------------------------------- |
| map                | 关联数组，保存关键字-值对         |
| set                | 值保存关键字的容器                |
| multimap           | 关键字可重复出现的map             |
| multiset           | 关键字可重复出现的set             |
| unordered_map      | 用哈希函数组织的map               |
| unordered_set      | 用哈希函数组织的set               |
| unordered_multimap | 哈希组织的map；关键字可以重复出现 |
| unordered_multiset | 哈希组织的set；关键字可以重复出现 |

### 11.1 使用关联容器

map是关键词-值对的集合。

为了定义一个map，我们必须指定关键字和值的类型。

````cpp
// 统计每个单词在输入中出现的次数
map<string, size_t> word_count;
string word;
while (cin >> word)
    ++word_count[word];
for (const auto &w : word_count)
  	count << w.first << " cccurs " < w.second 
  				<< ((w.second > 1) ? " times" : "time") << endl;
````

set是关键字的简单集合。

为了定义一个set，必须指定其元素类型。

```cpp
// 统计输入中每个单词出现的次数，并忽略常见单词
map<string, size_t> word_count;
set<string> exclude = {"the", "But"};
string word;
while (cin >> word)
		// 只统计不在exclude中的单词
		if (exclude.find(word) == exclude.end())
				++word_count[word]; //获取并递增word的计数器
```

### 11.2 关联容器概述

（1）定义关联容器

定义map时，必须指明关键字类型又指明值类型；

定义set时，只需指明关键字类型。

（2）pair类型

pair标准库类型定义在头文件utility中。

一个pair保存两个数据成员。当创建一个pair时，必须提供两个类型名。

```cpp
pair<string, string> anon; // 保存两个string
pair<string, string> author{"James", "Joyce"}; // 也可为每个成员提供初始化器
```

pair的数据类型是public的，两个成员分别命名为first和second。

pair上的操作，见表，P380

### 11.3 关联容器操作

| 关联容器额外的类型别名 |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| key_type               | 此容器类型的关键字类型                                       |
| mapped_type            | 每个关键字关联的类型，只适用于map                            |
| value_type             | 对于set，与key_type相同；对于map，为pair<const key_type, mapped_type> |

（1）关联容器迭代器

set的迭代器是const的

set和map的关键字都是const的

**遍历关联容器**：

map和set都支持begin和end操作。使用beigin、end获取迭代器，然后用迭代器来遍历容器。

（2）添加元素

| 关联容器insert操作 |                          |
| ------------------ | ------------------------ |
| c.insert(v)        | v是value_type类型的对象; |
| c.emplace(args)    | args用来构造一个元素     |
| c.insert(b, e)     |                          |
| c.insert(il)       |                          |
| c.insert(p, v)     |                          |
| c.emplace(p ,args) |                          |

（3）删除元素

| 从关联容器删除元素 |                                                              |
| ------------------ | ------------------------------------------------------------ |
| c.erase(k)         | 从c中删除每个关键字为k的元素。返回一个size_type值，指出删除的元素的数量 |
| c.erase(p)         | 从c中删除迭代器p指定的元素。p必须指向c中一个真实元素，不能等于c.end()。返回一个指向p之后元素的迭代器，若p指向c中的尾元素，则返回.end() |
| c.erase(b, e)      | 删除迭代器b和e所表示的范围中的元素。返回e                    |

（4）map的下标操作

| map和unorder_map的下标操作 |                                                              |
| -------------------------- | ------------------------------------------------------------ |
| c[k]                       | 返回关键字为k的元素；如果k不在c中，添加一个关键字为k的元素，对其进行值初始化 |
| c.at[k]                    | 访问关键字为k的元素，带参数检查；若k不在c中，抛出一个out_of_range异常 |

::: tip

map进行下标操作，会获得mapped_type对象；当解引用时，会得到value_type对象。

:::

（5）访问元素

```cpp
c.find(k)  // 返回一个迭代器，指向第一个关键字k的元素，如k不在容器中，则返回尾后迭代器
c.count(k)  // 返回关键字等于k的元素的数量。对于不允许重复关键字的容器，返回值永远是0或1
c.lower_bound(k)  // 返回一个迭代器，指向第一个关键字不小于k的元素;不适用于无序容器
c.upper_bound(k)  // 返回一个迭代器，指向第一个关键字大于k的元素；不适用于无序容器
c.equal_bound(k)  // 返回一个迭代器pair，表示关键字等于k的元素的范围。如k不存在，pair的两个成员均等于c.end()
```

### 11.4 无序容器

无序容器使用关键字类型的==运算符和一个hash<key_type>类型的对象来组织元素。

无序容器在存储上组织为一组桶，适用一个哈希函数将元素映射到桶。

无序容器管理操作，表格，P395

还可以自定义自己的hash模板 P396

```cpp
using SD_multiset = unordered_multiset<Sales_data, decltype(hasher)*, decltype(eqOp)*>;
SD_multiset bookStore(42, haser, eqOp);
```