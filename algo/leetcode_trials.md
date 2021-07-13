# LeetCode



```
// 只对父节点进行大小判断，会漏掉祖先节点的限制，需要记住最大值和最小值的限制。
// 左子树 要小于父节点，更新最大值，继承最小值。
// 右子树 要大于父节点，更新最小值，继承最大值。
boolean isValidBST(TreeNode root) {
    return isValidBST(root, null, null);
}

/* 限定以 root 为根的子树节点必须满足 max.val > root.val > min.val */
boolean isValidBST(TreeNode root, TreeNode min, TreeNode max) {
    // base case
    if (root == null) return true;
    // 若 root.val 不符合 max 和 min 的限制，说明不是合法 BST
    if (min != null && root.val <= min.val) return false;
    if (max != null && root.val >= max.val) return false;
    // 限定左子树的最大值是 root.val，右子树的最小值是 root.val
    return isValidBST(root.left, min, root) 
        && isValidBST(root.right, root, max);
}
```





### 剑指 Offer 32 - III. 从上到下打印二叉树 III

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

```
建两个栈
来回倒

或者使用队列 （未实现）

BFS 使用队列
DFS 使用栈
```



### 剑指 Offer 33. 二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

```
最后一个节点为根节点，分割左右孩子
```







## 1145. 二叉树着色游戏

```
要必赢需要把第一步放在能堵死路线的位置。这个位置只有三个，根，左，右。根的不需要计算，可以根据 n 和左右算出。
```





```
背包问题
给你一个可装载重量为 `W` 的背包和 `N` 个物品，每个物品有重量和价值两个属性。其中第 `i` 个物品的重量为 `wt[i]`，价值为 `val[i]`，现在让你用这个背包装物品，最多能装的价值是多少？
// i 物品数量
// j 背包物品总量
dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - nums[i - 1]]);
// 416 分割等和子集
sum % 2 == 1 ? return false : sum /= 2;
vector<vector<int>> dp(n + 1, vector<int>(sum + 1, 0))
for (int i = 0; i < nums.size(); i++) {
	for (int j = 0; j < sum; j++) {
        if (j - nums[i - 1] > 0) {
			// 还有放下物品的空间
            // 上一次就已经放了 j 单位的物品
            // 或者这次正好能放 j 单位的物品
			dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - nums[i - 1]]);
        } else {
            dp[i][j] = dp[i - 1][j];
        }
	}
}
return dp[nums.size()][sum] != 0;

bool canPartition(vector<int>& nums) {
    int sum = 0, n = nums.size();
    for (int num : nums) sum += num;
    if (sum % 2 != 0) return false;
    sum = sum / 2;
    vector<bool> dp(sum + 1, false);
    // base case
    dp[0] = true;

    for (int i = 0; i < n; i++) 
        for (int j = sum; j >= 0; j--) 
            if (j - nums[i] >= 0)
                dp[j] = dp[j] || dp[j - nums[i]];

    return dp[sum];
}

```

