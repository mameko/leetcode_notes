# L131 Building Outline

Given\_N\_buildings in a x-axis，each building is a rectangle and can be represented by a triple (start, end, height)，where start is the start position on x-axis, end is the end position on x-axis and height is the height of the building. Buildings may overlap if you see them from far away，find the outline of them。

An outline can be represented by a triple, (start, end, height), where start is the start position on x-axis of the outline, end is the end position on x-axis and height is the height of the outline.

![](<../../.gitbook/assets/image (17).png>)

## Notice

Please merge the adjacent outlines if they have the same height and make sure different outlines cant overlap on x-axis.

**Example**

Given 3 buildings：

```
[
  [1, 3, 3],
  [2, 4, 4],
  [5, 6, 1]
]
```

The outlines are：

```
[
  [1, 2, 3],
  [2, 4, 4],
  [5, 6, 1]
]
```

写下来以供参考，做法看注释。这里扫描线算法，只在高度发生变化的时候，需要看一下。

```java
public class Solution {
    /**
     * @param buildings: A list of lists of integers
     * @return: Find the outline of those buildings
     */
    public ArrayList<ArrayList<Integer>> buildingOutline(int[][] buildings) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if (buildings == null || buildings.length == 0) {
            return res;
        }
        ArrayList<QueueNode> nodes = new ArrayList<>();
        // make node and put them in heap
        for (int[] elem : buildings) {
            int s = elem[0];
            int e = elem[1];
            int h = elem[2];
            QueueNode n1 = new QueueNode(s, h, true);
            QueueNode n2 = new QueueNode(e, h, false);
            nodes.add(n1);
            nodes.add(n2);
        }

        // sort building start and end nodes according to rules below
        Collections.sort(nodes);

        // check one point at a time, use heap to get max
        // may have duplicated height so use int -> height, int -> number
        TreeMap<Integer, Integer> heap = new TreeMap<>();
        ArrayList<Integer> aPoint = null;
        for (QueueNode queueNode : nodes) {
            if (queueNode.isStart) {
                // if encounter a start point
                // and the new starting point bigger than the current heap's max,
                // meaning the new point will be part of the skyline. so add it.
                if (heap.isEmpty() || queueNode.height > heap.lastKey()) {
                    aPoint = new ArrayList<>();
                    aPoint.add(queueNode.pos);
                    aPoint.add(queueNode.height);
                    res.add(aPoint);
                }
                // add this point in the heap for future comparison
                if (heap.containsKey(queueNode.height)) {
                    heap.put(queueNode.height, heap.get(queueNode.height) + 1);
                } else {
                    heap.put(queueNode.height, 1);
                }
            } else {
                // if encounter a end point
                // we delete it from heap
                if (heap.get(queueNode.height) > 1) {
                    heap.put(queueNode.height, heap.get(queueNode.height) - 1);
                } else if (heap.get(queueNode.height) == 1) {
                    heap.remove(queueNode.height);
                }
                // if the removed point is the highest point in heap,
                // means max height changes need to add it to skyline
                if (heap.isEmpty() || queueNode.height > heap.lastKey()) {
                    if (heap.isEmpty()) {
                        aPoint = new ArrayList<>();
                        aPoint.add(queueNode.pos);
                        aPoint.add(0);
                    } else {
                        aPoint = new ArrayList<>();
                        aPoint.add(queueNode.pos);
                        aPoint.add(heap.lastKey());
                    }
                    res.add(aPoint);
                }
            }

        }
        // if it's in leetcode you can just return res now.
        // return res;
        // but lintcode need add one more step to transfer the result to correct output format
        return format(res);
    }

    public ArrayList<ArrayList<Integer>> format(ArrayList<ArrayList<Integer>> res) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<ArrayList<Integer>>();
        if (res.size() > 1) {
            int prePos = res.get(0).get(0);
            int preHeight = res.get(0).get(1);
            for (int i = 1; i < res.size(); i++) {
                ArrayList<Integer> cur = new ArrayList<>();
                int pos = res.get(i).get(0);
                if (preHeight > 0) {
                    cur.add(prePos);
                    cur.add(pos);
                    cur.add(preHeight);
                    ans.add(cur);
                }

                prePos = pos;
                preHeight = res.get(i).get(1);
            }
        }
        return ans;
    }
}

class QueueNode implements Comparable<QueueNode> {
    int pos;
    int height;
    boolean isStart;

    public QueueNode(int p, int h, boolean s) {
        pos = p;
        height = h;
        isStart = s;
    }

    @Override
    public int compareTo(QueueNode o) {
        if (o.pos != pos) {// if not at same place, larger x means later in the queue
            return pos - o.pos;
        } else {
            // if pos are the same
            // 1. both are start, point with higher height comes first
            // 2. both are end, point with lower height comes first
            // 3. if start and end overlap, the start should comes first
            return (this.isStart ? -this.height : this.height) - (o.isStart ? -o.height : o.height);
        }

    }
}
```
