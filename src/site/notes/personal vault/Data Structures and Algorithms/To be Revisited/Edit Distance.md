---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/edit-distance/"}
---

### Question:
Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

>**Example 1:**
**Input:** word1 = "horse", word2 = "ros"
**Output:** 3
**Explanation:** 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

>**Example 2:**
**Input:** word1 = "intention", word2 = "execution"
**Output:** 5
**Explanation:** 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

**Constraints:**

- `0 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.


#### Solution:

##### 1. Recursion and Memoization:
```java
class Solution {
    public int minDistance(String w1, String w2) {
        int[][] dp = new int[w1.length()][w2.length()];
        
        for(int[] row: dp){
            Arrays.fill(row, -1);
        }
        return minDistanceUtil(w1, w2, 0, 0, dp);
    }

    public int minDistanceUtil(String w1, String w2, int i1, int i2, int[][] dp){
        if(i1 >= w1.length()){
            return w2.length() - i2; //Number of operations to "insert" all the remaining characters
        }
        if(i2 >= w2.length()){
            return w1.length() - i1; //Number of operations to "delete" all the remaining characters
        }

        if(dp[i1][i2] != -1){
            return dp[i1][i2];
        }
        
        Character ch1 = w1.charAt(i1);
        Character ch2 = w2.charAt(i2);
        Integer minDist = 0;

        if(ch1 != ch2){
            int insert = 1 + minDistanceUtil(w1, w2, i1, i2 + 1, dp);
            int delete = 1 + minDistanceUtil(w1, w2, i1 + 1, i2, dp);
            int replace = 1 + minDistanceUtil(w1, w2, i1 + 1, i2 + 1, dp);
            
            minDist = Math.min(insert, Math.min(delete, replace));
        } else{
            minDist = minDistanceUtil(w1, w2, i1 + 1, i2 + 1, dp);
        }

        return dp[i1][i2] = minDist;
    }
}
```

##### 2. Tabulation
```java
class Solution {
    public int minDistance(String w1, String w2) {
        int m = w1.length();
        int n = w2.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;  // Deleting all characters from w2 to match an empty w1
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;  // Inserting all characters of w1 to match an empty w2
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                Character ch1 = w1.charAt(i - 1);
                Character ch2 = w2.charAt(j - 1);

                if(ch1 != ch2){
                    int insert = 1 + dp[i][j - 1];
                    int delete = 1 + dp[i - 1][j];
                    int replace = 1 + dp[i - 1][j - 1];

                    dp[i][j] = Math.min(insert, Math.min(delete, replace));
                } else{
                    dp[i][j] = dp[i-1][j-1];
                }
            }
        }
        return dp[m][n];
    }
}
```

##### 4. Space optimized
```java
class Solution {
    public int minDistance(String w1, String w2) {
        int m = w1.length();
        int n = w2.length();
        int[] prev = new int[n + 1];
        int[] curr = new int[n + 1];

        for (int i = 0; i <= n; i++) {
            prev[i] = i;  // Deleting all characters from w2 to match an empty w1
        }

        for(int i = 1; i <= m; i++){
            curr[0] = i;
            for(int j = 1; j <= n; j++){
                Character ch1 = w1.charAt(i - 1);
                Character ch2 = w2.charAt(j - 1);

                if(ch1 != ch2){
                    int insert = 1 + curr[j - 1];
                    int delete = 1 + prev[j];
                    int replace = 1 + prev[j - 1];

                    curr[j] = Math.min(insert, Math.min(delete, replace));
                } else{
                    curr[j] = prev[j - 1];
                }
            }
            prev = curr.clone();
        }
        return prev[n];
    }
}
```