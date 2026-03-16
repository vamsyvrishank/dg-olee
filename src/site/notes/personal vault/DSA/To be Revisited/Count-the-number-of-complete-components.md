---
{"dg-publish":true,"permalink":"/personal-vault/dsa/to-be-revisited/count-the-number-of-complete-components/"}
---

Question:
![Screenshot 2024-05-15 at 9.55.05 AM.png](/img/user/personal%20vault/Attachments/Screenshot%202024-05-15%20at%209.55.05%20AM.png)

Solution:
1. First get all the components.
2. With the help of the function `isCompleteComponent(ArrayList<Integer> component, ArrayList<ArrayList<Integer>> graph){}` check if the component is completely connected.
	1. For each node of component there should be neighbors(in graph) == component.size() - 1.
	2. component.size() - 1(neightbors excluding the node itself) kyuki graph mei node k index pe bs neighbors pade hote but component mei src khud bhi pada hai.
```Java
class Solution {
    public int countCompleteComponents(int n, int[][] edges) {
        ArrayList<ArrayList<Integer>> graph = buildGraph(n, edges);
        boolean[] visited = new boolean[n];
        int count = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                ArrayList<Integer> component = new ArrayList<>();
                getComponent(i, graph, visited, component);
                if (isCompleteComponent(component, graph)) {
                    count++;
                }
            }
        }

        return count;
    }

    private ArrayList<ArrayList<Integer>> buildGraph(int n, int[][] edges) {
        ArrayList<ArrayList<Integer>> graph = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }

        for (int[] edge : edges) {
            int src = edge[0];
            int nbr = edge[1];
            graph.get(src).add(nbr);
            graph.get(nbr).add(src);
        }

        return graph;
    }

    private void getComponent(int node, ArrayList<ArrayList<Integer>> graph, boolean[] visited, ArrayList<Integer> component) {
        visited[node] = true;
        component.add(node);

        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                getComponent(neighbor, graph, visited, component);
            }
        }
    }

    private boolean isCompleteComponent(ArrayList<Integer> component, ArrayList<ArrayList<Integer>> graph) {

        for (int node : component) {
            int count = 0;
            for (int neighbor : graph.get(node)) {
                if (component.contains(neighbor)) {
                    count++;
                }
            }
            //component.size() - 1(neightbors excluding the node itself)
            //kyuki graph mei node k index pe bs neighbors pade hote but component
            //mei src khud bhi pada hai
            if (count != component.size() - 1) {
                return false;
            }
        }

        return true;
    }
}


```