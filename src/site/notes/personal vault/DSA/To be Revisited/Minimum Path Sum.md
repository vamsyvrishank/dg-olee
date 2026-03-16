---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/minimum-path-sum/"}
---

#### Question:
Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

**Input:** grid = \[[1,3,1],[1,5,1],[4,2,1\|1,3,1],[1,5,1],[4,2,1]]
**Output:** 7
**Explanation:** Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.


### Solution:

##### 1. Recursion and Memoized
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int[][] dp = new int[n][m];

        for(int[] a: dp){
            Arrays.fill(a, -1);
        }
        return minPathHelper(grid, 0, 0, n - 1, m - 1, dp);
    }

    public int minPathHelper(int[][] grid, int sr, int sc, int dr, int dc, int[][] dp){
        if(sr > dr || sc > dc){
            return Integer.MAX_VALUE;
        }
        if(sr == dr && sc == dc){
            return grid[sr][sc];
        }

        if(dp[sr][sc] != -1){
            return dp[sr][sc];
        }

        int down = minPathHelper(grid, sr + 1, sc, dr, dc, dp);
        int right = minPathHelper(grid, sr, sc + 1, dr, dc, dp);

        return dp[sr][sc] = grid[sr][sc] + Math.min(down, right);
    }
}
```

##### 2. Tabulation
> Same code converted to tabulation (Bottom Up)
###### Time and Space Complexity

- **Time Complexity**: 𝑂(𝑛×𝑚)
    - The solution iterates through each cell in the grid exactly once.
- **Space Complexity**: 𝑂(𝑛×𝑚) - dp size
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int[][] dp = new int[n][m];

        dp[n-1][m-1] = grid[n-1][m-1];

        for(int i = n - 2; i >= 0; i--){
            dp[i][m - 1] = grid[i][m - 1] + dp[i + 1][m - 1];
        }

        for(int j = m - 2; j >= 0; j--){
            dp[n - 1][j] = grid[n - 1][j] + dp[n - 1][j + 1];
        }

        for(int i = n - 2; i >= 0; i--){
            for(int j = m - 2; j >= 0; j--){
                int down = dp[i + 1][j];
                int right = dp[i][j + 1];
                dp[i][j] = grid[i][j] + Math.min(down, right);
            }
        }

        return dp[0][0];
        
    }

}
```

##### 3. Space optimized
1. **Only Two Rows Needed**:
    - In a grid-based dynamic programming problem where the solution for each cell depends only on the cells directly below and to the right, we only need the current row and the previous row for computation. This allows us to reduce the 2D DP array to two 1D arrays.
2. **Overwriting Current Row**:
    - By updating the `curr` array for the current row and then copying it to `prev`, we can reuse the space efficiently. The `prev` array holds the results of the previous row, which are required for the next row's computations.
###### Time and Space Complexity

- **Time Complexity**: 𝑂(𝑛×𝑚)
    - The solution iterates through each cell in the grid exactly once.
- **Space Complexity**: 𝑂(𝑚)
    - The space is optimized to use two 1D arrays of size `m`, where `m` is the number of columns in the grid.
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int[] prev = new int[m];
        int[] curr = new int[m];
		
		// Initialize the last row
        prev[m - 1] = grid[n - 1][m - 1];
        for(int j = m - 2; j >= 0; j--){
            prev[j] = grid[n - 1][j] + prev[j + 1];
        }

		// Fill the dp array from bottom to top (as we only need prev ke bottom ki value and same row k curr k right ki value).
        for(int i = n - 2; i >= 0; i--){
            
            curr[m - 1] = grid[i][m - 1] + prev[m - 1]; // Initial value for the last column in the current row
            
            for(int j = m - 2; j >= 0; j--){
                int down = prev[j];
                int right = curr[j + 1];
                curr[j] = grid[i][j] + Math.min(down, right);
            }
            prev = curr;
        }
        return prev[0];
    }
}
```