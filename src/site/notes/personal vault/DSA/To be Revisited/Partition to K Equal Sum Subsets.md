---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/partition-to-k-equal-sum-subsets/"}
---

Given an integer array `nums` and an integer `k`, return `true` if it is possible to divide this array into `k` non-empty subsets whose sums are all equal.

**Example 1:**

**Input:** nums = [4,3,2,3,5,2,1], k = 4
**Output:** true
**Explanation:** It is possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

**Example 2:**

**Input:** nums = [1,2,3,4], k = 3
**Output:** false

**Constraints:**

- `1 <= k <= nums.length <= 16`
- `1 <= nums[i] <= 104`
- The frequency of each element is in the range `[1, 4]`.


Solution:
```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        int sum = Arrays.stream(nums).sum();
        boolean[] used = new boolean[nums.length];
        if (sum % k != 0) {
            return false;
        }
        Arrays.sort(nums);
        return dfs(nums, k, 0, 0, used, sum / k);
    }
    
    boolean dfs(int[] nums, int k, int currSum, int idx, boolean[] used, int target) {
        if (k == 1) {
            return true;
        }
        if (currSum == target) {
            return dfs(nums, k - 1, 0, 0, used, target);
        }
        for (int i = idx; i < nums.length; i++) {
            if (used[i] || i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue;
            }
            used[i] = true;
            if (dfs(nums, k, currSum + nums[i], i + 1, used, target)) {
                return true;
            }
            used[i] = false;
        }
        return false;
    }
}
```