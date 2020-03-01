---
typora-root-url: ./
---

## 回溯法



回溯法对解空间进行DFS搜索（DFS + 状态维护）

* DFS：深度优先以构造一个完整的解
* 状态维护：回溯到干净的状态
* 解的扩展
  * 树的边：选择的扩展
  * 树的顶点：剩余的可选扩展



回溯法实现：

* 结束状态

* for循环扩展

  * **添加**当前扩展
  * 递归
  * **清理**当前扩展  // 回溯



### 全排列问题

* 回溯法生成全排列的解空间：

<img src="/img/permutation.jpg" style="zoom:50%;" />

* 全排列问题也可以使用递归求解：当求出n-1规模的问题时，插入生成n规模的全排列

```c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        vector<int> current;

        backtracking(current, result, nums);

        return result;
    }

    void backtracking(vector<int>& current, 
        vector<vector<int>>& result, 
        vector<int>& nums)
    {
        // 已无数据
        if (nums.size() == 0)
        {
            result.push_back(current);
            return;
        }

        // 扩展分支：每条边一个数字
        for (int i = 0; i < nums.size(); ++i)
        {
            current.push_back(nums[i]);

            vector<int> less = nums;
            less.erase(less.begin() + i);

            backtracking(current, result, less);

            current.pop_back();  // 回溯
        }
    }
};
```

### 组合问题（子集问题）

回溯法生成组合问题：

	* 任意k个元素的组合
	* k确定时，在n个数字中选择k个数字

<img src="/img/combination.jpg" style="zoom:50%;" />

* 组合问题也可以使用复制扩展法求解：复制已有子集，并添加一个元素生成扩展集。	

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;

        for (int i = 0; i <= nums.size(); ++i)
        {
            vector<int> current;
            backtracking(i, nums, result, current);
        }

        return result;
    }

    // 含有k个元素的子集
    void backtracking(int k, vector<int>& nums, 
        vector<vector<int>>& result, vector<int>& current)
    {
        if (current.size() == k)
        {
            result.push_back(current);
            return;
        }

        // n种扩展方式
        for (int i = 0; i < nums.size(); ++i)
        {
            if (current.size() > 0 && current[current.size() - 1] > nums[i])
            {
                // 重复剪枝
                continue;
            }

            current.push_back(nums[i]);
            
            vector<int> less = nums;
            less.erase(less.begin() + i);

            backtracking(k, less, result, current);

            current.pop_back(); // 回溯
        }
    }
};
```




