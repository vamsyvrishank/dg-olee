---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/unique-paths-ii/"}
---


Same question just with Obstacles in the grid.

1. Solve without obstacles first arrive at the best space optimized solution.
2. Then put these extra checks for obstacles.
3. Refer: [[personal vault/Data Structures and Algorithms/To be Revisited/Unique Paths#Space optimization\|Unique Paths#Space optimization]]

```cpp

class Solution {
public:
    int solve(int m , int n , vector<vector<int>>& obstacleGrid , vector<vector<int>> &dp){
        if(n<0 || m < 0 || obstacleGrid[m][n]==1) return 0;
        if(m==0 and n==0) return 1;
        if(dp[m][n] != -1) return dp[m][n];

        dp[m][n] = solve(m-1,n,obstacleGrid, dp) + solve(m,n-1, obstacleGrid,dp);

        return dp[m][n];

    }
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {

        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<vector<int>> dp(m+1 , vector<int>(n+1 , -1));
        return solve(m-1 , n-1 ,obstacleGrid ,  dp);
        
    }
};

```


