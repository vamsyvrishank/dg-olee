---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/rotate-array-lc-189/"}
---



Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3
**Output:** [5,6,7,1,2,3,4]
**Explanation:**
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

**Example 2:**

**Input:** nums = [-1,-100,3,99], k = 2
**Output:** [3,99,-1,-100]
**Explanation:** 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

**Follow up:**

- Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
- Could you do it in-place with `O(1)` extra space?



```python
```


This might seem little confusing , but always draw examples , dry run and try writing the code 

here we reverse the entire array ,
then swap the first k elements  then swap the next n-k elements, easy peasy but lots of swaps
Also always check for edge cases
This is the most optimal way asked in question.

Other approaches ? 

```python

class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """

        if k == 0:
            return

        def reverse(left , right):
            while left < right:
                nums[left] , nums[right] = nums[right] , nums[left]
                left+=1
                right-=1
            
        
        #reverse the entire array
        n = len(nums)
        left = 0
        right = n-1
        reverse(left,right)
        #case , k might exceed size of array 
        k = k%n
        #current state -> 7 6 5 4 3 2 1 
        #reverse first k elements and then n-k elements 
        left = 0
        right = k-1
        reverse(left , right)
        left = k
        right = n-1
        reverse(left,right)
        

```