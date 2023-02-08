# 书架问题

### 主要功能

- 定义变量
- 输入输出
- 使用数据结构保存数据
- 检测两条数据是否拥有相同的ISBN号
- 包含循环来处理销售档案中的每条记录



### 销售记录数据模型

| ISBN号        | 售出册数 | 单价  |
| ------------- | -------- | ----- |
| 0-201-70353-x | 4        | 24.99 |



### Sales_item类

1、isbn函数：提取ISBN号

2、使用输入输出运算符读写Sales_item对象

3、使用赋值运算符将一个Sales_item的内容化赋值到另一个Sales_item

4、使用加法运算符将两个Sales_item相加，这两个对象所包含的ISBN号需要相同，其计算结果ISBN号不变，售出册数和单价应该为两者之和



```c++
#pragma once
#ifndef SALESITEM_H
#define SALESITEM_H
#include <iostream>
#include <string>

class Sales_item
{
public:
	Sales_item(const std::string& book) :isbn(book), units_sold(0), revenue(0.0) {}
	Sales_item(std::istream& is) { is >> *this; }
	friend std::istream& operator>>(std::istream&, Sales_item&);
	friend std::ostream& operator<<(std::ostream&, const Sales_item&);
public:
	Sales_item& operator+=(const Sales_item&);
public:
	double avg_price() const;
	bool same_isbn(const Sales_item& rhs)const
	{
		return isbn == rhs.isbn;
	}
	Sales_item() :units_sold(0), revenue(0.0) {}
public:
	std::string isbn;
	unsigned units_sold;
	double revenue;
};

using std::istream;
using std::ostream;
Sales_item operator+(const Sales_item&, const Sales_item&);
inline bool operator==(const Sales_item& lhs, const Sales_item& rhs)
{
	return lhs.units_sold == rhs.units_sold && lhs.revenue == rhs.revenue && lhs.same_isbn(rhs);
}
inline bool operator!=(const Sales_item& lhs, const Sales_item& rhs)
{
	return !(lhs == rhs);
}

inline Sales_item& Sales_item::operator +=(const Sales_item& rhs)
{
	units_sold += rhs.units_sold;
	revenue += rhs.revenue;
	return *this;
}
inline Sales_item operator+(const Sales_item& lhs, const Sales_item& rhs)
{
	Sales_item ret(lhs);
	ret += rhs;
	return ret;
}
inline istream& operator>>(istream& in, Sales_item& s)
{
	double price;
	in >> s.isbn >> s.units_sold >> price;
	if (in)
		s.revenue = s.units_sold * price;
	else
		s = Sales_item();
	return in;
}
inline ostream& operator<<(ostream& out, const Sales_item& s)
{
	out << s.isbn << "\t" << s.units_sold << "\t" << s.revenue << "\t" << s.avg_price();
	return out;
}
inline double Sales_item::avg_price() const
{
	if (units_sold)
		return revenue / units_sold;
	else
		return 0;
}
#endif
```



# C++基础

每个C++程序包含一个或多个函数，其中一个必须命名为main



函数定义包含四部分：

1、返回类型

2、函数名

3、形参列表

4、函数体



简单的函数：

```c++
int main() 
{
    return 0;
}
```



>在C++中，大多数C++语句以分号标识结束，如果缺失了分号，将会导致莫名其妙的编译错误



### 输入输出

C++没有定义单独的输入输出语句，而是使用全面的标准库来提供IO机制



iostream库包含两个基础类型：istream和ostream



4个IO对象：

标志输入：cin

标准输出：cout

标准错误：cerr

一般性信息：clog



计算两数之和：

```c++
#include <iostream>
int main()
{
    std::cout << "Enter two numbers:" << endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The sum of " << v1 << " and " << v2 << " is " << v1 + v2 << std::endl;
    return 0;
}
```



endl的效果：

- 结束当前行
- 将设备关联的缓冲区刷到当前设备，保证刷新操作可以保证目前程序产生的所有数据真正写入输出流，而不是在内存中等待写入流



std前缀：

- std为命名空间，可以避免不经意的名称定义冲突，以及使用库中相同名字的冲突，std为标准库的命名空间
- 副作用：使用标准库中的名字，必须显式说明，例如想要使用标准输入cin，需要使用作用域运算符::显式指出其所在的命名空间，std::cin



练习1.3：

```c++
#include <iostream>

int main()
{
    std::cout << "Hello, World";
}
```



练习1.4

```c++
#include <iostream>
int main()
{
    std::cout << "Enter two numbers:" << endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The product of " << v1 << " and " << v2 << " is " << v1 * v2 << std::endl;
    return 0;
}
```



练习1.5：

```c++
#include <iostream>
int main()
{
    std::cout << "Enter two numbers:" << endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The product of ";
    std::cout << v1;
    std::cout << " and ";
    std::cout << v2;
    std::cout << " is ";
    std::cout << v1 * v2;
    std::cout << std::endl;
    return 0;
}
```



练习1.6：

不合法，在第一句和第二句打印语句末尾添加了分号，直接结束了运行，应该将分号删除



### 注释

注释界定符不能嵌套

当我们需要在调试期间注释一些代码，做好的办法是用单行注释来注释代码的每一行



### 控制流

#### while

while执行的过程是交替地检测条件和执行相关的语句，直至条件为假



#### for

for循环包含两个部分：循环头和循环体

- 循环头控制循环的执行次数
  - 初始化语句
  - 循环条件
  - 表达式



练习1.14：

for书写简练，对内存较节省，局部变量循环结束后自动化清除

while可以对一些不确定循环次数的循环进行较好的控制



不断读取数据直至没有新的输入为止：

```c++
#include <iostream>
int main() 
{
    int sum = 0, val = 0;
    // 读取数据直至文件尾，计算所有读入的值的和
    while (std::cin >> val) 
        sum += val;
    std::cout << "The sum is " << sum << std::endl;
    return 0;
}
```



while的检测条件是执行表达式：std::cin >> val，此表达式不断从标准输入读取下一个数，保存在val中，输入运算符>>返回给左侧运算对象，因此检测条件实际上是std::cin

当我们使用istream作为检测条件，其效果为检测流的状态：

- 如果流是有效的或者流没有遇到错误，那么检测成功
- 遇到文件结束符EOF或者遇到一个无效输入（读入的类型不匹配），istream的状态便会变为无效



>如何指出文件结束：
>
>windows：Ctrl+Z，然后输入Enter
>
>UNIX：Ctrl+D



### 类简介

C++一个设计焦点就是能够定义使用像内置类型一样自然的类类型



我们需要使用头文件来访问为自己的应用程序定义的类：

- 头文件根据为其中定义的类名来定义，使用.h作为头文件的后缀



对于不属于标准库的头文件，则使用双引号包围



>文件重定向：
>
>当我们测试程序时，反复从键盘敲入记录作为程序的输入效率低下，大部分操作系统支持文件重定向，这种机制允许我们将标准输入和标准输入和命名文件关联起来
>
>1、编译addItems.exe文件
>
>2、addTtems <infile>outfile
>
>从名为infile的文件读取记录，并将结果写入outfile文件，这两文件都位于当前目录
>
>



#### 成员函数

成员函数是类定义的部分函数，有时被称为方法



使用点运算符号.访问一个成员函数，同时我们需要使用调用运算符()

调用运算符是一对圆括号，可以放置实参列表，也可以什么都不放置