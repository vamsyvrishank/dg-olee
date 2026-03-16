---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/minimum-falling-path-sum/"}
---





#### Solution:

##### 1. Recursive and Memoized:

> Remove the commented part and the dp part for recursive soln.

```java
class Solution {
    public int minFallingPathSum(int[][] mat) {
        int nRows = mat.length;
        int nCols = mat[0].length;

        int[][] dp = new int[nRows][nCols];
        for(int[] mem: dp){
            Arrays.fill(mem, -1);
        }

        int min = Integer.MAX_VALUE;
        for(int sc = 0; sc < nCols; sc++){
            minFallingPathSumUtil(mat, 0, sc, nRows - 1, dp);

            // int minPathSum = minFallingPathSumUtil(mat, 0, sc, nRows - 1, dp);
            // min = Math.min(min, minPathSum);
        }

        for(int i = 0; i < dp[0].length; i++){
            min = Math.min(min, dp[0][i]);
        }

        return min;
    }

    public int minFallingPathSumUtil(int[][] grid, int sr, int sc, int dr, int[][] dp){
        if(sr < 0 || sc < 0 || sr > dr || sc >= grid[0].length){
            return Integer.MAX_VALUE;
        }
        if(sr == dr){
            return dp[sr][sc] = grid[sr][sc];
        }
        if(dp[sr][sc] != -1){
            return dp[sr][sc];
        }

        int down = minFallingPathSumUtil(grid, sr + 1, sc, dr, dp);
        int dright = minFallingPathSumUtil(grid, sr + 1, sc + 1, dr, dp); //diagonal right
        int dleft = minFallingPathSumUtil(grid, sr + 1, sc - 1, dr, dp); //diagonal left

        return dp[sr][sc] = grid[sr][sc] + Math.min(down, Math.min(dright, dleft));
    }
}
```

##### 2. Tabulation:
TC: O(nm)
SC: O(nm)
```java
class Solution {
    public int minFallingPathSum(int[][] mat) {
        int nRows = mat.length;
        int nCols = mat[0].length;

        // Initialize dp array with the last row of the matrix
        int[][] dp = new int[nRows][nCols];
        for (int j = 0; j < nCols; j++) {
            dp[nRows - 1][j] = mat[nRows - 1][j];
        }

        // Fill the dp array from the bottom up
        for (int i = nRows - 2; i >= 0; i--) {
            for (int j = 0; j < nCols; j++) {
                int down = dp[i + 1][j];
                int dRight = (j + 1 >= nCols) ? Integer.MAX_VALUE : dp[i + 1][j + 1];
                int dLeft = (j - 1 < 0) ? Integer.MAX_VALUE : dp[i + 1][j - 1];
                dp[i][j] = mat[i][j] + Math.min(down, Math.min(dRight, dLeft));
            }
        }

        // The minimum path sum will be the minimum value in the first row of dp
        int min = Integer.MAX_VALUE;
        for (int j = 0; j < nCols; j++) {
            min = Math.min(min, dp[0][j]);
        }

        return min;
    }
```



##### 3. Space Optimized.
> we only need prev row to calculate curr row

TC: O(nm)
SC: O(m)

```java
class Solution {
    public int minFallingPathSum(int[][] mat) {
        int nRows = mat.length;
        int nCols = mat[0].length;

        int[] prev = new int[nCols];
        int[] curr = new int[nCols];

        for(int j = 0; j < nCols; j++){
            prev[j] = mat[nRows - 1][j];
        }

        for(int i = nRows - 2; i >= 0; i--){
            //curr = new int[nCols]; //re-assign or not doesn't matter
            for(int j = nCols - 1; j >= 0; j--){
                int down = prev[j];
                int dRight = (j + 1 >= nCols) ? Integer.MAX_VALUE : prev[j + 1];
                int dLeft = (j - 1 < 0) ? Integer.MAX_VALUE : prev[j - 1];

                curr[j] = mat[i][j] + Math.min(down, Math.min(dRight, dLeft));
            }
            /*
            The line `prev = curr` does not create a new copy of the `curr` array; instead, it assigns the reference of `curr` to `prev`. 
            This means that subsequent modifications to `curr` will also affect `prev`.
            */
            prev = curr.clone();
        }

        int min = Integer.MAX_VALUE;
        for(int i = 0; i < nCols; i++){
            min = Math.min(min, prev[i]);
        }

        return min;
    }
}
```