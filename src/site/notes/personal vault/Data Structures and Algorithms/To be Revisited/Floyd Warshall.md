---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/floyd-warshall/"}
---

1. MultiSource shortest path algorithm.
2. The algorithm can handle graphs with negative edge weights, but it cannot handle graphs with negative cycles (i.e., cycles with a total negative weight). 
3. If a negative cycle exists, the algorithm will detect it.
4. Time complexity: O(n^3)
5. Space Complexity: O(n^2)
6. In-place computation
7. Applicability: The Floyd-Warshall algorithm has various applications, such as finding the shortest paths in transportation networks, 
   solving all-pairs shortest path problems, and detecting negative cycles in graphs.

```java
public class FloydWarshall {
    final static int INF = 99999;

    public static void floydWarshall(int[][] graph) {
        int n = graph.length;
        int[][] dist = new int[n][n];

        // Initialize the distance matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                dist[i][j] = graph[i][j];
            }
        }

        // Update the shortest paths
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (dist[i][k] != INF && dist[k][j] != INF
                            && dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }

        // Check for negative cycles
        //Distance of 0 -> 0 or 1->1,etc should be ZERO but -ve cycles mei kam hota jayega.
        for (int i = 0; i < n; i++) {
            if (dist[i][i] < 0) {
                System.out.println("Negative cycle found");
                return;
            }
        }

        // Print the shortest path matrix
        printMatrix(dist);
    }

    public static void printMatrix(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == INF) {
                    System.out.print("INF ");
                } else {
                    System.out.print(matrix[i][j] + "   ");
                }
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        int[][] graph = {
            {0, 5, INF, 10},
            {INF, 0, 3, INF},
            {INF, INF, 0, 1},
            {INF, INF, INF, 0}
        };

        floydWarshall(graph);
    }
}
```