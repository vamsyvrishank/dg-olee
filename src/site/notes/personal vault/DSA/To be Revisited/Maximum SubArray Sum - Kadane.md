---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/maximum-sub-array-sum-kadane/"}
---



Given an integer array `nums`, find the 

subarray with the largest sum, and return _its sum_.


**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**

**Input:** nums = [1]
**Output:** 1
**Explanation:** The subarray [1] has the largest sum 1.

**Example 3:**

**Input:** nums = [5,4,-1,7,8]
**Output:** 23
**Explanation:** The subarray [5,4,-1,7,8] has the largest sum 23.

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.


Brute Force O(n^3)
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        //brute force O(n^3)
        int maxi  = INT_MIN;
        int n = nums.size();

        for(int i =0; i< n;i++){
            for(int j=0; j<n;j++){
                int temp = nums[i];
                for(int k=i+1 ; k<=j ; k++){
                    temp+= nums[k];
                }
                maxi = max(maxi , temp);
            }
        }

        return maxi;

    }
};

```

O(n^2)
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        
        int n = nums.size();
        int max_sum = INT_MIN;

        if(n==1) return nums[0];
    
        for(int i =0 ; i< n ; i++) { 
            int sum = nums[i];
            max_sum = max(sum , max_sum);
            for(int j =i+1 ; j < n; j++){
               sum+=nums[j];
               max_sum = max(sum , max_sum);
            }
        }
        return max_sum;
    }
};

```


Dp - o(n) , space O(n)
```cpp

class Solution {
public:
    int maxSubArray(vector<int>& nums) {

        vector<int> dp(nums.size(),0);
       

        dp[0] = nums[0];
        int ans = dp[0];
        
        for(int i = 1; i < nums.size();i++){
            int prev = dp[i-1];
            dp[i] = max(nums[i] , prev + nums[i]);
            ans = max(ans , dp[i]);
        }
        return ans;
    }
};
```

O(n) - Kadane's Algorithm
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {

		// keep track of previous
        int prev = nums[0];
        int ans = prev;
        
        for(int i = 1; i < nums.size();i++){
	        // update the curr_max to have either the new element or prev + new element
            int curr_max = max(nums[i] , prev + nums[i]);
            ans = max(ans , curr_max);
			// update the previous for next iteration
            prev = curr_max;
        }
        return ans;

        
    }
};
```



# Comparison of Ridge Regression (L2) vs Lasso Regression (L1)

| Feature                    | Ridge Regression (L2)                                                   | Lasso Regression (L1)                                                         |
| -------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Regularization Type**    | \( L2 \text{ Norm: } \sum_{j=1}^p \beta_j^2 \)                          | \( L1 \text{ Norm: } \sum_{j=1}^p \|\beta_j\| \)                              |
| **Effect on Coefficients** | Shrinks coefficients toward zero but never sets them to zero            | Shrinks coefficients and can set irrelevant ones **exactly to zero**          |
| **Feature Selection**      | No (retains all predictors)                                             | Yes (performs feature selection by eliminating irrelevant predictors)         |
| **Best When**              | Most features are relevant and/or highly correlated                     | Only a few features are truly relevant                                        |
| **Solution Method**        | Closed-form solution: \( \beta = (X^T X + \lambda I)^{-1} X^TY \)       | No closed-form; uses iterative methods (e.g., coordinate descent)             |
| **Used In**                | Portfolio optimization, factor models, multicollinearity-heavy datasets | Sparse models, feature selection in trading strategies, high-dimensional data |
| **Interpretability**       | Lower (retains all features, even irrelevant ones)                      | Higher (removes noise by discarding irrelevant features)                      |



