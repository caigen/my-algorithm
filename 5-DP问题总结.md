---
typora-root-url: img
---

## DP问题总结

### 1. 数字数组DP

#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

思路：两步DP

1. DP[i]定义：以**当前元素为结束**的最大子序和
2. 求所有的max

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> end_max = vector<int>(nums.size(), 0);

        end_max[0] = nums[0];
        int all_max = nums[0];

        for (int i = 1; i < nums.size(); ++i)
        {
            end_max[i] = max(end_max[i - 1] + nums[i], nums[i]);

            all_max = max(end_max[i], all_max);
        }

        return all_max;
    }
};
```

#### [152. 乘积最大子序列](https://leetcode-cn.com/problems/maximum-product-subarray/)

思路：包含正负整数，双DP记录（dp_max/dp_min），遇到负数时对dp[i-1]的max/min交换使用

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int max = INT_MIN;

        int dp_min = 1;
        int dp_max = 1;
        for (int i = 0; i < nums.size(); ++i)
        {
            if (nums[i] < 0)
            {
                std::swap(dp_max, dp_min);
            }
            
            dp_max = std::max(nums[i], dp_max * nums[i]);
            dp_min = std::min(nums[i], dp_min * nums[i]);

            max = std::max(max, dp_max);
        }

        return max;
    }
};
```

#### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

思路：target = n，先定出小于n的小目标

* dp[i]最初为n个1，逐步缩减

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp = vector<int>(n+1, 0);

        for (int i = 1; i <= n; ++i)
        {
            dp[i] = i;  //  1 + 1 + ...
            
            for(int j = 1; i - j * j >= 0; ++j)
            {
                dp[i] = min(dp[i], dp[i - j*j] + 1);
            }
        }

        return dp[n];
    }
};
```





#### [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

思路：LIS问题，涉及元素的跳跃比较，需要双重循环

1. DP[i]定义为：以**当前元素为结束**的最长上升子序列
2. DP[i]的循环更新：使用j < i的DP[j]更新DP[i]。

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() == 0)
        {
            return 0;
        }
        
        vector<int> dp;
        dp.push_back(1);
        int result = 1;
        for (int i = 1; i < nums.size(); ++i)
        {
            dp.push_back(1);
            for (int j = 0; j < i; ++j)
            {
                if (nums[j] < nums[i])
                {
                    dp[i] = max(dp[j] + 1, dp[i]);
                }
            }
            
            result = max(result, dp[i]);
        }
        
        return result;  
    }
};
```

#### [303. 区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)

思路：前缀和，**记忆子问题（动态规划思想）**

1. 左开右闭区间的求和方式：[0, j) = [0, i) + [i, j)
2. 前缀和数组包含一个前导零，方便云算

```c++
class NumArray {
public:
    vector<int> prefix_sum;
    NumArray(vector<int>& nums) {
        prefix_sum.push_back(0);
        for (int j = 0; j < nums.size(); ++j)
        {
            prefix_sum.push_back(nums[j] + prefix_sum[j]);
        }
    }
    
    int sumRange(int i, int j) {
        return prefix_sum[j + 1] - prefix_sum[i];
    }
};
```

#### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

思路：先达成比最终目标小的目标

