---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/course-schedule/"}
---

###### Question:
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

**Input:** numCourses = 2, prerequisites = \[[1,0\|1,0]]
**Output:** true
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.

**Example 2:**

**Input:** numCourses = 2, prerequisites = \[[1,0],[0,1\|1,0],[0,1]]
**Output:** false
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.


###### Solution: (Essentially detect a cycle in directed graph)
1. Prerequisites as Directed Edges: 
	- We can represent the courses as nodes in a directed graph, where an edge from course A to course B indicates that course A is a prerequisite for course B.
2. Acyclic Property:
	-  For a valid course schedule to exist, there should be no cyclic dependencies among the courses.
	- If there is a cycle in the graph, it means there are circular prerequisites, which makes it impossible to take all the courses.
3. Topological Order:
	 - Topological sorting gives us a linear ordering of the nodes in a DAG such that for every directed edge from node A to node B, node A comes before node B in the ordering.
	- In the context of the Course Scheduling problem, the topological order represents a valid sequence in which courses can be taken while respecting their prerequisites.
4. Topological sorting is used in the Course Scheduling problem to determine a valid order of courses based on their prerequisites. It helps us ensure that the prerequisites are satisfied and that there are no cyclic dependencies.

```Java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        ArrayList<Integer>[] graph = buildGraph(prerequisites, numCourses);
        return topologicalSort(graph, numCourses);
    }

    private boolean topologicalSort(ArrayList<Integer>[] graph, int numCourses){
        int[] inDegrees = new int[numCourses];
        List<Integer> topologicalSort = new ArrayList<>();
        Queue<Integer> q = new LinkedList<>();

        for(int i = 0; i < numCourses; i++){
            for(int nbr: graph[i]){
                inDegrees[nbr]++;
            }
        }

        for(int i = 0; i < numCourses; i++){
            if(inDegrees[i] == 0){
                q.offer(i);
            }
        }

        while(!q.isEmpty()){
            int currV = q.poll();
            topologicalSort.add(currV);

            for(int nbr: graph[currV]){
                inDegrees[nbr]--;
                if(inDegrees[nbr] == 0){
                    q.offer(nbr);
                }
            }
        }

        if(numCourses == topologicalSort.size()){
            return true;
        } else return false;
    }

    private ArrayList<Integer>[] buildGraph(int[][] prerequisites, int n){
        ArrayList<Integer>[] graph = new ArrayList[n];
        
        for(int i = 0; i < n; i++){
            graph[i] = new ArrayList<>();
        }

        for(int[] edge: prerequisites){
            int course = edge[0];
            int prerequisite = edge[1];
            graph[course].add(prerequisite);
        }

        return graph;
    }
}
```