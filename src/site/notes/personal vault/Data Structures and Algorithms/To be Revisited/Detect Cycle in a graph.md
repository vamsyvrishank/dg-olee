---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/detect-cycle-in-a-graph/"}
---


###### Undirected Graph:
1. [[personal vault/Data Structures and Algorithms/To be Revisited/Detect Cycle in a graph#BFS (Un-directed graph)\|Detect Cycle in a graph#BFS (Un-directed graph)]]
2. [[personal vault/Data Structures and Algorithms/To be Revisited/Detect Cycle in a graph#DFS(Un-directed graph)\|Detect Cycle in a graph#DFS(Un-directed graph)]]

###### Directed Graph:
1. [[personal vault/Data Structures and Algorithms/To be Revisited/Detect Cycle in a graph#DFS(directed graph)\|Detect Cycle in a graph#DFS(directed graph)]]
2. [[personal vault/Data Structures and Algorithms/To be Revisited/Detect Cycle in a graph#BFS(directed graph) - Refer to Topological Sort\|Detect Cycle in a graph#BFS(directed graph) - Refer to Topological Sort]]

##### BFS (Un-directed graph)
```java
public class Main{
	static class Edge{
		int src;
        int nbr;
        Edge(int src,int nbr){
            this.nbr=nbr;
            this.src=src;
        }
    }

	static class Pair{
		int v;
		String psf;
		Pair(int v, String psf){
			this.v = v;
			this.psf = psf;
		}
	}

	public static boolean isCyclicBFS(ArrayList<Edge>[] graph, boolean[] visited, int src) {
	    ArrayDeque<Pair> q = new ArrayDeque<>();
	    q.add(new Pair(src, src + ""));
	    visited[src] = true; // mark the source node as visited
	
	    while (q.size() > 0) {
	        Pair rem = q.removeFirst(); // remove
	        int currV = rem.v;
	        // work - No work
	
	        // add*
	        for (Edge e : graph[currV]) {
	            if (!visited[e.nbr]) {
	                q.add(new Pair(e.nbr, rem.psf + e.nbr));
	                visited[e.nbr] = true; // mark the neighbor node as visited
	            } else {
	                return true; // cycle detected
	            }
	        }
	    }
	
	    return false;
	}
}
```


 ##### DFS(Un-directed graph)
```Java
   public class Main{
	static class Edge{
		int src;
        int nbr;
        Edge(int src,int nbr){
            this.nbr=nbr;
            this.src=src;
        }
    }

	static class Pair{
		int v;
		String psf;
		Pair(int v, String psf){
			this.v = v;
			this.psf = psf;
		}
	}

	public static boolean isCyclicDFS(ArrayList<Edge>[] graph, int v){

		boolean[] visited = new boolean[v];
		int[] parent = new int[v]; //simple "int parent = -1" se bhi kaam ho jayega. We can update parent for each src node during traversal.
		Arrays.fill(parent, -1);
		
		for(int i = 0; i < v; i++){
			if(!visited[i]){
				if(dfs(graph, visited, i, parent)){
					return true;
				}
			}
		}
		return false;
	}
	public boolean dfs(ArrayList<Edge>[] graph, boolean[] visited, int src, int[] parent){
		visited[src] = true;

		for(Edge e: graph[src]){
			if(!visited[e.nbr]){
				parent[e.nbr] = src;
				if(dfs(graph, visited, e.nbr, parent)){
					return true;
				}
			} else{
				if(parent[src] != e.nbr){ //if "int parent = -1" liya hota toh this condition would've been: (parent != e.nbr)
					return true;
				}
			}
		}
		return false;
	}
}
```
Here's an example to illustrate the difference:

![[Screenshot 2024-05-28 at 12.27.17 PM.png \| 100]]
>Suppose we start the DFS traversal from node 1. When we reach node 3, its neighbor node 1 is already visited. However, node 1 is the parent of node 3 (parent[3] = 1), so it is not considered a cycle. The condition parent[src] != e.nbr will be false, and the DFS traversal will continue.

>On the other hand, if there was an additional edge connecting nodes 4 and 5, forming a cycle, the else part would detect it. When node 5 is visited, its neighbor node 4 is already visited, but node 4 is not the parent of node 5 (parent[5] != 4). In this case, the condition parent[src] != e.nbr will be true, indicating the presence of a cycle, and the method will return true.
I hope this explanation clarifies the purpose and functionality of the else part in the dfs method.

##### DFS(directed graph)
	1. The `visited` array keeps track of the nodes that have been visited during the entire DFS traversal.
	2. The `dfsVisited` array keeps track of the nodes that have been visited in the current DFS call stack.
```java
public static boolean isCyclicDFS(ArrayList<Edge>[] graph, int v){
    boolean[] visited = new boolean[v];
    boolean[] dfsVisited = new boolean[v];
    
    for(int i = 0; i < v; i++){
        if(!visited[i]){
            if(dfs(graph, i, visited, dfsVisited)){
                return true;
            }
        }
    }
    
    return false;
}

public static boolean dfs(ArrayList<Edge>[] graph, int src, boolean[] visited, boolean[] dfsVisited){
    visited[src] = true;
    dfsVisited[src] = true;
    
    for(Edge e: graph[src]){
        if(!visited[e.nbr]){
            if(dfs(graph, e.nbr, visited, dfsVisited)){
                return true;
            }
        } else if(dfsVisited[e.nbr]){
            return true;
        }
    }
    
    dfsVisited[src] = false;
    return false;
}
```

##### BFS(directed graph) - Refer to [[personal vault/Data Structures and Algorithms/To be Revisited/Topological Sort\|Topological Sort]]