---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/coin-change-ii/"}
---

#### Question:
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.
Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.
You may assume that you have an infinite number of each kind of coin.
The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**
**Input:** amount = 5, coins = [1,2,5]
**Output:** 4
**Explanation:** there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

**Example 2:**
**Input:** amount = 3, coins = [2]
**Output:** 0
**Explanation:** the amount of 3 cannot be made up just with coins of 2.


### Solution:

##### Recursive and Memoized Solution:
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length][amount + 1];
        for(int[] mem: dp){
            Arrays.fill(mem, -1);
        }
        return changeUtil(amount, 0, coins, dp);
    }

    public int changeUtil(int amount, int idx, int[]coins, int[][] dp){
        if(amount == 0){
            return 1;
        }
        if(idx >= coins.length){
	        // If the amount is also 0, there's 1 way to make the change 
	        // Otherwise, there's no way to make the change
            return amount == 0 ? 1 : 0;
        }

        if(dp[idx][amount] != -1){
            return dp[idx][amount];
        }
        
		// Number of ways by excluding the current coin
        int noCallWays = changeUtil(amount, idx + 1, coins, dp);

        // Number of ways by including the current coin one or more times 
        //(for example: coin of denomination 2 for amount 10, can be picked 5 times)
        int yesCallWays = 0;
        for(int time = 1; time <= amount/coins[idx]; time++){
	        int remainingAmount = amount - (coins[idx] * time);
            yesCallWays += changeUtil(remainingAmount, idx + 1, coins, dp);
        }

        return dp[idx][amount] = noCallWays + yesCallWays;
    }
}
```

##### Tabulation:
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length + 1][amount + 1];
        
        //Base case
        // Initialize dp[i][0] to 1 for all i, as there's one way to make 0 amount (using no coins)
        for(int i = 0; i < dp.length; i++){
            dp[i][0] = 1;
        }

        for(int i = 1; i <= coins.length; i++){
            for(int j = 1; j <= amount; j++){
                dp[i][j] = dp[i - 1][j]; //noCall

                if(j >= coins[i - 1]){ //yesCall
                    dp[i][j] += dp[i][j - coins[i - 1]];
                }
            }
        }
        
        return dp[coins.length][amount];
    }
}
```

##### Space Optimized:
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] prev = new int[amount + 1];
        int[] curr = new int[amount + 1];
        
        prev[0] = 1;
        for(int i = 1; i <= amount; i++){
            prev[i] = 0;
        }

        for(int coin: coins){
            for(int i = 0; i <= amount; i++){
                curr[i] = prev[i]; // no call
                
                if(i >= coin){
                    // yesCall (prev + ways to make the reduced amt in prev array)
                    curr[i] = prev[i] + curr[i - coin];
                    //or curr[i] += curr[i - coin];
                }
            }

            prev = curr.clone();
        }
        
        return prev[amount];
    }
}
```