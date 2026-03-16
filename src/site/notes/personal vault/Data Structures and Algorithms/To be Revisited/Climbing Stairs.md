---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/climbing-stairs/"}
---


### Problem Statement:
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

>**Example 1:**
**Input:** n = 2
**Output:** 2
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

>**Example 2:**
**Input:** n = 3
**Output:** 3
**Explanation:** There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

**Constraints:**

- `1 <= n <= 45`


#### Solution:

##### 1. Recursion and memoization:
```cpp
class Solution {
public:
    int climbStairs(int n) {

        if(n==0) return 1;
        if(n<0) return 0;
        
        return climbStairs(n-1) + climbStairs(n-2);
    }
};


//memoized

class Solution {
public:
    int solve(int n , vector<int> & dp){

        if(n<0) return 0;
        if(n==0) return 1;

        if(dp[n] != -1) return dp[n];
        dp[n] = solve(n-1,dp) + solve(n-2,dp);
        return dp[n];

    }

    int climbStairs(int n) {

        vector<int> dp(n+1,-1);        
        return solve(n,dp);
    }
};
```

##### 2. Tabulation:
```cpp
class Solution {
public:


    int climbStairs(int n) {
        //tablulation
        vector<int> dp(n+1);

        dp[0] = 1;
        dp[1] = 1;

        for(int i = 2 ; i<=n;i++){
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
};

```

##### 3. Space optimized:
```cpp

class Solution {
public:
    int climbStairs(int n) {
        
        if(n<2) return 1;

        int prev2 = 1;
        int prev1 = 1;
        int curr = 0;

        for(int i =2 ; i<=n;i++){
            curr = prev1  + prev2;
            prev2 = prev1;
            prev1 = curr;
        }

        return curr;
    }
};

```