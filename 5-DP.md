## 动态规划(dynamic programming)

参考地址：[CS 97SI: Introduction to Programming Contests](https://web.stanford.edu/class/cs97si/04-dynamic-programming.pdf)



### DP：将问题**拆分**成**更简单**的子问题去解决

解决DP问题的步骤如下（与递归有类似地方）：

1. **定义**要求解的问题或子问题
2. 写下子问题的**递推关系**
3. 解决最基本的情况（base cases）

注意，每一步都非常重要，尽量清晰化。



### 1维DP示例

问题：求n有多少方式可以写成1,3,4的和。 

解决过程：

1. 定义：$$D_n$$为将n写成1,3,4的方式数量
2. 递推：$$D_n = D_{n-1} + D_{n-3} + D_{n-4}$$
3. 基本情况：$$D_0 = 1, D1 = 1, D2 = 1, D3 = 2$$

```c++
D[0] = D[1] = D[2] = 1; D[3] = 2;
for(i = 4; i <= n; i++)
	D[i] = D[i-1] + D[i-3] + D[i-4];
```

### 2维DP示例

问题：最长公共子序列(longest common subsequence, LCS)的长度

解决过程：

1. 定义：$$D_{ij}$$是$x_{1..i}$和$y_{1..j}$的最长公共子序列的长度
2. 递归：
   * 如果$x_i = y_i$, $$D_{ij} = D_{i-1,j-1} + 1$$
   * 否则 $$D_{ij} = max(D_{i-1,j}, D_{i, j-1})$$
3. 基本情况：$D_{i0} = D_{0j} = 0$ ，相当于空串的处理。

```c++
for(i = 0; i <= n; i++) D[i][0] = 0;
for(j = 0; j <= m; j++) D[0][j] = 0;
for(i = 1; i <= n; i++) {
	for(j = 1; j <= m; j++) {
		if(x[i] == y[j])   // c++实现需要调整下标或添加空白字符
			D[i][j] = D[i-1][j-1] + 1;
		else
			D[i][j] = max(D[i-1][j], D[i][j-1]);
	}
}
```

