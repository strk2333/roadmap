# C++











Template

```
// 函数模板
// class == typename
template <class 形参名，typename 形参名，......> 返回类型 函数名(参数列表)
{
    函数体
}

template <class T> void swap(T& a, T& b){}

// 类模板
template<class 形参名，class 形参名，…>   class 类名
{ ... };

template<class T> class A{public: T a; T b; T hy(T c, T &d);};

```











## 问题

什么是接口继承？
指针的强制类型转换有几种？
指针的指针怎么用？
指针和引用有什么联系？
const指针和指向const的指针有什么不同？





```
vector<vector<int>> aa;
vector<int> a;
aa.push_back(a);
a.push_back(1);
aa[0][0]; // !!!!
```

