---
{"dg-publish":true,"permalink":"/personal-vault/dsa/graph-questions/leetcode-994-rotting-oranges/"}
---



```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();

        queue<pair< pair<int , int > , int >> q;
        vector<vector<int>> vis(n , vector<int>(m,0));
        
        for(int i =0 ; i<n ; i ++){
            for(int j = 0; j < m ; j++){
                if(grid[i][j] == 2){
                    q.push({{i,j} , 0});
                    vis[i][j] = 2;
                }
            }
        }

        int time = 0;

        vector<vector<int>> directions = { {0,1} , {0,-1} , {1,0} , {-1,0}};

        while(!q.empty()){

            auto node = q.front();
            int curr_row = node.first.first;
            int curr_col = node.first.second;
            int curr_time = node.second;
            q.pop();

            time = max(time , curr_time);

            for(auto i : directions)
            {

                int nrow = curr_row + i[0];
                int ncol = curr_col + i[1];
                
                if(nrow < n and ncol < m and nrow >=0 and ncol >=0 and vis[nrow][ncol]==0 and grid[nrow][ncol]==1){
                    vis[nrow][ncol] = 2;
                    q.push({{nrow , ncol} ,curr_time + 1});
                }

            }
        }

        for(int i = 0 ; i< n; i++) {
            for (int j=0 ; j < m ; j++) {
                if(vis[i][j]!=2 and grid[i][j] ==1){
                    return -1;
                }
            }
        }

        return time;


    }
};
```