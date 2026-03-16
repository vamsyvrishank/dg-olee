---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/graph-questions/dijkstra-s-algorithm-gfg/"}
---



```cpp

// User Function Template
typedef pair<int,int> pii;
class Solution {
  public:
    vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
        // Code here
        vector<int> dist(V,INT_MAX);
        dist[src] = 0;
        
        vector<vector< pii >> adj(V);
        
        //construct adj
        for(auto edge : edges){
            int u = edge[0];
            int v = edge[1];
            int w = edge[2];
            adj[u].push_back({v,w});
        }
        
        //construct minheap
        priority_queue< pii , vector<pii> , greater<pii>> pq;
        pq.push({0,src });
        
        while(!pq.empty()){
            
            int u = pq.top().second;
            pq.pop();
            
            for(auto edge : adj[u]){
                int v = edge.first;
                int w = edge.second;
                
                if(dist[u] + w < dist[v]){
                    dist[v] = dist[u] + w;
                    pq.push({dist[v] , v });
                }
            }
        }
        
        return dist;
        
        
        
    }
};
```