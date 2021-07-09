# Algorithms (Fourth Edition)

## 第一章 基础

### 1.1 基本编程模型

Java的原始数据类型

- int -2^31 ~ 2^31 - 1之间的整数（32位，二进制补码）
- double 双精度实数（64位，IEEE 754标准）
- boolean
- char 字符（16位）



其他

- long 64位
- short 16位
- byte 8位
- float 32位



类型转换

如果不会损失信息，数值会被自动提升为高级的数据类型



数组

```java
// 完整
double[] a;
a = new double[N];
for (int i = 0; i < N; i++)
	a[i] = 0.0; // 多余

// 简化
double[] a = new double[N];
int[] a = { 1, 2 };

// 起别名
int[] b = a;

// 矩阵相乘
int N = a.length;
double[][] c = new double[N][N];
for (int i = 0; i < N; i++) {
	for (int j = 0; j < N; j++) {
		for (int k = 0; k < N; k++) {
            c[i][i] += a[i][k] * b[k][j];
        }
    }
}
```





静态方法

```java
// 判断素数
public static boolean isPrime(int N) {
	if (N < 2) return false;
	for (int i = 2; i * i <= N; i++)
		if (N % i == 0) return false;
	return true;
}

// 计算平方根（牛顿迭代法）
public static double sqrt(double c) {
    if (c < 0) return Double.NaN;
    double err = 1e-15;
    double t = c;
    while (Math.abs(t - c/t) > err * t)
        t = (c/t + t) / 2.0;
    return t;
}
```



递归的三个要点

- 递归总有一个最简单的情况——方法的第一条语句总是一个包含 return 的条件语句。
- 递归调用总是尝试解决一个规模更小的子问题，这样递归才能收敛到最简单的情况。
- 递归调用的父问题和尝试解决的子问题之间不应该有交集。



重定向与管道

```shell
// 重定向至一个文件
% java RandomSeq 1000 100.0 200.0 > data.txt

// 重定向输入流
% java Average < data.txt

// 一个程序的输出重定向为另一个程序的输入
// 管道
% java RandomSeq 1000 100.0 200.0 | java Average

```



二分查找

```java
// lo -   - -  mid - - - hi
// lo mid - hi -   - - - -

import java.util.Arrays
public class BinarySearch
{
	public static int rank(int key, int[] a) {
		// 数组必须有序
        int lo = 0;
        int hi = a.length - 1;
        while(lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (key < a[mid])	hi = mid - 1;
            else if (key > a[mid]) lo = mid + 1;
            else return mid;
        }
        return -1;
	}
	psvm(args) {
		int[] whiteList = In.readInts(args[0]);
		Arrays.sort(whiteList);
		while (!StdIn.isEmpty())
		{
			int key = StdIn.readInt();
			if (rank(key, whiteList) < 0) {
				StdOut.println(key);
			}
		}
	}
}

```







答疑

- 什么是 Java 字节码？
  - 它是程序的一种低级表示，可以运行于 Java 虚拟机。将程序抽象为字节码可以保证程序员的代码能够运行在各种设备之上。
- Java 允许整型溢出并返回错误值的做法是错误的。Java 是否应该自动检查溢出？
  - 这个问题在程序员中是有争议的。简单的回答是它们之所以被称为原始数据类型就是因为缺乏此类检查。避免此类问题不需要很高深的知识。我们会使用 int 类型表示较小的数（小于10个十进制位），使用 long 表示10亿以上的数。
  - Math.abs(-2147483648) 返回值为 -2147483648，这是整数溢出的典型例子。
- 如何将一个 double 变量初始化为无穷大？
  - Java 内置常数 Double.POSITIVE_INFINITY 和 Double.NEGATIVE_INFINITY
- 能够将 double 类型的值和 int 类型的值互相比较吗？
  - 不通过类型转换是不行的，但 Java一般会自动进行转换。
- Java 表达式 1/0 和 1.0/0.0 的值是什么？
  - 第一个产生运行时除以零异常，第二个值是无穷大。
- 能使用 < 和 > 比较 String 变量吗？
  - 不行，只有原始数据类型定义了这些运算符。
- 负数的除法和余数是什么？
  - a/b 会向0取整；a%b的定义是 (a/b)*b + a % b 恒等于 a。
  - -14 / 3 = -4, 14 / -3 = -4
  - -14 % 3 = -2, 14 % -3 = 2
- for while 循环的主要区别
  - 循环变量在循环结束后能否继续使用
- 声明数组的方式
  - int a[]		C 语言的声明方式
  - int[] a        Java 提倡的声明方式





### 1.2 数据抽象

 暂略



### 1.3 背包、队列和栈

暂略



### 1.4 算法分析



幂次法则

定义

我们用 ~f(N) 表示所有随着 N 的增大除以 f(N) 的结果趋近于 1 的函数。

我们用 g(N) ~ f(N) 表示 g(N)/f(N) 随着 N 的增大趋近于 1。



| 函数                | 近似     | 增长的数量级 |
| ------------------- | -------- | ------------ |
| (N^3)/6 - N^2 + N/3 | ~(N^3)/6 | N^3          |
| (N^2)/2 - N/2       | ~(N^2)/2 | N^2          |
| lgN + 1             | ~lgN     | lgN          |
| 3                   | ~3       | 1            |



常见增长数量级函数

| 描述         | 函数  |
| ------------ | ----- |
| 常数级别     | 1     |
| 对数级别     | logN  |
| 线性级别     | N     |
| 线性对数级别 | NlogN |
| 平方级别     | N^2   |
| 立方级别     | N^2   |
| 指数级别     | 2^N   |





