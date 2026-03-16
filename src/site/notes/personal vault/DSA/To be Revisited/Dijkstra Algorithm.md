---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/dijkstra-algorithm/"}
---

1. It's same as BFS, but Queue is replaced by PriorityQueue!
2. It is used to find the shortest path from a single source vertex to all other vertices in a weighted graph. 
   It calculates the minimum distance or cost to reach each vertex from the source vertex.
   
   ![[Dijkstra Negative Cycle Issue!\| 600]]

>[!warning] When not to use Dijkstra's algorithm?
> - **Negative cycle:**  Dijkstra's algorithm will give TLE for  graphs with -ve cycles (cycles with overall negative weight), it'll keep on running & running. Jitna uss cycle pe ghumenge utna overall cost kam hota rahega!
> - **Negative edge weights**: Dijkstra's algorithm does not work correctly when the graph contains negative edge weights. In such cases, you should use algorithms like 
   *Bellman-Ford or Floyd-Warshall* algorithm. (Refer, the above diagram)
>- **All-pairs shortest paths:** If you need to find the shortest paths between all pairs of vertices in the graph, Dijkstra's algorithm is not the most efficient choice. 
   Floyd-Warshall algorithm would be more suitable for all-pairs shortest paths.
>- **Dense graphs:** For dense graphs, where the number of edges is close to the square of the number of vertices, Dijkstra's algorithm may not be the most efficient. 
   In such cases, algorithms like Floyd-Warshall algorithm might perform better.
>- **Unweighted graphs:** BFS is enough.


``` Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.PriorityQueue;

public class Main {
    static class Edge {
        int src;
        int nbr;
        int wt;

        Edge(int src, int nbr, int wt) {
            this.nbr = nbr;
            this.src = src;
            this.wt = wt;
        }
    }

    static class Pair implements Comparable<Pair> {
        int v;
        String psf;
        int wsf; // weight so far

        Pair(int v, String psf, int wsf) {
            this.v = v;
            this.psf = psf;
            this.wsf = wsf;
        }

        public int compareTo(Pair o) {
            return this.wsf - o.wsf;
        }
    }
    
	// dijkstra algo starts
    private static void dijkstra(ArrayList<Edge>[] graph, int src) {
        PriorityQueue<Pair> pq = new PriorityQueue<>();
        boolean[] visited = new boolean[graph.length];
        pq.add(new Pair(src, src + "", 0));

        while (pq.size() > 0) {
            Pair rem = pq.remove();
            if (visited[rem.v]) {
                continue;
            }
            visited[rem.v] = true;
            System.out.println(rem.v + " via " + rem.psf + " weight: " + rem.wsf);

            for (Edge edge : graph[rem.v]) {
                if (!visited[edge.nbr]) {
                    pq.add(new Pair(edge.nbr, rem.psf + edge.nbr, rem.wsf + edge.wt));
                }
            }
        }
    }
}
```