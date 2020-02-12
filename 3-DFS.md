## DFS（深度优先搜索）

### DFS的递归实现套路

1. 结束条件 （即**递归**的套路）

2. 一个循环，扩展结点，进行递归

3. 搜索结果的记录

4. 搜索状态的记录，避免重复搜索，即closed list



[二叉树是否有路径和等于给定值](https://leetcode-cn.com/problems/path-sum/)

* 结束条件叶子结点
* 手动循环扩展左右结点
* 搜索结果用返回值记录
* 无环无重复

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL)
        {
            return false;
        }
        
        if (root->left == NULL 
            && root->right == NULL 
            && root->val == sum)
        {
            return true;
        }
        
        // visit the node
        sum -= root->val;
        
        // visit all linked node
        return hasPathSum(root->left, sum) || hasPathSum(root->right, sum);
    }
};
```



[二叉树相同判定](https://leetcode-cn.com/problems/same-tree/)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == NULL && q == NULL)
        {
            return true;
        }
        if (p == NULL || q == NULL)
        {
            return false;
        }
        
        if (p->val != q->val)
        {
            return false;
        }
        
        return isSameTree(p->left, q->left)
            && isSameTree(p->right, q->right);
        
    }
};
```



[二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL)
        {
            return 0;
        }
        
        int left = maxDepth(root->left);
        int right = maxDepth(root->right);
        
        return left > right ? left + 1 : right + 1;
    }
};
```

