---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/partition-equal-subset-sum/"}
---

#### Question:
Given an integer array `nums`, return `true` _if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = \[1,5,11,5]
**Output:** true
**Explanation:** The array can be partitioned as \[1, 5, 5] and \[11].

**Example 2:**

**Input:** nums = \[1,2,3,5]
**Output:** false
**Explanation:** The array cannot be partitioned into equal sum subsets.


### Solution:

- Same question as [[personal vault/Data Structures and Algorithms/To be Revisited/Subset Sum Equal To K\|Subset Sum Equal To K]], just check if the total Sum of array can be divided by 2. 
- If yes, then find a subset equal to that totalSum/2.
- If no, return false.

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int total = 0;
        for(int i = 0; i < nums.length; i++){
            total += nums[i];
        }

        if(total % 2 != 0){
            return false;
        }

        int targetSum = total/2; 

        return subsetSumToK(nums.length, targetSum, nums);
    }

    public static boolean subsetSumToK(int n, int target, int arr[]){
	    //2-D which will contain indexes from 0 to n-1 and target from 0 to target
		boolean[] prev = new boolean[target + 1];
		boolean[] curr = new boolean[target + 1];
	
		//Base case
		prev[0] = true;//true for target zero
		for(int j = 1; j <= target; j++){
			prev[j] = false;
		}
		
		curr[0] = true; // A sum of 0 is always possible
		for(int j = 1; j <= target; j++){
			curr[j] = false; //baaki toh assign false, because these will be filled in the below loop
		}
	
		for(int i = 1; i <= n; i++){
			for(int j = 1; j <= target; j++){
				//If we don't include the current element
				curr[j] = prev[j]; //Whatever the answer was till the last element
	
				//if the current target is greater than current element then only we can subtract the target
				//and check at the previous result of previous row if it was achieved or not.
				if(j >= arr[i - 1]){
					curr[j] = curr[j] || prev[j - arr[i - 1]];
				}
			}
			prev = curr.clone();
		}
	
		return prev[target];
	}
}
```