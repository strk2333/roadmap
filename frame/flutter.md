# Flutter

## Application

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





## 常用布局结构

- 



## 常用组件

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

