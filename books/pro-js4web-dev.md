# JavaScript高级程序设计（第三版）

Professional JavaScript for Web Developers

## 第一章 JavaScript简介

  诞生于1995年，主要目的是输入验证。在 JavaScript 问世之前，必须把表单数据发送到服务器端才能确定用户是否没有填写某个必填域，是否输入了无效的值

### 1.1 历史

### 1.2 实现

  JavaScript：ECMAScript，DOM，BOM

DOM级别

  DOM1级：

- DOM Core 映射基于XML的文档结构，简化对文档的访问操作
- DOM HTML 针对HTML的对象方法

  DOM2级

- DOM视图
- DOM事件
- DOM样式
- DOM遍历和范围

  DOM3级

- DOM加载和保存
- DOM验证
- XML1.0规范

  其他DOM标准

- SVG
- MathML
- SMIL

### 1.3 版本



## 第二章 在HTML中使用JavaScript

### 2.1 <script> 元素

与解析嵌入式 JavaScript 代码一样， 在解析外部 JavaScript 文件(包括下载该文件)时，页面的处理也会暂时停止。

如果是在 XHTML 文档 中，也可以省略前面示例代码中结束的</script>标签，例如:

<script type="text/javascript" src="example.js" />

但是，不能在 HTML 文档使用这种语法。原因是这种语法不符合 HTML 规范，而且也得不到某些 浏览器(尤其是 IE)的正确解析。

async defer(有顺序问题)

![img](./pro-js4web-dev_assets/b9dc8358-a046-4234-9ade-ccc1d4b32c45.png)

XHTML CDATA （注释）

### 2.2 嵌入代码与外部文件

### 2.3 文档模式

- 混杂模式（quarks mode）
- 标准模式（standards mode）

   混杂模式会让 IE 的行为与(包 含非标准特性的)IE5 相同，而标准模式则让 IE 的行为更接近标准行为。虽然这两种模式主要影响 CSS 内容的呈现，但在某些情况下也会影响到 JavaScript 的解释执行。

### <noscript>元素

## 第三章 基本概念

### 3.1 语法

- 标识符 

- - 第一个字符必须是一个字母、下划线(_)或一个美元符号($);
  - 其他字符可以是字母、下划线、美元符号或数字。

- 严格模式 "use strict";

- undefined类型 对未初始化和未声明的变量执行 typeof 操作符都返回了 undefined

- null类型 空对象指针

- - 如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 null 而不是其他值

- Boolean类型

- - Boolean()
  - false: false, “”, 0, NaN, null, undefined

- Number类型

