---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/prims-algorithm/"}
---


1. Prim's algorithm is specifically designed to find the MST of a weighted undirected graph *
2. Dijkstra's algorithm, on the other hand, is used to find the shortest path from a single source vertex to all other vertices in a weighted graph
3. The MST is a tree that connects all the points with the minimum total cost of edges. ⭐️ 
4. When you have a graph with non-negative edge weights use Prims.


Run this to see how PRIMS algorithm find MST.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.PriorityQueue;

public class Main {
    static class Edge {
        int src;
        int dest;//Neighbour
        int weight;

        Edge(int src, int dest, int weight) {
            this.src = src;
            this.dest = dest;//Neighbour
            this.weight = weight;
        }
    }

    static class PrimEdge {
	    int vertex;
	    int acquiringVertex;
	    int weight;
	
	    PrimEdge(int vertex, int acquiringVertex, int weight) {
	        this.vertex = vertex;
	        this.acquiringVertex = acquiringVertex;
	        this.weight = weight;
	    }
	}

    // Prim's algorithm starts
    private static void prim(ArrayList<Edge>[] graph, int src) {
	    PriorityQueue<PrimEdge> pq = new PriorityQueue<>((a, b) -> a.weight - b.weight);
	    boolean[] visited = new boolean[graph.length];
	
	    pq.add(new PrimEdge(src, -1, 0)); // vertex, acquiringVertex/parent, weight
	
	    while (!pq.isEmpty()) {
	        PrimEdge currEdge = pq.poll();
	        int currVertex = currEdge.vertex;
	        int acquiringVertex = currEdge.acquiringVertex;
	        int weight = currEdge.weight;
	
	        if (visited[currVertex]) {
	            continue;
	        }
	
	        visited[currVertex] = true;
	
	        if (acquiringVertex != -1)
	            System.out.println(currVertex + " acquired-via " + acquiringVertex + " @: " + weight);
	
	        for (Edge edge : graph[currVertex]) {
	            if (!visited[edge.dest]) {
	                pq.add(new PrimEdge(edge.dest, currVertex, edge.weight));
	            }
	        }
	    }
	}

    public static void main(String[] args) {
        // Create and populate the graph
        ArrayList<Edge>[] graph = new ArrayList[5];
        for (int i = 0; i < 5; i++) {
            graph[i] = new ArrayList<>();
        }
        
        // Add edges to the graph
        graph[0].add(new Edge(0, 1, 2));
        graph[0].add(new Edge(0, 3, 6));
        graph[1].add(new Edge(1, 0, 2));
        graph[1].add(new Edge(1, 2, 3));
        graph[1].add(new Edge(1, 3, 8));
        graph[1].add(new Edge(1, 4, 5));
        graph[2].add(new Edge(2, 1, 3));
        graph[2].add(new Edge(2, 4, 7));
        graph[3].add(new Edge(3, 0, 6));
        graph[3].add(new Edge(3, 1, 8));
        graph[4].add(new Edge(4, 1, 5));
        graph[4].add(new Edge(4, 2, 7));
        
        // Run Prim's algorithm starting from vertex 0
        prim(graph, 0);
    }
}

```