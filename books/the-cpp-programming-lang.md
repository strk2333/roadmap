# C++程序设计语言 特别版

------

第一部分 基本功能

1. 给全局变量赋值 ::x = 1;
2. const初始化

![img](./the-cpp-programming-lang_assets/3a9fc6d4-a68b-426c-a8e9-11d284cfdeb0.png)

对变量的赋值经常会变成对临时值的赋值，对于常量引用不会出现这种问题。

1. 运算符概览 P136
2. new 

![img](./the-cpp-programming-lang_assets/fa1ffdd8-e179-4988-86db-5e9886560027.png)

1. 使用（T）e显式转换类型是一件很危险的事情，最好使用*_cast

static_cast 相关类型转换

reinterpret_cast 不相关类型转换

dynamic_cast 运行中检查转换

const_cast 清除const, volatile限定符的转换

![img](./the-cpp-programming-lang_assets/c72b1015-d4c0-42f2-a07b-499687e90e1c)![img](./the-cpp-programming-lang_assets/8220b28b-bac0-4e94-a1ca-21f54d1b6881.png)

 typedef 会破坏这种保护

1. 函数指针

![img](./the-cpp-programming-lang_assets/21382048-1648-4571-82e7-cd12410129b0.png)

1. 头文件中不该有以下内容

![img](./the-cpp-programming-lang_assets/d51dcfd6-5513-40f3-aad0-eb6347a4d36c.png)

1. 与非C++代码连接

1. 全局变量初始化的顺序没有任何保证，在初始式之间建立任何顺序依赖都是不明智的。

可以利用函数返回强加顺序性。

![img](./the-cpp-programming-lang_assets/75eab54e-56ff-40a0-bc7d-8fd5842e9caa.png)

1. 程序结束的方式

![img](./the-cpp-programming-lang_assets/8230e727-e13f-47e7-8dd5-dedb48f2d173.png)

1. 唯一定义原则

------

第二部分 抽象机制

1. 常量成员函数

![img](./the-cpp-programming-lang_assets/a04a6b1f-0d4a-4120-9fbd-3049de77193f.png)

1. 自引用

![img](./the-cpp-programming-lang_assets/65089d78-e121-4560-8179-6cc967794fde.png)

1. 缓存技术

![img](./the-cpp-programming-lang_assets/cb557ca8-88c0-4f9c-ae4c-0a37fdeaadec.png)

1. 对象的放置

![img](./the-cpp-programming-lang_assets/6fa6dc25-e8fa-43a3-b8e0-9c53101c34fa.png)

1. 运算符重载

![img](./the-cpp-programming-lang_assets/b1dbf501-1dbb-4ff6-8b34-2ad922b05b6f.png)

1. 必须使用初始化列表进行初始化的：const对象、引用对象

1. 