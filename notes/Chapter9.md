## 第九章 顺序容器

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P292-P332

顺序容器为程序员提供了控制元素存储和访问顺序的能力。

### 9.1 顺序容器概述

| 类型         | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| vector       | 可变数组大小。支持快速随机访问。在尾部之外的位置插入或删除元素可能很慢。 |
| deque        | 双端队列。支持快速随机访问。在头尾位置插入/删除速度很快。    |
| list         | 双向链表。只支持双向顺序访问。在list中任何位置进行插入/删除操作速度都很快。 |
| forward_list | 单向链表。只支持单向顺序访问。在链表任何位置进行插入/删除操作速度都很快。 |
| array        | 固定大小数组。支持快速随机访问。不能添加或删除元素。         |
| string       | 与vector相似的容器，但专门用于保存字符、随机访问快。在尾部插入/删除速度快。 |

### 9.2 容器库概述

一般，每个容器都定义在一个头文件中。

容器均定义为模板类。

| 类型别名        |                                                        |
| --------------- | ------------------------------------------------------ |
| iterator        | 此容器类型的迭代器类型                                 |
| const_iterator  | 可以读取元素，但不能修改元素的迭代器类型               |
| size_type       | 无符号整数类型，足够保存此种容器类型最大可能容器的大小 |
| difference_type | 带符号整数类型，足够保存两个迭代器之间的距离           |
| value_type      | 元素类型                                               |
| reference       | 元素的左值诶性：与value_type&含义相同                  |
| const_reference | 元素的const左值类型（即，const value_type&）           |

| 构造函数        |                                                             |
| --------------- | ----------------------------------------------------------- |
| C c;            | 默认构造函数，构造空容器                                    |
| C c1(c2)        | 构造c2的拷贝c1                                              |
| C c(b, e)       | 构造c，将迭代器b和e指定的范围内的元素拷贝到c（array不支持） |
| C c{a, b, c...} | 列表初始化c                                                 |

| 赋值与swap        |                                               |
| ----------------- | --------------------------------------------- |
| c1=c2             | 将c1中的元素替换为c2中元素                    |
| c1 = {a, b, c...} | 将c1中的元素替换为列表中元素（不适用于array） |
| a.swap(b)         | 交换a和b的元素                                |
| swap(a, b)        | 与a.swap(b)等价                               |

| 大小         |                                         |
| ------------ | --------------------------------------- |
| c.size()     | c中元素的数组（不支持forward_list）     |
| c.max_size() | c中可保存的最大元素数目                 |
| c.empty()    | 若c中存储了元素，返回false,否则返回true |

| 添加/删除元素（不适用于array） |                             |
| ------------------------------ | --------------------------- |
| c.insert(args)                 | 将args中的元素拷贝进c       |
| c.emplace(inits)               | 使用inits构造c中的一个元素  |
| c.erase(args)                  | 删除args指定的元素          |
| c.clear()                      | 删除c中的所有元素，返回void |

