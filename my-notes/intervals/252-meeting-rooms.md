# 252 Meeting Rooms

Given an array of meeting time intervals consisting of start and end times`[[s1,e1],[s2,e2],...]`(si< ei), determine if a person could attend all meetings.

For example,\
Given`[[0, 30],[5, 10],[15, 20]]`,\
return`false`.

这题很简单，就是按照start排序，然后check一下有没有overlap就行了。

```java
public boolean canAttendMeetings(Interval[] intervals) {
    if (intervals == null || intervals.length < 2) {
        return true;
    }

    Arrays.sort(intervals, new Comparator<Interval>(){
        public int compare(Interval in1, Interval in2) {
            return in1.start - in2.start;
        }
    });

    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i].start < intervals[i - 1].end) {
            return false;
        }
    }

    return true;
}
```
