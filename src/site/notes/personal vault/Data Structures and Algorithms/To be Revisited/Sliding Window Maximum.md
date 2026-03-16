---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/sliding-window-maximum/"}
---



You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

**Example 1:**

**Input:** nums = [1,3,-1,-3,5,3,6,7], k = 3
**Output:** [3,3,5,5,6,7]
**Explanation:** 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       **3**
 1 [3  -1  -3] 5  3  6  7       **3**
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       **5**
 1  3  -1  -3 [5  3  6] 7       **6**
 1  3  -1  -3  5 [3  6  7]      **7**

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`


> [!NOTE] Important
> This is an interesting problem inside sliding window , how do we keep track of max and min elements in a window while moving forward ? we use monotonic queue , increasing and decreasing


```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int start = 0 , end = 0 , curr_max = INT_MIN ;
        priority_queue<pair<int ,int >> pq;
        vector<int>ans;


        while(end < nums.size()){
            
            pq.push({nums[end] , end});

            if(end - start + 1 == k ){

                ans.push_back(pq.top().first);

                while(!pq.empty() and pq.top().second  <= start) pq.pop();
                start++;
            }
            end++;
        }

        return ans;
        
    }
};
```


Can we optimise this further ?  Yes we can using deque , monotonically decreasing deque , 

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int start = 0 , end = 0 , curr_max = INT_MIN ;
        vector<int>ans;
        // basically we need a queue so that every front element is greater than next in queue
        deque<int> desc;
        while(end < nums.size()){
            
            while(!desc.empty() and nums[desc.back()] < nums[end]) desc.pop_back();
            desc.push_back(end);

            if(desc.front() < start){
                desc.pop_front();
            }

            if(end - start + 1 == k ){

                ans.push_back(nums[desc.front()]);
                start++;
            }
            end++;
        }

        return ans;
        
    }
};
```