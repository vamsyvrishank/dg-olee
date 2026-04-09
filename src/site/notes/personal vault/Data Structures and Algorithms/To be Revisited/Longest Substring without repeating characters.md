---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/longest-substring-without-repeating-characters/"}
---



Given a string `s`, find the length of the **longest** **substring** without repeating characters.

**Example 1:**

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

**Example 2:**

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

**Example 3:**

**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.



```cpp

class Solution {
public:
    int lengthOfLongestSubstring(string s) {

        int start = 0, end =0 , ans = 0;

        unordered_map<char , int > umap;

        while(end < s.size()){
            umap[s[end]]++;

            while(umap[s[end]] > 1){
                umap[s[start]]--;
                start++;
            }

            ans = max(ans, end - start + 1);
            end++;
        }

        return ans;

    }
};
```


longest valid window, 
we check while the window is invalid we to keep ignoring based on the question.
as soon as it becomes valid compute the answer.

we used while inside the while.  why ? because if a single operation using if would always give us valid answer , then we can do it, but even after the ``` umap[s[start]]-=1```  we might see ``` umap[s[end]] > 1``` right , so we use while instead of if. 



```python

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        start = 0
        end = 0
        umap = defaultdict(int)
        ans = 0
        while end < len(s):
            umap[s[end]]+=1

            while umap[s[end]] >= 2:
                umap[s[start]]-=1
                if umap[s[start]]==0:
                    del umap[s[start]]
                start+=1
            ans = max(ans , end-start+1)
            end+=1

        return ans
```