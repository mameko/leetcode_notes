# 435 Non-overlapping Intervals

Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

**Note:**

1. You may assume the interval's end point is always bigger than its start point.
2. Intervals like \[1,2] and \[2,3] have borders "touching" but they don't overlap each other.

**Example 1:**

```
Input:
 [ [1,2], [2,3], [3,4], [1,3] ]


Output:
 1


Explanation:
 [1,3] can be removed and the rest of intervals are non-overlapping.
```

**Example 2:**

```
Input:
 [ [1,2], [1,2], [1,2] ]


Output:
 2


Explanation:
 You need to remove two [1,2] to make the rest of intervals non-overlapping.
```

**Example 3:**

```
Input:
 [ [1,2], [2,3] ]


Output:
 0


Explanation:
 You don't need to remove any of the intervals since they're already non-overlapping.
```

这题做法是排个序，然后loop着找重叠的区间，找到就”去掉“然后result++。不过这里不用真的去掉，我们用一个变量记录上一个是哪个就ok了。这题还有一个难题，就是重叠了怎么判断留哪个呢？答案是留end小的那个，这样就可以尽可能地避免与后面的产生更多overlap。

```java
public class Solution {
    public int eraseOverlapIntervals(Interval[] intervals) {
        if (intervals == null || intervals.length == 0) {
            return 0;
        }

        Arrays.sort(intervals, new Comparator<Interval>() {
           public int compare(Interval in1, Interval in2) {
               return in1.start - in2.start;
           }
        });

        Interval last = intervals[0];
        int res = 0;
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i].start < last.end) {
                res++;
                if (intervals[i].end >= last.end) {// keep the one has lesser end range
                    continue;
                }
            }
            last = intervals[i];
        }        
        return res;
    }
}
```
