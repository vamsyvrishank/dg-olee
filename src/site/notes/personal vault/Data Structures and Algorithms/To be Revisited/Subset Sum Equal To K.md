---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/subset-sum-equal-to-k/"}
---

#### Question:

You are given an array/list ‘ARR’ of ‘N’ positive integers and an integer ‘K’. Your task is to check if there exists a subset in ‘ARR’ with a sum equal to ‘K’.

Note: Return true if there exists a subset with sum equal to ‘K’. Otherwise, return false.

**For Example :**
If ‘ARR’ is {1,2,3,4} and ‘K’ = 4, then there exists 2 subsets with sum = 4. These are {1,3} and {4}. Hence, return true.

Sample Input 1:
```
2
4 5
4 3 2 1
5 4
2 5 1 6 7
```

Sample Output 1:
```
true
false
```

Explanation For Sample Input 1:
```
In example 1, ‘ARR’ is {4,3,2,1} and ‘K’ = 5. There exist 2 subsets with sum = 5. These are {4,1} and {3,2}. Hence, return true.
In example 2, ‘ARR’ is {2,5,1,6,7} and ‘K’ = 4. There are no subsets with sum = 4. Hence, return false.
```


### Solution:

##### Recursion and Memoized
TC: O(n \* target)
SC: O(n \* target) + O(n) <-\[stack space]
```java
public class Solution {
    public static boolean subsetSumToK(int n, int target, int arr[]){
	    //2-D which will contain indexes from 0 to n-1 and target from 0 to target
	    int[][] dp = new int[n][target + 1];
	    for(int[] row: dp){
		    Arrays.fill(row, -1);
	    } 
        return subsetSumUtil(arr, 0, target, dp);
    }

	public static boolean subsetSumUtil(int[] arr, int idx, int target, int[][] dp){
		if(target == 0){
			return true;
		}
		
		if(idx >= arr.length || target < 0){
			return false;
		}
		
		if(dp[idx][target] != -1){
			return dp[idx][target] == 1;
		}

		boolean noCall = subsetSumUtil(arr, idx + 1, target, dp);
		boolean yesCall = subsetSumUtil(arr, idx + 1, target - arr[idx], dp);
		dp[idx][target] = (noCall || yesCall) ? 1 : 0;
		return noCall || yesCall;
	}
}
```

##### Tabulation:
> At (i, j) we define meaning as till ith array index can we achieve jth target ??
```java
public static boolean subsetSumToK(int n, int target, int arr[]){
	    //2-D which will contain indexes from 0 to n-1 and target from 0 to target
	boolean[][] dp = new boolean[n + 1][target + 1];

	//Base case
	for(int i = 0; i <= n; i++){
		dp[i][0] = true;
	}

	for(int i = 1; i <= n; i++){
		for(int j = 1; j <= target; j++){
			//If we don't include the current element
			dp[i][j] = dp[i - 1][j]; //Whatever the answer was till the last element

			//if the current target is greater than current element then only we can subtract the target
			//and check at the previous result of previous row if it was achieved or not.
			if(j >= arr[i - 1]){
				dp[i][j] = dp[i][j] || dp[i - 1][j - arr[i - 1]];
			}
		}
	}

	return dp[n][target];
}
```

##### Space Optimized:
```java
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

```