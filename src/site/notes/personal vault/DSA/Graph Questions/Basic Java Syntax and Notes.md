---
{"dg-publish":true,"permalink":"/personal-vault/dsa/graph-questions/basic-java-syntax-and-notes/"}
---


1. [HashMap](#HashMap)
2. [ArrayList](#arraylist)
3. [Stack](#stack)
4. [Queue](#queue)
5. [String/StringBuilder](#string-stringbuilder)
6. [HashSet](#hashset)
7. [Heap](#heap)
8. [Primitives](#primitives)
9. [Extras](#extras)
    - [Graph Edges](#graph-edges)

# Collections:

 # HashMap:
1. **Definition**: 
    ```JAVA
    Map<K, V> map = new HashMap<>();
    ```

2. insert / update: `V put(k1, v1);`  *TC: O(1)*
3. delete: `V remove(k1);`  *TC: O(1)*
4. get: `V get(k1);`  *TC: O(1)*
5. size: `int size();`  *TC: O(1)*
6. check for Empty: `boolean isEmpty();`  *TC: O(1)*
7. value present: `boolean containsKey(k1);`  *TC: O(1)*
8. remove all map values: `clear();`  *TC: O(2n + 1): O(n)* (n-key, n-value, 1 for map itself)<br/>
9. Increment an occurrence in map
    ```JAVA
    for(char current : input.toCharArray()){
          count.put(current, count.getOrDefault(current, 0) + 1);
    }
    ```
    
10. Let's say you want to decrement the occurrence and when occurrence becomes zero, remove entry.
    ```JAVA
    char charToRemove = 'c';
    count.put(charToRemove, count.get(charToRemove) - 1);
    count.remove(charToRemove, 0);
    
    ```
11. Accessing:	
    ```JAVA
    for (Map.Entry<String, String> entry : hm.entrySet()) {
        System.out.println(entry.getKey() + “ “ + entry.getValue());
    }

    for (String key : hm.keySet()) { 
        System.out.println(key); 
    }

    for (String value : hm.values()) { 
        System.out.println(value); 
    }
    ```

# ArrayList:
1. **Definition**: 
    ```JAVA
    - ArrayList list = new ArrayList<>();
    - List<Integer> list = new ArrayList<>();
    - List<List<Integer>> res = new ArrayList<>();List of List
    ```


2. insert: `boolean add(t)` [TC: O(1)] / `add(int index, T)` [*TC: O(n)*]
3. delete: `T remove(int index);` *TC: O(n)* as you have to shuffle the elements above that point
4. set/update index value: `T set(int index, T);` *TC: O(1)*
5. get inde: `T get(int index);` *TC: O(1)*
6. size: `int list.size();` *TC: O(1)*
7. clear elements: `void clear();` *TC: O(n)* & `removeAll` : O(n^2).
8. check for Empty: `boolean isEmpty();` *TC: O(1)*
9. value contain check: `boolean contains(t);` *TC: O(n)*
10. get Index of value: `int indexOf(t);` *TC: O(n)*, checking each element one by one
11. non primitive to primitive list: `toArray();` *TC: O(n)*
12. Sorting for List:
    ```JAVA
        Collections.sort(list)
        Collections.sort(list, (a, b)-> a - b); ascending , TC: O(nlogn)
        
        Collections.sort(list, Collections.reverseOrder());
        Collections.sort(list, (a, b)-> b - a); descending , TC: O(nlogn)
    ```

# Stack:
1. **Definition**: 
    ```JAVA
    Deque<T> stack = new ArrayDeque<>(); (recommended)
    ```

2. insert: `T push(t);` *TC: O(1)*
3. size: `int size();` *TC: O(1)*
4. look up for head element: `T peek();` *TC: O(1)*
5. remove head element: `T pop();` *TC: O(1)*
6. check for Empty: `boolean isEmpty();` *TC: O(1)*
 
# Queue:
1. **Definition**: 
    ```JAVA
    Queue queue = new LinkedList<>();
    ```

2. insert: `boolean add(t);` *TC: O(1)*
3. size: `int size();` *TC: O(1)*
4. look up for head element: `T peek();` *TC: O(1)*
5. remove head element: `T poll();` *TC: O(1)*
6. check for Empty: `boolean isEmpty();` *TC: O(1)*
7. points to remember :
    - queue poll vs stack pop
    - queue add vs stack push
    - we can define queue via LinkedList, PriorityQueue based on use case
 
# String-StringBuilder:

1. **Definition**: 
	**Strings are immutable in JAVA**
    ```JAVA
    String str = new String();
    ```

2. size: `int length();`  *TC: O(1)*
3. convert to char Array: `toCharArray();` *TC: O(n)*
4. value for specific index: `charAt(int index);` *TC: O(1)*
5. substring from string: `substring(a,b]` a : inclusive, b: Exclusive, *TC: O(n)*
6. transform to Lowercase: `toLowerCase();` *TC: O(n)*
7. transform to UpperCase: `toUpperCase();` *TC: O(n)*
8. replace all characters in string: `replaceAll(from, to)` *TC: O(n)*
9. Some useful Character properties
    - Character.isLetter();
    - Character.isAlphabetic();
    - Character.isUpperCase();
    - Character.isLowerCase();
    - Character.isDigit(); <br>

> Concatenation: `T str1 + str2`

>#### StringBuilder:

- new StringBuilder() / new StringBuilder(int)
- append("adding string") better way to do
- toString() converting back to string
 
# HashSet:
 
1. **Definition**:
    ```JAVA
    Set set = new HashSet<>();
    ```
2. insert / update: `boolean add(t);` *TC: O(1)*
3. delete: `boolean remove(t);` *TC: O(1)*
4. get: `boolean contains(t);` *TC: O(1)*
5. size: `int size();` *TC: O(1)*
6. check for Empty: `boolean isEmpty();` *TC: O(1)*
7. remove all set values: `clear();` *TC: O(n)*

# Heap:
1. **Definition**:
    ```JAVA
    PriorityQueue<Integer> pq = new PriorityQueue<>(),
    PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder)
    ```
    ```
    <!-- Max heap that contains pairs - if values for pairs are the same,
    then they will be sorted ascending (a-z) according to key: -->

    PriorityQueue<Map.Entry<String, Integer>> pq = new PriorityQueue<>(
        (a, b) -> a.getValue().equals(b.getValue()) ?
        a.getKey().compareTo(b.getKey()) : a.getValue() - b.getValue()
    );	

    ```
2. add element: `T add(t)` *TC: O(log(n))*
3. view top element: `T peek()` *TC: O(1)*  // Returns but does not remove the top element
4. remove: `T poll()` *TC: O(log(n))*  // Returns and removes the top element
5. size: `int size()` *TC: O(1)*

# Primitives

>### Array:
1. **Definition**: 
    `T arr [ ] = new T[N];` N: static size , T : datatype
2. insert: `arr[index] = v1;` *TC: O(1)*

3. update: `arr[index] = v2;` *TC: O(1)*
4. get: `T arr[index]` *TC: O(1)*
5. size: `int arr.length` *TC: O(1)*
6. `Arrays.fill(arr, 0);` filled array with value=0, *TC: O(n)*
7. Sorting: TC: O(nlogn)
    - primitive (int[] ..)
        - `Arrays.sort(arr);` default ascending,
    - non-premetive (Integer[] ..)
        - `Arrays.sort(arr);` default ascending
        - `Arrays.sort(arr, (a,b): b-a);` descening

>### 2. String:
1. Creation: String gopha = new String(“ok”);
2. String literal: String gopha = “ok”;
3. Size: gopha.length();

4. Accessing:
```JAVA
char[] chArr = gopha.toCharArray();

for (char c : chArr) { 
    System.out.print(c); 
}

for (int i = 0; i < gopha.length(); i++) { 
    System.out.print(gopha.charAt(i)); 
}
```


# Extras:
### Graph-Edges

> Mostly in graph problems we need to add edges between nodes. If node is not created, we first create list for the children and then add edges 

``` java
Map<Integer, List<Integer>> graph = new HashMap<>();
    for(int[] edge : edgeList){
        if(graph.containsKey(edge[0]) == false){
            graph.put(edge[0], new ArrayList<>());
        }
        graph.get(edge[0]).add(edge[1]);
    }
```
    
 > Above code can be abstracted as follows :
```JAVA
for(int[] edge : edgeList){
    graph.computeIfAbsent(edge[0], val -> new ArrayList<>()).add(edge[1]);
}
```
