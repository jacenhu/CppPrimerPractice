## 第六章 函数

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P182-P225

### 6.1 函数基础

（1）形参和实参：

实参的类型必须与对应的形参类型匹配。

函数的调用规定实参数量应与形参数量一致。

（2）局部对象

形参和参数体内部定义的变量统称为**局部变量**，它们对函数而言是"局部"的，仅在函数的作用域内可见，同时局部变量还会隐藏外层作用域中同名的其他变量。

**自动对象**：只存在于块执行期间的对象。

局部静态对象：在程序的执行路径第一次经过对象定义语句时候进行初始化，并且直到程序终止才会被销毁。

```c++
size_t count_calls()
{
	static size_t ctr = 0;
	return ++ctr;
}
```

（3）函数声明

函数的三要素：（返回类型、函数名、形参类型）。

函数可被声明多次，但只能被定义一次。

（4）分离式编译

分离式编译允许把程序分割到几个文件中去，每个文件独立编译。

编译->链接

### 6.2 参数传递

当形参是引用类型，这时它对应的实参被引用传递或者函数被传引用调用。

当实参被拷贝给形参，这样的实参被值传递或者函数被传值调用。

（1）传值参数

（2）被引用传参

（3）const形参和实参

（4）数组形参

为函数传递一个数组时，实际上传递的是指向数组首元素的指针。

```c++
void print(const int*);
void pring(const int[]);
void print(const int[10]);
// 以上三个函数等价
```

数组引用实参： f(int (&arr)[10])

```c++
int *matrix[10];   // 10个指针构成的数组
int (*matrix)[10]; // 指向含有10个整数的数组的指针
```

（5）含有可变形参的数组

initializer_list

```c++
for err_msg(initializer_list<string> li)
```

### 6.3 返回类型和return语句

2种：无返回值函数和右返回值函数。

```c++
return;
return expression;
```

函数完成后，它所占用的存储空间也会随着被释放掉。

::: warning

返回局部对象的引用是错误的；返回局部对象的指针也是错误的。

:::

### 6.4 函数重载

重载函数：同一作用域内的几个函数名字相同但形参列表不通，我们称之为重载函数。（overloaded）。

不允许2个函数除了返回类型外其他所有的要素都相同。 

**重载与作用域**

如果在内存作用域中声明名字，它将隐藏外层作用域中声明的同名实体。

### 6.5 特殊用途语言特性

（1）默认实参

函数调用时，实参按其位置解析，默认实参负责填补函数调用缺少的尾部实参。

```c++
typedef string::size_type sz;
string screen(sz ht = 24, sz wid = 80, char background = ' ');
```

::: tip

当设计含有默认实参的函数时，需要合理设置形参的顺序。一旦某个形参被赋予了默认值，它后面的所有形参都必须有默认值。

:::

（2）内联函数

使用关键词inline来声明内联函数。

内联用于优化规模较小，流程直接，频繁调用的函数。

（3）constexpr函数

constexpr函数是指能用于常量表达式的函数。

### 6.6 函数匹配

Step1:确定候选函数和可选函数。

Step2:寻找最佳匹配。

### 6.7 函数指针

函数指针指向的是函数而非对象。

```c++
void useBigger (const string &s1, const string &s2, bool pf(const string &, const string &));
等价于
void useBigger (const string &s1, const string &s2, bool (*pf)(const string &, const string &));
```