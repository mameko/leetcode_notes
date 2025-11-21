# 218 The Skyline Problem

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are **given the locations and height of all the buildings**as shown on a cityscape photo (Figure A), write a program to **output the skyline** formed by these buildings collectively (Figure B).

[![Buildings](https://leetcode.com/static/images/problemset/skyline1.jpg)](https://leetcode.com/static/images/problemset/skyline1.jpg)

[![Skyline Contour](https://leetcode.com/static/images/problemset/skyline2.jpg)](https://leetcode.com/static/images/problemset/skyline2.jpg)

The geometric information of each building is represented by a triplet of integers`[Li, Ri, Hi]`, where`Li`and`Ri`are the x coordinates of the left and right edge of the ith building, respectively, and`Hi`is its height. It is guaranteed that`0 ≤ Li, Ri ≤ INT_MAX`,`0 < Hi ≤ INT_MAX`, and`Ri - Li > 0`. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

For instance, the dimensions of all buildings in Figure A are recorded as:`[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ]`.

The output is a list of "**key points**" (red dots in Figure B) in the format of`[ [x1,y1], [x2, y2], [x3, y3], ... ]`that uniquely defines a skyline.**A key point is the left endpoint of a horizontal line segment**. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:`[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]`.

**Notes:**

* The number of buildings in any input list is guaranteed to be in the range`[0, 10000]`.
* The input list is already sorted in ascending order by the left x position`Li`.
* The output list must be sorted by the x position.
*   There must be no consecutive horizontal lines of equal height in the output skyline. For instance,

    `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]`

    is not acceptable; the three lines of height 5 should be merged into one in the final output as such:

    `[...[2 3], [4 5], [12 7], ...]`

就输出不同，不过还是写下来以供参考。

```java
public class Solution {
    public List<int[]> getSkyline(int[][] buildings) {
        ArrayList<int[]> res = new ArrayList<>();
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
        PriorityQueue<Integer> heap = new PriorityQueue<>(nodes.size(), Collections.reverseOrder());
        int[] aPoint = null;
        for (QueueNode queueNode : nodes) {
            if (queueNode.isStart) {
                // if encounter a start point
                // and the new starting point bigger than the current heap's max,
                // meaning the new point will be part of the skyline. so add it.
                if (heap.isEmpty() || queueNode.height > heap.peek()) {
                    aPoint = new int[2];
                    aPoint[0] = queueNode.pos;
                    aPoint[1] = queueNode.height;
                    res.add(aPoint);
                }
                // add this point in the heap for future comparison
                heap.offer(queueNode.height);
            } else {
                // if encounter a end point
                // we delete it from heap
                heap.remove(queueNode.height);
                // if the removed point is the highest point in heap,
                // means max height changes need to add it to skyline
                if (heap.isEmpty() || queueNode.height > heap.peek()) {
                    if (heap.isEmpty()) {
                        aPoint = new int[2];
                        aPoint[0] = queueNode.pos;
                        aPoint[1] = 0;
                    } else {
                        aPoint = new int[2];
                        aPoint[0] = queueNode.pos;
                        aPoint[1] = heap.peek();
                    }
                    res.add(aPoint);
                }
            }

        }
        // if it's in leetcode you can just return res now.
        return res;
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
}
```
