---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/topological-sort/"}
---


1. **For topological sorting, the graph should represent the dependencies in the opposite direction in adjacency list. For example in [[personal vault/Data Structures and Algorithms/To be Revisited/Course Schedule\|Course Schedule]] The adjacency list of each course should contain the courses that depend on it, not its prerequisites.** ⭐️
   ![[Screenshot 2024-05-19 at 6.00.57 PM.png \| 262]]![[Screenshot 2024-05-19 at 6.01.31 PM.png \| 150]]
2. Topological sorting provides a way to arrange the vertices of a DAG in a sequence where all the edges point from left to right.
3. It's basically linear ordering of vertices such that if there's an edge b/w `u & v`, `u` appears before `v` in ordering.
4. There can be multiple valid topological orderings for a given DAG. 
	1. *The algorithm may produce different orderings depending on the order in which the vertices are processed.*
5. DFS-based approach,
	1. After visiting all the neighbors of a vertex, the vertex is added to a stack. 
	2. The final topological ordering is obtained by popping vertices from the stack. 
	3. **TC: O(V + E)**
6. Kahn's algorithm: 
	1. This algorithm maintains a queue of vertices with no incoming edges (in-degree of 0). 
	2. It repeatedly removes vertices from the queue, adds them to the topological ordering, and updates the in-degrees of their neighbors. 
	3. **TC: O(V + E)**

*When to use Topological Sort:*
1. **Dependency Resolution**: Topological sorting is commonly used in scenarios where there are dependencies or prerequisites among tasks or items. For example, in a build system where certain tasks must be completed before others can start, topological sorting can help determine a valid order of execution.
2. **Course Scheduling**: In an educational context, topological sorting can be used to determine the order in which courses should be taken based on their prerequisites. By representing courses as vertices and prerequisites as directed edges, topological sorting can provide a valid sequence of courses.
3. **Job Scheduling**: Topological sorting can be applied to job scheduling problems where certain jobs must be completed before others can begin. By modeling the jobs as vertices and their dependencies as directed edges, topological sorting can provide a feasible order for executing the jobs.

*When not to use Topological Sort:*
1. Graphs with Cycles
2. Undirected Graphs

![[DAG-Topo \| 500]]

1. According to Directed Cyclic graph 'A' should be after 'D' but before 'B' but this is impossible.
2. In undirected graph 'A' is before 'B' and 'A' is after 'B' which is again impossible.


Code:

**DFS**:
- The key idea behind the DFS-based topological sorting is that a vertex should be added to the topological order only after all its dependencies (i.e., its neighbors) have been added.
- By pushing the vertices onto the stack after exploring all their neighbors, we ensure that the vertices are added to the stack in the reverse order of their completion of the DFS traversal.
- When we pop the vertices from the stack and add them to the `topologicalOrder` list, we get the correct topological ordering of the vertices.
```java

public static void dfs(List<List<Integer>> graph, boolean[] visited, Stack<Integer> stack, int src){
	visited[src] = true;
	for(int neighbour: graph.get(src)){
		if(!visited[neighbour]){
			dfs(graph, visited, stack, neighbour);
		}
	}

	stack.push(src);
}


public static List<Integer> topologicalSort(List<List<Integer>> graph){
	int numVertices = graph.size();
	boolean[] visited = new boolean[numVertices];
	Stack<Integer> stack = new Stack<>();

	for(int vertex = 0; vertex < numVertices; vertex++){
		if(!visited[vertex]){
			dfs(graph, visited, stack, vertex);
		}
	}
	
	List<Integer> topologicalOrder = new ArrayList<>()
	while(!stack.isEmpty()){
		Integer element = stack.pop();
		topologicalOrder.add(element);
	}
	
	return topologicalOrder;
}
```


**BFS (Kahn's algorithm)**
- Compute the in-degree (number of incoming edges) for each vertex in the graph.
- Initialize a queue and enqueue all vertices with in-degree 0.
- Initialize an empty list to store the topological order.
- While the queue is not empty:
    - Dequeue a vertex from the queue and add it to the topological order list.
    - For each neighbor of the dequeued vertex:
        - Decrement the in-degree of the neighbor by 1.
        - If the in-degree of the neighbor becomes 0, enqueue it.
- If the topological order list contains all the vertices, return it. Otherwise, the graph contains a cycle, and topological sorting is not possible.
- The algorithm works by systematically processing vertices based on their dependencies, ensuring that each vertex is added to the topological order only when all its dependencies have been processed. The in-degree calculation and the queue help maintain the correct order of processing.


```java
public static List<Integer> topologicalSort(List<List<Integer>> graph) {
	int numVertices = graph.size();
	int[] inDegrees = new int[numVertices];
	Queue<Integer> q = new LinkedList<>();
	List<Integer> topologicalOrder = new ArrayList<>();

	for(int src = 0; src < numVertices; src++){
		for(int neighbour: graph[src]){
			inDegrees[neighbour]++;
		}
	}

	for(int vertex = 0; vertex < numVertices; vertex++){
		if(inDegrees[vertex] == 0){
			q.offer(vertex);
		}
	}

	while(!q.isEmpty()){
		int currV = q.poll();
		topologicalOrder.add(currV);

		for(int neighbour: graph[currV]){
			inDegrees[neighbour]--;
			if(inDegrees[neighbour] == 0){
				q.offer(neighbour);
			}
		}
	}

	if(topologicalOrder.size() != numVertices){
		throw new IllegalArgumentException("The graph contains a cycle.");
	}
	
	return topologicalOrder;
}
```