# HTML & CSS

## HTML

### 基础

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

  

  

  

  

  ### iframe

  

  

  

  

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





## CSS





## 常见场景

### 文本选择

- 禁止 user-select: none

- 颜色 ::selection

  ```
  ::selection {
      background:#d3d3d3; 
      color:#555;
  }
  ```



### 实现自定义原生 select 控件的样式

在 ios 手机上直接修改 select 的样式不行，这个时候禁用原生的样式即可。

css 代码如下：

```
select {
  -webkit-appearance: none;
}
```



### 文本溢出处理

```
//单行
.single {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

//多行
.more {
  display: -webkit-box !important;
  overflow: hidden;
  text-overflow: ellipsis;
  work-break: break-all;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2; //指定行数
}
```





### 弹性滚动

```
body {
  overflow: scroll;
  -webkit-overflow-scrolling: touch;
}
// 注意：Android 不支持原生的弹性滚动，但可以借助第三方库 iScroll 来实现。
```



###  一像素边框设置

如果想保持边框的大小在任何设置上都是 1px，但 1px 使用 2dp 渲染会显示为 2px，需要缩放一下。

```
.folder li {
  position: relative;
  padding: 5px;
}
.folder li + li:before {
  position: absolute;
  top: -1px;
  left: 0;
  content: " ";
  width: 100%;
  height: 1px;
  border-top: 1px solid #ccc;
  -webkit-transform: scaleY(0.5);
}
```



### 防止鼠标选中事件

```
// 给元素添加onslectstart="return false"`
<div class="mask" onselectstart="return false"></div>
<div class="link">
  <a href="javascrip;;">登录</a>
</div>
```



### 兼容 IE 浏览器的透明度处理

```
.ui {
  width: 100%;
  height: 100%;
  opacity: 0.4;
  filter: Alpha(opacity=40); //兼容IE浏览器
}
```



### 常用的全屏居中 CSS 函数

```
body {
  height: 100vh;
  text-align: center;
  line-height: 100vh;
}
```











## problems

- width turns to 100% but hope to adjust the width by elements inside automaticlly [ display: table ]
- use vw for responsive site, font may be too small when use in font-size [use px]
- img has size 0 when src is null. [display: block]

