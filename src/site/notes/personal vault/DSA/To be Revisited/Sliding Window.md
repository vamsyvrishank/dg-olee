---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/sliding-window/"}
---



Two types

- Fixed size
- Variable size ( if sum is 5 , return the max(length) of subarray whose sum is 5)

Identification :

subarray + window_size + largest ? smallest = possibility of sliding window

Identify the type - fixed or variable

Problems :

fixed :

- max/min sub array of size k
- 1st -ve in every min size of k
- count occurance of anagram
- max of all subarrays of size k
- max of min for every window size

variable :

- largest substring with k distinct character
- length of largest substring with no repeating characters
- pick toy
- minimum window substring

[https://leetcode.com/discuss/interview-question/3722472/mastering-sliding-window-technique-a-comprehensive-guide](https://leetcode.com/discuss/interview-question/3722472/mastering-sliding-window-technique-a-comprehensive-guide)



## Fixed Window


- [1456. Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)
    
    Given a string `s` and an integer `k`, return _the maximum number of vowel letters in any substring of_ `s` _with length_ `k`.
    
    **Vowel letters** in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.
    
    **Example 1:**
    
    ```
    Input: s = "abciiidef", k = 3
    Output: 3
    Explanation: The substring "iii" contains 3 vowel letters.
    
    ```
    
    **Example 2:**
    
    ```
    Input: s = "aeiou", k = 2
    Output: 2
    Explanation: Any substring of length 2 contains 2 vowels.
    
    ```
    
    **Example 3:**
    
    ```
    Input: s = "leetcode", k = 3
    Output: 2
    Explanation: "lee", "eet" and "ode" contain 2 vowels.
    
    ```
    
    ```cpp
    class Solution {
    public:
        int maxVowels(string s, int k) {
    
           //better code than previous submissions
            set<char> st = {'a','e','i','o','u'};
    
            unordered_map< char , int > umap;
            int start = 0 , end = 0;
    
            int temp = 0;
            int ans = 0;
            while(end < s.size()){
    
                if(st.contains(s[end])) {
                    temp++;
                }
                if( end - start + 1 == k){
                    ans = max(ans , temp);
                    if(st.contains(s[start])){
                        temp--;
                    }
                    start++;
                }
                end++;
            }
    
            return ans;
            
        }
    };
    ```
    
- [1297. Maximum Number of Occurrences of a Substring](https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring/)
    
    Given a string `s`, return the maximum number of occurrences of **any** substring under the following rules:
    
    - The number of unique characters in the substring must be less than or equal to `maxLetters`.
    - The substring size must be between `minSize` and `maxSize` inclusive.
    
    **Example 1:**
    
    ```
    Input: s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
    Output: 2
    Explanation: Substring "aab" has 2 occurrences in the original string.
    It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).
    
    ```
    
    **Example 2:**
    
    ```
    Input: s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
    Output: 2
    Explanation: Substring "aaa" occur 2 times in the string. It can overlap.
    
    ```
    
    **Constraints:**
    
    - `1 <= s.length <= 105`
    - `1 <= maxLetters <= 26`
    - `1 <= minSize <= maxSize <= min(26, s.length)`
    - `s` consists of only lowercase English letters.
    
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
    
- [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
    
    You are given an integer array `nums` consisting of `n` elements, and an integer `k`.
    
    Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return _this value_. Any answer with a calculation error less than `10-5` will be accepted.
    
    **Example 1:**
    
    ```
    Input: nums = [1,12,-5,-6,50,3], k = 4
    Output: 12.75000
    Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
    
    ```
    
    **Example 2:**
    
    ```
    Input: nums = [5], k = 1
    Output: 5.00000
    
    ```
    
    **Constraints:**
    
    - `n == nums.length`
    - `1 <= k <= n <= 105`
    - `104 <= nums[i] <= 104`
    
    ```cpp
    class Solution {
    public:
        double findMaxAverage(vector<int>& nums, int k) {
    
        /*    brute force - gives TLE
            double ans=INT_MIN, temp=0;
            for(int i =0 ; i< nums.size();i++){
                temp = 0;
                for(int j=i ; j< nums.size();j++){
                    temp+=nums[j];
                    if(j-i+1 ==k){
                        double average = temp/((double)(j-i+1));
                        ans = max(ans , average);
                        break;
                    }
                }
            }
    
            return ans;
            */
    
            int start = 0;
            int end = 0;
            double ans=INT_MIN, temp=0;
    
            while(end < nums.size()){
                temp+=nums[end];
    
                if(end - start + 1 == k){
                    double average = temp /((double)(end-start+1));
                    ans = max(ans , average);
    
                    temp-= nums[start];
                    start++;
                }
    
                end++;
            }
            
            return ans;
           
    
        }
    
    };
    ```
    

## Variable Window

- [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
    
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
    
- [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
    
    ```cpp
    class Solution {
    public:
        int characterReplacement(string s, int k) {
            
            int start = 0 , end = 0 , ans = 0 , maxf =0;
            unordered_map<char, int> umap;
    
            while(end < s.size()){
                umap[s[end]]++;
                maxf = max(maxf , umap[s[end]]);
    
                if((end-start+1)- maxf > k){
    
                    umap[s[start]]--;
                    start++;
    
                    maxf = 0;
                    for(auto i : umap){
                        maxf = max(maxf , i.second);
                    }
                }
                else{
                    ans = max(ans , end - start + 1);
                }
                end++;
            }
    
            return ans;
        }
    };
    
    ```
    
- [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
    
    You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.
    
    Return _the max sliding window_.
    
    **Example 1:**
    
    ```
    Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
    Output: [3,3,5,5,6,7]
    Explanation:
    Window position                Max
    ---------------               -----
    [1  3  -1] -3  5  3  6  7       3
     1 [3  -1  -3] 5  3  6  7       3
     1  3 [-1  -3  5] 3  6  7       5
     1  3  -1 [-3  5  3] 6  7       5
     1  3  -1  -3 [5  3  6] 7       6
     1  3  -1  -3  5 [3  6  7]      7
    ```
    
    **Example 2:**
    
    ```
    Input: nums = [1], k = 1
    Output: [1]
    
    ```
    
    **Constraints:**
    
    - `1 <= nums.length <= 105`
    - `104 <= nums[i] <= 104`
    - `1 <= k <= nums.length`
    
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