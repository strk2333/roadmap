# Unity Shader 入门精要

目录

------

第1章 欢迎

第2章 渲染流水线

第3章 Unity Shader基础

第4章 数学基础

第5章 开始学习

第6章 基础光照

第7章 基础纹理

第8章 透明效果

第9章 复杂光照

第10章 高级纹理

第11章 动画

第12章 屏幕后处理效果

第13章 深度和法线纹理

第14章 非真实感渲染

第15章 使用噪声

第16章 渲染优化技术

第17章 表面着色器探秘

第18章 物理渲染

第19章 更新内容

第20章 更多内容

第1章 欢迎

------

略

第2章 渲染流水线

------

1. 渲染流程三个阶段: 应用阶段, 几何阶段, 光栅化阶段 (step1 输出渲染图元 step2 输出屏幕空间的顶点信息)

应用阶段 Application Stage

  CPU负责.

  三个任务:

- 准备场景数据 (摄像机 视锥体 模型 光源)
- 粗粒度剔除
- 设置渲染状态, 输出几何信息(渲染图元)

几何阶段 Geometry Stage

  GPU负责

  把顶点坐标变换到屏幕空间, 交给光栅器处理.

光栅化阶段 Rasterizer Stage

  GPU负责

  决定每个渲染图元中哪些像素要被绘制在屏幕上. 对上阶段得到的逐顶点数据进行插值, 然后进行逐像素处理.

1. CPU与GPU之间的通信 (应用阶段)

应用阶段:

- 把数据加载到显存 

​         HDD -> RAM -> VRAM (RAM中数据可以保留作为缓存)

- 设置渲染状态 

​         使用的Shader, 光源属性, 材质等

- 调用Draw Call 

​         CPU通过Draw Call告诉GPU开始进行一个渲染过程. 一个DrawCall指向本次调用需要渲染的图元列表, 不包括上个阶段完成的材质信息.

1. GPU流水线

几何阶段:   顶点数据 -> 顶点着色器 -> 曲面细分着色器 -> 几何着色器 -> 裁剪 -> 屏幕映射 ->

光栅化阶段:   三角形设置 -> 三角形遍历 -> 片元着色器 -> 逐片元着色 -> 屏幕图像

- 顶点着色器 完全可编程 用于实现顶点空间变换, 着色等功能.
- 曲面细分着色器 可选 用于细分图元
- 几何着色器 可选 用于执行逐图元着色操作
- 裁剪 将不在摄像机视野内的顶点裁掉, 并剔除某些三角图元的面片 不可编程 可配置
- 屏幕映射 负责把每个图元坐标转换到屏幕坐标系中 不可配置或编程
- 三角形设置 三角形遍历 固定函数阶段
- 片元着色器 完全可编程 用于实现逐片元的着色操作
- 逐片元着色 修改颜色, 深度缓冲, 进行混合等操作 不可编程, 有很高的配置性

详细说明:

- 顶点着色器

- - 无法创建销毁顶点, 无法得到顶点间关系
  - 主要工作: 坐标变换, 逐顶点光照 (还可以计算顶点颜色等)
  - 必须完成的工作: 把顶点坐标从模型空间转换到齐次裁剪空间

- 裁剪 略

- 屏幕映射

- - 任务: 把每个图元的xy坐标转换到屏幕坐标系, 屏幕坐标系和z坐标构成窗口坐标系, 一起传递到光栅化阶段.
  - OpenGL: 左下角为(0,0) DirectX: 左上角为最小窗口坐标值.

- 三角形设置

- - 光栅化目标: 计算每个图元覆盖了哪些像素, 并计算它们的颜色.
  - 三角形设置: 计算光栅化一个三角网格需要的信息.
  - 由三角网格的顶点数据, 找到三角形边界的表示方式, 计算三角网格表示数据的过程.

- 三角形遍历 (扫描变换)

- - 检查每个像素是否被一个三角网格覆盖, 是则生成一个片元. 使用三个顶点的信息进行插值
  - 输出一个片元序列. 
  - 片元不是像素, 片元包含很多状态的集合, 用于计算像素最终颜色. 
  - 状态包括屏幕坐标, 深度信息, 顶点信息(法线, 纹理坐标 etc.)等

- 片元着色器 (像素着色器)