1. DP[i]定义为：目标为i的最少硬币数量
2. 初始值的处理：目标为0，方案为0。
3. 非法值的处理：默认为大于合法值得MAX值，最终判断是否为合法值

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int max = amount + 1;
        vector<int> dp;
        dp.push_back(0);
        for (int i = 1; i <= amount; ++i)
        {
            dp.push_back(max);
        }
        
        for (int i = 1; i <= amount; ++i)
        {
            for (int j = 0;  j < coins.size(); j++)
            {
                if (i >= coins[j])
                {
                    dp[i] = min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        
        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```

#### [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

思路：i & (i - 1)的结果比i小， 位数少一个1   (popcount计算)

```C++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> result;
        result.push_back(0);
        for (int i = 1; i <= num; ++i)
        {
            int count = result[i & (i - 1)] + 1;
            result.push_back(count); 
        }

        return result;
    }

    int popcount(int value)
    {
        int count = 0;
        while (value)
        {
            value &= value - 1;
            count++;
        }
        
        return count;
    }
};
```

#### [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

思路：target = sum / 2

1. `DP[i][j]`定义为：

   * i：前i个候选元素
   * j:  能否组成j

2. 当加入新的元素时，利用已求得的解：

   ```c++
   dp[i][j] = dp[i - 1][j] || dp[i - 1][j - value];
   ```

```c++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        if (nums.size() <= 1)
        {
            return false;
        }

        int sum = 0;
        for (int i = 0; i < nums.size(); ++i)
        {
            sum += nums[i];
        }

        if (sum % 2 == 1)
        {
            return false;
        }

        int target = sum / 2;

        vector<vector<bool>> dp = vector<vector<bool>>(
            nums.size(), vector<bool>(target + 1, false));

        // 第一行
        dp[0][nums[0]] = true;

        for (int i = 1; i < nums.size(); ++i)
        {
            int value = nums[i];
            for (int j = 1; j <= target; ++j)
            {
                // 放入
                if (j == value)
                {
                    dp[i][j] = true;
                }
                else if (j > value)
                {
                    dp[i][j] = dp[i - 1][j] ||  // 已求得解
                        dp[i - 1][j - value];
                }
            }

            if (dp[i][target])
            {
                return true;
            }
        }

        return dp[nums.size() - 1][target];
    }
};
```

#### [494. 目标和](https://leetcode-cn.com/problems/target-sum/)

思路：target

1. `DP[i][j]`定义为：
   * i：前i个候选元素
   * j:  组成j的方案数目
2. 当加入新的元素时，将不同的方案进行叠加：

```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int S) {
        if (S > 1000)
        {
            return 0;
        }

        vector<vector<int>> dp = vector<vector<int>>(
            nums.size(), vector<int>(2001, 0));
        
        dp[0][-nums[0] + 1000] = 1;
        dp[0][+nums[0] + 1000] += 1;    // !!当nums[0]为零时

        for (int i = 1; i < nums.size(); ++i)
        {
            for (int j = -1000; j <= 1000; ++j)
            {
                if (j - nums[i] + 1000 >= 0 && dp[i - 1][j - nums[i] + 1000] > 0)
                {
                    dp[i][j + 1000] += dp[i - 1][j - nums[i] + 1000];
                }

                if (j + nums[i] + 1000 < 2001 && dp[i - 1][j + nums[i] + 1000] > 0)
                {
                    dp[i][j + 1000] += dp[i - 1][j + nums[i] + 1000];
                }
            }
        }
        
        return dp[nums.size() - 1][S + 1000];
    }
};
```



### 2. 字符串DP

#### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

思路：二维DP，上三角矩阵计算

1. `DP[i][j]`从i到j是否为回文
2. 初始：单字符、双字符
3. 使用上三角按列填充

![](/longest_palindrome.JPG)



```c++
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.length() <= 1)
        {
            return s;
        }

        vector<vector<bool>> dp = vector<vector<bool>>(s.length(), 
            vector<bool>(s.length(), false));

        int max = 0;
        int left = 0;

        for (int i = 0; i < s.length(); ++i)
        {
            dp[i][i] = true;
            if (1 > max)
            {
                max = 1;
                left = i;
            }
        }

        for (int i = 0; i < s.length() - 1; ++i)
        {
            if (s[i] == s[i+1])
            {
                dp[i][i+1] = true;

                if (2 > max)
                {
                    max = 2;
                    left = i;
                }
            }
        }

        for (int j = 2; j < s.length(); ++j)
        {
            for (int i = 0; i <= j - 2; ++i)
            {
                dp[i][j] = (dp[i + 1][j - 1] && s[i] == s[j]);

                if (dp[i][j] && j - i + 1 > max)
                {
                    max = j - i + 1;
                    left = i;
                    cout<< max << endl;
                }
            }
        }
        
        return s.substr(left, max);
    }
};
```

#### [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

思路：哈希表记录单词列表，先解决子串是否可以被拆分

1. DP[i]定义为：[0, i) 可以被拆分
2. 使用j < i的DP[j]更新DP[i]。

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {        
        unordered_map<string, bool> words;
        for (int i = 0; i< wordDict.size(); ++i)
        {
            words[wordDict[i]] = true;
        }

        vector<bool> dp = vector<bool>(s.size() + 1, false);
        dp[0] = true;   // dp[i]: [0, i)    dp[n]: [0, n)
        for (int i = 1; i < dp.size(); ++i)
        {
            for (int j = 0; j < i; ++j)
            {
                // [0, j) + [j, i)
                if (dp[j])
                {
                    string right = s.substr(j, i - j);
                    if (words.count(right))
                    {
                        dp[i] = true;
                        break;  // 已经找到一个解
                    }
                }
            }
        }

        return dp[s.size()];
    }
};
```

