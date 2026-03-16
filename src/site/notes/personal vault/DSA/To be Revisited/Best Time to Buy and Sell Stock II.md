---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/best-time-to-buy-and-sell-stock-ii/"}
---

### Problem Statement
You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return _the **maximum** profit you can achieve_.

>**Example 1:**
**Input:** prices = [7,1,5,3,6,4]
**Output:** 7
**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.

>**Example 2:**
**Input:** prices = [1,2,3,4,5]
**Output:** 4
**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.

>**Example 3:**
**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.

**Constraints:**

- `1 <= prices.length <= 3 * 10^4`
- `0 <= prices[i] <= 10^4`

#### Solution:

##### 1. Recursion
```java
class Solution {
    public int maxProfit(int[] prices) {
        return trade(prices, 0, false, 0);
    }

    public int trade(int[] prices, int day, boolean hasStock, int profit){
        if(day == prices.length){
            return profit;
        }
        
        //Choice 1: Do nothing
        int doNothing = trade(prices, day + 1, hasStock, profit);

        //Choice 2: Buy the stock if not holding
        int buy = 0;
        if(!hasStock){
            buy = trade(prices, day + 1, true, profit - prices[day]);
        }
        
        //Choice 3: Sell the stock if holding
        int sell = 0;
        if(hasStock){
            sell = trade(prices, day + 1, false, profit + prices[day]);
        }

        return Math.max(doNothing, Math.max(buy, sell));
    }
}
```

##### 2. Recursion and Memoization:

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[][] dp = new int[prices.length][2];
        for (int i = 0; i < prices.length; i++) {
            Arrays.fill(dp[i], -1);
        }
        //hasStock = 0 : don't have stock
        //hasStock = 1 : have stock
        return trade(prices, 0, 0, dp);
    }

    public int trade(int[] prices, int day, int hasStock, int[][] dp){
        if(day == prices.length){
            return 0;
        }

        if(dp[day][hasStock] != -1){
            return dp[day][hasStock];
        }

        //Choice 1: Do nothing
        int doNothing = trade(prices, day + 1, hasStock, dp);

        //Choice 2: Buy the stock if not holding
        int buy = 0;
        if(hasStock == 0){
            buy = trade(prices, day + 1, 1, dp) - prices[day];
        }
        
        //Choice 3: Sell the stock if holding
        int sell = 0;
        if(hasStock == 1){
            sell = trade(prices, day + 1, 0, dp) + prices[day];
        }

        return dp[day][hasStock] = Math.max(doNothing, Math.max(buy, sell));
    }
}
```

##### 3. Tabulation:
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        // dp[i][0] represents the maximum profit at the end of day i without holding a stock
        // dp[i][1] represents the maximum profit at the end of day i holding a stock
        int[][] dp = new int[n][2];

        // Base case: At the beginning (day 0), the maximum profit without holding a stock is 0
        dp[0][0] = 0;

        // Base case: At the beginning (day 0), the maximum profit holding a stock is -prices[0]
        dp[0][1] = -prices[0];

        // Iterate through days starting from day 1
        for (int i = 1; i < n; i++) {
            // If not holding a stock on day i, we can either do nothing (stay not holding) or buy a stock
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);

            // If holding a stock on day i, we can either do nothing (stay holding) or sell the stock
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }

        // The maximum profit at the end of the last day without holding a stock is the final answer
        return dp[n - 1][0];
    }
}
```

##### 3. Tabulation with space optimization:

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        // dp[i][0] represents the maximum profit at the end of day i without holding a stock
        // dp[i][1] represents the maximum profit at the end of day i holding a stock
        int[] prev = new int[2];
        int[] curr = new int[2];

        // Base case: At the beginning (day 0), the maximum profit without holding a stock is 0
        prev[0] = 0;

        // Base case: At the beginning (day 0), the maximum profit holding a stock is -prices[0]
        prev[1] = -prices[0];

        // Iterate through days starting from day 1
        for (int i = 1; i < n; i++) {
            // If not holding a stock on day i, we can either do nothing (stay not holding) or buy a stock
            curr[0] = Math.max(prev[0], prev[1] + prices[i]);

            // If holding a stock on day i, we can either do nothing (stay holding) or sell the stock
            curr[1] = Math.max(prev[1], prev[0] - prices[i]);

            prev = curr.clone();
        }

        // The maximum profit at the end of the last day without holding a stock is the final answer
        return prev[0];
    }
}
```

##### 4. Greedy
> Account for only increasing sequences
```java
class Solution {
    public int maxProfit(int[] prices) {
        int profitSoFar = 0, buyDate = 0, sellDate = 0;

        for(int day = 1; day < prices.length; day++){
            if(prices[day] >= prices[day-1]){
                sellDate++;
            } else{
                profitSoFar += prices[sellDate] - prices[buyDate];
                sellDate = day;
                buyDate = day;
            }
        }
        //This the loop is necessary to account for the last potential profitable 
        //trade that may not have been settled within the loop.
        profitSoFar += prices[sellDate] - prices[buyDate];
        return profitSoFar;
    }
}
```