typedef的作用：

* 简化结构体的编写 
* 区分数据类型 typedef char* PCHAR
* 提高程序的可移植性 typedef long long MY_INT



void的作用：

* 无类型，不宜用来创建变量
* 限定函数的返回值和参数
* 万能指针：
  * 可以任意使用其他类型的指针，不要强转
  * sizeof 4



sizeof的作用：

- 本质：不是函数，运算符
- 返回值类型：无符号整型
- 用途：统计数组占用内存空间大小
  - 当数组名传入到函数中，数组名退化为一个指针，指针指向的数组中第一个元素的地址