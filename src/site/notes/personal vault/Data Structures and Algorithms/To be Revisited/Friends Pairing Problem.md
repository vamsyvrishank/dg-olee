---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/friends-pairing-problem/"}
---

Given N friends, each one can remain single or can be paired up with some other friend. Each friend can be paired only once. Find out the total number of ways in which friends can remain single or can be paired up.  
Note: Since answer can be very large, return your answer mod 10^9+7.

  
**Example 1:**

**Input:**N = 3
**Output:** 4
**Explanation**:
{1}, {2}, {3} : All single
{1}, {2,3} : 2 and 3 paired but 1 is single.
{1,2}, {3} : 1 and 2 are paired but 3 is single.
{1,3}, {2} : 1 and 3 are paired but 2 is single.
Note that {1,2} and {2,1} are considered same.


**Solution:**

```java
class Solution
{
    public long countFriendsPairings(int n) 
    { 
       //code here
       boolean[] visited = new boolean[n + 1];
       long[] res = new long[1];
       res[0] = 0;
       backtrack(1, n, visited, res);
       return res[0];
    }
    private void backtrack(int i, int n, boolean[] visited, long[] res){
        if(i > n){
            res[0] = (res[0]+1) % 1000000007;
            return;
        }
        
        if(visited[i] == true){
            backtrack(i + 1, n, visited, res); //No Call (already paired)
        } else{
            visited[i] = true;
            backtrack(i + 1, n, visited, res); //Single
            for(int j = i + 1; j <= n; j++){
                if(!visited[j]){
                    visited[j] = true;
                    backtrack(i + 1, n, visited, res);//Options call to make pairs
                    visited[j] = false;   
                }
            }
            visited[i] = false;
        }
    }
}    
 
```