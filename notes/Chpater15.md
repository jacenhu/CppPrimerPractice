## 第十五章 面向对象程序设计

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P526-P575

### 15.1 OOP:概述

（1）**面对对象程序设计（object-oriented programming）的核心思想**：

数据抽象、继承和动态绑定。

（2）**继承**：

> 继承是一种类联系在一起的一种层次关系。这种关系中，根部是*基类*，从基类继承而来的类成为*派生类*。

基类负责定义在层次关系中所有类共同拥有的成员，而每个派生类定义各自特有的成员。

虚函数：virtual function。基类希望派生类各自定义自身版本的函数。

```cpp
class Quote {
public:
		std::string isbn() const;
		virtual double net_price(std::size_t n) const;
}
```

（3）**动态绑定**：

::: tip

在C++语言中，当我们使用基类的引用（或者指针）调用一个虚函数时将发生动态绑定（也称*运行时绑定*）。P527

:::

### 15.2 定义基类和派生类

（1）定义基类

虚函数：基类希望派生类进行覆盖的函数。

>基类将该函数定义为虚函数（virtual）。

> 基类通过在其成员函数的声明语句之前加上关键词virtual使得该函数执行动态绑定。

> 关键词virtual只能出现在类内部的声明语句之前而不能用于类外部的函数定义。

> 如果基类把一个函数声明成虚函数，则该函数在派生类中隐式的也是虚函数。

（2）定义派生类

派生类必须通过派生类列表明确指出它是从哪个基类继承而来的。

```cpp
class Bulk_quote : public Quote {
... // 省略
}
```

对于派生类中的虚函数的处理：

若派生类未覆盖基类中的虚函数，则该虚函数的行为类似其他普通成员。

C++允许派生类显式注明覆盖了基类的虚函数，可通过添加override关键字。

**派生类对象**:

一个派生类对象包含多个部分：自己定义的成员的子对象，以及基类的子对象。

**派生到基类的类型转换**：

由于派生类对象中含有与其基类对象的组成部分，因此可以进行隐式的执行派生类到基类的转换。

```cpp
Quote item;        // 基类
Bulk_quote bulk;   // 派生类
Quote *p = &item;  // p指向Quote对象
p = &bulk;         // p指向bulk的Quote部分
Quote &r = bulk;   // r绑定到bulk的Quote部分。
```

**派生类构造函数**：

每个类控制自己的成员的初始化过程。派生类首先初始化基类的部分，然后按照声明的顺序依次初始化派生类的成员。

**派生类使用基类的成员**：

派生类可以访问基类的公有成员和受保护成员。

::: tip

派生类对象不能直接初始化基类的成员。派生类应该遵循基类的借口，通过调用基类的构造函数来初始化从基类继承来的成员。

:::

**被用作基类的类**:

若使用某个类作为基类，则该类必须已被定义而非仅仅声明。

派生类包含它的直接基类的子对象以及每个间接基类的子对象。

**防止继承发生**:

在类名后面跟着一个关键字final。

```cpp
class NoDerived final {};   // NoDerived不能作为基类
```

（3）类型转换与继承

我们可以将基类的指针或引用绑定到派生类对象上。

**静态类型与动态类型**:

静态类型：在编译时已知，是变量声明时的类型或表达式生成的类型。

动态类型：运行时才可知，是变量或表达式表示的内存中的对象的类型。

如果表达式既不是引用也不是指针，则动态类型与静态类型永远一致。

**不存在基类向派生类隐式类型转换**:

```cpp
Quote base;
Bulk_quote *bulkP = &base;    // 错误!
Bulk_quote *bulkRef = base;   // 错误!
```

::: warning

当我么用一个派生类对象为一个基类对象初始化或赋值时，只有该派生类对象中的基类部分会被拷贝、移动或赋值，它的派生类部分会被忽略掉。

:::

### 15.3 虚函数

**C++的多态性**：使用这些类型的多种形式，而无须在意它们的差异。

**派生类中的虚函数**:

