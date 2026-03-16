---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/longest-common-subsequence/"}
---

#### Problem Statement:
Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.
A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
- For example, `"ace"` is a subsequence of `"abcde"`.
A **common subsequence** of two strings is a subsequence that is common to both strings.

>
**Example 1:**
**Input:** text1 = "abcde", text2 = "ace" 
**Output:** 3  
**Explanation:** The longest common subsequence is "ace" and its length is 3.

>
>**Example 2:**
**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

>
>**Example 3:**
**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no such common subsequence, so the result is 0.

### Solution:

##### Recursion and Memoized:
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];
        for(int[] row: dp){
            Arrays.fill(row, -1);
        }

        return longestCommonSubsequence(text1, text2, 0, 0, dp);
    }

    public int longestCommonSubsequence(String t1, String t2, int idx1, int idx2, int[][] dp){
        if(idx1 >= t1.length() || idx2 >= t2.length()){
            return 0;
        }

        if(dp[idx1][idx2] != -1){
            return dp[idx1][idx2];
        }

        if(t1.charAt(idx1) == t2.charAt(idx2)){
            return dp[idx1][idx2] = 1 + longestCommonSubsequence(t1, t2, idx1 + 1, idx2 + 1, dp);
        }

        return dp[idx1][idx2] = Math.max(
            longestCommonSubsequence(t1, t2, idx1 + 1, idx2, dp), 
            longestCommonSubsequence(t1, t2, idx1, idx2 + 1, dp)
        );
    }
}
```

##### Tabulation:
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];
        
        for(int i = 1; i <= n1; i++){
            for(int j = 1; j <= n2; j++){
                if(text1.charAt(i - 1) == text2.charAt(j - 1)){
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[n1][n2];
    }
}
```

##### Space Optimized
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();
        int[] prev = new int[n2 + 1];
        int[] curr = new int[n2 + 1];

        Arrays.fill(prev, 0);
        curr[0] = 0;
        
        for(int i = 1; i <= n1; i++){
            //fill the rows of curr
            for(int j = 1; j <= n2; j++){
                if(text1.charAt(i - 1) == text2.charAt(j - 1)){
                    curr[j] = 1 + prev[j - 1];
                } else{
                    curr[j] = Math.max(prev[j], curr[j - 1]);
                }
            }
            prev = curr.clone();
        }
        return prev[n2];
    }
}
```