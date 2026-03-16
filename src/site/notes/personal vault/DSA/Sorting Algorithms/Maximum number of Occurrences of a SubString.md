---
{"dg-publish":true,"permalink":"/personal-vault/dsa/sorting-algorithms/maximum-number-of-occurrences-of-a-sub-string/"}
---


Given a string `s`, return the maximum number of occurrences of **any** substring under the following rules:

- The number of unique characters in the substring must be less than or equal to `maxLetters`.
- The substring size must be between `minSize` and `maxSize` inclusive.

**Example 1:**

**Input:** s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
**Output:** 2
**Explanation:** Substring "aab" has 2 occurrences in the original string.
It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).

**Example 2:**

**Input:** s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
**Output:** 2
**Explanation:** Substring "aaa" occur 2 times in the string. It can overlap.

**Constraints:**

- `1 <= s.length <= 105`
- `1 <= maxLetters <= 26`
- `1 <= minSize <= maxSize <= min(26, s.length)`
- `s` consists of only lowercase English letters.


Mostly Optimal Code ( we are going through the substring again , which is not good)
```cpp
class Solution {
public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) {

        int start = 0 ;
        int end = 0;
        int ans =0;
        unordered_map<string , int > umap;

        while(end < s.size()){

            if(end - start + 1 >= minSize and end-start+1 <=maxSize){
            
                string temp = s.substr(start , end-start+1);
                set<char> st;
                for(auto i : temp){
                    st.insert(i);
                }
                if(st.size() <= maxLetters){
                    umap[temp]++;
                }
                st.clear();
                start++;
            }

            end++;
        }

        for(auto i : umap){
            ans = max(ans , i.second);
        }

        return ans;
        
    }
};
```


Optimal Code ( we are not traversing the string again , we are keeping a map to store frequency /size of the substring)
```cpp
class Solution {
public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) {

        unordered_map<string , int> umap;
        unordered_map<char , int> umap_char;
        int unique_chars = 0;

        int start = 0;
        int end = 0;

        while(end < s.size()){

            umap_char[s[end]]++;

            if( (end -start + 1 <= maxSize) 
            and (end - start + 1 >= minSize))
            {
                if((umap_char.size() <= maxLetters)){
                    string temp = s.substr(start , end - start + 1);
                    umap[temp]++;
                }

                umap_char[s[start]]--;
                if(umap_char[s[start]]==0) umap_char.erase(s[start]);
                start++;
            }
            end++;
        }

        int ans = 0;
        for(auto i : umap){
            ans = max(ans , i.second);
        }

        return ans;
    }
};
```