| 关系运算符 |                                 |
| ---------- | ------------------------------- |
| ==, !=     | 所有容器都支持相等(不等运算符)  |
| <,<=,>,>=  | 关系运算符(无序关联容器不支持） |

| 获取迭代器           |                                           |
| -------------------- | ----------------------------------------- |
| c.begin(), c.end()   | 返回指向c的首元素和尾元素之后位置的迭代器 |
| c.cbengin(),c.cend() | 返回const_iterator                        |

| 反向容器的额外成员（不支持forward_list） |                                           |
| ---------------------------------------- | ----------------------------------------- |
| reverse_iterator                         | 按逆序寻址元素的迭代器                    |
| const_reverse_iterator                   | 不能修改元素的逆序迭代器                  |
| c.rbegin(), c.rend()                     | 返回指向c的尾元素和首元素之前位置的迭代器 |
| c.crbegin(), c.crend()                   | 返回const_reverse_iterator                |

（1）迭代器

标准库的迭代器允许我们访问容器中的元素，所有迭代器都是通过解引用运算符来实现这个操作。

一个迭代器返回由一对迭代器表示，两个迭代器分别指向同一个容器中的元素或者是尾元素之后的位置。它们标记了容器中元素的一个范围。

左闭合区间：[begin, end)

```cpp
while (begin !=end){
		*begin = val;
		++begin;
}
```

（2）容器类型成员

见概述

通过别名，可以在不了解容器中元素类型的情况下使用它。

（3）begin和end成员

begin是容器中第一个元素的迭代器

end是容器尾元素之后位置的迭代器

（4）容器定义和初始化

P290

```cpp
C c;            // 默认构造函数
C c1(c2)
C c1=c2
C c{a,b,c...}   // 列表初始化
C c={a,b,c...}
C c(b,e)        // c初始化为迭代器b和e指定范围中的元素的拷贝
// 只有顺序容器（不包括array）的构造函数才能接受大小参数
C seq(n)
C seq(n,t)
```

**将一个容器初始化为另一个容器的拷贝**:

当将一个容器初始化为另一个容器的拷贝时，两个容器的容器类型和元素类型都必须相同。

不过，当传递迭代器参数来拷贝一个范围时，就不要求容器类型相同，只要能将要拷贝的元素转换为要初始化的容器的元素类型即可。

**标注库array具有固定大小**:

不能对内置数组类型进行拷贝或对象赋值操作，但array并无此限制。P301

（5）赋值与swap

arrray类型不允许用花括号包围的值列表进行赋值。

```cpp
array<int, 10> a2={0}; //所有元素均为0
s2={0};  // 错误！
```

seq.assign(b,e)   // 将seq中的元素替换为迭代器b和e所表示的范围中的元素。迭代器b和e不能指向seq中的元素。

swap用于交换2个相同类型容器的内容。调用swap之后，两个容器中的元素将交换。

（6）容器大小操作

size 返回容器中元素的数目

empty 当size为0返回布尔值true，否则返回false

max_size 返回一个大于或等于该类型容器所能容纳的最大元素数的值

（7）关系运算符

关系运算符左右两边的元素符对象必须是相同类型的容器。

::: tip

只有当元素类型也定义了相应的比较运算符，才可以使用关系元素安抚来比较两个容器

:::

### 9.3 顺序容器操作

（1）向顺序容器添加元素

表格P305

**使用push_back**:追加到容器尾部

**使用push_front**:插入到容器头部

**在容器中的特定位置添加元素**:使用insert

```cpp
vector <string> svec;
svec.insert(svec.begin(), "Hello!");
```

**插入范围内元素**:使用insert

**使用emplace操作**:

emplace_front、emplace和emplace_back分别对应push_front、insert和push_back。

emplace函数直接在容器中构造函数，不是拷贝。

（2）访问元素

P309

注意end是指向的是容器尾元素之后的元素。

| 在顺序容器中访问元素的操作 |                                                              |
| -------------------------- | ------------------------------------------------------------ |
| c.back()                   | 返回c中尾元素的引用。若c为空，函数行为未定义                 |
| c.front()                  | 返回c中首元素的引用。若c为空，哈数行为未定义                 |
| c[n]                       | 返回c中下标为n的元素的引用，n是一个无符号整数。若n>=size(),则函数行为未定义 |
| c.at[n]                    | 返回下标为n的元素的引用。如果下标越界，则抛出out_of_range异常 |

（3）删除元素

| 顺序容器的删除操作 |                                                              |
| ------------------ | ------------------------------------------------------------ |
| c.pop_back()       | 删除c中尾元素。若c为空，则函数行为未定义。返回返回void       |
| c.pop_front()      | 删除c中首元素。若c为空，则函数行为未定义。返回void           |
| c.erase(p)         | 删除迭代器p所指定的元素，返回一个指向被删除元素之后元素的迭代器，如p指向尾元素，则返回尾后(off-the-end)迭代器。若p是尾后迭代器，则函数行为未定义 |
| c.erase(b, e)      | 删除迭代器b和e所指定范围内的元素。返回一个指向最后一个被删除元素之后元素的迭代器。若e本身就是尾后迭代器，则函数也返回尾后迭代器 |
| c.claer()          | 删除c中的所有元素。返回void                                  |

（4）特殊的forwar_list操作

P313

befor_begin();cbefore_begin();insert_after;emplace_after;erase_after;

（5）改变容器大小

reseize用于扩大或者缩小容器。

resize操作接受一个可选的元素值参数，用来初始化添加到容器内的元素。

如果容器保存的是类类型元素，且resize向容器中添加新元素，则必须提供初始值，或者元素类型必须提供一个默认构造函数。

### 9.4 vector对象是如何增长的

为了避免影性能，标准库采用了可以减少容器空间重新分配次数的策略。当不得不获取新的内存空间时，vector和string通常会分配比新的新的空间需求更大的内存空间。容器预留这些空间作为备用，可以用来保存更多的新元素。

**容器管理的成员函数**:

| 容器大小管理擦作  |                                                              |
| ----------------- | ------------------------------------------------------------ |
| c.shrink_to_fit() | 请将capacity()减少为与size()相同大小                         |
| c.capacity()      | 不重新分配内存空间的话,c可以保存多少元素                     |
| c.reverse()       | 分配至少能容纳n个元素的内存空间。reverse并不改变容器中元素的数量，它仅影响vector预先分配多大的内存空间。调用reverse永远不减少容器占用的内存空间。 |

**capcacity和size**:

区别：

容器的size是指它已经保存的元素的数目；

capcacity则是在不分配新的内存空间的前提下它最多可以保存多少元素。

注意：只有当迫不得已时才可以分配新的内存空间。

### 9.5 额外的string操作

（1）构造string的其他方法

| 构造string的其他方法      |                                            |
| ------------------------- | ------------------------------------------ |
| string s(cp, n)           | s是cp指向的数组中前n个字符的拷贝           |
| string s(s2, pos2)        | s是string s2从下标pos2开始的字符的拷贝。   |
| string s (s2, pos2, len2) | s是string s2从下标pos2开始len2个字符的拷贝 |

**substr操作**:

substr操作返回一个string，它是原始string的一部分或全部的拷贝。

s.substr(pos, n)  返回一个string，包含s中从pos开始的n个字符的拷贝。pos的默认值为0。n的默认值为s.size() - pos， 即拷贝从pos开始的所有字符

（2）改变string的其他方法

assign  替换赋值，总是替换string中的所有内容

insert  插入

append 末尾插入，总是将新字符追加到string末尾

replace 删除再插入

（3）string搜索操作

| string搜索操作            |                                               |
| ------------------------- | --------------------------------------------- |
| s.find(args)              | 查找s中args第一次出现的位置                   |
| s.rfind(args)             | 查找s中args最后一次出现的位置                 |
| s.find_first_of(args)     | 在s中查找args中任何一个字符第一次出现的位置   |
| s.find_last_of(args)      | 在s中查找args中任何一个字符最后一次出现的位置 |
| s.find_first_not_of(args) | 在s中查找第一个不在args中的字符               |
| s.find_last_not_of(args)  | 在s中查找最后一个不在args中的字符             |

（4）compare函数

compare有6个版本，P327

（5）数值转换

P328

tostring

stod

### 9.6 容器适配器

顺序容器适配器：

stack; queue; priority_queue;

适配器是一种机制，能使某种事物看起来像另外一种事物。

**定义一个适配器**:

适配器有2个构造函数:

> 1、默认构造函数创建一个空对象
>
> 2、接受一个容器的构造函数

**栈适配器**:

| 栈的操作        |                                                              |
| --------------- | ------------------------------------------------------------ |
| s.pop()         | 删除栈顶元素，但不返回该元素值                               |
| s.push(item)    | 创建一个新元素压入栈顶，该元素通过拷贝或移动item而来，或者由args构造 |
| s.emplace(args) | 由arg构造                                                    |
| s.top()         | 返回栈顶元素，但不将元素弹出栈                               |

**队列适配器**:

| queue和priority_queue操作           |                                                              |
| ----------------------------------- | ------------------------------------------------------------ |
| q.pop()                             | 返回queue的首元素或priority_queue的最高优先级的元素，但不删除此元素 |
| q.front()                  q.back() | 返回首元素或尾元素，但不删除此元素。只适用于queue            |
| q.top()                             | 返回最高优先级元素，但不删除该元素。只适用于priority_queue   |
| q.push(item) q.empalce(args)        | 在queue末尾或priority_queue中恰当的位置创建一个元素，其值为item,或者由args构造 |

### 术语

begin容器操作：返回一个指向容器首元素的迭代器，如果容器为空，则返回尾后迭代器。是否返回const迭代器依赖于容器的类型。

cbegin容器操作：返回一个指向容器尾元素之后的const_iterator。
