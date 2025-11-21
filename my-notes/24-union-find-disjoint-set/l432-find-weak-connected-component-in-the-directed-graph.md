# L432 Find Weak Connected Component in the Directed Graph

Find the number Weak Connected Component in the directed graph. Each node in the graph contains a label and a list of its neighbors. (a connected set of a directed graph is a subgraph in which any two vertices are connected by direct edge path.)

## Notice

Sort the element in the set in increasing order

**Example**

Given graph:

```
A----->B  C
 \     |  | 
  \    |  |
   \   |  |
    \  v  v
     ->D  E <- F
```

Return`{A,B,D}, {C,E,F}`. Since there are two connected component which are`{A,B,D} and {C,E,F}`

因为是弱联通有向图，所以不能像L431那样用bfs/dfs来找到所有的点。所以要用union find。

```java
    HashMap<Integer, DSNode> map = new HashMap<>();
    /**
     * @param nodes a array of Directed graph node
     * @return a connected set of a directed graph
     */
    public List<List<Integer>> connectedSet2(ArrayList<DirectedGraphNode> nodes) {
        List<List<Integer>> res = new ArrayList<>();
        if (nodes == null || nodes.size() == 0) {
            return res;
        }

        for (DirectedGraphNode n : nodes) {
            makeSet(n.label);
        }

        HashSet<DirectedGraphNode> visited = new HashSet<>();
        for (DirectedGraphNode n : nodes) {
            // visited.add(n);
            for (DirectedGraphNode nei : n.neighbors) {
                // if (!visited.contains(nei)) {
                    connect(n.label, nei.label);
                    // visited.add(nei);
                // }
            }
        }

        HashMap<DSNode, ArrayList<Integer>> gather = new HashMap<>();
        for (DirectedGraphNode n : nodes) {
            ArrayList<Integer> tmp = null;
            DSNode curNode = map.get(n.label);
            DSNode p = findSet(curNode);
            if (gather.containsKey(p)) {
                tmp = gather.get(p);
            } else {
                tmp = new ArrayList<>();
            }
            tmp.add(n.label);
            gather.put(p, tmp);
        }

        for (Map.Entry<DSNode, ArrayList<Integer>> e : gather.entrySet()) {
            res.add(e.getValue());
        }

        return res;
    }

    // private boolean query(int label1, int label2) {
    //     DSNode node1 = map.get(label1);
    //     DSNode node2 = map.get(label2);


    // }

    private void connect(int a, int b) {
        DSNode na = map.get(a);
        DSNode nb = map.get(b);

        DSNode nap = findSet(na);
        DSNode nbp = findSet(nb);

        if (nap == nbp) {//already in same set
            return;
        }

        nap.parent = nbp;
        nbp.rank += nap.rank;
        nap.rank = -1;

    }

    private DSNode findSet(DSNode node) {
        DSNode parent = node.parent;
        if (parent == node) {
            return parent;
        }

        node.parent = findSet(node.parent);
        return node.parent;
    }

    private void makeSet(int val) {
        DSNode newNode = new DSNode(val);
        newNode.rank = 1;
        newNode.parent = newNode;
        map.put(val, newNode);
    }
}

class DSNode {
    int val;
    int rank;
    DSNode parent;

    public DSNode(int val) {
        this.val = val;
    }
}
```
