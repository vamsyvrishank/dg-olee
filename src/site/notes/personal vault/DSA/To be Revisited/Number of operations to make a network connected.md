---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/number-of-operations-to-make-a-network-connected/"}
---

##### Question:
There are `n` computers numbered from `0` to `n - 1` connected by ethernet cables `connections` forming a network where `connections[i] = [ai, bi]`represents a connection between computers `ai` and `bi`. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network `connections`. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return _the minimum number of times you nee
d to do this in order to make all the computers connected_. If it is not possible, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png)

**Input:** n = 4, connections = \[[0,1],[0,2],[1,2\|0,1],[0,2],[1,2]]
**Output:** 1
**Explanation:** Remove cable between computer 1 and 2 and place between computers 1 and 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png)

**Input:** n = 6, connections = \[[0,1],[0,2],[0,3],[1,2],[1,3\|0,1],[0,2],[0,3],[1,2],[1,3]]
**Output:** 2

**Example 3:**

**Input:** n = 6, connections = \[[0,1],[0,2],[0,3],[1,2\|0,1],[0,2],[0,3],[1,2]]
**Output:** -1
**Explanation:** There are not enough cables.


#### Solution:
![[OperationToMakeNetworkConnected.jpg \| 200]]

 > For Disjoint Set class copy template from [[personal vault/DSA/To be Revisited/DisJoint Set#UNION BY RANK (LESS INTUITIVE)\|DisJoint Set#UNION BY RANK (LESS INTUITIVE)]].
```Java
public int makeConnected(int n, int[][] connections) {//connections or edges
        DisjointSet dsu = new DisjointSet(n);
        int m = connections.length;
        int extraEdges = 0; //count extra edges to connect to the ultimate parents

        for(int i = 0; i < m; i++){
            int u = connections[i][0];
            int v = connections[i][1];
            if(dsu.findUltimateParent(u) == dsu.findUltimateParent(v)){
                extraEdges++;
            } else{
                dsu.unionByRank(u,v);
            }
        }

        int uParents = 0;
        for(int i = 0; i < n; i++){
            if(dsu.parent[i] == i){
                uParents++;
            }
        }
	    
	    int totalEdgesNeeded = uParents - 1;
	    
        if(extraEdges >= totalEdgesNeeded){
            return totalEdgesNeeded;
        } else{
            return -1;
        }
    }
```