---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/number-of-enclaves/"}
---


You are given an `m x n` binary matrix `grid`, where `0` represents a sea cell and `1` represents a land cell.

A **move** consists of walking from one land cell to another adjacent (**4-directionally**) land cell or walking off the boundary of the `grid`.

Return _the number of land cells in_ `grid` _for which we cannot walk off the boundary of the grid in any number of **moves**_.

**Example 1:**
![[Pasted image 20240625000415.png \| 500]]


**Input:** grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0\|0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
**Output:** 3
**Explanation:** There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.

**Example 2:**

![[Pasted image 20240625000428.png \| 500]]


**Input:** grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0\|0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
**Output:** 0
**Explanation:** All 1s are either on the boundary or can reach the boundary.

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 500`
- `grid[i][j]` is either `0` or `1`.

```cpp
class Solution {
public:
    void dfs(int row , int col , vector<vector<int>> &vis , vector<vector<int>>&grid, int m, int n){

        vis[row][col] = 1;
        vector<pair<int,int>> directions = {{1,0},{-1,0},{0,1},{0,-1}};

        for(auto d : directions){
            int nrow = row + d.first;
            int ncol = col + d.second;

            if(nrow>=0 and ncol >=0 and nrow<m and ncol<n and grid[nrow][ncol]==1){
                if(!vis[nrow][ncol]){
                    dfs(nrow,ncol,vis,grid,m,n);
                }
            }
        }
    }
    int numEnclaves(vector<vector<int>>& grid) {

        int m = grid.size();
        int n = grid[0].size();
        int count = 0;

        vector<vector<int>> vis(m,vector<int>(n,0));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if( (i==0 || i==m-1) and grid[i][j]==1){
                    dfs(i,j,vis,grid,m,n);
                }
                else if((j==0 || j==n-1) and grid[i][j]==1){
                    dfs(i,j,vis,grid,m,n);
                }
            }
        }

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1 and vis[i][j]==0){
                    count++;
                }
            }
        }

        return count;
        
    }
};
```