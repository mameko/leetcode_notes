# 547 Friend Circles

There are **N** students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a **direct** friend of B, and B is a **direct** friend of C, then A is an **indirect** friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a **N\*N** matrix **M** representing the friend relationship between students in the class. If M\[i]\[j] = 1, then the ithand jthstudents are**direct**friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

**Example 1:**

```
Input:
[[1,1,0],
 [1,1,0],
 [0,0,1]]

Output: 2

Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. The 2nd student 
            himself is in a friend circle. So return 2.
```

**Example 2:**

```
Input:
[[1,1,0],
 [1,1,1],
 [0,1,1]]

Output: 1

Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
```

**Note:**

1. N is in range \[1,200].
2. M\[i]\[i] = 1 for all students.
3. If M\[i]\[j] = 1, then M\[j]\[i] = 1.

这题一看还以为是[200 Number of Island](../graph/200-number-of-islands.md)然而得用union find. T:O(n^3),遍历半个矩阵N^2然后union find的时候最坏情况O(N),不过用了path compress其实很可能是O(1)。然后S：O(N)，那个hashmap。

```java
class Solution {

    Map<Integer, DSNode> valToNode = new HashMap<>();

    public int findCircleNum(int[][] M) {
        if (M == null || M.length == 0 || M[0].length == 0) {
            return 0;
        }

        int totalGroup = M.length;
        // make a node for each student
        for (int i = 0; i < M.length; i++) {
            makeNode(i);
        }

        for (int i = 0; i < M.length; i++) {
            for (int j = i; j < M[0].length; j++) { // 只traverse半个矩阵就ok了
                // found a friendship
                if (M[i][j] == 1) {
                    if (connect(i, j)) {
                        totalGroup--;
                    }
                }
            }
        }

        return totalGroup;
    }

    private DSNode findGroup(DSNode node) {
        if (node == node.parent) {
            return node;
        }

        node.parent = findGroup(node.parent);
        return node.parent;
    }

    // if connect successfully return true, if already connected return false
    private boolean connect(int aVal, int bVal) {
        DSNode aNode = valToNode.get(aVal);
        DSNode bNode = valToNode.get(bVal);

        DSNode aGroup = findGroup(aNode);
        DSNode bGroup = findGroup(bNode);

        if (aGroup == bGroup) {
            return false;
        }

        // connect 2 nodes
        if (aGroup.rank >= bGroup.rank) {
            aGroup.rank += bGroup.rank;
            bGroup.rank = -1;
            bGroup.parent = aGroup;            
        } else {
            bGroup.rank += aGroup.rank;
            aGroup.rank = -1;
            aGroup.parent = bGroup;
        }

        return true;
    }

    private void makeNode(int val) {
        DSNode newNode = new DSNode(val);
        newNode.rank = 1;
        newNode.parent = newNode;
        valToNode.put(val, newNode);
    }

}

class DSNode {
    DSNode parent;
    int rank;
    int val;

    public DSNode(int v) {
        val = v;
    }
}
```

然而这题其实是给你一个adjacent matrix求联通块数。其实bfs和dfs都是可以的。想太复杂了。bfs，O(N^2)囧

```java
public int findCircleNum(int[][] M) {
    if (M == null || M.length == 0 || M[0].length == 0) {
        return 0;
    }

    int group = 0;
    boolean[] visited = new boolean[M.length];
    for (int i = 0; i < M.length; i++) {
        for (int j = i; j < M.length; j++) {
            if (M[i][j] == 1 && !visited[i]) {
                bfs(M, visited, i, j);
                group++;
            }
        }
    }

    return group;
}

private void bfs(int[][] M, boolean[] visited, int from, int to) {
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(from);
    visited[from] = true;

    while (!queue.isEmpty()) {
        int cur = queue.poll();
        for (int i = 0; i < M.length; i++) {
            if (!visited[i] && M[cur][i] == 1) {
                queue.offer(i);
                visited[i] = true;
            }
        }
    }
}
```
