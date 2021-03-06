# Notes on C++
## Index
[引用](#1-引用)  
[const](#2-const关键字)  
[动态内存分配](#3-动态内存分配)  
[内联函数/函数参数缺省](#4-内联函数inline/函数重载/函数缺省参数)  
[类和对象](#5-类和对象)  
[构造函数](#6-构造函数)  
[复制构造函数](#7-复制构造函数)  
[类型转换构造函数](#8-类型转换构造函数)  
[析构函数](#9-析构函数)  
[this指针](#10-this指针)  
[静态成员变量](#11-静态成员变量)  
[成员对象和封闭类](#12-成员对象和封闭类)  
[常量对象/常量成员函数](#13-常量对象/常量成员函数/常引用)  
[友元](#14-友元（friend关键字）)  
[运算符重载](#15-运算符重载)  
### 1-引用
1.1 定义引用时必须初始化，初始化后就一直引用了。

1.2 引用只能引用变量，不能引用常量和表达式。

1.3 引用可以作为函数的返回值

```
int n = 4;
int & SetValue(){return n;}
int main()
{
  SetValue()=20; //这样可以修改n的值。
  return 0;
}
```

1.4 不能通过常引用修改其引用的内容，但是可以直接给其引用赋值来修改。

1.5 常引用和非常引用的转换
    ·T类型变量/T & 类型引用可以初始化 const T &
    ·const T 类型/const T & 类型引用不可以初始化 T &，除非强制类型转换

### 2-const关键字
2.1 不可通过常量指针修改其指向的内容。

```
int n, m;
const int *p = &n;
*p = 5;//错误
p = &m;//正确，常量指针的指向可以变化
```

2.2 不能把常量指针赋值给非常量指针（除非强制类型转换，把常量指针转换为非常量指针），反之可以。
    函数参数为常量指针，可避免函数内部不小心改变参数指针所指的内容。
    
### 3-动态内存分配
3.1 p = new T;此时 p 是 T* 类型指针，内存空间的起始地址赋值给p. delete p; delete [] p; 释放。

3.2 一片空间不能delete多次。

### 4-内联函数inline/函数重载/函数缺省参数

4.1 内联函数在编译时，将函数代码插入到调用语句处。导致可执行程序体积增大。

4.2 调用时，只能最右边的连续若干个参数缺省。

4.3 缺省参数的目的在于提高程序的可扩充性。

### 5-类和对象
5.1 对象：通过类定义的变量，也称为类的实例。

5.2 对象所占用内存空间大小=成员变量大小之和。类的成员函数在内存中统一存储，被所有对象共享。

5.3 对象之间可以用 = 赋值，但不能用 == != > <等进行比较，除非进行了重载。

5.4 使用类的成员变量和成员函数
    ·对象名.成员名
    ·指针->成员名
    ·引用名.成员名

5.5 可访问范围
    ·private 只能在成员内访问
    ·public 可在任何地方访问
    ·protected 保护成员
    ·缺省的默认是私有成员
    
5.6 类的成员函数内部可以访问同类其他对象的全部属性、函数。

5.7 成员函数可以重载，参数可以缺省。

### 6-构造函数
6.1 名字与类名相同，不能有返回值，不负责为类分配空间。

6.2 一个类可以有多个构造函数，对象一旦生成不能再执行构造函数。

6.3 声明某类型的指针数组不一定会调用构造函数
```
A* arr[3] = {new A(), new A()};//只进行了两个初始化，第三个元素没有。
```

### 7-复制构造函数
7.1 复制构造函数只有一个参数，即对同类对象引用。不定义会默认生成。

7.2 起作用的情况
    ·用一个对象去初始化同类另一个对象。
    ·初始化语句（非赋值语句）。
    ·某函数的参数是类A的对象，调用该函数时，A的复制构造函数将被调用。
    ·函数的返回值是类A的对象，返回时调用A的复制构造函数
    
7.3 对象间的赋值不导致复制构造函数被调用。

7.4 使用引用作为函数参数，将不会调用复制构造函数。

### 8-类型转换构造函数
8.1 实现类型的自动转换，只有一个参数。接收参数创建临时无名对象

### 9-析构函数
9.1 名字与类名相同，前面加~，没有参数和返回值，一个类只能有一个析构函数。

9.2 参数对象消亡也会导致析构函数被调用，返回值（临时调用）被用过后，也会调用析构函数

### 10-this指针
10.1 指向成员函数所作用的对象

10.2 静态成员函数中不能使用this指针，因为静态成员函数并不具体作用于某个对象。

### 11-静态成员变量
11.1 静态成员变量一共就一份，所有对象共享，静态成员不需通过对象就能访问，类名::成员名，对象名/引用.成员名，指针->成员名

11.2 sizeof运算符不会计算静态成员变量

11.3 普通成员函数必须具体作用于某个对象，静态成员函数并不具体作用于某个对象

11.4 静态成员变量本质上是全局变量，静态成员函数本质上是全局函数

11.5 必须在定义类的文件中对静态成员变量进行一次声明或初始化

11.6 静态成员函数不能访问非静态成员变量，也不能调用非静态成员函数

### 12-成员对象和封闭类
12.1 包含成员对象的类是封闭类

12.2 任何生成封闭类对象的语句，都要让编译器明白，对象中的成员对象是如何初始化的。
    ·具体做法就是，通过封闭类的构造函数的初始化列表

12.3 封闭类对象生成时，先执行所有对象成员的构造函数
    ·对象成员构造函数次序和在类中的说明次序一致，与在初始化成员列表中出现的次序无关
    ·封闭类对象消亡时，先执行封闭类的析构函数，次序和构造函数的调用次序相反

### 13-常量对象/常量成员函数/常引用
13.1 常量成员函数执行期间不应修改其所作用的对象，不能修改成员变量的值，不能调用同类的非常量成员函数，静态成员变量和静态成员函数除外。

13.2 常量对象上不能执行非常量成员函数

13.3 常量成员函数的重载
    ·两个同名函数，一个有const，算重载
    ·非常量对象使用非常量成员函数，常量对象使用常量成员函数

### 14-友元（friend关键字）
14.1 友元函数：一个类的友元函数可以访问该类的私有成员，友元函数不是该类的成员函数

14.2 如果A是B的友元类，那么A的成员函数可以访问B的私有成员

14.3 友元类之间的关系不能传递和继承

### 15-运算符重载
15.1 运算符重载实质是函数重载

15.2 可以重载为成员函数，参数个数为运算符目数-1。重载为普通函数时，参数个数为运算符数目。

15.3 形式
```
返回值类型 operator 运算符(形参表){}
```

15.4 浅拷贝和深拷贝

15.5 对运算符进行重载，好的风格应该尽量保留运算符原本的特性

15.6 运算符重载为友元函数

