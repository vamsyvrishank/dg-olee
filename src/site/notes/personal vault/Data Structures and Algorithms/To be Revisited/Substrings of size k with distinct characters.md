---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/substrings-of-size-k-with-distinct-characters/"}
---



- [1876. Substrings of Size Three with Distinct Characters](https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters/)
    
    A string is **good** if there are no repeated characters.
    
    Given a string `s`, return _the number of **good substrings** of length **three** in_ `s`.
    
    Note that if there are multiple occurrences of the same substring, every occurrence should be counted.
    
    A **substring** is a contiguous sequence of characters in a string.
    
    **Example 1:**
    
    ```
    Input: s = "xyzzaz"
    Output: 1
    Explanation: There are 4 substrings of size 3: "xyz", "yzz", "zza", and "zaz".
    The only good substring of length 3 is "xyz".
    
    ```
    
    **Example 2:**
    
    ```
    Input: s = "aababcabc"
    Output: 4
    Explanation: There are 7 substrings of size 3: "aab", "aba", "bab", "abc", "bca", "cab", and "abc".
    The good substrings are "abc", "bca", "cab", and "abc".
    
    ```
    
    **Constraints:**
    
    - `1 <= s.length <= 100`
    - `s` consists of lowercase English letters.

	Complexity is O(k* n) - cuz it checks the (n-k) substrings (k) times
    ```cpp
    class Solution {
    private:
        bool isGood(string temp){
            unordered_map<char , int> umap;
            for(auto  i : temp){
                umap[i]++;
                if(umap[i] > 1) return false;
            }
            return true;
        }
    public:
        int countGoodSubstrings(string s) {
            int k = 3;
            int start = 0 , end = 0;
            
            int count = 0;
    
            while(end < s.size()){
    
                if(end - start + 1 == k){
                    string temp = s.substr(start , end-start+1);
    
                    if(isGood(temp)) count++;
    
                    start++;
                }
    
                end++;
            }
            return count;
        }
    };
    
    ```


Optimal Solution
```cpp
class Solution {
public:
    int countGoodSubstrings(string s) {

        int start = 0;
        int end = 0;
        int k = 3;
        int count = 0;
        unordered_map<char, int> umap;
        
        while(end < s.size()){

            umap[s[end]]++;

            if(end - start + 1 == k){
                if(umap.size()==k) count++;

                umap[s[start]]--;
                //if(umap[s[start]]==0) umap.erase(s[start]);
                start++;
            }
            end++;
        }
        return count;
        
    }
};
```


Python code 

```python

class Solution:
    def countGoodSubstrings(self, s: str) -> int:

        start = 0
        end = 0
        n = len(s)
        umap = defaultdict(int)
        count = 0

        while end < n:
            umap[s[end]]+=1

            if end-start+1==3:
                if len(umap)==3:
                    count+=1
                #shrink
                umap[s[start]]-=1
                #remove key if value = 0
                if umap[s[start]]==0:
                    del umap[s[start]]

                start+=1
            end+=1
        
        return count 
```