一个派生类如果覆盖了某个继承而来的虚函数，则它的形参类型必须与被它覆盖的基类函数完全一致。

**final和override说明符**:

如果用override标记了某个函数，但是该函数并没有覆盖已存在的虚函数，此时编译器将报错。

如果用final标记了某个函数， 则之后任何尝试覆盖该函数的操作都将错误。

**虚函数与默认实参**：

如果虚函数某次被调用使用了默认实参，则该实参值由本次调用的静态类型决定。

### 15.4 抽象基类

**纯虚函数**:

书写=0可以将一个虚函数说明为纯虚函数（pure virtual），纯虚函数无须定义。

不能在类的内部为一个=0的函数提供函数体。

```cpp
class Disc_quote : public Quote {
public:
		double net_price(std::size_t) const = 0;
}
```

**抽象基类**：

含有纯虚函数的类是抽象基类。

不能创建抽象基类的对象。

### 15.5 访问控制与继承

**受保护的成员**:

派生类的成员和友元只能访问派生类对象中的基类部分的受保护成员；对于普通的基类对象中的成员不具有特殊的访问权限。P543

**公有、私有和受保护继承**：

派生访问说明符对于派生类的成员（及友元）能否访问其直接基类的成员无影响；

对基类成员的访问权限只与基类中的访问说明符有关。

派生访问说明符的目的是控制派生类用户对于基类成员的访问权限。

**改变个别成员的可访问性**:

通过在类的内部使用using声明语句，我们可以将该类的直接或间接基类中的任何可访问成员标记出来。

```cpp
class Derived : private Base {
public:
		using Base::size;
}
```

::: tip

派生类只能为它可访问的名字提供using声明。

:::

**默认的继承保护级别**:

使用class关键字定义的派生类是私有继承的；

使用struct关键字定义的派生类是共有继承的。

```cpp
class Base {};
struct D1 : Base {};  // 默认public继承
class D2 : Base {};   // 默认private继承
```

### 15.6 继承中的类作用域

**在编译时进行名字查找**:

一个对象、引用或指针的静态类型决定了该对象的哪些成员是可见的。

**名字冲突与继承**:

派生类的成员将隐藏同名的基类成员。

::: tip

出了覆盖继承而来的虚函数外，派生类最好不雅重用其他定义在基类中的名字。

:::

如果派生类的成员函数与基类的某个成员函数同名，则派生类将在其作用域内隐藏掉该基类成员函数。

::: tip

非虚函数不会发生动态绑定。

:::

### 15.7 构造函数与拷贝控制

（1）虚析构函数

在基类中将析构函数定义成虚函数以确保执行正确的析构函数版本。

```cpp
Quote *itemP = new Quote;
delete itemP;           // 调用Quote的析构函数
itemP = new Bulk_quote;
delete itemP;           // 调用Bulk_quote的析构函数
```

虚析构函数会阻止合成移动操作。

（2）合成拷贝控制与继承

基类缺少移动操作会阻止派生类拥有自己的合成移动操作，所以当确实要执行移动操作的时候就要首先在基类中进行显式定义。P554

（3）派生类的拷贝控制成员

**派生类的拷贝或移动构造函数**:

::: tip

默认情况下，基类默认构造函数初始化派生类对象的基类部分。如果我们想拷贝（或移动）基类部分，则必须在派生类的构造函数初始值列表中显式的使用基类的拷贝（或移动）构造函数。

:::

**派生类的赋值运算符**:

派生类的赋值运算符必须显式的为其基类部分赋值。

**派生类的析构函数**:

派生类函数只负责销毁由派生类自己分配的资源。

### 15.8 容器与继承

当使用容器存放继承体系中的对象时，必须采用间接存储的方式。因为不允许在容器中保存不同类型的元素。

### 术语

覆盖：override，派生类中定义的虚函数如果与基类中定义的同名虚函数与相同的形参列表，则派生类版本将覆盖基类的版本。

多态：程序能够通引用或指针的动态类型获取类型特定行为的能力。