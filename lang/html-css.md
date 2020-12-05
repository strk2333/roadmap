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





权重

**1.** 类型选择器（元素原则器）， 如h1,div；

 伪元素选择器 ,如 ::before

```
权重1
```

**2.** 类选择器， 如 .colorRed

  属性选择器， 如 [type=radio]

  伪类，如 :hover

```
权重10
```

⚠ 1中的选择器级别相同，2中的也是相同的优先级。

**3.** ID 选择器， 如： #example

```
权重100
```

**4.** 行内样式， style

```
权重1000
```

**5.** !important

```
权重最高
```



```
* { }                           -> 0
   li { }                          -> 1
   li:first-line { }               ->2 (一个元素，一个伪元素)
   ul li { }                       -> 2 (两个元素)
   ul ol+li { }                    ->3 (3个元素)
   h1 + *[rel=up] { }              -> 11 (一个元素，一个属性选择器)
   ul ol li.red { }                ->13 (三个元素，一个类)
   li.red.level { }                ->21 (一个元素，两个类)
   style=“”                        ->1000 (一个行内样式)
  p { }                           ->1 (一个元素)
  div p { }                       -> 2 (两个元素)
  .sith                           -> 10 (一个类)
  div p.sith { }                  -> 12 (两个元素，一个类)
  #sith                           ->100 ( 一个id)
  body #darkside .sith p { }      -> 112 (一个元素+ 一个ID + 一个类 + 一个元素 ；1+100+10+1)
```





隐藏滚动条

        scrollbar-width: none;
        -ms-overflow-style: none;
        &::-webkit-scrollbar {
          display: none;
        }




## problems

- width turns to 100% but hope to adjust the width by elements inside automaticlly [ display: table ]
- use vw for responsive site, font may be too small when use in font-size [use px]
- img has size 0 when src is null. [display: block]



zindex 不起作用

- -1

**失效的情况:**

1、父标签 position属性为relative；

2、问题标签无position属性（不包括static）；

3、问题标签含有浮动(float)属性。

4、问题标签的祖先标签的z-index值比较小



解决方法:

第一种: position:relative改为position:absolute；

第二种:浮动元素添加position属性（如relative，absolute等）；

第三种:去除浮动。

第四种:提高父标签的z-index值







![在这里插入图片描述](/Users/strk2333/Documents/Workspace/Doc/roadmap/lang/html-css_assets/70.jpeg)



pattern: /^[\u2E80-\u9FFF()（）]+$/,