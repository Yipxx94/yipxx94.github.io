# 关于常见的三种递归模型


<!--more-->


## 1. 递归实现指数型枚举

[78. 子集](https://leetcode.cn/problems/subsets/)
每个数有两种情况，选或不选，总方案数为$2^n$。

### 代码

```cpp
class Solution {
public:
    int n;
    vector<int> path;
    vector<vector<int>> res;
    vector<vector<int>> subsets(vector<int>& nums) {
        n = nums.size();
        dfs(nums, 0);
        return res;
    }
    void dfs(vector<int>& nums, int u)
    {
        if (u == n)
        {
            res.push_back(path);
            return;
        }

        // 不选当前数
        dfs(nums, u + 1);
        
        // 选当前数
        path.push_back(nums[u]);
        dfs(nums, u + 1);
        path.pop_back();    // 回溯
    }
};
```

## 2. 递归实现排列型枚举

[46. 全排列](https://leetcode.cn/problems/permutations/)

### 代码

```cpp
class Solution {
public:
    int n;
    vector<int> nums;
    vector<int> path;
    vector<bool> st;
    vector<vector<int>> res;
    vector<vector<int>> permute(vector<int>& _nums) {
        nums = _nums;
        n = nums.size();
        st.resize(n);
        dfs(0);
        return res;
    }
    void dfs(int u)
    {
        if (u == n)
        {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < n; i ++ )
        {
            if (!st[i])    // 如果当前数没使用过
            {
                st[i] = true;
                path.push_back(nums[i]);
                dfs(u + 1);
                path.pop_back();
                st[i] = false;
            }
        }
    }
};
```

## 3. 递归实现组合型枚举

[77. 组合](https://leetcode.cn/problems/combinations/)
从$n$个数里选$m$个数，且含有相同数字的集合视为同一个集合。

### 代码

```cpp
class Solution {
public:
    int n, k;
    vector<int> path;
    vector<vector<int>> res;
    vector<vector<int>> combine(int _n, int _k) {
        n = _n, k = _k;
        dfs(1, 1);
        return res;
    }
    void dfs(int u, int idx)
    {
        if (n + u - idx < k)
            return;    // 剪枝，当把后面的数全选上也不够k个时，直接退出
        if (u > k)
        {
            res.push_back(path);
            return;
        }
        for (int i = idx; i <= n; i ++ )
        {
            path.push_back(i);
            dfs(u + 1, i + 1);
            path.pop_back();
        }
    }
};
```

