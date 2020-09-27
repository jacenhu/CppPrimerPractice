## 第十二章 动态内存

**声明:**
本文为《C++ Primer 中文版（第五版）》学习笔记。
原书更为详细，本文仅作学习交流使用。

P400-P436

### 12.1 动态指针与智能指针

| 智能指针   | 用途                                                         |
| ---------- | ------------------------------------------------------------ |
| shared_ptr | 提供所有权共享的智能指针：对共享对象来说，当最后一个指向它的shared_ptr被销毁时会被释放。 |
| unique_ptr | 提供独享所有权的智能指针：当unique_ptr被销毁的时，它指向的独享被释放。unique_ptr不能直接拷贝或赋值。 |
| weak_ptr   | 一种智能指针，指向由shared_ptr管理的对象。在确定是否应释放对象视，shared_ptr并不把weak_ptr统计在内。 |

（1）shared_ptr类

```cpp
shared_ptr<string> p1;
```

**make_shared函数**:

make_shared在动态内存中分配一个对象并初始化它，返回此对象的shared_ptr。

```cpp
share_ptr<int> p3 = make_shared<int>(42);
```

**shared_ptr的拷贝和赋值**:

每个shared_ptr都有一个关联的计数器，称为引用计数。一旦一个shared_ptr的引用计数变为0，就会自动释放自己所管理的对象。

（2）直接管理内存

运算符new分配分配内存，delete释放new分配的内存。

**使用new动态分配和初始化对象**:

```cpp
// 默认情况下，动态分配的对象是默认初始化的
int *pi = new int; // pi指向一个动态分配的、未初始化的无名对象
```

```cpp
// 直接初始化方式
int *pi = new int(1024); // pi指向的对象的值为1024
```

```cpp
// 对动态分配的对象进行值初始化，只需在类型名之后加上一对空括号
int *pi1 = new int;   // 默认值初始化；*pi1的值未定义
int *pi2 = new int(); // 值初始化为0；*pi2为0
```

**动态分配的const对象**:

```cpp
const int *pci = new const int(1024);
```

**释放动态内存**:

```cpp
delete p;
```

delete表达式执行两个动作：销毁给定的指针指向的对象；释放对应的内存。

（3）unique_ptr

某个时刻，只能有一个unique_ptr指向一个给定对象。

当unique_ptr销毁时，它所指向的对象也被销毁。

| unique_ptr操作        |
| --------------------- |
| unique_ptr<T> u1      |
| unique_ptr<T, D> u2   |
| unique_ptr<T, D> u(d) |
| u = nullptr           |
| u.release()           |
| u.reset()             |
| u.reset(p)            |
| u.reset(nullptr)      |

（4）weak_ptr

weak+ptr是一种不受控制所指向对象生存期的智能指针，它指向由一个shared_ptr管理的对象，而且不会改变shared_ptr的引用计数。

| weak_ptr 操作     |                               |
| ----------------- | ----------------------------- |
| weak_ptr<T> w     |                               |
| weak_ptr<T> w(sp) |                               |
| w = p             |                               |
| w.reset()         | 将w置空                       |
| w.use_count()     | 与w共享对象的shared_ptr的数量 |
| w.expired()       |                               |
| w.lock()          |                               |

使用weak_ptr之前，需要调用lock，检查weak_ptr指向的对象是否存在。

### 12.2 动态数组

（1）new和数组

在类型名之后跟一对方括号，在其中指明要分配的对象的数目。

释放动态数组：

```cpp
delete p;      // p必须指向一个动态分配的对象或为空
delete [] pa;  // pa必须指向一个动态分配的数组或为空
```

智能指针和动态数组

```
unique_ptr<T []> u;
unique_ptr<T []> u(p);
u[i];
```

（2）allocator类

标准库allocator类定义在头文件memory中，帮助将内存和对象构造分离开来。

```cpp
allocator<string> alloc;
auto const p = alloc.allocate(n);
```

| 表达式               | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| allocator[T] a       | 定义了一个名为a的allocator对象，它可以为类型为T的对象分配内存 |
| a.allocate(n)        | 分配一段原始的、未构造的内存，保存n个类型为T的对象           |
| a.construct(p, args) | 为了使用allocate返回的内存，我们必须使用construct构造对象。使用未构造的内存，其行为是未定义的。 |
| a.destroy(p)         | p为T*类型的指针，此算法对p指向的对象执行析构函数             |

### 术语

*new* : 从自由空间分配内存。new T 分配并构造一个类型为T的指针。如果T是一个数组类型，new 返回一个指向数组首元素的指针。类似的，new  [n]  T 分配 n 个类型为T的对象，并返回指向数组首元素的指针。

*空悬指针*：一个指针，指向曾经保存一个对象但现在已释放的内存。

*智能指针*：标准库类型。负责在恰当的时候释放内存。