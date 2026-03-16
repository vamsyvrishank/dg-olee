---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/graph/"}
---


![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/58611a7b-b79d-4c17-ad13-5bc1a040be34/Untitled.png)

Circles → nodes | vertex | vertices | V

Lines → edge ( directed or undirected )

## Path

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/d19ae898-e344-4851-8b41-8269eb403945/Untitled.png)

contain a lot of nodes and each of them are reachable.

nodes should not repeat itself.

## Degrees in a graph

**Degree of a undirected graph**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/545cae33-29d5-418c-b939-edd54182ee38/Untitled.png)

Total degree of a graph = 2 x Edges

**Degree of Directed Graph**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/d03e89f6-e2bc-4e2f-afb6-7aa12d5b4fa1/Untitled.png)

## Representing graph in C++

```cpp
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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/83ee39a6-e0a0-4033-8615-5ce94685f18e/Untitled.png)

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

**Adjacency List**

```cpp
// we have n = 5 and 1 based indexing on nodes.
// we will create an array of n+1 size.

vector<int> adj[n+1]; 
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/54c08985-4f1d-4c03-84da-e15a35bbc51e/Untitled.png)

**Space = O(2*E)** → why ? every edge has two nodes.

```cpp
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

**What if it is a weighted edge ?**

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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/c43b2d72-a7c0-47e6-bc4a-7502104c0df2/Untitled.png)

```cpp
// Adjacency List
// in the list we can store it as a pair 

//undirected graph
int main()
	int n , m ;
	cin >> n >> m;
	vector<pair<int,int>> adj[n+1];
	for(int i =0; i< m; i++) 
		int u , v , weight;
		cin >> u >> v >> weight;
		adj[u].push_back({v , weight});
		adj[v].push_back({u , weight});
		
	//space - O(2*E)
```

## Connected Components

```cpp
/* Given an undirected graph with 10 nodes and 8 edges , tell if it is connected or not.

input : 

10 8
1 2
1 3
2 4
3 4
5 6
5 7
8 9
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/53f8ca52-164c-4581-a533-6cf5424b0445/Untitled.png)

To traverse we will always use visited array , so that we make sure we are visiting all the nodes.

## Breath First Search Technique

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/e095517b-89d8-4b7c-bf63-8ce1b423389a/Untitled.png)

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

## Depth First Search

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/32d424a7-e69c-4dcd-987f-fc3b7ea9648e/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/42321e5a-b9d3-46f9-8d97-2c2d7592e9d5/Untitled.png)

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

**Number of Provinces**

```cpp
class Solution {
public:
    void dfs(int start , vector<int> &vis , vector<int> adj[]){
        vis[start] = 1;

        for(auto i : adj[start]){
            if(!vis[i]){
                dfs(i , vis , adj);
            }
        }
    }
    int findCircleNum(vector<vector<int>>& isConnected) {

        int n = isConnected.size();

        vector<int> adj[n];

        for(int i =0 ; i<n;i++){
            for(int j =0 ; j < n; j++){
                if(isConnected[i][j] and i!=j){
                    adj[i].push_back(j);
                    adj[j].push_back(i);
                }
            }
        }

        vector<int> vis(n , 0);
        int countProvinces = 0;

        for(int i = 0; i < n ; i++){
            if(!vis[i]){
                dfs(i , vis , adj);
                countProvinces++;
            }
        }
        
        return countProvinces;
    }
};

//space -> o(n) + o(n)
//time -> O(n) + O(2*E  + V)
```

**Number of Islands | No of connected components in a Matrix**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/8b04eb39-cc4c-493c-8fe2-c452d269978b/Untitled.png)

```cpp
/* you will be given an mXn matrix , where 1 represents a land and 0 represents water , you have to find out total no of islands , it is basically connected components.

*/

