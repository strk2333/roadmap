# OC

## 基本



## 消息传递

```objc
obj.method(argument); // c++

[obj method: argument]; // oc
[car fly]; // 运行期才处理消息，允许发送未知消息给对象
```





## 数据类型

```objc
NSString		字符串
CGfloat 		浮点值的基本类型
NSInteger 	整型
BOOL 				布尔型
```





## 打印日志

```objc
NSlog(@"");
```





## 字符串

```objc
NSString* myString = @"My String\n"; // 常量值创建NSString对象
NSString* anotherString = [NSString stringWithFormat:@"%d %s", 1, @"String"];

// 从一个C语言字符串创建Objective-C字符串
NSString*  fromCString = [NSString stringWithCString:"A C string" 
encoding:NSASCIIStringEncoding];
```



## 数组

```objc
NSMutableArray *aMutableArray = [[NSMutableArray alloc]init];
[anArray addObject:@"firstobject"];
NSArray *aImmutableArray = [[NSArray alloc]
initWithObjects:@"firstObject",nil];
```





## 类

```
// 类定义
@interface MyObject : NSObject {
    int memberVar1; // 实体变量
    id  memberVar2;
}

+(return_type) class_method; // +：类方法，不需要实例就可以调用，与 C++ 的静态函数（static member function）相似
-(return_type) instance_method1; // -：实例方法，一般的实例方法（instance method）。
-(return_type) instance_method2: (int) p1;
-(return_type) instance_method3: (int) p1 andPar: (int) p2;
@end

// 类方法实现，定义私有（private）变量及方法
@implementation MyObject {
  int memberVar3; // 私有实体变量
}

+(return_type) class_method {
    .... //method implementation
}
-(return_type) instance_method1 {
     ....
}
-(return_type) instance_method2: (int) p1 {
    ....
}
// andPar 为可选参数
-(return_type) instance_method3: (int) p1 andPar: (int) p2 {
    ....
}
@end

// 创建对象
MyObject * my = [[MyObject alloc] init];
MyObject * my = [MyObject new];

// 调用
[my instance_method3:p1 andPar:p2];


```





## 属性 ( Properties )

```
@property （参数1，参数2，参数3）类型名字；
参数1：atomicity(nonatomic) atomicity的默认值是atomic，读取函数为原子操作
参数2：setter/getter方法(assign/retain/copy) assign/retain/copy 代表赋值的方式
参数3：读写属性(readwrite/readonly)readonly关键字代表setter不会被生成， 所以它不可以和 copy/retain/assign组合使用。

// 访问属性
self.myString = @"Test";
[self setMyString:@"Test"];


```





## 类别 ( Categories )

```
@interface MyClass(customAdditions)
- (void)sampleCategoryMethod;
@end

@implementation MyClass(categoryAdditions)

-(void)sampleCategoryMethod{
   NSLog(@"Just a test category");
}
@end
```





## int 字符串转换

```
 1，字符串拼接

NSString *newString = [NSString stringWithFormat:@"%@%@",tempA,tempB];


2，字符转int

int intString = [newString intValue];


3，int转字符

NSString *stringInt = [NSString stringWithFormat:@"%d",intString];


4，字符转float

float floatString = [newString floatValue];


5，float转字符

NSString *stringFloat = [NSString stringWithFormat:@"%f",intString];
```





## IOS



UIApplication对象是应用程序的象征，每一个应用都有自己的UIApplication对象，而且是单例的。通过[UIApplication sharedApplication]可以获得这个单例对象，一个iOS程序启动后创建的第一个对象就是UIApplication对象， 利用UIApplication对象，能进行一些应用级别的操作。
第一个参数表示参数的个数，第二个参数表示装载函数的数组，第三个参数，是UIApplication类名或其子类名，若是nil，则默认使用UIApplication类名。第四个参数是协议UIApplicationDelegate的实例化对象名，这个对象就是UIApplication对象监听到系统变化的时候通知其执行的相应方法。

启动完毕会调用 didFinishLaunching方法，并在这个方法中创建UIWindow，设置AppDelegate的window属性，并设置UIWindow的根控制器。如果有storyboard，会根据info.plist中找到应用程序的入口storyboard并加载箭头所指的控制器，显示窗口。storyboard和xib最大的不同在于storyboard是基于试图控制器的，而非视图或窗口。展示之前会将添加rootViewController的view到UIWindow上面（在这一步才会创建控制器的view）

