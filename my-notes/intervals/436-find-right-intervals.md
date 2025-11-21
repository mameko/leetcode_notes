# 436 Find Right Intervals

Given a set of intervals, for each of the interval i, check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, which can be called that j is on the "right" of i.

For any interval i, you need to store the minimum interval j's index, which means that the interval j has the minimum start point to build the "right" relationship for interval i. If the interval j doesn't exist, store -1 for the interval i. Finally, you need output the stored value of each interval as an array.

**Note:**

1. You may assume the interval's end point is always bigger than its start point.
2. You may assume none of these intervals have the same start point.

**Example 1:**

```
Input:
 [ [1,2] ]


Output:
 [-1]


Explanation:
 There is only one interval in the collection, so it outputs -1.
```

**Example 2:**

```
Input:
 [ [3,4], [2,3], [1,2] ]


Output:
 [-1, 0, 1]


Explanation:
 There is no satisfied "right" interval for [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point;
For [1,2], the interval [2,3] has minimum-"right" start point.
```

**Example 3:**

```
Input:
 [ [1,4], [2,3], [3,4] ]


Output:
 [-1, 2, -1]


Explanation:
 There is no satisfied "right" interval for [1,4] and [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point.
```

这题还是先排序，然后用TreeMap自带的ceiling方法来快速（logn）找到合适的区间。Map里存着《区间，原来位置》这里我脑残用了两个数据结构，有空会改用treemap的。

```java
public int[] findRightInterval(Interval[] intervals) {
    if (intervals == null || intervals.length == 0) {
        return null;
    }

    int n = intervals.length;
    int[] res = new int[n];

    // use for keep track of interval and original location info
    HashMap<Interval, Integer> in2LocMap = new HashMap<>();

    TreeSet<Interval> ordered = new TreeSet<>(new Comparator<Interval>() {
        public int compare(Interval in1, Interval in2) {
            return in1.start - in2.start;
        }
    });

    // put interval in map & set
    for (int i = 0; i < n; i++) {
        in2LocMap.put(intervals[i], i);
        ordered.add(intervals[i]);
    }

    // find right bound
    for (int i = 0; i < n; i++) {
        Interval cur = intervals[i];
        Interval ceiling = ordered.ceiling(new Interval(cur.end, cur.end));

        if (ceiling != null) {
            res[i] = in2LocMap.get(ceiling);
        } else {
            res[i] = -1;
        }
    }

    return res;
}
```
