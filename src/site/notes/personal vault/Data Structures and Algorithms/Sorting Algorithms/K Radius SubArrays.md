---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/sorting-algorithms/k-radius-sub-arrays/"}
---



You are given a **0-indexed** array `nums` of `n` integers, and an integer `k`.

The **k-radius average** for a subarray of `nums` **centered** at some index `i` with the **radius** `k` is the average of **all** elements in `nums` between the indices `i - k` and `i + k` (**inclusive**). If there are less than `k` elements before **or** after the index `i`, then the **k-radius average** is `-1`.

Build and return _an array_ `avgs` _of length_ `n` _where_ `avgs[i]` _is the **k-radius average** for the subarray centered at index_ `i`.

The **average** of `x` elements is the sum of the `x` elements divided by `x`, using **integer division**. The integer division truncates toward zero, which means losing its fractional part.

- For example, the average of four elements `2`, `3`, `1`, and `5` is `(2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75`, which truncates to `2`.

**Example 1:**
![[Pasted image 20240609183658.png \| 600]]

**Input:** nums = [7,4,3,9,1,8,5,2,6], k = 3
**Output:** [-1,-1,-1,5,4,4,-1,-1,-1]
**Explanation:**
- avg[0], avg[1], and avg[2] are -1 because there are less than k elements **before** each index.
- The sum of the subarray centered at index 3 with radius 3 is: 7 + 4 + 3 + 9 + 1 + 8 + 5 = 37.
  Using **integer division**, avg[3] = 37 / 7 = 5.
- For the subarray centered at index 4, avg[4] = (4 + 3 + 9 + 1 + 8 + 5 + 2) / 7 = 4.
- For the subarray centered at index 5, avg[5] = (3 + 9 + 1 + 8 + 5 + 2 + 6) / 7 = 4.
- avg[6], avg[7], and avg[8] are -1 because there are less than k elements **after** each index.

**Example 2:**

**Input:** nums = [100000], k = 0
**Output:** [100000]
**Explanation:**
- The sum of the subarray centered at index 0 with radius 0 is: 100000.
  avg[0] = 100000 / 1 = 100000.

**Example 3:**

**Input:** nums = [8], k = 100000
**Output:** [-1]
**Explanation:** 
- avg[0] is -1 because there are less than k elements before and after index 0.

**Constraints:**

- `n == nums.length`
- `1 <= n <= 105`
- `0 <= nums[i], k <= 105`

```cpp
class Solution {
public:
    vector<int> getAverages(vector<int>& nums, int k) {

    /* 
        if you read this question carefully , the window element is (i-k)(start index) to (i+k)(end index), when you substract,
        (i+k)(end) - (i-k)(start) = 2*k , we can say that the window contains 2*k element index , so end-start+1 = 2*k+1.
    */

        int n = nums.size();
        vector<int>ans(n , -1);

        int start = 0;
        int end = 0;
        double temp = 0;

        while(end < n){
            temp+=nums[end];
            if(end-start+1 == 2*k+1){

                double average = temp/double(end-start+1);
                int index = (start+end)/2;
                ans[index] = floor(average);

                temp = temp - nums[start];
                start++;
            }
            end++;
        }

        return ans;
    }
};
```