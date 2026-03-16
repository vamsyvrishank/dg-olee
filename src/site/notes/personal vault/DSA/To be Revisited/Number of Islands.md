---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/number-of-islands/"}
---






```cpp
class Solution {
public:
    void bfs(int row, int col, vector<vector<char>>& grid,
             vector<vector<int>> & vis) {

        queue<pair<int, int>> q;
        vis[row][col] = 1;
        q.push({row, col});

        int n = grid.size();
        int m = grid[0].size();

        vector<pair<int, int>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        while (!q.empty()) {
            pair<int, int> temp = q.front();
            q.pop();

            int curr_row = temp.first;
            int curr_col = temp.second;
            
			// we traverse in all 4 directions 

            for (auto d : directions) {
                int neigh_row = curr_row + d.first;
                int neigh_col = curr_col + d.second;

                if (neigh_row >= 0 && neigh_row < n && neigh_col >= 0 &&
                    neigh_col < m && !vis[neigh_row][neigh_col] and grid[neigh_row][neigh_col]=='1') {
                    vis[neigh_row][neigh_col] = 1;
                    q.push({neigh_row, neigh_col});
                }
            }
        }
    }
    int numIslands(vector<vector<char>>& grid) {

        int n = grid.size();
        int m = grid[0].size();
        int count = 0;

        vector<vector<int>> vis(n, vector<int>(m, 0));

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (!vis[i][j] && grid[i][j] == '1') {
                    bfs(i, j, grid, vis);
                    count++;
                }
            }
        }
        return count;
    }
};
```