---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/wildcard-matching/"}
---

### Problem Statement:
Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'` where:

- `'?'` Matches any single character.
- `'*'` Matches any sequence of characters (including the empty sequence).

The matching should cover the **entire** input string (not partial).

>**Example 1:**
**Input:** s = "aa", p = "a"
**Output:** false
**Explanation:** "a" does not match the entire string "aa".

>**Example 2:**
**Input:** s = "aa", p = "*"
**Output:** true
**Explanation:** '*' matches any sequence.

>**Example 3:**
**Input:** s = "cb", p = "?a"
**Output:** false
**Explanation:** '?' matches 'c', but the second letter is 'a', which does not match 'b'.


#### Solution:

##### 1. Recursive and memoization:
```java
class Solution {
    public boolean isMatch(String s, String p) {
        Boolean[][] memo = new Boolean[s.length() + 1][p.length() + 1];

        return isMatchUtil(s, p, 0, 0, memo);
    }

    public boolean isMatchUtil(String s, String p, int i, int j, Boolean[][] dp){
        if(i == s.length() && j == p.length()){
            return true;
        }

        if(j == p.length()){
            return false;
        }

        if(i == s.length()){
            for(int k = j; k < p.length(); k++){
                if(p.charAt(k) != '*'){
                    return false;
                }
            }
            return true;
        }
	    
	    if(dp[i][j] != null){
            return dp[i][j];
        }


        if(s.charAt(i) == p.charAt(j) || p.charAt(j) == '?'){
            return dp[i][j] = isMatchUtil(s, p, i + 1, j + 1, dp);
        } else if(p.charAt(j) == '*'){
            return dp[i][j]  = isMatchUtil(s, p, i, j + 1, dp) || isMatchUtil(s, p, i + 1, j, dp);
        } else{
            return dp[i][j] = false;
        }
    }
}
```

##### 2. Tabulation:
```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1];

        //Base case: both strings are empty
        dp[0][0] = true;

        // Base case: Pattern p contains only '*' at the beginning
        for(int j = 1; j <= n; j++){
            if(p.charAt(j - 1) == '*'){
                dp[0][j] = dp[0][j - 1];
            }
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '?'){
                    dp[i][j] = dp[i - 1][j - 1];
                } else if(p.charAt(j - 1) == '*'){
                    dp[i][j]  = dp[i][j - 1] || dp[i - 1][j];
                } else{
                    dp[i][j] = false;
                }
            }
        }
        
        return dp[m][n];
    }
}
```

##### 3. Tabulation and Space optimized:
```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        boolean[] prev = new boolean[n + 1];
        boolean[] curr = new boolean[n + 1];

        //Base case: both strings are empty
        prev[0] = true;

        // Base case: Pattern p contains only '*' at the beginning
        for(int j = 1; j <= n; j++){
            if(p.charAt(j - 1) == '*'){
                prev[j] = prev[j - 1];
            }
        }

        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '?'){
                    curr[j] = prev[j - 1];
                } else if(p.charAt(j - 1) == '*'){
                    curr[j]  = curr[j - 1] || prev[j];
                } else{
                    curr[j] = false;
                }
            }
            prev = curr.clone();
        }
        
        return prev[n];
    }
}
```