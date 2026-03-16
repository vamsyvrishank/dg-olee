---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/surrounded-regions/"}
---



You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: The region is surrounded with `'X'` cells if you can **connect the region** with `'X'` cells and none of the region cells are on the edge of the `board`.

A **surrounded region is captured** by replacing all `'O'`s with `'X'`s in the input matrix `board`.

**Example 1:**

**Input:** board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"\|"X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

**Output:** [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"\|"X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

**Explanation:**

![[Pasted image 20240624234600.png \| 500]]


In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

**Example 2:**

**Input:** board = [["X"\|"X"]]

**Output:** [["X"\|"X"]]

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is `'X'` or `'O'`.


```cpp
class Solution {
public:
    void dfs(int row , int col , vector<vector<int>> &vis , int m , int n, vector<vector<char>> &board){
        vis[row][col] = 1;
        
        vector<pair<int,int>> directions = {{1,0},{-1,0},{0,1},{0,-1}};
        for(auto d : directions ){
            int nrow = row + d.first;
            int ncol = col + d.second;

            if(nrow >=0 and ncol >=0 and nrow <m and ncol <n and board[nrow][ncol]=='O'){
                if(!vis[nrow][ncol]){
                    dfs(nrow,ncol,vis,m,n,board);
                }
            }
        }
    }
    void solve(vector<vector<char>>& board) {

        //traverse the board in 4 directions only around the edges
        int m = board.size();
        int n = board[0].size();

        vector<vector<int>> vis(m,vector<int>(n,0));

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if((i==0 || i==m-1) and board[i][j]=='O'){
                    dfs(i,j,vis,m,n,board);
                }
                else if((j==0 || j==n-1) and board[i][j]=='O'){
                    dfs(i,j,vis,m,n,board);
                }
            }
        }

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]=='O' and vis[i][j]==0){
                    board[i][j] = 'X';
                }
            }
        } 
    }
};
```