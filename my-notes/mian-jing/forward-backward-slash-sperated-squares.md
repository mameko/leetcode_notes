# Forward backward slash sperated squares

输入一个矩阵，里面全是‘/’或‘\’，求这些线把现在这个矩阵切成多少块。来点例子好理解点：

例子一："\\//","///\\" 输出：6，例子二，“/\\""\\/"，输出：5

图是这样子的：（左边的最后一格忘了换颜色，应该是红色的\）

![](<../../.gitbook/assets/forward backword slash seperated squares pics.PNG>)

这题看了一阵子才搞懂怎么个分割法。面经里写了是用union find。所以试着做了。一开始，我把每个格子分成两份，然后每一份做一个disjoint set 点。所以例子一有16个，例子二有8个。标号从0开始到n，从左到右，从上到下标。然后根据观察知道，每一行中间的那些三角形（除了首尾的那两个），（1， 2）（3， 4）和（5，6）都是连在一起的，所以先把每行的连上。然后呢，要观察什么条件下连到下面的三角形。分情况讨论，先分奇偶，再分方向。最后一行一行处理，处理完以后就可以知道分了多少块。具体做法参照[L590 Connecting Graph II](../24-union-find-disjoint-set/l590-connecting-graph-ii.md).这里需要注意的是j的值，因为我们把方块一分为二，如果不小心的话会out of bound

```java
public class ForwardBackwardSlashSeperatedSquare {
    HashMap<Integer, DSNode> hm = new HashMap<>();
    int total;

    public int getPieceNum(List<String> slashs) {
        if (slashs == null || slashs.isEmpty()) {
            return -1;
        }

        // originally there are 2 * n nodes, becsause each slash cut the square in half
        int n = slashs.size();
        int m = slashs.get(0).length() * 2;
        total = n * m;
        // make set for each node
        for (int i = 0; i < total; i++) {
            makeSet(i);
        }

        // connect middle nodes in each row
        for (int i = 0; i < slashs.size(); i++) {
            for (int j = 1; j < m - 1; j = j + 2) {
                int index = i * m + j;
                connect(hm.get(index), hm.get(index + 1));
            }
        }

        // connect nodes between rows
        for (int i = 0; i < slashs.size() - 1; i++) {
            for (int j = 0; j < m / 2; j++) {
                int index = i * m + j * 2;
                if (index % 2 == 0) {// even
                    if (slashs.get(i).charAt(j) == 'l' && slashs.get(i + 1).charAt(j) == 'l') {// '\' & '\'
                        connect(hm.get(index), hm.get(index + m + 1));
                    } else if (slashs.get(i).charAt(j) == 'l' && slashs.get(i + 1).charAt(j) == 'r') {// '\' & '/'
                        connect(hm.get(index), hm.get(index + m));
                    }
                }

                index = index + 1;
                // odd
                if (slashs.get(i).charAt(j) == 'r' && slashs.get(i + 1).charAt(j) == 'r') {// '/' & '/'
                    connect(hm.get(index), hm.get(index + m - 1));
                } else if (slashs.get(i).charAt(j) == 'r' && slashs.get(i + 1).charAt(j) == 'l') {// '/' & '\'
                    connect(hm.get(index), hm.get(index + m));
                }
            }
        }

        return total;

    }

    public static void main(String[] args) {
         ForwardBackwardSlashSeperatedSquare sol = new ForwardBackwardSlashSeperatedSquare();
         List<String> slashes = new ArrayList<>();
         slashes.add("llrr");
         slashes.add("rrrl");
         System.out.println(sol.getPieceNum(slashes));// 6

        List<String> slashe2 = new ArrayList<>();
        slashe2.add("lr");
        slashe2.add("rl");
        System.out.println(sol.getPieceNum(slashe2));// 4

        List<String> slashe3 = new ArrayList<>();
        slashe3.add("rl");
        slashe3.add("lr");
        System.out.println(sol.getPieceNum(slashe3));// 5

        List<String> slashe4 = new ArrayList<>();
        slashe4.add("ll");
        slashe4.add("rr");
        System.out.println(sol.getPieceNum(slashe4));// 4

        List<String> slashe5 = new ArrayList<>();
        slashe5.add("llrr");
        slashe5.add("rrrl");
        slashe5.add("lrrr");
        System.out.println(sol.getPieceNum(slashe5));// 7
    }

    private void makeSet(int val) {
        DSNode newNode = new DSNode(val);
        newNode.rank = 1;
        newNode.parent = newNode;
        hm.put(val, newNode);
    }

    private void connect(DSNode n1, DSNode n2) {
        DSNode p1 = findParent(n1);
        DSNode p2 = findParent(n2);

        if (p1 == p2) {
            return;
        } else {
            if (p1.rank > p2.rank) {
                p1.rank += p2.rank;
                p2.parent = p1;
                p2.rank = 0;
            } else {
                p2.rank += p1.rank;
                p1.parent = p2;
                p1.rank = 0;
            }
            total--;
        }
    }

    private DSNode findParent(DSNode node) {
        if (node.parent == node) {
            return node;
        }

        node.parent = findParent(node.parent);
        return node.parent;
    }
}

class DSNode {
    int val;
    int rank;
    DSNode parent;

    public DSNode(int v) {
        val = v;
    }
}
```
