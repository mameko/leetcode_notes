# 253 Meeting Rooms II

Given an array of meeting time intervals consisting of start and end times`[[s1,e1],[s2,e2],...]`(si< ei), find the minimum number of conference rooms required.

For example,\
Given`[[0, 30],[5, 10],[15, 20]]`,\
return`2`.

跟其他line sweep一样，要把起始点和重点分别拿出来排序，然后数。还是得注意排序时的顺序，不然就会多算或少算。一开始有个test case \[1, 13] \[13, 15]没过，应该返回1，结果我返回了2。所以知道了如果时间点相同的话end得排在start前面。改了一下排序的方法，就过了。

```java
public class Solution {
    public int minMeetingRooms(Interval[] intervals) {
        if (intervals == null || intervals.length == 0) {
            return 0;
        }

        ArrayList<Pair> timeLine = new ArrayList<>();
        for (Interval in : intervals) {
            timeLine.add(new Pair(in.start, true));
            timeLine.add(new Pair(in.end, false));
        }

        Collections.sort(timeLine);

        int rooms = 0;
        int tmp = 0;
        for (Pair p : timeLine) {
            if (p.isStart) {
                tmp++;
            } else {
                tmp--;
            }

            rooms = Math.max(rooms, tmp);
        }

        return rooms;
    }
}

class Pair implements Comparable<Pair> {
    int time;
    boolean isStart;

    public Pair(int t, boolean start) {
        time = t;
        isStart = start;
    }

    public int compareTo(Pair p2) {
        if (this.time == p2.time) {
            if (this.isStart) {
                return 1;
            } else if (p2.isStart) {
                return -1;
            }
        }
        return this.time - p2.time;
    }
}
```
