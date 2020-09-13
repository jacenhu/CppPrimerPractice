## 第五章 语句

P154-P178

### 5.1 简单语句

（1）空语句

```c++
;    // 空语句
```

（2）复合语句

复合语句是指用花括号括起来的（可能为空的）语句和声明的序列，复合语句也被称作块（block）。

```c++
{}
```

### 5.2 语句作用域

定义在控制结构当中的变量只在相应语句的内部可见，一旦语句结束，变量就超出其作用范围。

### 5.3 条件语句

（1）if 语句

（2）switch 语句

case关键字和它对应的值一起被称为case标签。

case标签必须是整形常量表达式。

如果某个case标签匹配成功，将从该标签开始往后顺序执行所有case分支，除非程序显示的中断了这一过程。

dedault 标签：如果没有任何一个case标签能匹配上switch表达式的值，程序将执行紧跟在default标签后面的语句。

### 5.4 迭代语句

（1）while 语句

```
while (condition)
			statement
```

（2）传统 for 语句

```c++
for (initializar; condition; expression)
		statement
```

for 语句中定义的对象只在for循环体内可见。

（3）范围 for 语句

```c++
for (declaration : expression)
		statement
```

（4）do while 语句

```c++
do 
	statement
while (condition)
```

### 5.5 跳转语句

**break**

break只能出现在迭代语句或者switch语句内部。仅限于终止离它最近的语句，然后从这些语句之后的第一条语句开始执行。

**continue**

continue语句终止最近的循环中的当前迭代并立即开始下一次迭代。

**goto**

goto的作用是从goto语句无条件跳转到同一函数内的另一条语句。

容易造成控制流混乱，应禁止使用。

**return**

### 5.6 try语句块和异常处理

C++中异常处理包括：throw表达式、try语句块。

try和catch，将一段可能抛出异常的语句序列括在花括号里构成try语句块。catch子句负责处理代码抛出的异常。

throw表达式语句，终止函数的执行。抛出一个异常，并把控制权转移到能处理该异常的最近的catch字句。