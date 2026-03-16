---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/representation-of-graph-c/"}
---



```
/* input : n , m = 5 , 6 
 n nodes and m edge are given
 in the next lines they will give m lines that would represent edges. 
 2 1
 1 3
 2 4
 3 4
 2 5 
 4 5
 
 store ? 
 
 1. Matrix way 
 adjacency matrix 
 
 adj[n+1][n+1]
 
 Space = n x n ( costly) 
```

![[Pasted image 20240623134912.png \| 600]]

Adjacency Matrix 
```cpp
//storage
int main()
	int n , m ;
	cin >> n >> m;
	int adj[n+1][m+1];
	for(int i =0; i<m;i++)
		int u , v;
		cin >> u >> v;
		adj[u][v] = 1;
		adj[v][u] = 1;
	
	return 0;

```




![[Pasted image 20240623134850.png \| 600]]
Adjacency List
```cpp

// we have n = 5 and 1 based indexing on nodes.
// we will create an array of n+1 size.

vector<int> adj[n+1]; 
//undirected graph
int main()
	int n , m ;
	cin >> n >> m;
	vector<int> adj[n+1];
	for(int i =0; i< m; i++) 
		int u , v;
		cin >> u >> v;
		adj[u].push_back(v);
		adj[v].push_back(u);
		
	//space - O(2*E)
		
//directed graph
int main()
	int n , m ;
	cin >> n >> m;
	vector<int> adj[n+1];
	for(int i =0; i< m; i++) 
		int u , v;
		cin >> u >> v;
		// u --> v edge exists 
		adj[u].push_back(v);
		//adj[v].push_back(u);
		
	//Space - O(E) 
	
```


Weighted Edge

```cpp

// Matrix 
// we store the weight instead of 1 in the code / for matrix 
//storage
int main()
	int n , m ;
	cin >> n >> m;
	int adj[n+1][m+1];
	for(int i =0; i<m;i++)
		int u , v;
		cin >> u >> v;
		adj[u][v] = weight;
		adj[v][u] = weight;
	
	return 0;
```