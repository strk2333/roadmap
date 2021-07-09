# JS

## 基础

- 数据类型
  - 基本类型
    - number
    - string
    - boolean
    - undefined
  - 引用类型
    - object
      - null
    - array
    - function
  
- 变量声明
  
  - var 变量作用域
  
- 类型转换

- 数值

  ```
  1/0 !== 1/-0 // Infinity != -Infinity
  x != x // 判断NaN
  -Number.MIN_VALUE // 下溢 -0
  
  ```

  

  ```
  date.getMonth() // 月份 从0开始
  date.getDate() // 天数 从1开始
  date.getDay() // 星期 0星期日 1-6
  ```

  字符串 16位值序列

  

  

  

  

  

- 注释 打印

- 运算符

- 常用数据类型及操作

- 修饰符

- 函数 方法

- 类

- 条件与循环

- 错误处理

- 特色

- 注意点











## 容易忽视的 ES6

### 函数、数组和对象

#### 函数传值`undefined`

函数有默认值的参数在参数个数中间，并且后面的参数不实用默认值，这时要使用默认值的参数可以传入`undefined`。

```js
function detanx(x = null, y = 6) {
  console.log(x, y);
}
detanx(undefined, null); // null null
```



#### 函数 length

指定了默认值以后，函数的`length`属性，将返回**没有指定默认值的参数个数**，即该函数需要的**最少参数个数**。指定了默认值后，`length`属性将失真。

```
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```







## 小技巧



### 基础类型



#### 类型转换

##### String2Number

- '5277' * 1 = 5277
