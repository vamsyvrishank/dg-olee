---
{"dg-publish":true,"permalink":"/personal-vault/dsa/graph-questions/leetcode-695-max-area-of-island/"}
---



```cpp
class Solution {
public:
    void bfs(int row , int col , vector<vector<int>> &grid , vector<vector<int>> & vis , int & max_area , int m , int n ){ 
        vis[row][col] = 1; 
        int curr_max = 0;
        queue<pair<int , int >> q;
        q.push({row , col});

        curr_max++;
        vector<vector<int>> directions = {{1, 0} , {-1,0} , {0,1} , {0,-1}};
        
        while(!q.empty()){ 

            auto node = q.front();
            q.pop();

            int r = node.first;
            int c = node.second;

            for(auto i : directions) {

                int nrow = r + i[0];
                int ncol = c + i[1];
                

                if( nrow < m and ncol < n and nrow >=0 
                and ncol >=0 and !vis[nrow][ncol] and grid[nrow][ncol]==1){
                    vis[nrow][ncol] = 1;
                    curr_max++;
                    q.push({nrow,ncol});
                }

            }

            max_area = max(max_area , curr_max);
        }
    }
    int maxAreaOfIsland(vector<vector<int>>& grid) {

        int m = grid.size();
        int n = grid[0].size();
        
        vector<vector<int>> vis(m , vector<int> ( n , 0));
        int max_area = 0;

        for(int i = 0 ; i < m ; i++) {

            for(int j =0; j < n ; j++){
                if(!vis[i][j] and grid[i][j]==1){
                    bfs(i , j , grid , vis , max_area, m , n );
                }
            }
        }

        return max_area;

    }
};

```