# Android

## 基本流程

```
rails new prjname
rails server
gem server
```











QA

- 依赖包安装 sync
- gradle 
- github 包 maven { url 'https://jitpack.io' }



error

- API 'variant.getJavaCompile()' is obsolete and has been replaced with 'variant.getJavaCompileProvider()'.
  - allprojects repositories google() 放第一位

- NonExistentClass cannot be converted to Annotation
  - kotlin gradle版本













## 面试笔记

1. 简述四大组件

- Activity

- - 一个Activity通常就是一个单独的屏幕 (窗口)
  - Activity之间通过Intent进行通信
  - android应用中每一个Activity都必须要在AndroidManifest.xml配置文件中声明, 否则系统将不识别也不执行该Activity

- Service

- - 用于在后台完成用户指定的操作
  - 当服务是started状态时, 其生命周期与启动它的组件无关, 并且可以在后台无限期运行, 即使启动服务的组件已经被销毁. 因此, 服务需要在完成任务后调用stopSelf()方法停止, 或者由其他组件调用stopService()方法停止; 使用bindService()方法启用服务, 调用者与服务绑定在了一起, 调用者一旦退出, 服务也就终止
  - 开发人员需要在应用程序配置文件中声明全部的service, 使用<service></service>标签

- Content Provider

- - ContentProvider使一个应用程序的指定数据集提供给其他应用程序. 其他应用可以通过ContentResolver类从该内容提供者中获取或存入数据
  - 只有需要在多个应用程序间共享数据是才需要内容提供者. 例如, 通讯录数据被多个应用程序使用, 且必须存储在一个内容提供者中。它的好处是统一数据访问方式。
  - 是不同应用程序间共享数据的唯一方式
  - 不会直接使用ContentProvider类的对象, 大多数是通过ContentResolver对象实现对ContentProvider的操作
  - ContentProvider使用URI来唯一标识其数据集, 这里的URI以content://作为前缀, 表示该数据由ContentProvider来管理

- Broadcast Receiver

- - 可以使用它对外部事件进行过滤, 只对感兴趣的外部事件 (如当电话呼入时, 或者数据网络可用时) 进行接收并做出响应. 广播接收器没有用户界面. 然而, 它们可以启动一个activity或service来响应它们收到的信息, 或者用NotificationManager来通知用户. 通知可以用很多种方式来吸引用户的注意力, 例如闪动背灯, 震动, 播放声音等. 一般来说是在状态栏上放一个持久的图标, 用户可以打开它并获取消息
  - 广播接收者的注册有两种方法, 分别是程序动态注册和AndroidManifest文件中进行静态注册
  - 动态注册广播接收器特点是当用来注册的Activity关掉后, 广播也就失效了. 静态注册无需担忧广播接收器是否被关闭, 只要设备是开启状态, 广播接收器也是打开着的. 也就是说哪怕app本身未启动, 该app订阅的广播在触发时也会对它起作用

- 四大组件的注册

- - 都需要在AndroidManifest文件中进行配置
  - broadcast receiver广播接收者的注册分静态注册（在AndroidManifest文件中进行配置）和通过代码动态创建并以调用Context.registerReceiver()的方式注册至系统

- 激活

- - 当接收到ContentResolver发出的请求后, 内容提供者被激活. 而其它三种组件activity, 服务和广播接收器被一种叫做intent的异步消息所激活。

- 关闭

- - 内容提供者仅在响应ContentResolver提出请求的时候激活. 而一个广播接收器仅在响应广播信息的时候激活. 所以, 没有必要去显式的关闭这些组件. Activity关闭: 可以通过调用它的finish()方法来关闭一个activity. 服务关闭: 对于通过startService()方法启动的服务要调用Context.stopService()方法关闭服务, 使用bindService()方法启动的服务要调用Contex.unbindService()方法关闭服务. 

- android中的任务 (activity栈)

- - 任务其实就是activity的栈, 它由一个或多个Activity组成, 共同完成一个完整的用户体验. 栈底的是启动整个任务的Activity, 栈顶的是当前运行的用户可以交互的Activity, 当一个activity启动另外一个的时候, 新的activity就被压入栈, 并成为当前运行的activity. 而前一个activity仍保持在栈之中. 当用户按下BACK键的时候, 当前activity出栈, 而前一个恢复为当前运行的activity. 栈中保存的其实是对象, 栈中的Activity永远不会重排, 只会压入或弹出
  - 任务中的所有activity是作为一个整体进行移动的, 整个的任务 (即activity栈) 可以移到前台, 或退至后台
  - Android系统是一个多任务的操作系统, 可以在用手机听音乐的同时, 也执行其他多个程序. 每多执行一个应用程序, 就会多耗费一些系统内存，当同时执行的程序过多, 系统就会得越来越慢. 为了解决这个问题, Android引入了一个新的机制, 即生命周期

1. Activity的生命周期

​       onCreate onStart onResume onPause onStop onDestroy

1. Service的生命周期

​       Service的启动方式有两种.

- 通过startService启动, 生命周期为 startService onCreate onStartCommand onDestroy

- - 其中startService可以被调用多次, 而onCreate只在第一次调用, onStartCommand会被多次调用. 
  - 当通过startService启动时, 通过intent传值, 在onStartConmand()方法中获取值的时候, 一定要先判断intent是否为null.

- 通过bindService启动, 生命周期为 bindService onCreate onBind unBind onDestroy

- - 更加便利activity中操作service
  - 比如加入service中有几个方法, a, b, 如果要在activity中调用, 在需要在activity获取ServiceConnection对象, 通过ServiceConnection来获取service中内部类的类对象, 然后通过这个类对象就可以调用类中的方法, 当然这个类需要继承Binder对象

1. Activity的启动过程 (非生命周期)

1. 六大布局

​       声明Android程序布局有两种方式: 

- 使用XML文件描述界面布局
- 在Java代码中通过调用方法进行控制

​       既可以使用任何一种声明界面布局的方式, 也可以同时使用两种方式

​       线性布局 (LinearLayout) 

​       框架布局 (FrameLayout) 

​       表格布局 (TableLayout) 

​       相对布局 (RelativeLayout) 

​       绝对布局 (AbsoluteLayout) 

​       网格布局 (GridLayout) 

1. 五大存储：

​       在Android中, 可供选择的存储方式有SharedPreferences, 文件存储, SQLite数据库方式, 内容提供器 (Content provider) 和网络







##  Android sdk

export ANDROID_HOME=/Users/strk2333/Library/Android/sdk

export PATH=${PATH}:${ANDROID_HOME}/tools

export PATH=${PATH}:${ANDROID_HOME}/platform-tools

export PATH="$PATH:~/Library/Android/sdk/emulator”





