---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/graph-questions/cycle-detection-in-undirected-graph-gfg/"}
---

\  


DFS way 
```cpp
class Solution {
  public:
    bool dfs(int node , int parent , vector<int> &vis , vector<vector<int>> & adj) {
        vis[node] = 1;
        for(auto i : adj[node]){
            if(!vis[i]){
                if(dfs(i,node , vis , adj)) return true;
            }
            
            else if (i!=parent) return true;
        }
        
        return false;
    }
    bool isCycle(int V, vector<vector<int>>& edges) {
        // Code here
        
        vector<vector<int>> adj(V);
        for(auto i : edges) { 
            adj[i[0]].push_back(i[1]);
            adj[i[1]].push_back(i[0]);
    }
    
    vector<int> vis(V);
    
    for(int i=0 ; i < V ; i++){
        if(!vis[i]) {
            if(dfs(i, -1 , vis , adj)) return true;
        }
    }
    
    
    return false;
    }
};

```


BFS Way
```cpp
class Solution {
  public:
    bool bfs(int start , int parent , vector<int> &vis , vector<vector<int>> &adj){
        queue<pair<int,int>> q;
        vis[start] = 1;
        q.push({start, -1});
        
        while(!q.empty()){
            auto node = q.front().first;
            auto parent = q.front().second;
            q.pop();
            
            for(auto i : adj[node]){
                if(!vis[i]){
                    vis[i] = 1; 
                    q.push({i , node});
                }
                else if( i != parent){
                    return true;
                }
            }
        }
        return false;
    }
    bool isCycle(int V, vector<vector<int>>& edges) {
        // Code here
        
        vector<int> vis(V,0);
        vector<vector<int>> adj(V);
        for(auto i : edges){
            adj[i[0]].push_back(i[1]);
            adj[i[1]].push_back(i[0]);
        }
        
        for(int i=0; i < V ; i++) {
            if(!vis[i]){
                if(bfs(i, -1, vis, adj)) return true;
            }
            
        }
        
        return false;
        
    }
};
```