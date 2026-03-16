---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/dis-joint-set/"}
---


> [!info]
> Why disjoint set is used ??
> Let's imagine a graph with two components.
> ![[DSU1\|DSU1]]
> - To find if `1` and `5` belong to the same component we'll do `DFS` (O(N+E)) which is a brute force method.
> - Disjoint joint tells this in CONSTANT TIME.
>  - It's used in dynamic graphs, graphs whose configurations keep on changing.
>  - It gives us functionalities like:
> - findParent()
> - Union - It basically connects two nodes during graph formation. There're two methods for this:
> 	- By Rank
> 	- By Size

###### 1. 
 ![[Screenshot 2024-05-24 at 6.32.23 PM.png \| 300]]
- Imagine we did union till (6,7) and before we proceeds someone asks  `4` & `1` belong to the same component or not
	- The answer will be NO.

###### 2.
![[Screenshot 2024-05-24 at 6.34.29 PM.png \| 300]]
- Now after all the operations are done, `4` & `1` belong a single set.

##### Notes (Click to expand):

![[DSU-Notes.pdf \| #height=200]]

##### UNION BY RANK (LESS INTUITIVE):
``` JAVA
public class Main {
    static class DisjointSet {
        private int[] parent;
        private int[] rank;

        public DisjointSet(int n) {
            // Resized parent and rank to n+1 instead of n as it'll work for both 0-based indexing & 1 for 1-based indexing.
            parent = new int[n + 1];
            rank = new int[n + 1];
            for (int i = 0; i <= n; i++) {
                parent[i] = i;
                rank[i] = 0;
            }
        }

        public int findUltimateParent(int x) {
            if (parent[x] != x) {
                parent[x] = findUltimateParent(parent[x]); // Path compression
            }
            return parent[x];
        }

        public void unionByRank(int u, int v) {
            int ulp_u = findUltimateParent(u);
            int ulp_v = findUltimateParent(v);
            if (ulp_u == ulp_v) return;

            if (rank[ulp_u] < rank[ulp_v]) {
                parent[ulp_u] = parent[ulp_v];
            } else if (rank[ulp_u] > rank[ulp_v]) {
                parent[ulp_v] = ulp_u;
            } else {
                /*
                 * In the else block, when the ranks of both ultimate parents are equal,
                 * we arbitrarily choose one ultimate parent (ulp_u) in this case) and
                 * make the other ultimate parent (ulp_v) its child.
                 * We also increment the rank of the chosen ultimate parent by 1.
                 */
                parent[ulp_v] = ulp_u; // Update parent directly with the ultimate parent
                rank[ulp_u]++;
            }
        }
    }

    public static void main(String[] args) {
        int numElements = 6;
        DisjointSet dsu = new DisjointSet(numElements);

        // Perform union operations
        dsu.unionByRank(0, 1);
        dsu.unionByRank(2, 3);
        dsu.unionByRank(4, 5);
        dsu.unionByRank(1, 3);

        // Find the ultimate parent of elements
        System.out.println("Ultimate parent of element 0: " + dsu.findUltimateParent(0));
        System.out.println("Ultimate parent of element 2: " + dsu.findUltimateParent(2));
        System.out.println("Ultimate parent of element 3: " + dsu.findUltimateParent(3));

        // Check if elements belong to the same set
        System.out.println("Elements 0 and 2 belong to the same set: " + (dsu.findUltimateParent(0) == dsu.findUltimateParent(2)));
        System.out.println("Elements 1 and 5 belong to the same set: " + (dsu.findUltimateParent(1) == dsu.findUltimateParent(5)));
    }
}
```


##### UNION BY SIZE (MORE INTUITIVE):
- Because in unionByRank we initially calculate the rank with the height but after path compression that height destroys and rank is no more intuitive.
- Here, smaller size set attaches to larger size set.
- By merging the smaller set into the larger set, we minimize the depth of the resulting tree and keep the tree more balanced.

``` JAVA
public class Main {
    static class DisjointSet {
        private int[] parent;
        private int[] size;

        public DisjointSet(int n) {
            // Resized parent and rank to n+1 instead of n as it'll work for both 0-based indexing & 1 for 1-based indexing.
            parent = new int[n + 1];
            size = new int[n + 1];
            for (int i = 0; i <= n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
        }

        public int findUltimateParent(int x) {
            if (parent[x] != x) {
                parent[x] = findUltimateParent(parent[x]); // Path compression
            }
            return parent[x];
        }

        public void unionBySize(int u, int v) {
            int ulp_u = findUltimateParent(u);
            int ulp_v = findUltimateParent(v);
            if (ulp_u == ulp_v) return;

            if (size[ulp_u] < size[ulp_v]) {
                parent[ulp_u] = ulp_v;
                size[ulp_v] += size[ulp_u];
            } else {
                parent[ulp_v] = ulp_u;
                size[ulp_u] += size[ulp_v];
            }
        }
    }

    public static void main(String[] args) {
        int numElements = 6;
        DisjointSet dsu = new DisjointSet(numElements);

        // Perform union operations
        dsu.unionBySize(0, 1);
        dsu.unionBySize(2, 3);
        dsu.unionBySize(4, 5);
        dsu.unionBySize(1, 3);

        // Find the ultimate parent of elements
        System.out.println("Ultimate parent of element 0: " + dsu.findUltimateParent(0));
        System.out.println("Ultimate parent of element 2: " + dsu.findUltimateParent(2));
        System.out.println("Ultimate parent of element 3: " + dsu.findUltimateParent(3));

        // Check if elements belong to the same set
        System.out.println("Elements 0 and 2 belong to the same set: " + (dsu.findUltimateParent(0) == dsu.findUltimateParent(2)));
        System.out.println("Elements 1 and 5 belong to the same set: " + (dsu.findUltimateParent(1) == dsu.findUltimateParent(5)));
    }
}
```