- - 输出颜色值
  - 纹理采样: 经过顶点着色器输出纹理坐标, 光栅化插值, 得到片元的纹理坐标
  - 仅可以作用于单个片元. (除了导数信息)

- 逐片元操作 (输出合并阶段)

- - 主要任务: 

  - - 决定每个片元的可见性. 如深度测试, 模板测试
    - 通过测试的片元, 将其颜色值和存储在颜色缓冲区的颜色混合

  - 高度可配置性

  - 模板测试: 首先GPU会读取 (使用读取掩码) 模板缓冲区中该片元的模板值, 然后和读取的参考值对比, 开发者进行处理

  - 深度测试: GPU将该片元深度值和缓冲区中的深度值比较, 开发者进行处理.

  - Early-Z: 深度测试提前到片元着色器之前进行, 提高效率.

  - 双重缓冲: 对场景的渲染发生在幕后, 在后置缓冲中, 渲染完成后交换前置缓冲区和后置缓冲区的内容.

1. 困惑

- OpenGL/DirectX

- - 图像应用编程接口, 用于渲染图形. 
  - 一个应用程序向这些接口发送渲染命令, 接口向显卡驱动发送渲染命令.

- HLSL GLSL Cg

- - 着色器语言
  -  DirectX的HLSL (High Level Shading Language)
  - OpenGL的GLSL (OpenGL Shading Language)
  - NVIDIA的Cg (C for Graphic)
  - GLSL 跨平台 由显卡驱动完成着色器的编译
  - HLSL 平台有限 由微软控制着色器的编译
  - Cg 跨平台 根据平台编译成相应的语言 可以无缝移植成HLSL 可能无法发挥OpenGL的最新特性

- Draw Call

- - CPU调用应用程序接口, 命令GPU进行渲染
  - CPU和GPU并行: 命令缓冲区
  - CPU准备数据需要完成很多工作, 例如检查渲染状态等. 如果DrawCall过多, CPU就会在提交上花费大量时间.
  - 利用批处理, 在RAM中合并网格, 再发送给GPU, 减少DrawCall. (一个网格中只能使用一种渲染状态)
  - 注意点: 不要过多使用小网格, 尽量合并, 不要过多使用材质. 尽量在不同网格间公用材质.

- 固定管线渲染

- - 在较旧的GPU上实现的渲染流水线. 只给开发者配置操作, 不能完全控制.

第3章 Unity Shader基础

------

1. 概述

- 材质和shader搭配使用
- Unity Shader是高层级的渲染抽象层, ShaderLab是编写它的说明性语言. 基本结构如下:

Shader “ShaderName” {

  Properties {

​     // 属性

  }

  SubShader {

​     // 显卡A使用的子着色器

  }

  SubShader {

​     // 显卡B使用的子着色器

  }

  Fallback “VertexLit”

}

- Unity Shader结构

- - 名字 Shader “Custom/MyShader” {   }
  - 属性

Properties {

  Name (“display name”, PropertyType) = DefaultValue

  Name (“display name”, PropertyType) = DefaultValue

  // 更多属性

}

Int               number           _Int (“Int”, Int) = 2

Float             number           _Float (“Float”, Float) = 1.5

Range(min, max)   number           _Range(“Range”, Range(0.0, 5.0)) = 3.0

Color             (n,n,n,n)           _Color(“Color”, Color) = (1, 1, 1, 1)

Vector            (n,n,n,n)           _Vector(“Vector”, Vector) = {2, 3, 6, 1}

2D               “defaulttexture” {}  _2D (“2D”, 2D) = “” {}

Cube             “defaulttexture” {}  _Cube (“Cube”, Cube) = “white” {}

3D               “defaulttexture” {}  _3D (“3D”, 3D) = “black” {}

- - - Properties仅仅是为了让属性出现在面板上, 没有在里面声明也可以使用

- - SubShader

  - - Unity Shader文件中可以包含多个, 但至少有一个SubShader.
    - 加载时, 选择第一个能在目标平台运行的SubShader, 如果都不支持, 使用Fallback制定的Unity Shader

SubShader {

  // 可选的

  [Tags]

  // 可选的

  [RenderSetup]

   

  Pass {

  }

  // other Passes

}

- - - 每个Pass定义了一次完整的渲染流程, 如果Pass过多, 会造成渲染性能的下降, 越少越好
    - 