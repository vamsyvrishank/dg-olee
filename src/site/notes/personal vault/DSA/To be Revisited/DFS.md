---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/dfs/"}
---


```cpp
/* create a visited array, similar to what we did in bfs , now call the recursive function.

*/

public:

void dfs(int node , vector<int> adj[],int &vis[], vector<int> &ans)
	vis[node] = 1;
	ans.push_back(node);
	
	for(auto i : adj[node])
		if(!vis[i])
			dfs(i , adj , vis , ls);
			
			

vector<int> dfsOfGraph(int V , vector<int> adj[])
	//starting index is lets say 0
	int vis[V] = {0};
	int start = 0;

	vector<int> ans;
	dfs(start , adj , vis , ans)
	
	return ans;
	
	
	//space -> at max n nodes O(n) worst case -> O(n)
	//time complexity  -> O( 2 x E) + O(n) 
	
	
```