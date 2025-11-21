# 444 Sequence Reconstruction

Check whether the original sequence`org`can be uniquely reconstructed from the sequences in`seqs`. The`org`sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 104. Reconstruction means building a shortest common supersequence of the sequences in`seqs`(i.e., a shortest sequence so that all sequences in`seqs`are subsequences of it). Determine whether there is only one sequence that can be reconstructed from`seqs`and it is the`org`sequence.

**Example 1:**

```
Input:
org: [1,2,3], seqs: [[1,2],[1,3]]

Output:
false

Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.
```

**Example 2:**

```
Input:
org: [1,2,3], seqs: [[1,2]]

Output:
false

Explanation:
The reconstructed sequence can only be [1,2].
```

**Example 3:**

```
Input:
org: [1,2,3], seqs: [[1,2],[1,3],[2,3]]

Output:
true

Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].
```

**Example 4:**

```
Input:
org: [4,1,5,2,6,3], seqs: [[5,2,6,3],[4,1,5,2]]

Output:
true
```

又过了几年去上九章，老师终于归类了这种题了。这个topo sort是求唯一topo sort order的。与一般的topo sort不同处在于，因为是唯一的topo sort，所以每次queue的size其实都是1。这里分步写，容易点理解。还是抄九章的答案。

```java
public boolean sequenceReconstruction(int[] org, int[][] seqs) {
    if (org == null || seqs == null) {
        return false;
    }

    // 1. build graph
    Map<Integer, Set<Integer>> graph = buildGraph(seqs);
    
    // 2. top sort
    List<Integer> topoSortedResult = topoSort(graph);

    // 3. check org and topo sorted result
    if (topoSortedResult == null || topoSortedResult.size() != org.length) {
        return false;
    }
    for (int i = 0; i < org.length; i++) {
        if (org[i] != topoSortedResult.get(i)) {
            return false;
        }
    }

    return true;
}

private Map<Integer, Set<Integer>> buildGraph(int[][] seqs) {
    Map<Integer, Set<Integer>> graph = new HashMap<>();
    // add all nodes to graph
    for (int[] seqPair : seqs) {
        for (int i = 0; i < seqPair.length; i++) {
            // 这里没直接用下标是因为有些输入只有一个点，譬如，1， [[1]]
            if (!graph.containsKey(seqPair[i])) {
                graph.put(seqPair[i], new HashSet<>());
            }
        }
    }

    // setup edges
    for (int[] seqPair : seqs) {
        for (int i = 1; i < seqPair.length; i++) {
            graph.get(seqPair[i - 1]).add(seqPair[i]);
        }            
    }

    return graph;
}

private List<Integer> topoSort(Map<Integer, Set<Integer>> graph) {
    Queue<Integer> queue = new LinkedList<>();
    // <node, indegree>
    Map<Integer, Integer> indegree = getIndegree(graph);
    List<Integer> topoSortedResult = new ArrayList<>();

    // add indegree 0 node to queue
    for (Integer node : indegree.keySet()) {
        if (indegree.get(node) == 0) {
            queue.offer(node);
        }
    }

    while (!queue.isEmpty()) {
        // contains more than one topo order sort
        if (queue.size() > 1) {
            return null;                
        }

        int curNode = queue.poll();
        topoSortedResult.add(curNode);
        Set<Integer> curNeis = graph.get(curNode);
        for (Integer nei : curNeis) {                
            indegree.put(nei, indegree.get(nei) - 1);
            if (indegree.get(nei) == 0) {                
                queue.offer(nei);
            }                
        }
    }

    // has loop
    if (topoSortedResult.size() != graph.size()) {
        return null;            
    }

    return topoSortedResult;
}

private Map<Integer, Integer> getIndegree(Map<Integer, Set<Integer>> graph) {
    Map<Integer, Integer> indegree = new HashMap<>();
    for (Integer node : graph.keySet()) {
        indegree.put(node, 0);
    }
    for (Integer node : graph.keySet()) {
        Set<Integer> neis = graph.get(node);
        for (Integer nei : neis) {
            indegree.put(nei, indegree.getOrDefault(nei, 0) + 1);
        }           
    }
    return indegree;
}
```

