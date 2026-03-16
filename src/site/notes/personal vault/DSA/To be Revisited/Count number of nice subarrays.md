---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/count-number-of-nice-subarrays/"}
---


Good Question

Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.

Return _the number of **nice** sub-arrays_.

**Example 1:**

**Input:** nums = [1,1,2,1,1], k = 3
**Output:** 2
**Explanation:** The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].

**Example 2:**

**Input:** nums = [2,4,6], k = 1
**Output:** 0
**Explanation:** There are no odd numbers in the array.

**Example 3:**

**Input:** nums = [2,2,2,1,2,2,1,2,2,2], k = 2
**Output:** 16

**Constraints:**

- `1 <= nums.length <= 50000`
- `1 <= nums[i] <= 10^5`
- `1 <= k <= nums.length`



> [!NOTE] Point to note
>  When we add (end-start+1) -> this basically is adding all subarrays from start to end (count) with at most k odd numbers , similarly we calculate for at most k-1 elements ,  when we take diff  solve(k) - solve(k-1) we get the count for exactly  subarrays  with k odd_count. ( nice approach)
> 


```cpp
class Solution {
public:
    int solve(vector<int> nums, int k) {

        int start = 0;
        int end = 0;

        int odd_count = 0;
        int count = 0;

        while (end < nums.size()) {
            if (nums[end] % 2 == 1)
                odd_count++;

            while (odd_count > k) {

                if (nums[start] % 2 == 1) {
                    odd_count--;
                }
                start++;
            }
            count += (end - start + 1);
            end++;
        }
        return count;
    }
    int numberOfSubarrays(vector<int>& nums, int k) {

        return solve(nums, k) - solve(nums, k - 1);
    }
};
```