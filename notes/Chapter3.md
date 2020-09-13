## 第三章 字符串、向量和数组

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P74-P118

string表示可变长的字符序列，vector存放的是某种给定类型对象的可变长序列。

### 命名空间的 using 声明

```c++
using namespace:name;
```

头文件不应包含using声明。

### 标准库类型 string

```
#include <string>
using namespace std;
```

（1）定义和初始化

```
string s1;
sting s2(s1);
string s3("value");
string s3 = "value";
string s4(n, 'c');
```

（2）string对象的操作

```
s.empty();      // 判空
s.size();       // 字符个数
s[n];           // s中第n个字符的引用
s1+s2;          // s1和s2连接
<,<=,>,>=       // 比较
```

::: warning 

标准局允许把字面值和字符串字面值转换成string对象。字面值和string是不同的类型。

:::

(3)处理string对象中的字符

::: tip
C++程序的头文件应该使用cname，而不应该使用name.h的形式
:::

遍历给定序列中的每个值执行某种操作

```c++
for (declaration : expression)
    statement
```

### 标准库类型 vector

标准库vector表示对象的集合，其中所有对象的类型都相同。

vector是一个类模板，而不是类型。

（1）定义和初始化vector对象

```c++
vector<T> v1;
vector<T> v2(v1);
vector<T> v2 = v1;
vector<T> v3(n, val);
vector<T> v4(n);
vector<T> v5{a,b,c...}
vecrot<T> v5={a,b,c...}
```

如果用圆括号，那么提供的值是用来构造vector对象的。

如果用花括号，则是使用列表初始化该vector对象。

（2）向vector对象添加元素

先定义一个空的vector对象，在运行的时候使用push_back向其中添加具体指。

（3）其他vector操作

```c++
v.empty();
v.size();
v.push_back(t);
v[n];
```

::: warning

只能对确认已存在的元素执行下标操作。

:::

### 迭代器介绍

迭代器运算符

```c++
*iter            // 解引用，返回引用
iter->mem        // 等价于  (*iter).mem
++iter
--iter
iter1 == iter2
iter1 != iter2
iter + n
iter - n
iter += n
iter -= n
iter1 - iter2     // 两个迭代器相减的结果是它们之间的距离
>, >=, <, <=      // 位置比较
```

::: warning

凡是使用了迭代器的循环体，都不能向迭代器所属的容器添加元素。

:::

### 数组

（1）数组、指针

使用数组下标的时候，通常将其定义为size_t类型。

::: warning

定义数组必须指定数组的类型，不允许用auto推断。

不存在引用的数组。

如果两个指针分别指向不相关的对象，则不能进行对这2个指针进行比较。

:::

### 多维数组

多维数组实际上是数组的数组。

```
size_t cnt = 0;
for(auto &row : a)
	for (auto &col : row){
		col = cnt;
		++cnt;
	}
```

```c++
int *ip[4];    // 整型指针的数组
int (*ip)[4];  // 指向含有4个整数的数组
```

### 术语

**begin** string和vector的成员，返回指向第一个元素的迭代器。也是一个标准库函数，输入一个数组，返回指向该数组首元素的指针。

**end** string和vector的成员，返回一个尾后迭代器。也是一个标准库函数，输入一个数组，返回指向该数组尾元素的下一个位置的指针。