这里的条件是，后面seqs里不能出现前面没有的数，而且在seqs里的顺序要跟org里的一样，eg，org里1在2前面，在seqs上也得是1在2前面。这里两个方法的indegree有些许不一，只供参考。

```java
 public boolean sequenceReconstruction(int[] org, int[][] seqs) {

    // 1 make adjacent list
    List<List<Integer>> edges = new ArrayList<>();
    HashMap<Integer, Integer> indegree = new HashMap<>();
    for (int i = 0; i <= org.length; i++) {
        edges.add(new ArrayList<Integer>());
        indegree.put(i, 0);
    }

    indegree.remove(0);

    // 2 collect indegree
    // <node, indegree>
    int cnt = 0;
    for (int[] s : seqs) {
        cnt += s.length;
        if (s.length != 0 && !indegree.containsKey(s[0])) {
                return false;
        }
        for (int i = 1; i < s.length; i++) {
            if (!indegree.containsKey(s[i])) {
                return false;
            }
            indegree.put(s[i], indegree.get(s[i]) + 1);
            edges.get(s[i - 1]).add(s[i]);
        }
    }

    // in case we have [1],[[]]
    if (cnt < org.length) {
        return false;
    }

    Queue<Integer> q = new LinkedList<>();
    // add 0 indegree to queue
    for (int i = 1; i <= org.length; i++) {
        if (indegree.get(i) == 0) {
            q.offer(i);
        }
    }

    // bfs topo sort
    int index = 0;
    while (!q.isEmpty()) {
        if (q.size() > 1) {
            return false;
        }

        Integer cur = q.poll();

        if (index == org.length || cur != org[index++]) {
            return false;
        }

        for (Integer nei : edges.get(cur)) {
            if (indegree.containsKey(nei)) {
                indegree.put(nei, indegree.get(nei) - 1);
                if (indegree.get(nei) == 0) {
                    q.offer(nei);
                }
            }
        }
    }

    return index == org.length && indegree.size() == index;
}
```

```java
public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {

    // 1 make a graph
    List<List<Integer>> edges = new ArrayList<>();

    int n = org.length;
    for (int i = 0; i <= n; i++) {
        edges.add(new ArrayList<Integer>());
    }


    // collect in degree
    HashMap<Integer, Integer> indegree = new HashMap<>();
    int cnt = 0;
    for (List<Integer> s : seqs) {
        cnt += s.size();
        if (s.size() != 0 && (s.get(0) < 1 || s.get(0) > n)) {
            return false;
        }
        for (int i = 1; i < s.size(); i++) {
            int cur = s.get(i);
            if (cur < 1 || cur > n) {
                return false;
            }

            if (indegree.containsKey(cur)) {
                indegree.put(cur, indegree.get(cur) + 1);
            } else {
                indegree.put(cur, 1);
            }

            edges.get(s.get(i - 1)).add(cur);
        }
    }

    if (cnt < n) {
        return false;
    }

    // add indegree == 0 to queue
    Queue<Integer> q = new LinkedList<>();
    for (int i = 1; i <= n; i++) {
        if (!indegree.containsKey(i)) {
            q.offer(i);
        }
    }

    int index = 0;
    while (!q.isEmpty()) {
        if (q.size() != 1) {
            return false;
        }

        int cur = q.poll();
        if (org[index++] != cur) {
            return false;
        }

        for (Integer nei : edges.get(cur)) {
            if (indegree.containsKey(nei)) {
                indegree.put(nei, indegree.get(nei) - 1);
                if (indegree.get(nei) == 0) {
                    q.offer(nei);
                    indegree.remove(nei);
                }
            }
        }
    }

    if (indegree.size() > 0) {
        return false;
    }

    return index == n;
}
```
