## TS

## 基础

### 数据类型

- 兼容JS

- 数组

  - `let list: Array<number\> = [1, 2, 3];`

- 元祖

  - `let x: [string, number];`
  - 已知元素数量和类型的数组，各元素的类型不必相同
  - 访问一个越界的元素，会使用联合类型替代

- 枚举

  - `enum Color {Red=1, Green, Blue = 4}`
    `let c: Color = Color.Green;`

  - 默认情况下从`0`开始为元素编号. 也可以手动的指定成员的数值.

  - 可以由枚举的值得到名字

    `let colorName: string = Color[2]; // 'Green'`

- Any

  - `let notSure: any = 4;`
    `notSure = "maybe a string instead";`
    `notSure = false; // okay, definitely a boolean`
  - `let list: any[] = [1, true, "free"];`

- Void

  - `void`类型像是与`any`类型相反, 表示没有任何类型
  - 声明一个`void`类型的变量没有什么大用, 只能为它赋予`undefined`和`null`

  

- Null Undefined

  - 默认情况下`null`和`undefined`是所有类型的子类型. 可以把 null和undefined赋值给number类型的变量. 当指定`--strictNullChecks`标记，`null`和`undefined`只能赋值给`void`和它们自己.

- Never

  - 永不存在的值的类型
  - `never`类型是任何类型的子类型, 也可以赋值给任何类型. *没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）. 即使 `any`也不可以赋值给`never`

- Object

- 类型断言

  - 尖括号 (<string\>someValue).length
  - as (someValue as string).length

### 变量声明



### 其他

- 解构 [first, second] = [second, first];
