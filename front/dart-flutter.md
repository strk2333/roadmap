## Dart

---

### 数据类型

- 兼容JS

- 数组

  - `let list: Array<number\> = [1, 2, 3];`

- 元组

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
- ??
- 注解



## Flutter

---





Graph Comparison (url)

Application

- Components
  - 
  - 
- Data Processing
  - Lang (Dart)
    - Type
    - Function
    - Class
    - Net
  - Dio
  - Provider
    - scope: runtime
    - scene: data cross pages, global data
    - essential: context
  - shared_preferences
    - scope: device
    - scene: local store, token etc.
    - essential: none
  - State
    - scope: single page
    - scene: values for view
    - essential: in state class
  - Debug
    - Log
      - print()
    - 
- View
  - Size
  - Theme
  - Position
  - Events
  - Components
    - Layout
      - Scaffold
      - Container
      - Expanded
      - Flexible
      - Center
      - Column
      - Row
    - Common
      - Icon
      - MaterialButton
      - DropdownButton
      - ListView
      - Text
      - TextField
      - PasswordInput
        - TextField obscureText: true
      - GustureDetector
    - Form
      - Intro
        - get key from Form's attribute *key*, key.currentState.validate()..save() ???
        - One side bind / controller.text
      - Form
      - TextFormField
        - InputDecoration
  - Route
    - Navigator



starter

1. 





#### 常用布局结构

- 



### 常用组件

- charts_flutter
- cupertino_icons
- path_provider
- sprintf
- dio





- 页面缓存
  - stack
  - offstge tickermode
  - AutomaticKeepAliveClientMixin

- 打印函数：print()

- 刷新子组件 子组件需要stateless

- 键盘顶起 resizeToAvoidBottomPadding: false
- row 统一高度 IntrinsicHeight

跳转

```
Navigator.push(
	context,
	new MaterialPageRoute(builder: (context) => LoginView()),
);         
```





```
flutter doctor -v // 完整性检查，尝试修复

flutter create myapp

flutter devices

flutter run

add xxx to pubspec.yaml
flutter pub get xxx
```

