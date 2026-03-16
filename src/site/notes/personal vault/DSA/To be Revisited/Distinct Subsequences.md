---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/distinct-subsequences/"}
---

### Problem Statement:
Given two strings s and t, return _the number of distinct_ **_subsequences_** _of_ s _which equals_ t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

>**Example 1:**
**Input:** s = "rabbbit", t = "rabbit"
**Output:** 3
**Explanation:**
As shown below, there are 3 ways you can generate "rabbit" from s.
>![[Screenshot 2024-06-02 at 1.29.41 AM.png \| 60]]


#### Solution:

##### 1. Recursion and Memoization:
```java
class Solution {
    public int numDistinct(String s, String t) {
        if(s.length() < t.length()){
            return 0;
        }

        int[][] dp = new int[s.length()][t.length()];
        for(int[] row: dp){
            Arrays.fill(row, -1);
        }
        
        return numDistinctUtil(s, t, 0, 0, dp);
    }
    
    public int numDistinctUtil(String s, String t, int i1, int i2, int[][] dp){
        if(i2 == t.length()){
            return 1; // Successfully matched all characters of t
        }
        if(i1 >= s.length()){
            return 0; // Reached the end of s without matching all of t
        }

        if(dp[i1][i2] != - 1){
            return dp[i1][i2];
        }
        
        int distinctSubs = 0;
        if(s.charAt(i1) == t.charAt(i2)){
            // Use current character of s + don't use it 
            distinctSubs = numDistinctUtil(s, t, i1 + 1, i2 + 1, dp) + numDistinctUtil(s, t, i1 + 1, i2, dp); 
        } else{
            distinctSubs = numDistinctUtil(s, t, i1 + 1, i2, dp); // Skip current character of s
        }
        
        return dp[i1][i2] = distinctSubs;
    }
    
}
```

##### 2. Tabulation
```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();

        if(m < n){
            return 0;
        }

        int[][] dp = new int[m + 1][n + 1];
        // An empty t is a subsequence of any prefix of s
        for (int i = 0; i <= m; i++) {
            dp[i][0] = 1;
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(s.charAt(i - 1) == t.charAt(j - 1)){
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        
        return dp[m][n];
    }
}
```

##### 3. Space optimization [2-D array]
```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();

        if (m < n) {
            return 0;
        }

        int[] prev = new int[n + 1];
        int[] curr = new int[n + 1];

        // Initialize the base case
        prev[0] = 1;  // An empty t is a subsequence of any prefix of s

        for (int i = 1; i <= m; i++) {
            curr[0] = 1;  // Reset curr[0] for the new row
            for (int j = 1; j <= n; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    curr[j] = prev[j - 1] + prev[j];
                } else {
                    curr[j] = prev[j];
                }
            }
            // Copy curr to prev for the next iteration
            prev = curr.clone();
        }

        return prev[n];
    }
}

```

##### 4. Space optimization [1-D array]
```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();

        if (m < n) {
            return 0;
        }

        int[] prev = new int[n + 1];
        

        // Initialize the base case
        prev[0] = 1;  // An empty t is a subsequence of any prefix of s

        for (int i = 1; i <= m; i++) {
            for (int j = n; j >= 1; j--) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    prev[j] = prev[j - 1] + prev[j];
                }
            }
        }

        return prev[n];
    }
}
```
