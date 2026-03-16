---
{"dg-publish":true,"permalink":"/personal-vault/dsa/graph-questions/shortest-path-in-directed-acyclic-graph/"}
---




![Pasted image 20250611015510.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020250611015510.png)

```cpp
// User function Template for C++
typedef pair<int,int> pii;
class Solution {
  public:
    void dfs(int node ,vector<vector<pii >> &adj , vector<int> &vis , stack<int> & st ){
        vis[node] = 1;
        for(auto neighbours : adj[node]){
            int v = neighbours.first;
            if(!vis[v]){
                dfs(v , adj , vis , st);
            }
        }
        
        st.push(node);
    }
    vector<int> shortestPath(int V, int E, vector<vector<int>>& edges) {
        // code here
        
        vector<vector<pii >> adj(V);
        for(auto edge : edges){
            adj[edge[0]].push_back( { edge[1] , edge[2]});
        }
        vector<int> vis(V,0);
        
        stack<int> st;
        
        for(int i =0 ; i < V ; i++){
            if(!vis[i]){
                dfs(i , adj,vis,st);
            }
        }
        
        vector<int> dist(V,INT_MAX);
        dist[0] = 0;
        
        while(!st.empty()){
            int u = st.top();
            st.pop();
            
            for(auto node : adj[u]){
                int v = node.first;
                int w = node.second;
                
                if(dist[u] != INT_MAX and dist[u] + w < dist[v] ){
                    dist[v] = dist[u] + w;
                }
            }
            
        }
        
        for(auto &i : dist){
            if(i==INT_MAX){
                i = -1;
            }
        }
        
        return dist;
        
        
    }
};

```