# 210 Course Schedule II

There are a total ofncourses you have to take, labeled from`0`to`n - 1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair:`[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

For example:

```
2, [[1,0]]
```

There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is`[0,1]`

```
4, [[1,0],[2,0],[3,1],[3,2]]
```

There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is`[0,1,2,3]`. Another correct ordering is`[0,2,1,3]`.

**Note:**

1. The input prerequisites is a graph represented by **a list of edges**, not adjacency matrices. Read more about [how a graph is represented](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs).
2. You may assume that there are no duplicate edges in the input prerequisites.

[click to show more hints.](https://leetcode.com/problems/course-schedule-ii/?tab=Description)

**Hints:**

1. This problem is equivalent to finding the topological order in a directed graph. If a cycle exists, no topological ordering exists and therefore it will be impossible to take all courses.
2. [Topological Sort via DFS](https://class.coursera.org/algo-003/lecture/52)
   * A great video tutorial (21 minutes) on Coursera explaining the basic concepts of Topological Sort.
3. Topological sort could also be done via[BFS](http://en.wikipedia.org/wiki/Topological_sorting#Algorithms).

需要注意返回条件，例如如果没有course，返回空int\[]。没有入度为0的点时，没有topo sort时，返回int\[],有环时，size ！= 0，也要返回int\[]。

```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
    if (numCourses < 0) {
        return new int[0];
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

    // if no starting point to start topo sort return false;
    if (queue.size() == 0) {
        return new int[0];
    }

    // bfs
    int[] res = new int[numCourses];
    int cnt = 0;
    while (!queue.isEmpty()) {
        Integer cur = queue.poll();
        res[cnt++] = cur;
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

    if (inDegree.size() != 0) {
        return new int[0];
    }

    return res;
}
```
