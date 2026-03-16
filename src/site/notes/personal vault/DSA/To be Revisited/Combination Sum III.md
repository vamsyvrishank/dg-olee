---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/combination-sum-iii/"}
---


Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return _a list of all possible valid combinations_. The list must not contain the same combination twice, and the combinations may be returned in any order.

**Example 1:**

**Input:** k = 3, n = 7
**Output:** [[1,2,4\|1,2,4]]
**Explanation:**
1 + 2 + 4 = 7
There are no other valid combinations.

**Example 2:**

**Input:** k = 3, n = 9
**Output:** [[1,2,6],[1,3,5],[2,3,4\|1,2,6],[1,3,5],[2,3,4]]
**Explanation:**
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.

**Example 3:**

**Input:** k = 4, n = 1
**Output:** []
**Explanation:** There are no valid combinations.
Using 4 different numbers in the range [1,9], the smallest sum we can get is 1+2+3+4 = 10 and since 10 > 1, there are no valid combination.

**Constraints:**

- `2 <= k <= 9`
- `1 <= n <= 60`


```cpp
class Solution {
public:
    void backtrack(int index , vector<int> &candidates , int target, int k , vector<vector<int>> &ans, vector<int> poss_ans){
        if(index==candidates.size()){
            if(target==0 and poss_ans.size()==k){
                ans.push_back(poss_ans);
            }
            return ;
        }

        // pick it
        if(candidates[index] <= target){
            poss_ans.push_back(candidates[index]);
            backtrack(index+1, candidates , target-candidates[index], k, ans, poss_ans);
            poss_ans.pop_back();
        }
        //dont pick it
        backtrack(index+1, candidates , target,k,ans,poss_ans);
    }
    vector<vector<int>> combinationSum3(int k, int n) {

        vector<vector<int>> ans;
        vector<int> poss_ans;
        vector<int> candidates = {1,2,3,4,5,6,7,8,9};
        int index = 0;
        backtrack(0 , candidates, n , k , ans , poss_ans);

        return ans;

        
    }
};
```



```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        
        ans = []
        poss_ans = []

        candidates = [1,2,3,4,5,6,7,8,9]

        def backtrack(index , target , poss_ans):

            if index == len(candidates):
                if target==0 and len(poss_ans)==k:
                    ans.append(poss_ans[:])
                return
            
            # case 1 - pick
            poss_ele = candidates[index]
            if poss_ele <= target:
                poss_ans.append(poss_ele)
                backtrack(index+1,target-poss_ele, poss_ans)
                poss_ans.pop()
            
            #case 2 - not pick
            backtrack(index+1,target,poss_ans)
            return

        backtrack(0,n,poss_ans)
        return ans
        
```