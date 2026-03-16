---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/minimum-height-tree/"}
---

#### Question:
![Screenshot 2024-05-26 at 5.18.59 PM.png](/img/user/personal%20vault/Attachments/Screenshot%202024-05-26%20at%205.18.59%20PM.png)

##### Solution:
1. Brute Force: (Ran BFS for every node while maintaining a array which contains min distance for each node)
```Java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer> minDistList = new ArrayList<>();
        
        ArrayList<Integer>[] graph = new ArrayList[n];
        for(int i = 0; i < n; i++){
            graph[i] = new ArrayList<>();
        }
        for(int[] edge: edges){
            int u = edge[0];
            int v = edge[1];
            graph[u].add(v);
            graph[v].add(u);
        }

        for(int i = 0; i < n; i++){
            int minDist = Integer.MIN_VALUE;
            Queue<int[]> q = new LinkedList<>();
            boolean[] vis = new boolean[n];
            q.offer(new int[]{i, 0});

            while(!q.isEmpty()){
                int[] curr = q.remove();
                int currV = curr[0];
                int dist = curr[1];

                if(vis[currV] == true) continue;
                vis[currV] = true;

                minDist = Math.max(minDist, dist);

                for(int edge: graph[currV]){
                    if(!vis[edge]){
                        q.offer(new int[]{edge, dist + 1});
                    }
                }
            }
            minDistList.add(minDist);
        }
        int minVal = Integer.MAX_VALUE;
        for(int i = 0; i < minDistList.size(); i++){
            minVal = Math.min(minVal, minDistList.get(i));
        }

        List<Integer> ans = new ArrayList<>();
        for(int i = 0; i < minDistList.size(); i++){
            if(minDistList.get(i) == minVal){
                ans.add(i);
            }
        }

        return ans;
    }
}
```

 2. Optimized solution

<u>Observations and Intuition:</u>
1. In a tree, the minimum height is achieved when the root is chosen such that the heights of its subtrees are as balanced as possible.
2. The nodes that can be the roots of MHTs are the centroids of the tree.
3. The centroids of a tree are the nodes that minimize the maximum distance to any other node in the tree.
4. In a tree, there can be at most two centroids.
5. We can iteratively remove the leaves (nodes with only one neighbor) until we are left with one or two nodes, which will be the centroids.

<u>Optimized Approach (Topological Sorting):</u>
1. Create an adjacency list to represent the tree.
2. Initialize a list to store the degrees of each node (number of neighbors).
3. Initialize a queue to perform a topological sorting-like process.
4. Enqueue all the leaves (nodes with only one neighbor) into the queue.
5. While the number of remaining nodes is greater than 2:

6. Dequeue a node from the queue.
7. Decrement the degree of its neighbor.
8. If the neighbor becomes a leaf (degree becomes 1), enqueue it.

9. The remaining one or two nodes in the queue are the roots of the MHTs.
Time complexity: O(n), where n is the number of nodes.
Space complexity: O(n) to store the adjacency list and the queue.


```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) {
            return Collections.singletonList(0);
        }
        List<Integer> minDistList = new ArrayList<>();
        int[] inDegrees = new int[n];
        ArrayList<Integer>[] graph = new ArrayList[n];
        for(int i = 0; i < n; i++){
            graph[i] = new ArrayList<>();
        }
        for(int[] edge: edges){
            int u = edge[0];
            int v = edge[1];
            graph[u].add(v);
            graph[v].add(u);
        }

        for(int i = 0; i < n; i++){
            for(int neighbour: graph[i]){
                inDegrees[neighbour]++;
            }
        }

        Queue<Integer> q = new LinkedList<>();
        for(int i = 0; i < n; i++){
            if (inDegrees[i] == 1) {
                q.offer(i);
            }
        }
        List<Integer> remaining = new ArrayList<>();
        
        while(!q.isEmpty()){
            remaining = new ArrayList<>();
            int size = q.size();

            for(int i = 0; i < size; i++){
                int curr = q.remove();
                remaining.add(curr);
                for(int neighbour: graph[curr]){
                    inDegrees[neighbour]--;
                    if(inDegrees[neighbour] == 1){
                        q.offer(neighbour);
                    }
                }
            }
        }
        return remaining;
    }
}
```
 