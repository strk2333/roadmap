## Algorithm
### 动态规划
#### 组成
1. 确定状态  

   - 两个意识 (去掉 a<sub>k</sub>后, n-a<sub>k</sub>也是最优解)
     - 最后一步 a<sub>k</sub>

     - 子问题 求n-a<sub>k</sub>的最优解

   - 设置状态 f(x)=x

     - x=n
     - x=n-a<sub>k</sub>    
     - f(n)=f(n-a<sub>k</sub>) + 1

   -  a<sub>k</sub>的值

     - min{f(n-a<sub>k</sub>) + 1}    a<sub>k</sub>取所有可能值

   -  重复计算

     - 保存计算结果, 改变计算顺序

2. 2

3. 转移方程

4. 3

5. 4