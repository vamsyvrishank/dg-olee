---
{"dg-publish":true,"permalink":"/personal-vault/dsa/sorting-algorithms/find-all-anagrams-in-a-string/"}
---



Given two strings `s` and `p`, return _an array of all the start indices of_ `p`_'s anagrams in_ `s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** s = "cbaebabacd", p = "abc"
**Output:** [0,6]
**Explanation:**
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".

**Example 2:**

**Input:** s = "abab", p = "ab"
**Output:** [0,1,2]
**Explanation:**
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

**Constraints:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` and `p` consist of lowercase English letters.

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {

        unordered_map<char,int> p_freq , s_freq;

        for(auto i : p){
            p_freq[i]++;
        }
        // our window would be k since anagram length should be same
        int k = p.size();

        vector<int>ans;
        int start = 0 , end = 0;

        while(end < s.size()){
            s_freq[s[end]]++;

            if(end-start+1==k){
                if(s_freq==p_freq){
                    ans.push_back(start);
                }

                s_freq[s[start]]--;
                if(s_freq[s[start]]==0) s_freq.erase(s[start]);
                start++;
            }
            end++;
        }
        return ans;   
    }
};

```