---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/product-of-the-array-except-self/"}
---




Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

**Input:** nums = [1,2,3,4]
**Output:** [24,12,8,6]

**Example 2:**

**Input:** nums = [-1,1,0,-3,3]
**Output:** [0,0,9,0,0]

**Constraints:**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- The input is generated such that `answer[i]` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)


```python

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:

        n = len(nums)
        suff = [0]*n
        pref = [0]*n

        pref[0] = nums[0]
        suff[n-1] = nums[n-1]

        for i in range(1,n):
            pref[i] = pref[i-1]*nums[i]
            suff[n-1-i] = suff[n-i]*nums[n-1-i]

        ans = [0]*n
        for i in range(1,n-1):
            ans[i] = pref[i-1]*suff[i+1]
        
        ans[0] = suff[1]
        ans[n-1]=pref[n-2]
    
        return ans

```


O(1) space optimized : Directly store the results
```python

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        
        n = len(nums)
        ans = [1]*n
        curr = 1

        for i in range(0,n):
            ans[i] *=curr
            curr *=nums[i]
        curr = 1
        for i in range(n-1,-1,-1):
            ans[i] *=curr
            curr *=nums[i]
        
        return ans
```