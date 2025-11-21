# 57 Insert Intervals

Given a set ofnon-overlappingintervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**\
Given intervals`[1,3],[6,9]`, insert and merge`[2,5]`in as`[1,5],[6,9]`.

**Example 2:**\
Given`[1,2],[3,5],[6,7],[8,10],[12,16]`, insert and merge`[4,9]`in as`[1,2],[3,10],[12,16]`.

This is because the new interval`[4,9]`overlaps with`[3,5],[6,7],[8,10]`.

这题也是，先排序，然后通过循环来找新区间插入位置。

```java
public ArrayList<Interval> insert(ArrayList<Interval> intervals, Interval newInterval) {
    ArrayList<Interval> result = new ArrayList<Interval>();
    if (intervals == null || newInterval == null) {
        return result;
    } else if (intervals.size() == 0 && newInterval != null) {
        result.add(newInterval);
        return result;
    }

    // don't need to binary search for insert location, do it while adding elem to result
    int insertPos = 0;
    for (Interval cur : intervals) {
        // find intervals that's is outside the new Interval, add them as they are
        if (cur.end < newInterval.start) {
            result.add(cur);
            insertPos++;// keep track of insert postion
        } else if (cur.start > newInterval.end) {
            result.add(cur);
        } else {
            // if the current interval intersect with new interval, 
            // merge them into the new interval
            newInterval.start = Math.min(cur.start, newInterval.start);
            newInterval.end = Math.max(cur.end, newInterval.end);
        }
    }

        // insert the new interval to correct position then return.
    result.add(insertPos, newInterval);

    return result;
}
```
