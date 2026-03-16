---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/01-matrix/"}
---


###### Question:
Given an `m x n` binary matrix `mat`, return _the distance of the nearest_ `0` _for each cell_.
The distance between two adjacent cells is `1`.

**Example 1:**



**Input:** mat = \[[0,0,0],[0,1,0],[0,0,0\|0,0,0],[0,1,0],[0,0,0]]
**Output:** \[[0,0,0],[0,1,0],[0,0,0\|0,0,0],[0,1,0],[0,0,0]]
![[Pasted image 20240624230403.png \| 500]]


**Example 2:**

![[Pasted image 20240624230424.png \| 500]]


**Input:** mat = \[[0,0,0],[0,1,0],[1,1,1\|0,0,0],[0,1,0],[1,1,1]]
**Output:** \[[0,0,0],[0,1,0],[1,2,1\|0,0,0],[0,1,0],[1,2,1]]

**Constraints:**

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 104`
- `1 <= m * n <= 104`
- `mat[i][j]` is either `0` or `1`.
- There is at least one `0` in `mat`.
###### Solution:
- Hum 0 se nearest 1's pe jaate hai from BFS. Also, hum steps leke chalte hai apne saath, so as soon as we encounter 1 we update it's distance from 
  the nearest 0 by assigning it currStep + 1;
-  The BFS starts from the cells with value 0 and expands to their neighboring cells.
	- We initialize the queue with the cells that have a value of 0.
	- These cells serve as the starting points for the BFS traversal.
	- The distance of cells with value 0 is already known to be 0, so we mark them as visited and enqueue them with a step value of 0.
- The `visited` array serves two purposes:
	- Avoiding revisiting cells: It prevents processing the same cell multiple times and avoids infinite loops in case of cycles.
	- Ensuring correct distance updates: It guarantees that each cell gets assigned the shortest distance to its nearest 0 cell by skipping the visited cells.
	- **IF it's visited means that `1` is already assigned the shortest possible path.**
-  If a neighboring cell has a value of 1 and hasn't been visited before, we update its distance by adding 1 to the current step value and enqueue it for further exploration.
- The BFS starts from the cells with value 0 and expands to their neighboring cells, gradually updating the distances as it explores further.



> [!NOTE] Hint
> This is following similar pattern as Rotten oranges , there we keep track of time , here it is steps/distance and we dont need to run the for loop for bfs because for every node we are going in all directions 


```cpp

class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {

        int n = mat.size();
        int m = mat[0].size();

        vector<vector<int>> vis(n,vector<int>(m,0));
        vector<vector<int>> ans(n,vector<int>(m,0));
        queue<pair< pair<int,int> ,int>> q;

        for(int i=0;i<n;i++){
            for(int j =0;j<m;j++){
                if(mat[i][j]==0){
                    vis[i][j] = 1;
                    q.push({{i,j},0});
                }
            }
        }

        vector<pair<int,int>> directions = {{1,0},{-1,0},{0,1},{0,-1}};
        while(!q.empty()){
            auto temp = q.front();
            q.pop();
            int curr_row = temp.first.first;
            int curr_col = temp.first.second;
            int curr_dist = temp.second;

            ans[curr_row][curr_col] = curr_dist;

            for(auto d : directions){
                int nrow = curr_row + d.first;
                int ncol = curr_col + d.second;

                if(nrow>=0 and ncol>=0 and nrow<n and ncol<m){
                    if(!vis[nrow][ncol]){
                        vis[nrow][ncol] = 1;
                        q.push({{nrow,ncol},curr_dist+1});
                    }
                }
            }
        }

        return ans;
    }
};
```