---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/search-in-a-rotated-sorted-array/"}
---


### Search in a Rotated Sorted Array

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1

**Example 3:**

**Input:** nums = [1], target = 0
**Output:** -1

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `-104 <= target <= 104`

```cpp
class Solution {
public:

    int search(vector<int>& nums, int target) {
        int start = 0;
        int end = nums.size()-1;
        
        while(start <= end){
            int mid = start + (end-start)/2;
            if(target == nums[mid]){
                return mid;
            }

            //check if left half is sorted
            if(nums[start] <= nums[mid]){
                if(target >= nums[start] and target < nums[mid]){
                    end = mid-1;
                }
                else{
                    start = mid+1;
                }
            }

            // else the right half must be sorted
            else{
                if(target > nums[mid] and target <= nums[end]){
                    start = mid+1;
                }
                else{
                    end = mid-1;
                }
            }
        }

        return -1;
    }
};
```


### Search in a rotated Sorted Array II

There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` _if_ `target` _is in_ `nums`_, or_ `false` _if it is not in_ `nums`_._

You must decrease the overall operation steps as much as possible.

**Example 1:**

**Input:** nums = [2,5,6,0,0,1,2], target = 0
**Output:** true

**Example 2:**

**Input:** nums = [2,5,6,0,0,1,2], target = 3
**Output:** false

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- `nums` is guaranteed to be rotated at some pivot.
- `-104 <= target <= 104`

**Follow up:** This problem is similar to [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/), but `nums` may contain **duplicates**. Would this affect the runtime complexity? How and why?


Complexity -> best Case : O(logn) But worst case is O(n) i.e when we have to go through all the elements.
```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int start = 0;
        int end = nums.size()-1;
        
        while(start<=end){
            int mid = start + (end-start)/2;
            if(target==nums[mid]){
                return true;
            }
			// here we are skipping elements so that we are not going to the wrong side.
			while(start < mid and nums[start]==nums[mid]) start++;
			while(end > mid and nums[end]==nums[mid]) end--;

            //check if left half is sorted.
            if(nums[start] <=nums[mid]){
                if(target>=nums[start] and target<nums[mid]){
                    end = mid-1;
                }
                else{
                    start = mid+1;
                }
            }
            //if not then right must be sorted
            else{
                if(target>nums[mid] and target<=nums[end]){
                    start = mid+1;
                }
                else{
                    end = mid-1;
                }
            }
        }
        return false;
    }
};
```

Can we do better ? Remember what Aditya Verma told :- Jo aata hai ussi ko use karo.
Find the pivot element where it was rotated , then we have two sorted arrays , just apply binary search on them.

