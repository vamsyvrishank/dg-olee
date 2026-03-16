---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/combination-sum-iv/"}
---


Given an array of **distinct** integers `nums` and a target integer `target`, return _the number of possible combinations that add up to_ `target`.

The test cases are generated so that the answer can fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [1,2,3], target = 4
**Output:** 7
**Explanation:**
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.

**Example 2:**

**Input:** nums = [9], target = 3
**Output:** 0

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 1000`
- All the elements of `nums` are **unique**.
- `1 <= target <= 1000`

**Follow up:** What if negative numbers are allowed in the given array? How does it change the problem? What limitation we need to add to the question to allow negative numbers?


```cpp

class Solution {
public:
    int backtrack(int index , vector<int> &nums ,int target, vector<int> poss_ans , vector<int> &dp){
        if(index == nums.size()){
            if(target==0){
               return 1;
            }
            return 0;
        }
        if(dp[target] != -1) return dp[target];
        //pick 
        int count1 = 0;
        if(nums[index] <= target){
            poss_ans.push_back(nums[index]);
            count1 = backtrack(0, nums, target-nums[index],poss_ans,dp);
            poss_ans.pop_back();
        }
        //not pick ( remember dont keep it in else)
        int count2 = backtrack(index+1,nums,target,poss_ans,dp);
        dp[target] = count1 + count2;

        return dp[target];

    }
    int combinationSum4(vector<int>& nums, int target) {

       
        int index = 0;
        vector<int> poss_ans;
        vector<int> dp(target+1 , -1);

        return backtrack(index , nums , target, poss_ans ,dp);

     
        
    }
};
```