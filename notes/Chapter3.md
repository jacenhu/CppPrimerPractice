## 第三章 字符串、向量和数组

P74-P81

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

### 标准库类型 vector

### 迭代器介绍

### 数组

### 多维数组
