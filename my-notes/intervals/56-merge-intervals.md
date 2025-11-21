# 56 Merge Intervals

Given a collection of intervals, merge all overlapping intervals.

For example,\
Given`[1,3],[2,6],[8,10],[15,18]`,\
return`[1,6],[8,10],[15,18]`.

这题得先按start排个序，然后再loop。loop的时候用last记录上一个区间，如果现在loop到的区间跟那个重合了，就把两个区间合并到last里。如果没重合就把last区间放到答案里。最后别忘了跳出循环后，要把最后一个区间放到result里。

```java
public List<Interval> merge(List<Interval> intervals) {
        List<Interval> res = new ArrayList<>();
        if (intervals == null || intervals.size() == 0) {
            return res;
        }
        // 记住怎么写Comparator
        Collections.sort(intervals, new Comparator<Interval>(){
          public int compare(Interval i1, Interval i2) {
              return i1.start - i2.start;
          }
        }
        );

        Interval last = intervals.get(0);
        for (int i = 1; i < intervals.size(); i++) {
            Interval cur = intervals.get(i);
            // merge的时候比较现在的start时候与前面的区间重合，如果是的话，我们合并，具体方法是区两个区间end的大的那个
            if (cur.start <= last.end) {
                last.end = Math.max(cur.end, last.end);
            } else {
                res.add(last);
                last = cur;
            }
        }
        res.add(last);
        return res;
    }
```