class Solution {
public:
    void bfs(int row , int column , vector<vector<int>> &vis , vector<vector<char>> &grid){

        vis[row][column] = 1;
        queue<pair< int , int >> q;
        q.push({row , column});

        int n = grid.size();
        int m = grid[0].size();

        while(!q.empty())
        {
            pair<int , int > temp = q.front();
            int nrow = temp.first;
            int ncol = temp.second;
            q.pop();

            // now we need to traverse the neighbours and mark them.

            vector<pair<int, int>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
            for(auto d : directions)
            {
                int neighRow = nrow + d.first;
                int neighCol = ncol + d.second;

                if(neighRow >=0 && neighCol >=0 && neighRow < n && neighCol < m && grid[neighRow][neighCol]=='1' and !vis[neighRow][neighCol]){firefo
                        vis[neighRow][neighCol] = 1;
                        q.push({neighRow , neighCol});
                    }
            }
        }
        
    }
    int numIslands(vector<vector<char>>& grid) {

        int n = grid.size();
        int m = grid[0].size();
        int count = 0;

        vector<vector<int>> vis(n , vector<int> (m,0));

        for(int row = 0; row < n ; row++){
            for(int column = 0; column < m; column++){
                if(!vis[row][column] and grid[row][column]=='1'){
                    bfs(row , column , vis , grid);
                    count++;
                }
            }
        }

        return count;
        
    }
};
```

Flood Fill

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/5e398d7e-a8e9-4ed5-b9fd-993ac5fcbd41/ce632f89-c341-4449-bebf-7de50ed7c770/Untitled.png)

```cpp
class Solution {
private:
    void dfs(int sr , int sc , int color , vector<vector<int>> &image, int iniColor , int dr[] , int dc[]){
        
        image[sr][sc] = color;

        int m = image.size();
        int n = image[0].size();

        for(int i =0 ; i< 4 ; i++){
            int nrow = sr + dr[i];
            int ncol = sc + dc[i];
            if(nrow>=0 and nrow <m and ncol>=0 and ncol<n and image[nrow][ncol] == iniColor 
            and image[nrow][ncol] != color){
                dfs(nrow , ncol , color , image , iniColor , dr , dc);
            }
        }
    }
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {

        int iniColor = image[sr][sc];
        int dr[] = {-1 , 0 , +1 , 0};
        int dc[] = {0 , -1 , 0 , +1};
        if(iniColor != color){
            dfs(sr , sc  , color , image , iniColor, dr , dc);
        }
        return image;
        
    }
};
```

Rotten Oranges

([https://leetcode.com/problems/rotting-oranges/](https://leetcode.com/problems/rotting-oranges/))

```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {

        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> vis(n , vector<int> (m , 0));
        queue< pair<  pair<int ,int> , int >> q;

        for(int i =0 ; i< n; i++){
            for(int j=0 ; j<m ;j++){

                if(grid[i][j]==2){
                    q.push({{i,j},0});
                    vis[i][j] == 2;
                }
            }
        }

        int time = 0;
        
        // (-1 , 0) , (+1 , 0) , (0 , -1) , (0 , +1)
        int dr[] = {-1,+1,0,0};
        int dc[] = {0,0,-1,+1};
        
        while(!q.empty()){
            int row = q.front().first.first;
            int col = q.front().first.second;
            int t = q.front().second;
            time = max(time , t);
            q.pop();

            for(int i=0;i<4;i++){

                int nrow = row + dr[i];
                int ncol = col + dc[i];

                if(nrow >=0 and nrow <n and ncol >=0 and ncol <m and vis[nrow][ncol] ==0 and grid[nrow][ncol]==1){
                    q.push( { {nrow , ncol} , t+1});
                    vis[nrow][ncol]=2;
                }

            }
        }

        for(int i =0 ; i< n;i++){
            for(int j= 0; j<m;j++){
                if(vis[i][j] !=2 and grid[i][j] ==1){
                    return -1;
                }
            }
        }

        return time;

        
    }
};
```

## Detecting a cycle in a graph (BFS) (Undirected Graph)