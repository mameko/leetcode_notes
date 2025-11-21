# L465 Kth Smallest Sum In Two Sorted Arrays

Given two integer arrays sorted in ascending order and an integer k. Defin&#x65;_&#x73;um = a + b_, where\_a\_is an element from the first array and\_b\_is an element from the second one. Find the\_k\_th smallest sum out of all possible sums.

**Example**

Given`[1, 7, 11]`and`[2, 4, 6]`.

For k =`3`, return`7`.

For k =`4`, return`9`.

For k =`8`, return`15`.

[**Challenge**](http://www.lintcode.com/en/problem/kth-smallest-sum-in-two-sorted-arrays/#challenge)

Do it in either of the following time complexity:

1. O(k log min(n, m, k)). where n is the size of A, and m is the size of B.
2. O( (m + n) log maxValue). where maxValue is the max number in A and B.

这题其实是前一题[378](378-kth-smallest-element-in-a-sorted-matrix.md)的变体。因为两个数组的sum会组成一个sorted matrix，不是全有序，不过每行，每列都会有序。我们可以利用上一题的解法，这里的把下标控制好就ok了。还是找到k个为止。

```java
    public int kthSmallestSum(int[] A, int[] B, int k) {
       if (A == null || B == null || k < 0) {
        return Integer.MIN_VALUE;
    }

    if (A.length == 0 && B.length == 0) {
        return Integer.MIN_VALUE;
    } else if (A.length == 0 && B.length > 0) {
        return B[k];
    } else if (A.length > 0 && B.length == 0) {
        return A[k];
    }

    Queue<Loc> pq = new PriorityQueue<>();
    HashSet<Loc> visited = new HashSet<>();
    LinkedList<Integer> res = new LinkedList<>();
    Loc first = new Loc(0, 0, A[0] + B[0]);
    pq.offer(first);
    visited.add(first);
    while (res.size() < k) {
        Loc tmp = pq.poll();
        res.add(tmp.val);
        int nextI = tmp.i + 1;
        int nextJ = tmp.j + 1;

        if (nextI < A.length) {
            Loc next1 = new Loc(nextI, tmp.j, A[nextI] + B[tmp.j]);
            if (!visited.contains(next1)) {
                pq.offer(next1);
                visited.add(next1);
            }
        }

        if (nextJ < B.length) {
            Loc next2 = new Loc(tmp.i, nextJ, A[tmp.i] + B[nextJ]);
            if (!visited.contains(next2)) {
                pq.offer(next2);
                visited.add(next2);
            }
        }
    }
    return res.getLast();
}

class Loc implements Comparable<Loc> {
    int i;
    int j;
    int val;

    public Loc(int iLoc, int jLoc, int v) {
        i = iLoc;
        j = jLoc;
        val = v;
    }

    public int compareTo(Loc o) {
        return val - o.val;
    }

    @Override
    public String toString() {
        return String.valueOf(val);
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Loc) {
            Loc tmp = (Loc) obj;
            return i == tmp.i && j == tmp.j && val == tmp.val;
        }

        return false;
    }

    @Override
    public int hashCode() {
        return Objects.hash(i, j, val);
    }
}
```
