---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/longest-repeating-character-replacement/"}
---



You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return _the length of the longest substring containing the same letter you can get after performing the above operations_.

**Example 1:**

**Input:** s = "ABAB", k = 2
**Output:** 4
**Explanation:** Replace the two 'A's with two 'B's or vice versa.

**Example 2:**

**Input:** s = "AABABBA", k = 1
**Output:** 4
**Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of only uppercase English letters.
- `0 <= k <= s.length`

```cpp

class Solution {
public:
    int characterReplacement(string s, int k) {

        int start = 0;
        int end = 0;
        int ans =0 , maxf = 0;
        unordered_map<char , int > umap;

        while(end < s.size()){
            umap[s[end]]++;
            maxf = max(maxf , umap[s[end]]);

            if(end-start+1 - maxf > k ){
                umap[s[start]]--;
                start++;

                maxf = 0;
                for(auto i : umap){
                    maxf = max(maxf , i.second);
                }
            }
            else{
                ans = max(ans , end-start+1);
            }
            end++;
        }
        return ans;   
    }
};
```


Think about it, in order to replace characters your remaining length of the string after subtracting the maximum frequency should be less than k for us to be able to replace atmost k characters, then all the elements of the substring would be same and hence their length is one of the possible answers.
```python

class Solution:
    def characterReplacement(self, s: str, k: int) -> int:

        start = 0
        end = 0
        umap = defaultdict(int)
        maxf = -10e9
        ans = 0
        while end < len(s):
            umap[s[end]]+=1
            maxf = max(maxf ,  umap[s[end]])

            if end-start+1 -maxf <= k:
                ans = max(ans , end-start+1)
            else:
                umap[s[start]]-=1
                start+=1
            end+=1
        
        return ans

```