---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/minimum-cost-to-connect-all-points/"}
---

**Question:**
	You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.
	The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the **manhattan distance** between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.
	Return _the minimum cost to make all points connected._ All points are connected if there is **exactly one** simple path between any two points.
	**Example 1:**
	![d.png](https://assets.leetcode.com/uploads/2020/08/26/d.png)
	**Input:** points = \[[0,0],[2,2],[3,10],[5,2],[7,0]\]
	**Output:** 20
	**Explanation:**  We can connect the points as shown above to get the minimum cost of 20.
	Notice that there is a unique path between every pair of points.
	![](https://assets.leetcode.com/uploads/2020/08/26/c.png)
	**Example 2:**
	**Input:** points = \[[3,12],[-2,5],[-4,1]\]
	**Output:** 18


**Solution:**

``` Java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) -> a[2] - b[2]);
        boolean[] visited = new boolean[n];

        pq.offer(new int[]{0, 0, 0}); //[currVertex, nextVertex, cost] (EDGE)
        int totalCost = 0;

        while(pq.size() > 0){
            int[] currEdge = pq.poll(); //remove
            int currVertex = currEdge[0];
            int nextVertex = currEdge[1];
            int cost = currEdge[2];

            if(visited[nextVertex]) continue;//mark*
            visited[nextVertex] = true;

            totalCost += cost;//work

            for(int i = 0; i < n; i++){ //add*
                if(!visited[i]){
                    int newCost = Math.abs(points[nextVertex][0] - points[i][0]) +
                                  Math.abs(points[nextVertex][1] - points[i][1]);

                    pq.offer(new int[]{nextVertex, i, newCost});
                }
            }
        }

        return totalCost;
    }
}
```