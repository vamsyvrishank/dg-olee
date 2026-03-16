---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/minimum-recolors-to-get-k-consecutive-black-blocks/"}
---


You are given a **0-indexed** string `blocks` of length `n`, where `blocks[i]` is either `'W'` or `'B'`, representing the color of the `ith` block. The characters `'W'` and `'B'` denote the colors white and black, respectively.

You are also given an integer `k`, which is the desired number of **consecutive** black blocks.

In one operation, you can **recolor** a white block such that it becomes a black block.

Return _the **minimum** number of operations needed such that there is at least **one** occurrence of_ `k` _consecutive black blocks._

**Example 1:**

**Input:** blocks = "WBBWWBBWBW", k = 7
**Output:** 3
**Explanation:**
One way to achieve 7 consecutive black blocks is to recolor the 0th, 3rd, and 4th blocks
so that blocks = "BBBBBBBWBW". 
It can be shown that there is no way to achieve 7 consecutive black blocks in less than 3 operations.
Therefore, we return 3.

**Example 2:**

**Input:** blocks = "WBWBBBW", k = 2
**Output:** 0
**Explanation:**
No changes need to be made, since 2 consecutive black blocks already exist.
Therefore, we return 0.

**Constraints:**

- `n == blocks.length`
- `1 <= n <= 100`
- `blocks[i]` is either `'W'` or `'B'`.
- `1 <= k <= n`


```cpp
class Solution {
public:
    int minimumRecolors(string blocks, int k) {

        int start = 0;
        int end = 0;
        int blackblock =0 , ans = INT_MAX;
        
        //looked like variable window , but it is fixed window
        while(end < blocks.size()){

            if(blocks[end]=='B'){
                blackblock++; //(black clock big t-shirt billie eilish)
            }

            if(end-start+1==k){
                if(blackblock==k){
                    return 0;
                }
                ans = min(ans , end-start+1 - blackblock);
                if(blocks[start]=='B'){
                    blackblock--;
                }
                start++;
            }
            end++;
        }

        return ans;
    }
};
```