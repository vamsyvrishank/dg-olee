---
{"dg-publish":true,"permalink":"/personal-vault/dsa/graphs-revision/"}
---







| Graph Type                   | Description               |
| ---------------------------- | ------------------------- |
| Directed / Undirected        | Arrows or not             |
| Weighted / Unweighted        | Edge weights or not       |
| Cyclic / Acyclic             | Cycle detection needed?   |
| Connected / Disconnected     | All nodes reachable?      |
| DAG (Directed Acyclic Graph) | Topological sort possible |

**Representation of Graph** 

Represented through adjacency matrix and adjacency list , lets do it in both c++ and Python

```cpp

 n = no of vertices ,  m = no of edges 
 at max each vertex can have n edges 

int main() { 
	cin>> n >> m ;
	vector<int> adj[n+1];
	//for weighted
	vector<pair<int , int >> adj[n+1];
	for(int i = 0 ; i < m ; i++){ 
	
		int u, v;
		cin>>u >> v;
		adj[u].push_back(v);
		adj[v].push_back(u);

	// for weighted
	
		int u , v , weight; 
		cin >> u >> v >> weight; 
		adj[u].push_back({v , weight}); 
		adj[v].push_back({u , weight});
		
	}
	
}
```


```cpp

from collections import defaultdict 


adj_list = defaultdict(list)
edges = [(0, 1), (0, 2), (1, 2), (2, 3)]  # undirected example


for u , v in edges:
	adj[u].append(v)
	adj[v].append(u)


# Unweighted Undirected -> adjacency matrix 
n = 4
adj_matrix = [[0]*n for _ in range(n)]

edges = [(0, 1), (0, 2), (1, 2), (2, 3)]
for u, v in edges:
    adj_matrix[u][v] = 1
    adj_matrix[v][u] = 1  # omit if directed

```


**Traversal Algorithms**

```cpp


DFS 

class Solution {
  public:
    void dfs_helper( int node , vector<vector<int>> &adj , vector<int> &vis , vector<int> &ans) {
    
        vis[node] = 1;
        ans.push_back(node);
        
        for(auto i : adj[node]){
            if(!vis[i]) {
                dfs_helper(i , adj , vis , ans);
            }
        }
    }
    vector<int> dfs(vector<vector<int>>& adj) {
    
        int n = adj.size();
        vector<int> vis(n,0) , ans;
        int start = 0;
        dfs_helper(start , adj , vis , ans);
        return ans;
        
    }
};

BFS

class Solution {
  public:
    // Function to return Breadth First Traversal of given graph.
    vector<int> bfs(vector<vector<int>> &adj) {
        // Code here
        int n = adj.size();
        vector<int> vis(n,0) , ans;
        
        queue<int> q;
        int start = 0;
        vis[start] = 1;
        q.push(start);
        
        while(!q.empty()){
            int node = q.front();
            ans.push_back(node);
            q.pop();
            
            for(auto i : adj[node]){
                if(!vis[i]){
                    vis[i] = 1;
                    q.push(i);
                }
            }
        }
        return ans;
    }
};

```



### Patterns 

**DFS/BFS Template**

- [[Leetcode 200. Number of Islands\|Leetcode 200. Number of Islands]]
- [[personal vault/DSA/Graph Questions/Leetcode 695. Max Area of Island\|Leetcode 695. Max Area of Island]]
- Leetcode 133. Clone Graph

**Topological Sort**
(linear ordering of vertices such that if there is edge from u->v then u appears before v)

dfs is pre order print 
topo is post order push in stack. 

- Leetcode 207. Course Schedule
-  Leetcode 210. Course Schedule II

**Cycle Detection** 

- [[personal vault/DSA/Graph Questions/Cycle Detection in Undirected Graph GFG\|Cycle Detection in Undirected Graph GFG]]
- [[personal vault/DSA/Graph Questions/Shortest Path in Directed Acyclic Graph\|Shortest Path in Directed Acyclic Graph]] - dfs + toposorting
- Leetcode 261. Graph Valid Tree (Union Find)
-  Leetcode 785. Is Graph Bipartite?

**Shortest Path** 


- [[personal vault/DSA/Graph Questions/Dijkstra's Algorithm GFG\|Dijkstra's Algorithm GFG]] -> use when -ve weights are not there 
- [[personal vault/Misc/Bellman Ford GFG\|Bellman Ford GFG]] -> use when -ve weights are there.
- Floyd Warshall
- 0-1 BFS
- A*
- Leetcode 743. Network Delay Time (Dijkstra)
-  Leetcode 787. Cheapest Flights with K Stops (Bellman-Ford variation)
-  Leetcode 1631. Path With Minimum Effort (Binary Search + BFS)

**MST** 

- Leetcode 1584. Min Cost to Connect All Points
-  Leetcode 1135. Connecting Cities With Minimum Cost

**Union Find** 

- Leetcode 684. Redundant Connection
-  Leetcode 323. Number of Connected Components in Undirected Graph
-  Leetcode 1319. Number of Operations to Make Network Connected

**Strongly Connected Components** 

- Kosaraju’s or Tarjan’s on Leetcode 1192. Critical Connections in a Network
-  Leetcode 417. Pacific Atlantic Water Flow (DFS two-way reachability)


**2D Grid as a Graph**

- [[personal vault/DSA/Graph Questions/Leetcode 994. Rotting Oranges\|Leetcode 994. Rotting Oranges]]
- Leetcode 286. Walls and Gates
- Leetcode 1091. Shortest Path in Binary Matrix





![[graphs_notion.pdf \| 500]]