- - 八进制 超出范围十进制， 严格模式下八进制无效

  - 在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。

  - 浮点数

  - - var floatNum3 = .1; // 有效，但不推荐
    - var floatNum = 3.125e7; // 等于31250000 e/E
    - 在默认情况下，ECMASctipt 会将那些小数点后面带有 6 个零以上的浮点数值转换为以 e 表示法 表示的数值
    - 浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数
    - isFinite(num) 5e-32 1.7976931348623157e+308

  - NaN 

  - - 任何涉及 NaN 的操作(例如 NaN/10)都会返回 NaN

    - NaN 与任何值都不相等，包括 NaN 本身

    - isNaN 尝试转换数值 

    - - Object: valueOf() toString()

  - 数值转换

  - - Number() 空字符串返回0

    - parseInt() 

    - - 不建议八进制 空字符串返回NaN
      - var num1 = parseInt("AF", 16); //175
      - var num1 = parseInt(“0xAF", 16); //175
      - 不指定基数意味着让 parseInt()决定如何解析输入的字符串，因此为了避免错误的解析，我们建议无论在什么情况下都明确指定基数。

    - parseFloat()

    - - 类似parseInt()
      - 从第一个字符(位置 0)开始解析每个字符。而且也是一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止
      - 如果字符串包含的是一个可解析为整数的数(没有小数点，或者小数点后都是零)，parseFloat()会返回整数

- 字符串

- - 由零或多个16 位 Unicode 字符组成的字符序列

  - 转义字符作为一个字符解析

  - \xnn 以十六进制代码nn表示的一个字符(其中n为0~F)。例如，\x41表示"A"

  - \unnnn 以十六进制代码nnnn表示的一个Unicode字符(其中n为0~F)。例如，\u03a3表示希腊字符Σ

  - *** 这个属性返回的字符数包括 16 位字符的数目。如果字符串中包含双字节字符，那么 length 属性 可能不会精确地返回字符串中的字符数目。

  - toString(radix)

  - String()

  - - 如果值有 toString()方法，则调用该方法(没有参数)并返回相应的结果
    - 如果值是 null，则返回"null"
    - 如果值是 undefined，则返回"undefined"

- Object

- - var o = new Object; // 有效，但不推荐省略圆括号
  - constructor hasOwnProperty(propertyName) isPrototypeOf(object) propertyIsEnumerable(propertyName) 
  - toLocaleString() 返回对象的字符串表示，该字符串与执行环境的地区对应。
  - toString() 返回对象的字符串表示。
  - valueOf() 返回对象的字符串、数值或布尔值表示。通常与 toString()方法的返回值相同。

- 操作符

- - 位操作符 NOT AND OR XOR( ^ ) 左移 右移( 有符号>> 无符号>>> )

  - - var num1 = 25; // 二进制00000000000000000000000000011001

    - var num2 = ~num1; // 二进制11111111111111111111111111100110

    - var num2 = -num1 - 1;

    - 左移不会影响操作数的符号位

    - ? 无符号右移是以 0 来填充空位

    - 布尔操作符

    - - alert(!!0); // false

  - 相等操作符

  - - 相等与不相等

    - - null == undefined
      - 如果两个操作数都指向同一个对象，则相等操作符返回 true;否则，返回 false

    - 全等与不全等

  - 逗号操作符

  - - var num = (5, 1, 4, 8, 0); // num的值为0

- 语句

- - for-in

  - - 可以用来枚举对象的属性
    - ECMAScript 对象的属性没有顺序。因此，通过 for-in 循环输出的属性名的顺序是不可预测的。

  - label

  - with

  - - with (expression) statement;
    - 严格模式下不允许使用 with 语句，否则将视为语法错误。
    - 有性能问题，不建议使用

  - switch

  - - 可以在 switch 语句中使用任何数据类型（全等操作符）

- 函数

- - arguments

  - - 类数组对象
    - 值永远与对应命名参数的值保持同步
    - ECMAScript 中的所有参数传递的都是值，不可能通过引用传递参数。

# 第四章 变量，作用域和内存问题

### 4.1 基本类型和引用类型的值

- 属性

- - 不能给基本类型的值添加属性，尽管这样做不会导致任何错误

- 检测类型

- - instanceof

  - typeof

  - - null object
    - undefined undefined

## 4.2 执行环境和作用域

- 作用域

- - 延长作用域链

  - - catch
    - with

  - JavaScript 没有块级作用域

- 垃圾收集

- - 用于标识无用变量的策略

  - - 标记清除（mark-and-sweep）
    - 引用计数（reference counting）

- 管理内存

- - 分配给 Web 浏览器的可用内存数量通常要比分配给桌面应用程序的少
  - 一旦数据不再有用，最好通过将其值设置为 null 来释放其引用 解除引用(dereferencing). 适用于大多数全局变量和全局对象的属性。局部变量会在它们离开执行环境时自动被解除引用

## 第五章 引用类型

### 5.1 Object类型

- 创建

- - new

  - 字面量 {}

  - - 在最后一个属性后面添加逗号，会在 IE7 及更早版本和Opera 中导致错误。

- 访问

- - .attr
  - [‘attr']

### 5.2 Array类型

- ECMAScript 数组的大小可以动态调整

- 创建

- - new (可省略new)

  - 字面量 []

  - - [,] 可能有浏览器问题

- length属性不是只读的

- - 当把一个值放在超出当前数组大小的位置上时，数组就会重新计算其长度值，即长度值等于最后一项的索引加 1

- 检测数组

- - instanceof 操作符的问题在于，它假定只有一个全局执行环境。如果网页中包含多个框架，那实 际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的 Array 构造函数。如果你从 一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别具有各自不同的构造函数。
  - ECMAScript 5 新增了 Array.isArray()

- 转换方法

- - valueOf 返回数组
  - join 不传或传undefined使用，分隔。IE7及更早的版本使用undefined分隔
  - 数组中某一项是null/undefined，在join，toLocaleString，toString，valueOf返回结果中以空串表示

- 栈方法

- - push pop

- 队列方法

- - push shift
  - 反向队列 unshift pop
  - unshift方法 IE7及之前的版本返回undefined，而不是新数组长度

- 重排序方法

- - reverse 
  - sort 默认字符串升序 自定义方法：不换位-1，相等0，换位1

- 操作方法

- - concat

  - - 空 复制数组
    - 数组
    - 不是数组 添加到末尾

  - slice

  - - 不包括结束项
    - 负数，数组长度+负数确定位置
    - 不改变数组

  - splice

  - - 改变数组，返回删除数组
    - 删除 (p1, p2) p1删除首项位置，p2删除项数
    - 插入 (p1, 0, p3...)
    - 替换 (p1, p2, p3...)

- 位置方法

- - indexOf
  - lastIndexOf 