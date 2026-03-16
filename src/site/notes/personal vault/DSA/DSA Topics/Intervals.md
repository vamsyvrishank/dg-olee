---
{"dg-publish":true,"permalink":"/personal-vault/dsa/dsa-topics/intervals/"}
---


Must : Sort the intervals by their start time.


### The General Algorithm Template

1. **Sort** the intervals based on `start`.
2. **Initialize** a container (like a list or a "last seen" interval) with the first interval.
3. **Iterate** through the rest:
    - If `current.start <= last.end`, they overlap. **Merge** them by updating the end time: `last.end = max(last.end, current.end)`.
    - Otherwise, they don't overlap. Add the current interval as a new entry.



### Phase 1 -  Basic Merging and Inserting 
#####  Merge Intervals



Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

 

Example 1:

Input: intervals = [[1,3],[2,6],[8,10],[15,18\|1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18\|1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

Example 2:

Input: intervals = [[1,4],[4,5\|1,4],[4,5]]
Output: [[1,5\|1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.

Example 3:

Input: intervals = [[4,7],[1,4\|4,7],[1,4]]
Output: [[1,7\|1,7]]
Explanation: Intervals [1,4] and [4,7] are considered overlapping.

 

Constraints:

    1 <= intervals.length <= 104
    intervals[i].length == 2
    0 <= starti <= endi <= 104



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
##### Insert Intervals


You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

Note that you don't need to modify intervals in-place. You can make a new array and return it.

 

Example 1:

Input: intervals = [[1,3],[6,9\|1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9\|1,5],[6,9]]

Example 2:

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16\|1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16\|1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

 

Constraints:

    0 <= intervals.length <= 104
    intervals[i].length == 2
    0 <= starti <= endi <= 105
    intervals is sorted by starti in ascending order.
    newInterval.length == 2
    0 <= start <= end <= 105

 

```cpp

class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& interval, vector<int>& newInterval) {

        int n = interval.size();
        vector<vector<int>> merged;

        int i = 0;

        //add all the non overlapping intervals before newInterval
        while( i < n and interval[i][1] < newInterval[0]){
            merged.push_back(interval[i]);
            i++;
        }

        //merged all the overlapping intervals
        while(i < n and interval[i][0] <= newInterval[1]){
            newInterval[0] = min(interval[i][0] , newInterval[0]);
            newInterval[1] = max(interval[i][1] , newInterval[1]);
            i++;
        }
        //add the newInterval to the merged list
        merged.push_back(newInterval);

        //add the remaining intervals
        while(i < n){
            merged.push_back(interval[i]);
            i++;
        }

        return merged;


        
    }
};
```



### Phase 2 - Counting and Optimization 


