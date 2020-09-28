## 第十四章 重载运算与类型转换

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P490-P523

通过运算符重载可重新定义该运算符的含义。

### 14.1 基本概念

**定义**：重载运算符是具有特殊名字的函数。名字由operator和符号组成。重载运算符包含返回类型、参数列表和函数体。

::: tip

当一个重载的运算符是成员函数时，this绑定到左侧运算对象。成员运算符函数的显式参数数量比运算对象的数量少一个。

对于一个运算符来说，它或者是类的成员，或者至少含有一个类类型的参数。

我们只能重载已有的运算符。

:::

**直接调用一个重载的运算符函数**

```cpp
data1 + data2;
operator+(data1, data2);
// 以上2个调用等价
```

### 14.2 输入和输出运算符   

（1）重载输出运算符<<

```cpp
ostream &operator<<(ostream &os, const Sales_data &item)
{
	os << item.isbn() << " " << item.unites_sold << " " << item.revenue << " " << item.avg_price();
	return os;
}
```

（2）重载输入运算符>>

```cpp
istream &operator>>(istream &is, Sales_data &item)
{
	double price;
	is >> item.bookNo >> item.units_sold >> price;
	if (is)
		item.revenue = items.units_sold * price;
	else
		item = Sales_data();
	return is;
}
```

### 14.3 算术和关系运算符

（1）相等运算符

```cpp
bool operator==(const Sales_data &lhs, const Sales_data &rhs)
{
	return lhs.isbn() == rhs.isbn() &&
			lhs.unites_sold == rhs.units_sold &&
			lhs.revenue == rhs.revenue;
}
```

（2）关系运算符

operator<

### 14.4 赋值运算符

operator=

operator+=

### 14.5 下标运算符

operator[]

下标运算符必须是成员函数。

```cpp
class StrVec{
public:
	std::string& operator[](std::size_t n){
		return elements[n];
	}
	const std::string& operator[](std::size_t n) const{
		return elements[n];
	}
private:
	std::string *elements;
}
```

### 14.6 递减和递增运算符

递增运算符（++）

递减运算符（--）

**定义前置递增/递减运算符**:

```
class StrBlobPtr{
public:
	StrBlobPtr& operator++();  // 前置运算符
	StrBlobPtr& operator--();
}
```

**区分前置和后置运算符**:

```cpp
class StrBlobPtr{
public:
	StrBlobPtr operator++(int); // 后置运算符
	StrBlobPtr operator--(int);
}
```

### 14.7 成员访问运算符

operator*

operator->

### 14.8 函数调用运算符

如果类重载了函数调用运算符，则我们可以像使用函数一样使用该类的对象。

```cpp
struct absInt{
	int operator()(int val) const {
		return val < 0 ? -val : val;
	}
};

absInt absObj;
int ui = absObj(i);
```

如果定义了调用运算符，则该类的对象称为函数对象。

### 14.9 重载、类型转换与运算符

（1）类型转换运算符

类型转换运算符是类的一种特殊成员函数，将一个类类型的值转换成其他类型。形式：

operator type() const;

（2）避免有二义性的类型转换

（3）函数匹配与重载运算符

::: warning

如果对同一个类既提供了转换目标是算术类型的类型转换，也提供了重载的运算符，将会遇到重载运算符与内置运算符的二义性问题。

:::

### 术语

**类类型转换**:由构造函数定义的从其他类型到类类型的转换以及由类型转换运算符定义的从类类型到其他类型的转换。