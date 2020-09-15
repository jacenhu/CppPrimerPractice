## 第七章 类

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P228-P273

类的基本思想是数据抽象和封装。

抽象是一种依赖于接口和实现分离的编程技术。

封装实现了类的接口和实现的分离。

### 7.1 定义抽象数据类型

（1）this

任何对类成员的直接访问都被看作this的隐式引用。

```c++
std::string isbn() const {return bookNo;}
```

等价于

```c++
std::string isbn() const {return this->bookNo;}
```

（2）在类的外部定义成员函数

类外部定义的成员的名字必须包含它所属的类名。

```c++
double Sales_data::avg_price() const {
	if (units_sol)
		return revenue/units_sols;
	else
		return 0;
}
```

（3）构造函数

> 定义：类通过一个或几个特殊的成员函数来控制其对象的初始化过程，这些函数叫做构造函数。
>
> 构造函数没有返回类型；
>
> 构造函数的名字和类名相同。

类通过一个特殊的构造函数来控制默认初始化过程，这个函数叫做**默认构造函数**。

编译器创建的构造函数被称为**合成的默认构造函数**。

::: tip

只有当类没有声明任何构造函数的时，编译器才会自动的生成默认构造函数。

一旦我们定义了一些其他的构造函数，除非我们再定义一个默认的构造函数，否则类将没有默认构造函数

:::

### 7.2 访问控制与封装

（1）访问控制

| 说明符  | 用途                                                         |
| ------- | ------------------------------------------------------------ |
| public  | 使用public定义的成员，在整个程序内可被访问，public成员定义类的接口。 |
| private | 使用private定义的成员可以被类的成员函数访问，但是不能被使用该类的代码访问，private部分封装了类的实现细节。 |

（2）友元

类可以允许其他类或者函数访问它的非公有成员，方法是令其他类或者函数成为它的友元。

以friend关键字标识。

友元不是类的成员，不受访问控制级别的约束。

::: tip

友元的声明仅仅制定了访问的权限，而非通常意义的函数声明。必须在友元之外再专门对函数进行一次声明。

:::

```c++
// Sales_data.h

class Sales_data {
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::ostream &print(std::ostream&, const Sales_data&);
friend std::istream &read(std::istream&, Sales_data&);
}

// nonmember Sales_data interface functions
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);

//Sales_data.cpp

Sales_data 
add(const Sales_data &lhs, const Sales_data &rhs)
{
	Sales_data sum = lhs;  // copy data members from lhs into sum
	sum.combine(rhs);      // add data members from rhs into sum
	return sum;
}

// transactions contain ISBN, number of copies sold, and sales price
istream&
read(istream &is, Sales_data &item)
{
	double price = 0;
	is >> item.bookNo >> item.units_sold >> price;
	item.revenue = price * item.units_sold;
	return is;
}

ostream&
print(ostream &os, const Sales_data &item)
{
	os << item.isbn() << " " << item.units_sold << " " 
	   << item.revenue << " " << item.avg_price();
	return os;
}

```

### 7.3 类的其他特性

（1）重载成员变量

```c++
Screen myScrren;
char ch = myScreen.get();
ch = myScreen.get(0,0);
```

（2）类数据成员的初始化

类内初始值必须使用=或者{}的初始化形式。

```c++
class Window_mgr{
private:
    std::vector<Screen> screens{Screen(24, 80, ' ')};
}
```

（3）基于const的重载

```c++
class Screen {
public:
	// display overloaded on whether the object is const or not
    Screen &display(std::ostream &os) 
                  { do_display(os); return *this; }
    const Screen &display(std::ostream &os) const
                  { do_display(os); return *this; }
}
```

当某个对象调用display的时候，该对象是否是const决定了应该调用display的哪个版本。

（3）类类型

对于一个类来说，在我们创建他的对象之前该类必须被定义过，而不能仅被声明。

（4）友元

*友元类*

如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

```c++
class Screen {
	// Window_mgr的成员可以访问Screen类的私有部分
	friend class Window_mgr;
}
```

*令成员函数作为友元*

```
class Screen {
	// Window_mgr::clear必须在Screen类之前被声明
	friend void Window_mgr::clear(ScreenIndex);
}
```

### 7.3 类的其他特性

（1）重载成员变量

```c++
Screen myScrren;
char ch = myScreen.get();
ch = myScreen.get(0,0);
```

（2）类数据成员的初始化

类内初始值必须使用=或者{}的初始化形式。

```c++
class Window_mgr{
private:
    std::vector<Screen> screens{Screen(24, 80, ' ')};
}
```

（3）基于const的重载

```c++
class Screen {
public:
	// display overloaded on whether the object is const or not
    Screen &display(std::ostream &os) 
                  { do_display(os); return *this; }
    const Screen &display(std::ostream &os) const
                  { do_display(os); return *this; }
}
```

当某个对象调用display的时候，该对象是否是const决定了应该调用display的哪个版本。

（3）类类型

对于一个类来说，在我们创建他的对象之前该类必须被定义过，而不能仅被声明。

（4）友元

*友元类*

如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

```c++
class Screen {
	// Window_mgr的成员可以访问Screen类的私有部分
	friend class Window_mgr;
}
```

*令成员函数作为友元*

```
class Screen {
	// Window_mgr::clear必须在Screen类之前被声明
	friend void Window_mgr::clear(ScreenIndex);
}
```

### 7.4 类的作用域

一个类就是一个作用域。

### 7.5 构造函数再探

（1）构造函数的初始值有时必不可少

::: tip

如果成员是const、引用，或者属于某种未提供默认构造函数的类类型化。我们必须通过构造函数初始值列表为这些成员提供初值。

:::

```cpp
class ConstRef{
public:
	ConstRef (int i);
private:
	int i;
	const int ci;
	int &ri;
};

ConstRef:ConstRef(int ii) : i(ii), ci(ii), ri(i){  }
```

（2）成员初始化的顺序

成员初始化的顺序与它们在类定义中出现 的顺序一致。P259

（3）委托构造函数

使用它所述类的其他构造函数执行它自己的初始化过程。

（4）如果去抑制构造函数定义的隐式转换？

在类内声明构造函数的时候使用explicit关键字。

### 7.6 类的静态成员

（1）声明静态成员

在成员的声明之前加上关键词static。

类的静态成员存在于任何对象之外，对象中不包含任何与静态成员有关的数据。

（2）使用类的静态成员

```cpp
double r;
r = Account::rate();
```

### 小结

类有两项基本能力：

一是数据数据抽象，即定义数据成员和函数成员的能力；

二是封装，即保护类的成员不被随意访问的能力。