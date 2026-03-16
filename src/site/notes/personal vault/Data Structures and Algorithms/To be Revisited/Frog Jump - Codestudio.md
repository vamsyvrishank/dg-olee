---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/frog-jump-codestudio/"}
---


## Problem statement 

There is a frog on the '1st' step of an 'N' stairs long staircase. The frog wants to reach the 'Nth' stair. 'HEIGHT\[i]' is the height of the '(i+1)th' stair.If Frog jumps from 'ith' to 'jth' stair, the energy lost in the jump is given by absolute value of ( HEIGHT\[i-1] - HEIGHT\[j-1] ). If the Frog is on 'ith' staircase, he can jump either to '(i+1)th' stair or to '(i+2)th' stair. Your task is to find the minimum total energy used by the frog to reach from '1st' stair to 'Nth' stair.

**For Example**

```
If the given ‘HEIGHT’ array is [10,20,30,10], the answer 20 as the frog can jump from 1st stair 
to 2nd stair (|20-10| = 10 energy lost) and then a jump from 2nd stair to last stair (|10-20| = 10 energy lost). 
So, the total energy lost is 20.
```

###### Sample Input 1:

```
2
4
10 20 30 10
3
10 50 10
```

###### Sample Output 1:

```
20
0
```

###### Explanation of sample input 1:

```
For the first test case,
The frog can jump from 1st stair to 2nd stair (|20-10| = 10 energy lost).
Then a jump from the 2nd stair to the last stair (|10-20| = 10 energy lost).
So, the total energy lost is 20 which is the minimum. 
Hence, the answer is 20.

For the second test case:
The frog can jump from 1st stair to 3rd stair (|10-10| = 0 energy lost).
So, the total energy lost is 0 which is the minimum. 
Hence, the answer is 0.
```

##### Solution:
1. Memoized recursive solution: (TOP DOWN)
	1. SC = O(N) + O(N) \[memo + stack space]
	2. TC = O(N)
```cpp

//{ Driver Code Starts
#include <bits/stdc++.h>
using namespace std;


// } Driver Code Ends
#include <bits/stdc++.h> 
int solve(int n , vector<int> &heights , vector<int> & dp){
    //base case
    if(n==0) return 0;

    if(dp[n] != -1) return dp[n];
    int first_jump = abs(heights[n] - heights[n-1]) + solve(n-1,heights,dp);
    int second_jump= INT_MAX;

    if(n>=2){
        second_jump = abs(heights[n]- heights[n-2]) + solve(n-2,heights,dp);
    }

    //driver
    dp[n] = min(first_jump , second_jump);

    return dp[n];
}
int frogJump(int n, vector<int> &heights)
{
    // Write your code here.
    vector<int> dp(n+1, -1);
    return solve(n-1 , heights , dp);

    
}

[] => overlapping

				0
			/       \
		  1          [2]			
		/    \      / 
	   2   [3]      [3]
	   /      
	  3
```

2. Tabulation solution:(BOTTOM UP)
	- SC = O(N)
	- TC = O(N)
``` java
public class Solution {
    public static int func(int[] heights, int[] dp){
        int n = heights.length;
		
		//Base cases
		dp[n - 1] = 0;
        dp[n - 2] = Math.abs(heights[n - 2] - heights[n - 1]);
	    
	    // start filling from right to left
		for(int i = n - 3; i >= 0; i--){
			int jumpOne = dp[i + 1] + Math.abs(heights[i + 1] - heights[i]);
			int jumpTwo = dp[i + 2] + Math.abs(heights[i + 2] - heights[i]);
			dp[i] = Math.min(jumpOne, jumpTwo);
		}

		return dp[0];
	}
	public static int frogJump(int n, int[] heights){
		int[] dp = new int[n];
		Arrays.fill(dp, Integer.MAX_VALUE);
		return func(heights, dp);
	}

}
```

3. Space optimization
	1. SC = O(1)
	2. TC = O(N)
```java
public class Solution {
	public static int frogJump(int n, int[] heights){
		//Base cases
		int last = 0;//dp[n-1]
        int secondLast = Math.abs(heights[n - 2] - heights[n - 1]); //dp[n-2]
	    int curr = Integer.MAX_VALUE;
	    // start filling from right to left
		for(int i = n - 3; i >= 0; i--){
			int jumpOne = secondLast + Math.abs(heights[i + 1] - heights[i]);
			int jumpTwo = last + Math.abs(heights[i + 2] - heights[i]);
			curr = Math.min(jumpOne, jumpTwo);
			
			last = secondLast;
			secondLast = curr;
		}

		return curr;
	}
}
```