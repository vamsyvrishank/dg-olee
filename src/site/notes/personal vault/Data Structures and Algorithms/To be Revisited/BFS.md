---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/bfs/"}
---


Only read the BFS part if in hurry.
- BFS explores the graph level by level, starting from the source node and progressively moving to nodes at increasing distances.
- It processes all the neighbors of the current node before moving on to the next level of nodes. 
  In other words, it visits all the nodes at a distance of 1 from the source, then all the nodes at a distance of 2, and so on.
- This level-by-level exploration ensures that BFS reaches each node in the shortest possible number of steps (edges) from the source.
- The queue follows the FIFO order, meaning that the nodes are processed in the order they are discovered.
- This FIFO order guarantees that the nodes closer to the source are processed before the nodes farther away.
r m\* w a\*
(remove, mark\*, work, add\*)

```cpp

/* input : starting node given ,

1. queue needed
2. visisted array

*/

vector<int> bfsOfGraph(int V , vector<int> adj[])
	// zero based indexing 
	int vis[n] = {0};
	vis[0] = 1;
	
	queue<int> q;
	q.push(0);
	
	vector<int> bfs ; 
	
	while(!q.empty())
		int node = q.front();
		q.pop();
		bfs.push_back(node);
		
		for(auto i : adj[node])
			if(!vis[i])
				vis[i] =1;
				q.push(i);
				
	return bfs;


// space -> O(3*n) 
// time -> O(n) ( queue )  + O(2*E) ( nodes in edges ) 

```