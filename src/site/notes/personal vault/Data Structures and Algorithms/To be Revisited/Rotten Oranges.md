---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/rotten-oranges/"}
---


###### Question:
You are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

**Input:** grid = \[[2,1,1],[1,1,0],[0,1,1\|2,1,1],[1,1,0],[0,1,1]]
**Output:** 4

**Example 2:**

**Input:** grid = \[[2,1,1],[0,1,1],[1,0,1\|2,1,1],[0,1,1],[1,0,1]]
**Output:** -1
**Explanation:** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

**Input:** grid = \[[0,2\|0,2]]
**Output:** 0
**Explanation:** Since there are already no fresh oranges at minute 0, the answer is just 0.

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` is `0`, `1`, or `2`.



###### Solution:

* Each rotten orange, rottens the neighboring oranges "simultaneously". 
* Simple BFS solution.

Since we need to do it simultaneously ,  we have to traverse in BFS way , we cannot do it in DFS otherwise it will keep going inside one branch.

```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();
        int time =0;

        vector<vector<int>> vis(n,vector<int>(m,0));
        queue< pair<pair<int,int> , int >> q;

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(grid[i][j]==2){
                    // inital time t = 0;
                    q.push({{i,j},0});
                    vis[i][j] = 2;
                }
            }
        }

        vector<pair<int,int>> directions = {{1,0},{-1,0},{0,1},{0,-1}};
        
        while(!q.empty()){
            auto temp = q.front();

            int curr_row = temp.first.first;
            int curr_col = temp.first.second;
            int curr_time = temp.second;
            time = max(time, curr_time);
            q.pop();

            for(auto d : directions){
                int neigh_row = curr_row + d.first;
                int neigh_col = curr_col + d.second;

                if(neigh_row >=0 and neigh_col >=0 and neigh_row<n and neigh_col < m &&
                grid[neigh_row][neigh_col]==1 && vis[neigh_row][neigh_col]==0){
                    vis[neigh_row][neigh_col] = 2;
                    q.push({{neigh_row,neigh_col}, curr_time+1});
                }
            }
        }

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(vis[i][j]!=2 and grid[i][j]==1){
                    return -1;
                }
            }
        }
        return time;
    }
};
```