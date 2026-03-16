---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/number-of-provinces/"}
---


##### Question:
There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return _the total number of **provinces**_.

**Example 1:**

![[Pasted image 20240623135756.png \| 500]]


**Input:** isConnected = \[[1,1,0],[1,1,0],[0,0,1\|1,1,0],[1,1,0],[0,0,1]]
**Output:** 2

**Example 2:**

![[Pasted image 20240623135819.png \| 500]]


**Input:** isConnected = \[[1,0,0],[0,1,0],[0,0,1\|1,0,0],[0,1,0],[0,0,1]]
**Output:** 3


#### Solution:
1. **Union Find**
> For Disjoint Set class copy template from [[personal vault/DSA/To be Revisited/DisJoint Set#UNION BY RANK (LESS INTUITIVE)\|DisJoint Set#UNION BY RANK (LESS INTUITIVE)]].
```java
public int findCircleNum(int[][] isConnected) {
        int V = isConnected.length;
        DisjointSet dsu = new DisjointSet(V);

        for(int i = 0; i < V; i++){
            for(int j = 0; j < V; j++){
                if(isConnected[i][j] == 1){
                    dsu.unionByRank(i, j);
                }
            }
        }

        int count = 0;
        for(int i = 0; i < V; i++){
            if(dsu.findUltimateParent(i) == i){
                count++;
            }
        }
        return count;
    }
```


2. **DFS**
```cpp
class Solution {
public:
    void dfs(int start , vector<int> adj[],vector<int> &vis){
        vis[start] = 1;
        for(auto i : adj[start]){
            if(!vis[i]){
                dfs(i,adj,vis);
            }
        }
    }
    int findCircleNum(vector<vector<int>>& isConnected) {
        //given a matrix of cities that are connected.
        int n = isConnected.size();
        
        vector<int> adj[n+1];
        vector<int> vis(n+1,0);

        for(int i = 0; i < n; i++){
            for(int j =0; j<n;j++){
                if(isConnected[i][j] and i!=j){
                    adj[i].push_back(j);
                    adj[j].push_back(i);
                }
            }
        }

        int countProvinces = 0;

        for(int i =0; i< n ; i++){
            if(!vis[i]){
                dfs(i,adj,vis);
                countProvinces++;
            }
        }
        return countProvinces;
    }
};
```