---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/house-robber/"}
---


(Maximum sum of non adjacent elements)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [1,2,3,1]
**Output:** 4
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 2:**

**Input:** nums = [2,7,9,3,1]
**Output:** 12
**Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.


#### Solution:
1. Recursive + memoization:
```cpp

class Solution {
private:
    int dp(vector<int>&nums , int n , vector<int> & state){

        if(n<=0) return 0;
        if(state[n] != -1) return state[n];

        // if i take n-1 then i cannot take n-2 index element || i dont take n-1 at all.
        state[n] =  max( nums[n-1] + dp(nums,n-2,state) , dp(nums, n-1,state));

        return state[n];        
    }
public:
    int rob(vector<int>& nums) {
        // memiozation
        int n = nums.size();
        vector<int> state(n+1,-1);
        int ans = dp(nums, n, state);
        return ans;
    }
};
```

2. Tabulation:
```cpp

class Solution {
public:
    int rob(vector<int>& nums) {
        // memiozation

        int n = nums.size();
        vector<int> state(n+1,-1);
        
        if(n==1) return nums[0];
        if(n==2) return max(nums[0], nums[1]);

        state[0] = nums[0];
        state[1] = max(nums[0],nums[1]);

        for(int i=2; i<nums.size();i++){
            //same thing, if i take ith element , i cannot take the i-1th element || i do not take ith element at all.

            state[i] = max(nums[i] + state[i-2] , state[i-1]);
        }

        return state[nums.size()-1];
    }
};
```

3. Space optimized
```java
public class Solution {
    public int rob(int[] houses) {
        int n = houses.length;
        if (n == 1) {
            return houses[0];
        }
        if (n == 2) {
            return Math.max(houses[0], houses[1]);
        }

        int prev1 = houses[0];
        int prev2 = Math.max(houses[0], houses[1]);
		
        for (int i = 2; i < n; i++) {
            int res = Math.max(houses[i] + prev1, prev2);
            prev1 = prev2;
            prev2 = res;
        }

        return prev2;
    }
}
```