---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/ninja-training/"}
---


#### Question:

Ninja is planing this ‘N’ days-long training schedule. Each day, he can perform any one of these three activities. (Running, Fighting Practice or Learning New Moves). Each activity has some merit points on each day. As Ninja has to improve all his skills, he can’t do the same activity in two consecutive days. Can you help Ninja find out the maximum merit points Ninja can earn?

You are given a 2D array of size N*3 ‘POINTS’ with the points corresponding to each day and activity. Your task is to calculate the maximum number of merit points that Ninja can earn.

**For Example**

If the given ‘POINTS’ array is [[1,2,5], [3 ,1 ,1] ,[3,3,3] ],the answer will be 11 as 5 + 3 + 3.

**Constraints:**

```
1 <= T <= 10
1 <= N <= 100000.
1 <= values of POINTS arrays <= 100 .

Time limit: 1 sec
```

**Sample Input 1:**

```
2
3
1 2 5 
3 1 1
3 3 3
3
10 40 70
20 50 80
30 60 90
```



#### SOLUTION:

1. Recursive and Memoized
```cpp

int solve(int day , int last , vector<vector<int>> &points , vector<vector<int>> &dp ){

    if(day == 0){
        int maxi = 0;
        for(int task =0 ; task < 3 ; task++){
            if(task != last){
                maxi = max(maxi , points[0][task]);
            }
        }
        return maxi;
    }

    if(dp[day][last] != -1) return dp[day][last];

    int maxi = 0;
    for(int task =0 ; task < 3; task++){
        if(last!= task){
            int point = points[day][task] + solve(day-1 , task, points ,dp);
            maxi = max(maxi , point); 
        }
    }
    return dp[day][last] = maxi;
}

int ninjaTraining(int n, vector<vector<int>> &points)
{
    // Write your code here.
    vector<vector<int>> dp(n+1 , vector<int> (4 , -1));
    int last = 3;
    return solve(n-1, last , points , dp);
}


```

2. Tabulation (bottom up)
```java
import java.util.Arrays;

public class Solution {
    public static int ninjaTraining(int k, int[][] points) {
        int n = points.length;
        int m = points[0].length;
        int[][] dp = new int[n + 1][m]; //the additional row is used to handle the base case

        // Initialize the base case
        for (int j = 0; j < m; j++) {
            dp[n][j] = 0;
        }

        // Fill the dp array in a bottom-up manner
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j < m; j++) {
                int a1 = 0;
                if (j != 0) {
                    a1 = points[i][0] + dp[i + 1][0];
                }

                int a2 = 0;
                if (j != 1) {
                    a2 = points[i][1] + dp[i + 1][1];
                }

                int a3 = 0;
                if (j != 2) {
                    a3 = points[i][2] + dp[i + 1][2];
                }

                dp[i][j] = Math.max(a1, Math.max(a2, a3));
            }
        }

        // Find the maximum points earned starting from any activity on the first day
        int maxPoints = 0;
        for (int j = 0; j < m; j++) {
            maxPoints = Math.max(maxPoints, dp[0][j]);
        }

        return maxPoints;
    }
}
```

3. Space optimization
	1. **Initialization**: We use two 1D arrays, `prev` and `curr`, to store the intermediate results. The `prev` array represents the results for the day after the current day, and `curr` represents the results for the current day. 
	2. **Base Case**: Initialize `prev` with zeros since, on the last day, the maximum points for the next day is zero (there is no next day). 
	3. **Dynamic Programming Transition**: Iterate from the last day to the first day. For each day, calculate the maximum points for each activity considering the constraints: - If the activity is not performed on the previous day, add the points for that activity to the points of the next day. - Store the maximum of these values in the `curr` array.
	4. **Update `prev`**: After processing each day, update `prev` to be a clone of `curr` for the next iteration. 
	5. **Result**: After the loop, the `prev` array contains the maximum points starting from any activity on the first day. Find the maximum value in `prev` to get the final result. This approach reduces the space complexity from (O(n \* 3)) to \(O(3) \~ O(1)\) while maintaining the same time complexity of \(O(n \*3)\).
```java
public class Solution {
    public static int ninjaTraining(int k, int[][] points) {
        int n = points.length;
        int m = points[0].length;
        int[] prev = new int[m];
        int[] curr = new int[m];

        // Initialize the base case
        Arrays.fill(prev, 0);

        // Fill the dp array in a bottom-up manner
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j < m; j++) {
                int a1 = 0;
                if (j != 0) {
                    a1 = points[i][0] + prev[0];
                }

                int a2 = 0;
                if (j != 1) {
                    a2 = points[i][1] + prev[1];
                }

                int a3 = 0;
                if (j != 2) {
                    a3 = points[i][2] + prev[2];
                }

                curr[j] = Math.max(a1, Math.max(a2, a3));
            }
            prev = curr.clone();
        }

        // Find the maximum points earned starting from any activity on the first day
        int maxPoints = 0;
        for (int j = 0; j < m; j++) {
            maxPoints = Math.max(maxPoints, prev[j]);
        }

        return maxPoints;
    }
}
```