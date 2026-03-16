---
{"dg-publish":true,"permalink":"/personal-vault/data-structures-and-algorithms/to-be-revisited/alien-dictionary/"}
---

###### Question:
Given a sorted dictionary of an alien language having N words and k starting alphabets of standard dictionary. Find the order of characters in the alien language.  
**Note:** Many orders may be possible for a particular test case, thus you may return any valid order and output will be 1 if the order of string returned by the function is correct else 0 denoting incorrect string returned.  
 
> **Example 1:**
**Input:** 
N = 5, K = 4
dict = {"baa","abcd","abca","cab","cad"}
**Output:**
1

>**Explanation:**
Here order of characters is 
'b', 'd', 'a', 'c' Note that words are sorted 
and in the given language "baa" comes before 
"abcd", therefore 'b' is before 'a' in output.
Similarly we can find other orders.

> **Example 2:**
**Input:** 
N = 3, K = 3
dict = {"caa","aaa","aab"}
**Output:**
1

> **Explanation:**
Here order of characters is
'c', 'a', 'b' Note that words are sorted
and in the given language "caa" comes before
"aaa", therefore 'c' is before 'a' in output.
Similarly we can find other orders.


###### Solution:
```Java
class Solution {
    public String findOrder(String[] dict, int N, int K) {
        // Write your code here
        List<Integer>[] graph = new ArrayList[K];
        int[] inDegrees = new int[K];
        buildGraphAndInDegrees(dict, graph, inDegrees, N, K);

        //BFS - Topological Sort
        StringBuilder sb = new StringBuilder();
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < K; i++) {
            if (inDegrees[i] == 0) {
                queue.offer(i);
            }
        }

        while (!queue.isEmpty()) {
            int curr = queue.poll();//remove*
            //mark*
            //No marking
            
            sb.append((char) (curr + 'a'));//work
            
            //add*
            if (graph[curr] != null) {
                for (int next : graph[curr]) {
                    inDegrees[next]--;
                    if (inDegrees[next] == 0) {
                        queue.offer(next);
                    }
                }
            }
        }

        if (sb.length() < K) {
            return "";
        }
        return sb.toString();
    }

    private void buildGraphAndInDegrees(String[] dict, List<Integer>[] graph, int[] inDegrees, int N, int K) {
        for (int i = 0; i < K; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int i = 0; i < N - 1; i++) {
            String word1 = dict[i];
            String word2 = dict[i + 1];

			// Check if word2 is a prefix of word1 
			// If word1 is longer than word2 and word2 is a prefix of word1, it violates the lexicographical order 
			// In this case, skip to the next pair of words
            if (word1.length() > word2.length() && word1.startsWith(word2)) {
                continue;
            }
            for (int j = 0; j < Math.min(word1.length(), word2.length()); j++) {
                char ch1 = word1.charAt(j);
                char ch2 = word2.charAt(j);
                // If the characters at position j are different, add an edge in the graph
                if (ch1 != ch2) {
                    int index1 = ch1 - 'a'; // Calculate the index of ch1 in the adjacency list (0-25) 
                    int index2 = ch2 - 'a'; // Calculate the index of ch2 in the adjacency list (0-25)
                    
                    graph[index1].add(index2);
                    inDegrees[index2]++;
                    break;
                }
            }
        }
    }
}
```