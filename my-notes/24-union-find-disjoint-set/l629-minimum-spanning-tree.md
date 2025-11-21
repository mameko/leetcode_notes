# L629 Minimum Spanning Tree

## L629 Minimum Spanning Tree

## Description

Given a list of Connections, which is the Connection class (the city name at both ends of the edge and a cost between them), find some edges, connect all the cities and spend the least amount.\
Return the connects if can connect all the cities, otherwise return empty list.

Return the connections sorted by the cost, or sorted city1 name if their cost is same, or sorted city2 if their city1 name is also same.

## Example

Gievn the connections =`["Acity","Bcity",1], ["Acity","Ccity",2], ["Bcity","Ccity",3]`

Return`["Acity","Bcity",1], ["Acity","Ccity",2]`

这题用 是什么Kuskal算法，就是先把边按权值排序，然后一条一条挑。如果发现两个点已经连上的话，那么就跳过那条边，因为树是没环的，所以edge = V - 1。要复习一下sort怎么写。

```java
/**
 * Definition for a Connection.
 * public class Connection {
 *   public String city1, city2;
 *   public int cost;
 *   public Connection(String city1, String city2, int cost) {
 *       this.city1 = city1;
 *       this.city2 = city2;
 *       this.cost = cost;
 *   }
 * }
 */
public class Solution {
    /**
     * @param connections given a list of connections include two cities and cost
     * @return a list of connections from results
     */
    HashMap<String, DSNode> map = new HashMap<>();
    public List<Connection> lowestCost(List<Connection> connections) {
        List<Connection> res = new ArrayList<>();
        if (connections == null || connections.size() == 0) {
            return res;
        }

        Collections.sort(connections, new Comparator<Connection>() {
            public int compare(Connection i1, Connection i2) {
                if (i1.cost != i2.cost) {
                    return i1.cost - i2.cost;
                } else if (!i1.city1.equals(i2.city1)) {
                    return i1.city1.compareTo(i2.city1);
                } else {
                    return i1.city2.compareTo(i2.city2);
                }
            }
        });

        for (Connection con : connections) {
            String Acity = con.city1;
            String Bcity = con.city2;

            if (!map.containsKey(Acity)) {
                makeNode(Acity);
            }

            if (!map.containsKey(Bcity)) {
                makeNode(Bcity);
            }

            if (!connected(Acity, Bcity)) {
                connect(Acity, Bcity);
                res.add(con);
            }
        }

        return res.size() == map.size() - 1 ? res : new ArrayList<>();
    }

    private boolean connected(String c1, String c2) {
        if (c1.equals(c2)) {
            return true;
        }

        DSNode a = map.get(c1);
        DSNode b = map.get(c2);

        return findGroup(a) == findGroup(b);
    }

    private DSNode findGroup(DSNode a) {
        if (a == a.parent) {
            return a;
        }

        a.parent = findGroup(a.parent);
        return a.parent;
    }

    private void connect(String c1, String c2) {
        DSNode a = map.get(c1);
        DSNode b = map.get(c2);

        if (connected(a.val, b.val)) {
            return;
        }

        DSNode pa = findGroup(a);
        DSNode pb = findGroup(b);
        if (pa.rank > b.rank) {
            pa.rank += pb.rank;
            pb.rank = -1;
            pb.parent = pa;
        } else {
            pb.rank += pa.rank;
            pa.rank = -1;
            pa.parent = pb;
        }
    }

    private void makeNode(String val) {
        DSNode newNode = new DSNode(val);
        newNode.parent = newNode;
        newNode.rank = 1;
        map.put(val, newNode);
    }
}


class DSNode {
    String val;
    int rank;
    DSNode parent;

    public DSNode(String v) {
        val = v;
    }
}
```
