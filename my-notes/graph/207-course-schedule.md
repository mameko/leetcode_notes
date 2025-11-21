# 207 Course Schedule

There are a total of n courses you have to take, labeled from`0`to`n - 1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: \[0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

**Example**

Given n =`2`, prerequisites =`[[1,0]]`\
Return`true`

Given n =`2`, prerequisites =`[[1,0],[0,1]]`\
Return`false`

三刷认真看题才发现其实可以不用hashmap存，因为所有course都有，就一个数组ok啦。6ms

```java
 class Solution {
       public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (numCourses < 1 || prerequisites == null || prerequisites.length == 0 || prerequisites[0].length == 0) {
            return true;
        }

        int[] indegree = new int[numCourses];
        List<DirectedGraphNode> graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            graph.add(new DirectedGraphNode(i));
        }

        for (int[] course : prerequisites) {
            graph.get(course[0]).neighbours.add(graph.get(course[1]));
            indegree[course[1]]++;
        }

        Queue<DirectedGraphNode> queue = new LinkedList<>();        
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(graph.get(i));
            }
        }

        if (queue.size() == 0) {
            return false;
        }

        int cnt = 0;

        while (!queue.isEmpty()) {
            DirectedGraphNode cur = queue.poll();
            cnt++;
            for (DirectedGraphNode nei : cur.neighbours) {
                indegree[nei.val]--;
                if (indegree[nei.val] == 0) {
                    queue.offer(graph.get(nei.val));
                }
            }
        }

        return cnt == numCourses;
    }
}

class DirectedGraphNode {
    List<DirectedGraphNode> neighbours = new ArrayList<>();
    int val;
    public DirectedGraphNode(int value) {
        val = value;
    }
}
```

这两条course schedule都要先把输入转化成邻接表。这里写了两种邻接表的存储方式，一个用了list of list, 另一个用了list array。21ms

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    if (prerequisites == null || prerequisites.length == 0) {
        return true;
    }

    // make prerequisites a adjacent list
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        graph.add(new ArrayList<Integer>());
    }

    // collect indegree
    // <node, in-degree>
    HashMap<Integer, Integer> inDegree = new HashMap<>();
    for (int[] pre : prerequisites) {
        if (inDegree.containsKey(pre[0])) {
            inDegree.put(pre[0], inDegree.get(pre[0]) + 1);
        } else {
            inDegree.put(pre[0], 1);
        }

        graph.get(pre[1]).add(pre[0]);
    }

    // add in-degree == 0 to queue
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (!inDegree.containsKey(i)) {
            queue.offer(i);
        }
    }

    // bfs
    while (!queue.isEmpty()) {
        Integer cur = queue.poll();

        for (Integer nei : graph.get(cur)) {
            if (inDegree.containsKey(nei)) {
                inDegree.put(nei, inDegree.get(nei) - 1);
                if (inDegree.get(nei) == 0) {
                    inDegree.remove(nei);
                    queue.offer(nei);
                }
            } 
        }
    }

    return inDegree.size() == 0;
}
```

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    if (prerequisites == null || prerequisites.length == 0 || numCourses < 0) {
        return true;
    }

    // build adjacent list for graph
    List[] edges = new List[numCourses];
    for (int i = 0; i < numCourses; i++) {
        edges[i] = new ArrayList<>();
    }

    // <course, in-degree> 
    HashMap<Integer, Integer> indegree = new HashMap<>();

    // collect in-degree for lessons
    for (int[] pre : prerequisites) {
        if (indegree.containsKey(pre[0])) {
            indegree.put(pre[0], indegree.get(pre[0]) + 1);
        } else {
            indegree.put(pre[0], 1);
        }

        edges[pre[1]].add(pre[0]);
    }

    Queue<Integer> queue = new LinkedList<>();

    // add 0 in-degree lesson to res & queue
    for (int i = 0; i < numCourses; i++) {
        if (!indegree.containsKey(i)) {
            queue.offer(i);
        }
    }

    // if no starting point to start topo sort return false;
    if (queue.size() == 0) {
        return false;
    }

    // do bfs to find topo sort order
    while (!queue.isEmpty()) {
        int course = queue.poll();

        for (int i = 0; i < edges[course].size(); i++) {
            Integer edge = (int) edges[course].get(i);
            if (indegree.containsKey(edge)) {
                indegree.put(edge, indegree.get(edge) - 1);
                if (indegree.get(edge) == 0) {
                    indegree.remove(edge);
                    queue.offer(edge);
                }
            } 
        }
    }

    return indegree.size() == 0;
}
```
