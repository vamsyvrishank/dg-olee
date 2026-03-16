---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/maximize-the-confusion-of-an-exam/"}
---



A teacher is writing a test with `n` true/false questions, with `'T'` denoting true and `'F'` denoting false. He wants to confuse the students by **maximizing** the number of **consecutive** questions with the **same** answer (multiple trues or multiple falses in a row).

You are given a string `answerKey`, where `answerKey[i]` is the original answer to the `ith` question. In addition, you are given an integer `k`, the maximum number of times you may perform the following operation:

- Change the answer key for any question to `'T'` or `'F'` (i.e., set `answerKey[i]` to `'T'` or `'F'`).

Return _the **maximum** number of consecutive_ `'T'`s or `'F'`s _in the answer key after performing the operation at most_ `k` _times_.

**Example 1:**

**Input:** answerKey = "TTFF", k = 2
**Output:** 4
**Explanation:** We can replace both the 'F's with 'T's to make answerKey = "TTTT".
There are four consecutive 'T's.

**Example 2:**

**Input:** answerKey = "TFFT", k = 1
**Output:** 3
**Explanation:** We can replace the first 'T' with an 'F' to make answerKey = "FFFT".
Alternatively, we can replace the second 'T' with an 'F' to make answerKey = "TFFF".
In both cases, there are three consecutive 'F's.

**Example 3:**

**Input:** answerKey = "TTFTTFTT", k = 1
**Output:** 5
**Explanation:** We can replace the first 'F' to make answerKey = "TTTTTFTT"
Alternatively, we can replace the second 'F' to make answerKey = "TTFTTTTT". 
In both cases, there are five consecutive 'T's.

**Constraints:**

- `n == answerKey.length`
- `1 <= n <= 5 * 104`
- `answerKey[i]` is either `'T'` or `'F'`
- `1 <= k <= n`


```cpp
class Solution {
public:
    int maxConsecutiveAnswers(string answerKey, int k) {

        //same approach as previous questions for variable sliding window,
        // all cases falling under else condition are part of my answer for atmost k        times.

        int start = 0;
        int end = 0 , maxf = 0 , ans = 0;
        unordered_map<char, int> umap;

        while(end < answerKey.size()){
            umap[answerKey[end]]++;
            maxf = max(maxf ,umap[answerKey[end]]);

            if(end-start+1 - maxf > k){
                umap[answerKey[start]]--;
                start++;
                
                maxf = 0;
                for(auto i : umap){
                    maxf = max(maxf , i.second);
                }
            }
            else{
                ans = max(ans, end-start+1);
            }
            end++;
        }
        return ans;
        
    }
};
```