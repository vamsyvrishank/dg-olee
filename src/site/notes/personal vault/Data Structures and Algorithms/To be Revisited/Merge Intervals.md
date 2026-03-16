---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/merge-intervals/"}
---

**Question:**
	Given an array of intervals where intervals[i] = \[starti, endi\], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.
	
	Example 1:
	Input: intervals = \[[1,3],[2,6],[8,10],[15,18\|1,3],[2,6],[8,10],[15,18]]
	Output: \[[1,6],[8,10],[15,18\|1,6],[8,10],[15,18]]
	Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
	
	Example 2:
	Input: intervals = \[[1,4],[4,5\|1,4],[4,5]]
	Output: \[[1,5\|1,5]]
	Explanation: Intervals [1,4] and [4,5] are considered overlapping.
	
	Constraints:
	1 <= intervals.length <= 10^4
	intervals[i].length == 2
	0 <= starti <= endi <= 10^4


Solution:
```cpp

class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        sort(intervals.begin() , intervals.end());

        vector<vector<int>> merged;
        
        int n = intervals.size();
        for(int i = 0; i < n ; i++){
            // non overlapping last.end < current.start
            if(merged.empty() || merged.back()[1] < intervals[i][0]){
                merged.push_back(intervals[i]);
            }
            else{
                // overlapping hai
                merged.back()[1] = max(merged.back()[1] , intervals[i][1]);
            }
        }

        return merged;
        
    }
};
```

