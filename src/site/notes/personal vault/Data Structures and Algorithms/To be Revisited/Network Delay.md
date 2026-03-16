---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/network-delay/"}
---

Question:
You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return _the **minimum** time it takes for all the_ `n` _nodes to receive the signal_. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

**Example 1:**

<img src="https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png">

**Input:** times = \[[2,1,1],[2,3,1],[3,4,1]\], n = 4, k = 2
**Output:** 2

**Example 2:**
**Input:** times = \[[1,2,1]\], n = 2, k = 1
**Output:** 1

**Example 3:**
**Input:** times = \[[1,2,1]\], n = 2, k = 2
**Output:** -1

**Solution:**

```Java
class Solution {
    public class Edge{
        int nbr;
        int wt;
        Edge(int nbr, int wt){
            this.nbr = nbr;
            this.wt = wt;
        }
    }
    public int networkDelayTime(int[][] times, int n, int k) {
        ArrayList<Edge>[] graph = buildGraph(times, n);
        PriorityQueue<int[]> pq = new PriorityQueue<>((x,y) -> x[1] - y[1]);

        boolean[] visited = new boolean[n + 1]; //Many leetcode graph problems, the vertices are 1-indexed

        pq.add(new int[]{k, 0});
        int minTime = 0;

        while(pq.size() > 0){
            //remove
            int[] curr = pq.poll();
            int currNode = curr[0];
            int currWeight = curr[1];

            //mark*
            if(visited[currNode]) continue;
            visited[currNode] = true;

            //work
            minTime = Math.max(minTime, currWeight);
            
            //add*
            for(Edge e: graph[currNode]){
                if(!visited[e.nbr]){
                    pq.add(new int[]{e.nbr, currWeight + e.wt});
                }
            }
        }

        //check if every node is visited during BFS.
        for(int i = 1; i <= n; i++){
            if(!visited[i]){
                return -1;
            }
        }

        return minTime;
    }

    public ArrayList<Edge>[] buildGraph(int[][] times, int n){
	    //Many leetcode graph problems, the vertices are 1-indexed
        ArrayList<Edge>[] graph = new ArrayList[n + 1];
        
        for(int i = 0; i <= n; i++){
            graph[i] = new ArrayList<Edge>();
        }

        for(int[] edge: times){
            int src = edge[0];
            int nbr = edge[1];
            int wt = edge[2];
            graph[src].add(new Edge(nbr, wt));
        }

        return graph;
    }
}
```