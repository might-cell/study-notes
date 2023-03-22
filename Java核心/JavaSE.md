[TOC]

### 面向对象

概念：用代码去高度模拟现实世界，以便为人类服务。Java是面向对象的高级编程语言



面向对象最重要的两个概念：类和对象

- 类：描述相同事物的共同特征的抽象
- 对象：具体的实例



在代码层面，必须先有类，才能创建出对象



定义类的格式：

```java
修饰符 类名(){
    
}
```





:::caution

- 类名首字母必须大写，满足“驼峰写法”
- 一个Java代码文件中可以定义多个类，但是只能有一个类使用public修饰，而且public修饰的类名必须和文件名相符

:::



#### 类中的成分研究

类中有且仅有五大成分：

- 成员变量（Field，描述类和对象的属性信息）
- 成员方法（Method，描述类或对象的行为信息）
- 构造器（Constructor，初始化一个类的对象并返回引用）
- 代码块
- 内部类



:::caution

主要不是上述成分放在类下就会报错

:::



##### 构造器

作用：初始化一个类的对象并返回引用



格式：

```java
修饰符 类名(形参列表){
    
}
```

构造器初始化对象的格式：

```java
类名 对象名称 = new 构造器;
```



:::caution

一个类默认会携带一个无参构造器，即使不写也存在，如果一个类有了一个构造器，那么默认的无参构造器会被覆盖

:::



##### this关键字

作用：this代表当前对象的引用，可以用在实例方法和构造器中



特点：

- this用在方法中，谁调用方法，this就代表谁
- this用在构造器中，代表了构造器正在初始化的那个对象的引用



##### 封装

特征：Java语言的风格，在代码开发中必须遵循的规范，即使毫无意义，代码也必须按照此风格来编写



作用：

- 可以提高所谓的安全性
- 实现代码的组件化<img src="file:///C:\WINDOWS\TEMP\SGPicFaceTpBq\43076\2537D81C.png" alt="img" style="zoom:25%;" />



封装的规范：

- 建议成员变量私有，用private修饰
  - private修饰的方法或者成员变量，只能在本类内直接访问
- 提供成套的getter和setter方法，暴露成员变量的取值和赋值
  - public修饰的方法是公开的意义

:::caution

- 封装的核心思想为：合理隐藏，合理暴露
- 封装已经成为Java代码的风格，即使毫无意义，还是要按照封装的规范编写代码
  - 一来就是成员变量私有化，提供暴露取值和赋值的接口

:::



##### static（核心）

拿一个常见的Student类来举例，我们可以在其中定义很多成员变量（name，age，sex），虽然我们只写了一份，但是每个对象都可以使用，因此可以说明Java类中这些成员变量或者方法是存在所有属性的，有些属于某个对象，有些属于类本身



Java通过成员变量是否存在static修饰来区分是属于类本身的，还是属于对象的：

- static表示静态，修饰的成员（方法和成员变量）属于类本身



按照有无static修饰，成员变量和方法可以划分为：

- 成员变量：

  1、静态成员变量（类变量）：有static修饰的成员变量称为静态成员变量，也叫作类变量，属于类本身，直接使用类名访问

  2、实例成员变量：无static修饰的成员变量称为实例成员变量，属于类的每个对象的，必须使用类的对象来访问

- 成员方法：

  1、静态方法：有static修饰的成员方法称为静态成员方法，也叫类方法，属于类本身，直接使用类名访问

  2、实例方法：无static修饰的成员方法称为实例方法，必须使用类的对象来访问



###### 成员变量的访问

```java
package com.mightcell;

public class Student {
    // 定义静态成员变量
    public static String schoolName = "Might-Cell";

    // 定义实例成员变量
    private String name;
    private int age;

    public static void main(String[] args) {
        // 静态成员变量
        System.out.println(Student.schoolName);
        // 同一个类中访问静态成员变量可以省略类名
        System.out.println(schoolName);

        // 实例成员变量
        Student student = new Student();
        student.name = "might-cell";
        System.out.println(student.name);

        // 对象访问静态成员变量(不推荐)
        System.out.println(student.schoolName);
    }
}
```

![image-20230208172840090](JavaSE%E5%A4%8D%E4%B9%A0.assets/image-20230208172840090.png)



###### 成员方法的访问

```java
package com.mightcell;

public class Student {
    // 实例成员变量
    private String food;
    // 定义静态方法
    public static void getAddress() {
        System.out.println("China");
    }
    // 定义实例方法
    public void getFood() {
        System.out.println("I want " + food);
    }
    
    public static void main(String[] args) {
        // 静态方法
        Student.getAddress();
        // 在用一个类中访问静态成员方法，可以省略类名
        getAddress();
        // 实例方法
        Student student = new Student();
        student.food = "Apple";
        student.getFood();
    }
}

```

![image-20230208174223266](JavaSE%E5%A4%8D%E4%B9%A0.assets/image-20230208174223266.png)



##### 继承

面向对象的三大特征：封装，继承，多态



继承是Java中一般到特殊的关系，是一种子类到父类的关系（i.e. 学生类继承了人类，猫类继承了动物类）



被继承的类称为：父类或者超类

继承父类的类称为：子类



继承的作用：

1、可以提高代码的复用，相同的代码可以定义在类中，然后子类继承父类，就可以直接使用这些父类的代码

2、子类不仅用于父类的功能，而且可以添加自己独有的功能



继承的特点：

子类继承一个父类，子类可以直接得到父类的属性和行为

```java
class Animal {
    
}

class Cat extends Animal {
    
}
```



###### 无法继承的内容

子类继承父类，子类便能得到父类的属性和行为，但是并非所有的属性和行为，子类都可以继承



有争议的内容：

- 子类无法继承父类的构造器，子类拥有自身的构造器
- 子类可以继承父类的私有成员，但是不能直接访问
- 子类不能继承父类的静态成员，子类只是可以访问父类的静态成员，因为父类只有一份可以被子类共享访问的静态成员



