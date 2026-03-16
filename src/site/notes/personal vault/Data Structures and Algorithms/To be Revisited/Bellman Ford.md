---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/bellman-ford/"}
---


1. The Bellman-Ford algorithm is a single-source shortest path algorithm that can handle graphs with negative edge weights
2. It is used to find the shortest path from a source vertex to all other vertices in a weighted directed graph.

Key points to know about the Bellman-Ford algorithm:

1. Purpose: To find the shortest paths from a single source vertex to all other vertices in a weighted directed graph.
2. Input: A weighted directed graph represented by an adjacency list or matrix, and a source vertex.
3. Output: The shortest distances from the source vertex to all other vertices, or a message indicating the presence of a negative cycle.
4. Time Complexity: O(V\*E), where V is the number of vertices and E is the number of edges in the graph.
5. Algorithm:
    - Initialize the distance to the source vertex as 0 and all other vertices as infinity.
    - Relax all edges |V|-1 times, where |V| is the number of vertices in the graph.
    - In each iteration, for each edge (u, v) with weight w, 
      if `distance[u] + w < distance[v]`, update ` distance[v] = distance[u] + w
    - After |V|-1 iterations, check for negative cycles by running the relaxation step once more. If any distance value changes, there is a negative cycle in the graph.
6. Relaxation: The process of updating the distance to a vertex if a shorter path is found.
7. Negative Cycles: If a graph contains a negative cycle, the Bellman-Ford algorithm will detect it after |V|-1 iterations. In such cases, the algorithm cannot provide the shortest paths.

###### Why N-1 relaxations?
1. In worst case, for every iteration we'll find one value which can be used in another iteration and in turn, another iteration and so on.
2. **So, the final distance array is the shortest path from the source node to each of it's nodes.**
 ![[Bellman .jpg \| 250]]![[Screenshot 2024-05-21 at 10.27.43 PM.png \| 500]]
###### How to detect negative cycles?
1. On nth iteration, the relaxation will be done and distance array won't get updated after that but in case, if the distance array gets updated and distance keeps on decreasing
   then we'll conclude that there's a negative cycle. 
2. In every iteration the path weight will reduce to "-1".
![[Screenshot 2024-05-21 at 10.32.37 PM.png \| 400]]

 

###### Solution: 
TC: O(VE) => where V is the number of vertices and E is the number of edges in the graph. The algorithm performs V-1 iterations, and in each iteration, it relaxes all the edges.
SC: O(V) to store the distances array.
```Java
import java.util.*;

class Edge{
	int source, destination, weight;
	
	public Edge(int source, int destination, int weight){
		this.source = source;
		this.destination = destination;
		int weight = weight;
	}
}

class BellmanFord{
	public static void bellmanFord(List<Edge> edges, int numVertices, int source) {
		int[] distances = new int[numVertices];
		Arrays.fill(distances, Integer.MAX_VALUE);

		distances[source] = 0;

		//Relax edges (numVertices - 1) times.
		for(int i = 0; i < numVertices - 1; i++){
			for(Edge e: edges){
				int u = e.source;
				int v = e.destination;
				int wt = e.weight;
				//if you find a shorter path to vertex v by going through vertex u, you update the shortest distance to vertex v.
				//or relaxing an edge(u,v) with weight w
				if(distances[u] != Integer.MAX_VALUE && distances[u] + wt < distances[v]){
					distances[v] = distances[u] + wt;
				}
			}
		}
	
		//Check for -ve cycles (because relax wala step (numVertices - 1) times 
		//karne k baad aur relax ni hota but agar ho raha hai that means there is a -ve cycle.
		for(Edge e: edges){
			int u = e.source;
			int v = e.destination;
			int wt = e.weight;
			
			if(distances[u] != Integer.MAX_VALUE && distances[u] + wt < distances[v]){
				System.out.println("Graph contains negative weight cycle");
				return;
			}
		}

		System.out.println("Shortest distances from source vertex " + source + ":");
        for (int i = 0; i < numVertices; i++) {
            System.out.println("Vertex " + i + ": " + distances[i]);
        }
	}

	public static void main(String[] args) {
        List<Edge> edges = new ArrayList<>();
        edges.add(new Edge(0, 1, -1));
        edges.add(new Edge(0, 2, 4));
        edges.add(new Edge(1, 2, 3));
        edges.add(new Edge(1, 3, 2));
        edges.add(new Edge(1, 4, 2));
        edges.add(new Edge(3, 2, 5));
        edges.add(new Edge(3, 1, 1));
        edges.add(new Edge(4, 3, -3));

        int numVertices = 5;
        int source = 0;

        bellmanFord(edges, numVertices, source);
    }
}
```