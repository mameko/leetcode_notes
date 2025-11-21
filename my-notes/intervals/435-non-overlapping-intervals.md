# 352 Data Stream as Disjoint Intervals

Given a data stream input of non-negative integers a1, a2, ..., an, ..., summarize the numbers seen so far as a list of disjoint intervals.

For example, suppose the integers from the data stream are 1, 3, 7, 2, 6, ..., then the summary will be:

```
[1, 1]
[1, 1], [3, 3]
[1, 1], [3, 3], [7, 7]
[1, 3], [7, 7]
[1, 3], [6, 7]
```

**Follow up:**\
What if there are lots of merges and the number of disjoint intervals are small compared to the data stream's size?

这题有两种方法，可以用insert Interval的方法。把新建的区间loop一遍，找位置放进现有的集合里。这样每次需要O(n)。另一种方法是用TreeSet来快速找到插入位置。O（logn）

```java
public class SummaryRanges {
    TreeSet<Interval> treeSet;
    /** Initialize your data structure here. */
    public SummaryRanges() {//还是得建个comparator来按起点排序
        treeSet = new TreeSet<Interval>(new Comparator<Interval>() {
            public int compare(Interval in1, Interval in2) {
                return in1.start - in2.start;
            }
        });
    }

    public void addNum(int val) {
        Interval newInterval = new Interval(val, val);

        //每次插入时，首先查一下前一个相邻的区间是否有重合
        Interval floor = treeSet.floor(newInterval);
        if (floor != null) {
            // if the new interval is included in the previous Interval, we don't need to insert and return
            if (newInterval.start <= floor.end) {
                return;
            } else if (newInterval.start == floor.end + 1) {// if cur Interval can be combine with previous one
                newInterval.start = floor.start;// we update our new interval
                treeSet.remove(floor);// remove the old one, insert the new interval later in code
            }
        }

        // do same thing, to check whether the next interval can be combine as well
        Interval higher = treeSet.higher(newInterval);
        if (higher != null) {// if so, remove old interval and update current one
            if (newInterval.end + 1 == higher.start) {
                newInterval.end = higher.end;
                treeSet.remove(higher);
            }
        }

        treeSet.add(newInterval);// add the interval to current Set
    }

    public List<Interval> getIntervals() {
        return new ArrayList<Interval>(treeSet);
    }
}
```
