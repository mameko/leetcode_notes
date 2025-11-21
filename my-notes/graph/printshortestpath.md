# PrintShortestPath

2d grid has obstacles, given start and end point, find one shortest path.

我的做法是bfs，然后每个点把自己从哪里来的记录下来，最后从end backtrack到start。这里需要注意的是node得复写equal和hashcode方法。另外java是**值传递！值传递！值传递！**&#x8BB0;得返回改完以后的end node。print时候的循环记得每次++（换成前一个node）不然会**死循环**。onsite那天真心撞邪...follow up：如果end point和grid是固定的，然后重复call这函数，每次输出最短距离的话，可以反向bfs，把最短距离的grid填好，每次直接返回就ok了。

```java
package graph;

import java.util.LinkedList;
import java.util.Queue;

public class PrintShortestPath {
    public Node findPath(char[][] board, Node start, Node end) throws Exception {
        if (board == null || board.length == 0 || board[0].length == 0) {
            throw new Exception("invalid input");
        }

        int[] x = { 0, 0, 1, -1 };
        int[] y = { 1, -1, 0, 0 };

        int n = board.length;
        int m = board[0].length;
        boolean[][] visited = new boolean[n][m];
        Queue<Node> queue = new LinkedList<>();
        queue.offer(start);
        visited[start.curi][start.curj] = true;
        while (!queue.isEmpty()) {
            Node cur = queue.poll();

            for (int k = 0; k < 4; k++) {
                int nexti = cur.curi + x[k];
                int nextj = cur.curj + y[k];

                if (inBound(nexti, nextj, board) && !visited[nexti][nextj] && board[nexti][nextj] != '1') {
                    Node newNode = new Node(nexti, nextj, cur);
                    if (newNode.equals(end)) {
                        return newNode;
                    }
                    queue.offer(newNode);
                    visited[nexti][nextj] = true;
                }
            }

        }

        return null;
    }

    private boolean inBound(int nexti, int nextj, char[][] board) {
        if (nexti < 0 || nextj < 0 || nexti >= board.length || nextj >= board[0].length) {
            return false;
        }
        return true;
    }

    public void printPath(Node node) {
        StringBuilder sb = new StringBuilder();
        Node last = node;
        while (last != null) {
            sb.insert(0, "(" + last.curi + "," + last.curj + ")");
            last = last.prevNode;
        }

        System.out.println(sb.toString());
    }

    public static void main(String[] args) {
        PrintShortestPath sol = new PrintShortestPath();
        char[][] board = { { '0', '0', '1', '0', '0' }, { '0', '0', '0', '0', '0' }, { '0', '0', '1', '0', '0' },
                { '0', '0', '1', '1', '0' } };
        try {

            Node start = new Node(1, 1, null);
            Node end = new Node(0, 4, null);
            end = sol.findPath(board, start, end);
            sol.printPath(end);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class Node {
    Integer curi;
    Integer curj;
    Node prevNode;

    public Node(Integer ci, Integer cj, Node prev) {
        curi = ci;
        curj = cj;
        prevNode = prev;
    }

    public int hashCode() {
        return curi;
    }

    public boolean equals(Object obj) {
        boolean flag = false;
        Node n = (Node) obj;
        if (n.curi == curi && n.curj == curj) {
            flag = true;
        }
        return flag;
    }
}
```
