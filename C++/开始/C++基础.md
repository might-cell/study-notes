# 书架问题

### 主要功能

- 定义变量
- 输入输出
- 使用数据结构保存数据
- 检测两条数据是否拥有相同的ISBN号
- 包含循环来处理销售档案中的每条记录

### 销售记录数据模型

| ISBN号        | 售出册数 | 书记单价 |
| ------------- | -------- | -------- |
| 0-201-70353-x | 4        | 24.99    |



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