## LeetCode Functions

### C++

#### sort

```c++
sort(start, end, method);	//method的调用必须是静态成员, 声明为static.
```



#### struct

```c++
struct SAMPLE
{
     int x;
     int y;
     int add() {return x+y;}
}s1;

// 初始化
struct Test{
    int a;
    char b;
    double c;
    Test(){
        a = 1;
        b = 'a';
        c = 0;
    }
}
```



#### priority_queue

priority_queue<Type, Container, Function>

默认大顶堆

```c++
//大顶堆
priority_queue <int,vector<int>,less<int>> q;
//小顶堆
priority_queue <int,vector<int>,greater<int>> q;

// 和队列基本操作相同:
top // 访问队头元素
empty // 队列是否为空
size // 返回队列内元素个数
push // 插入元素到队尾 (并排序)
emplace // 原地构造一个元素并插入队列
pop // 弹出队头元素
swap // 交换内容
```



#### string和int互转

```c++
to_string(int)
stoi(string)
```



#### vector

```c++
vector<int> res; // 初始化

res.push_back(1); // 插入到尾部

res.insert(res.begin(),cur->val); // 插入到头部

vector.insert(pos,n,elem); // 在pos位置插入n个elem数据，无返回值
res.insert(res.end(),temp.begin(),temp.end()); // 合并两个vector
```



#### queue

```c++
queue<int> q1;

q.push(x) // 入队 将x元素接到队列的末端

q.pop(); // 出队，弹出队列的第一个元素，并不会返回元素的值；

q.front(); // 访问队首元素

q.back(); // 访问队尾元素

q.size(); // 访问队中的元素个数
```



#### map



#### set unordered_set

无重复，set是顺序的，unordered_set是无顺序的

```c++
// set里面判断存在不存在
set.count(target)
set.find(target) != set.end()

// set初始化
unordered_set<string> word_set(wordDict.begin(),wordDict.end());
```



#### 数组

```c++
// 一维数组初始化：
int value[100]; // 标准方式1 value[i]的值不定，没有初始化
int value[100] = {1,2}; // 标准方式2 value[0]和value[1]的值分别为1和2，而没有定义的value[i>1]，则初始化为0
int* value = new int[n]; // 指针方式 未初始化
delete []value;  // 一定不能忘了删除数组空间

// 二维数组初始化：
int value[9][9]; // 标准方式1 value[i][j]的值不定，没有初始化
int value[9][9] = {{1,1},{2}}； // 标准方式2 value[0][0,1]和value[1][0]的值初始化，其他初始化为0
int (*value)[n] = new int[m][n]; // 指针方式1
delete []value; // n必须为常量，调用直观。未初始化
int** value = new int* [m]; // 指针方式2
for(i) value[i] = new int[n];
for(i) delete []value[i];
delete []value; // 多次析构，存储麻烦，未初始化
int * value = new int[3][4]; // 指针方式3 数组的存储是按行存储的
delete []value; // 一定要进行内存释放，否则会造成内存泄露

// 多维数组初始化：
int * value = new int[m][3][4]; // 指针方式 只有第一维可以是变量，其他几维必须都是常量，否则会报错
delete []value; // 一定要进行内存释放，否则会造成内存泄露
```



#### pair



#### 位运算

