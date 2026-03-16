---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/combination-sum-ii/"}
---


Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = [10,1,2,7,6,1,5], target = 8
**Output:** 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

**Example 2:**

**Input:** candidates = [2,5,2,1,2], target = 5
**Output:** 
[
[1,2,2],
[5]
]

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

```cpp


class Solution {
public:
    void backtrack(int index, vector<int>& candidates, int target,
                   vector<vector<int>>& ans, vector<int> poss_ans) {
        if (index == candidates.size()) {
            if (target == 0) {
                ans.push_back(poss_ans);
            }
            return;
        }

        if (candidates[index] <= target) {
            poss_ans.push_back(candidates[index]);
            backtrack(index + 1, candidates, target - candidates[index], ans,
                      poss_ans);
            poss_ans.pop_back();
        }
		// we have explored all the path where we picked the element to form our ans , so after the above while , we 
		// can ignore the repeating elements and explore the path of non repeating ones , this way we can avoid same answers at the end of backtracking tree
		
        while (index + 1 <= candidates.size() - 1 and
               candidates[index] == candidates[index + 1]) {
            index++;
        }
        backtrack(index + 1, candidates, target, ans, poss_ans);
    }
    
    set<vector<int>> st;
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        vector<int> poss_ans;

        int n = candidates.size();

        sort(candidates.begin(), candidates.end());
        backtrack(0, candidates, target, ans, poss_ans);

        return ans;
    }
};

// [[1,2,2,2,5]]

// [1,2]
```


```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:

        ans = []
        poss_ans = []
        candidates.sort()

        def backtrack(index , target , poss_ans):
            if index == len(candidates):
                if target == 0:
                    ans.append(poss_ans[:])
                return
            
            poss_ele = candidates[index]
            if poss_ele <= target:
                poss_ans.append(poss_ele)
                backtrack(index+1 , target - poss_ele , poss_ans)
                poss_ans.pop()
            
            while (index+1 <= len(candidates)-1) and (candidates[index]==candidates[index+1]):
                index+=1

            backtrack(index+1,target,poss_ans)
            return

        backtrack(0,target,poss_ans)
        
        return ans
    
```