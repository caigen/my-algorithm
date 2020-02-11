## BFS（广度优先搜索）

### BFS的实现套路

1. 一个队列，用于扩展节点搜索，该队列为open queue
2. 一个搜索起点，放入队列
3. 一个循环，用于搜索进行
4. 搜索结果的记录
5. 搜索状态的记录，避免重复搜索，即closed list



[二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

队列queue，起点root，循环while，结果level。树的搜索不会重复不记录状态。

* 由于广度优先的特征，搜索到的第一个结果及满足问题要求。

* 注意：二叉树层序遍历搜索中，由于队列实时变化，层内的循环需先记录下该层大小。

`int n = q.size()`

```c++
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
    int minDepth(TreeNode* root) {
        if (root == NULL)
        {
            return 0;
        }
        
        int level = 0;
        
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty())
        {
            level++;
            
            // level size
            int n = q.size();
            for (int i = 0; i < n; ++i)
            {
                TreeNode* front = q.front();
                q.pop();
                
                // a leaf is found
                if (front->left == NULL && front->right == NULL)
                {
                    return level;
                }
                
                if (front->left != NULL)
                {
                    q.push(front->left);
                }
                if (front->right != NULL)
                {
                    q.push(front->right);
                }
            }
        }
        
        // can't reach
        return level;
    }
};
```



### BFS的识别套路

1. 问题的核心是一个图或者树，有结点及连接的特性。
2. 通过扩展结点，可能在搜索的过程中得到结果。
3. 当前结点的**所有扩展结点，其属性与当前结点相关**，且具有相似计算方式。
4. 广度扩展结点，能够将结点进行归聚。（洪泛填充特性）



[二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

结点及其连接，搜索到中间层即可获得结果，扩展结点的深度属性与当前结点相关且同层相同。



[二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

层的深度与上层相关，层是广度扩展。

```c++
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> queue;
        if (root != NULL)
        {
            queue.push(root); 
        }
        
        vector<vector<int>> result;
        while (!queue.empty())
        {
            int n = queue.size();
            vector<int> level;
            for (int i = 0; i < n; ++i)
            {
                TreeNode* front = queue.front();
                queue.pop();
                if (front != NULL)
                {
                    if (front->left != NULL)
                    {
                        queue.push(front->left);
                    }
                    if (front->right != NULL)
                    {
                        queue.push(front->right);
                    }
                    level.push_back(front->val);
                }
            }
            
            result.push_back(level);
        }
        
        return result;
    }
};
```



[二叉树左下节点值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

树的层序遍历，即BFS的每次扩展，记录第一个值。

```c++
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
    int findBottomLeftValue(TreeNode* root) {
        int res = 0;
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty())
        {
            // current size
            int n = q.size();
            for (int i = 0; i < n; ++i)
            {
                TreeNode* node = q.front();
                q.pop();
                if (i == 0)
                {
                    res = node->val;
                }
                if (node->left)
                {
                    q.push(node->left);
                }
                if (node->right)
                {
                    q.push(node->right);
                }
            }
        }

        return res;
    }
};
```



[小岛的数量](https://leetcode-cn.com/problems/number-of-islands/)

每个小岛通过一次BFS能够将该岛的结点归聚起来。

visited即用于岛内BFS，避免重复，也同时用于总体的遍历，避免重复。

* BFS + 循环遍历

```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int row = grid.size();
        if (row == 0)
        {
            return 0;
        }
        int col = grid[0].size();
        if (col == 0)
        {
            return 0;
        }
        
        vector<vector<bool>> visited(row, vector<bool>(col, false));

        int res = 0;
        for (int i = 0; i < row; i++)
        {
            for (int j = 0; j < col; j++)
            {
                if (visited[i][j] == false && grid[i][j] == '1')
                {
                    bfs(grid, visited, i, j, row, col);
                    res = res + 1;
                }
            }
        }

        return res;        
    }

    void bfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, 
        int i, int j, int row, int col)
    {
        if (i >= 0 && j >= 0 && i < row && j < col)
        {
            if (grid[i][j] == '1' && visited[i][j] == false)
            {
                visited[i][j] = true;
                bfs(grid, visited, i + 1, j, row, col);
                bfs(grid, visited, i - 1, j, row, col);
                bfs(grid, visited, i, j + 1, row, col);
                bfs(grid, visited, i, j - 1, row, col);
            }
        }
    }
};
```



### BFS实现技巧：std::pair用于二维搜索

BFS能够应用于二维搜索问题，此类问题由于利用二维建模，可以利用std::pair方便的进行结点扩展，使实现清晰简明。

1. std::pair类型定义：std::pair<int, int>
2. std::pair值构造：std::make_pair(first, second)
3. std::pair赋值：=
4. std::pair访问：.first/.second

BFS二维搜索的扩展数组：

```cpp
std::pair<int, int> dir[4] = {
    std::make_pair(-1, 0),
    std::make_pair( 1, 0),
    std::make_pair(0, -1),
    std::make_pair(0,  1),
};
```



BFS二维搜索的扩展方式，经过建模后，同层序遍历：

```cpp
while(!q.empty())
{
    for (int i = 0; i < 4; ++i)
    {
        expand!
    }
}
```



[01矩阵：计算矩阵中每格到0的最近距离](https://leetcode-cn.com/problems/01-matrix/)

先将所有0格放入队列，能够利用距离的比较，避免一些无效的操作。

* 非0格设置为最大距离

* 遍历所有的0格，且从0格洪范填充， 较小的距离保留



```c++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        queue<std::pair<int,int>> q;
        for (int i = 0; i < matrix.size(); ++i)
        {
            for (int j = 0; j < matrix[0].size(); ++j)
            {
                if (matrix[i][j] == 0)
                {
                    q.push(std::make_pair(i, j));
                }
                else
                {
                    matrix[i][j] = INT_MAX;
                }
            }
        }
        
        std::pair<int, int> dir[4] = {
            std::make_pair(-1, 0),
            std::make_pair( 1, 0),
            std::make_pair(0, -1),
            std::make_pair(0,  1),
        };
        
        while(!q.empty())
        {
            std::pair<int, int> current = q.front();
            q.pop();
            
            for (int index = 0; index < 4; ++index)
            {
                int x = current.first + dir[index].first;
                int y = current.second + dir[index].second;
                int value = matrix[current.first][current.second] + 1;
                if (x >= 0 && y >= 0 
                    && x < matrix.size() 
                    && y < matrix[0].size())
                {
                    if (matrix[x][y] > value)
                    {
                        matrix[x][y] = value;
                        q.push(std::make_pair(x, y));
                    }
                }
            }
        }
        
        return matrix;
    }
};
```



### BFS实现技巧：多起点洪范

在一个问题中，包含的子问题是BFS，且具有重复性／同步性。此时实现BFS不再是“一个起点“，而是"多起点"，多个起点相对等价，此时需将多个起点放入BFS队列中，再进行While循环套路。即：

- 一个队列
- 放入多个起点
- 循环

[01矩阵，求离1最远的0，到1的距离。](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)

```c++
class Solution {
public:
    int maxDistance(vector<vector<int>>& grid) {
        if (grid.size() == 0)
        {
            return -1;
        }
        
        vector<vector<int>> distance(grid.size(),
            vector<int>(grid[0].size(), INT_MAX));
        
        queue<std::pair<int, int>> q;
        bool hasWater = false;
        for (int i = 0; i < grid.size(); ++i)
        {
            for (int j = 0; j < grid[0].size(); ++j)
            {
                if (grid[i][j] == 1)
                {
                    distance[i][j] = 0;
                    q.push(std::make_pair(i, j));                   
                }
                else
                {
                    hasWater = true;
                }
            }
        }
        
        if (q.empty() || !hasWater)
        {
            return -1;
        }
        
        std::pair<int, int> dir[4] = {
            std::make_pair(-1, 0),
            std::make_pair( 1, 0),
            std::make_pair( 0, -1),
            std::make_pair( 0,  1)
        };
        while (!q.empty())
        {
            std::pair<int, int> current = q.front();
            q.pop();
            for (int index = 0; index < 4; ++index)
            {
                int x = current.first + dir[index].first;
                int y = current.second + dir[index].second;
                int value = distance[current.first][current.second] + 1;
                if (x >= 0 && y >= 0
                   && x < grid.size()
                   && y < grid[0].size()
                   && grid[x][y] == 0)
                {
                    if (distance[x][y] > value)
                    {
                        distance[x][y] = value;
                        q.push(std::make_pair(x, y));
                    }
                }
            }
        }
        
        int result = -1;
        for (int i = 0; i < distance.size(); ++i)
        {
            for (int j = 0; j < distance[0].size(); ++j)
            {
                if (distance[i][j] > result)
                {
                    result = distance[i][j];
                }
            }
        }
        
        return result;
    }
};
```

