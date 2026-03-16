---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/house-robber-ii/"}
---


### Question:
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [2,3,2]
**Output:** 3
**Explanation:** You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

**Example 2:**

**Input:** nums = [1,2,3,1]
**Output:** 4
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 3:**

**Input:** nums = [1,2,3]
**Output:** 3


### Solution:
1. Took the space optimized solution from [[personal vault/DSA/To be Revisited/House Robber#Solution\|House Robber#Solution]] and used it here.
2. ![[HouseRobber2.jpg \| 200]]


```cpp
class Solution {
public:
    int solve(vector<int>& nums) {


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

    int rob(vector<int>& nums) {

        int n = nums.size();
        if(n==1) return nums[0];

        vector<int> temp1 , temp2;

        for(int i =0 ; i< n;i++){
            if(i!=0){
                temp1.push_back(nums[i]);
            }
            if(i!=n-1){
                temp2.push_back(nums[i]);
            }
        }

        return max(solve(temp1) , solve(temp2));

        
    }
};

```


Shreyansh Java O(1) space solution
```java

class Solution {
    public int robUtil(int[] houses) {
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

    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return nums[0];
        }

        int[] temp1 = new int[n - 1];
        int[] temp2 = new int[n - 1];

        for (int i = 0; i < n; i++) {
            if (i != 0)
                temp1[i - 1] = nums[i];
            if (i != n - 1)
                temp2[i] = nums[i];
        }

        return Math.max(robUtil(temp1), robUtil(temp2));
    }
}
```