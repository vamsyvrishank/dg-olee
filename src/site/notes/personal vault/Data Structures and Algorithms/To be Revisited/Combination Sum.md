---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/combination-sum/"}
---


Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the 

frequency

 of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150`combinations for the given input.

**Example 1:**

**Input:** candidates = [2,3,6,7], target = 7
**Output:** [[2,2,3],[7\|2,2,3],[7]]
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = [2,3,5], target = 8
**Output:** [[2,2,2,2],[2,3,3],[3,5\|2,2,2,2],[2,3,3],[3,5]]

**Example 3:**

**Input:** candidates = [2], target = 1
**Output:** []

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`

```cpp

class Solution {
public:
    void backtrack(int index,vector<int>&candidates,int target,vector<vector<int>>&ans,vector<int> poss_ans){
        if(index==candidates.size()){
            if(target==0){
                ans.push_back(poss_ans);
            }
            return;
        }

        if(candidates[index] <=target){
            poss_ans.push_back(candidates[index]);
            backtrack(index,candidates,target-candidates[index],ans,poss_ans);
            poss_ans.pop_back();
        }
        backtrack(index+1,candidates,target,ans,poss_ans);

    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans; 
        vector<int> poss_ans;
        int n = candidates.size();

        backtrack(0,candidates,target,ans,poss_ans);
        return ans;
    }
};
```

![[Pasted image 20240616132409.png \| 600]]

```python

class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        ans = []
        poss_ans = []

        def backtrack(index , target , poss_ans):
            if index==len(candidates):
                if(target==0):
                    ans.append(poss_ans[:])
                
                return ans
            
            if(candidates[index] <= target):

                poss_ans.append(candidates[index])
                backtrack(index , target-candidates[index] , poss_ans)
                poss_ans.pop()

            backtrack(index+1,target,poss_ans)

            return ans

        return backtrack(0,target,poss_ans)

```