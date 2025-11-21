# meeting room 3

就是只有一个房间，每个meeting有不同的weight，怎么安排以得到最大的weighted sum。（重要的会议都能开）只要把会议的finishing time按照非递减排序，这问题就会转化成[300 LIS](397-longest-increasing-continuous-subsequence.md)

```java
package mianjing;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class S_WeightedJobScheduling {
    public int maxValue(List<WeightedInterval> intervals) {
        Collections.sort(intervals, new Comparator<WeightedInterval>() {
            public int compare(WeightedInterval in1, WeightedInterval in2) {
                return in1.end - in2.end;
            }
        });

        int n = intervals.size();
        int[] dp = new int[n];

        for (int i = 0; i < n; i++) {
            dp[i] = intervals.get(i).weight;
            for (int j = 0; j < i; j++) {
                if (intervals.get(j).end <= intervals.get(i).start) {
                    dp[i] = Math.max(dp[i], dp[j] + intervals.get(i).weight);
                }
            }
        }

        int max = 0;
        for (int i = 0; i < n; i++) {
            max = Math.max(max, dp[i]);
        }
        return max;
    }

    public static void main(String[] args) {
        S_WeightedJobScheduling sol = new S_WeightedJobScheduling();
        List<WeightedInterval> intervals = new ArrayList<>();
        WeightedInterval job1 = new WeightedInterval(1, 3, 5);
        WeightedInterval job2 = new WeightedInterval(2, 5, 6);
        WeightedInterval job3 = new WeightedInterval(4, 6, 5);
        WeightedInterval job4 = new WeightedInterval(6, 7, 4);
        WeightedInterval job5 = new WeightedInterval(5, 8, 11);
        WeightedInterval job6 = new WeightedInterval(7, 9, 2);
        intervals.add(job1);
        intervals.add(job2);
        intervals.add(job3);
        intervals.add(job4);
        intervals.add(job5);
        intervals.add(job6);
        System.out.println(sol.maxValue(intervals));

        List<WeightedInterval> intervals2 = new ArrayList<>();
        WeightedInterval job21 = new WeightedInterval(3, 10, 20);
        WeightedInterval job22 = new WeightedInterval(1, 2, 50);
        WeightedInterval job23 = new WeightedInterval(6, 19, 100);
        WeightedInterval job24 = new WeightedInterval(2, 100, 200);        

        intervals2.add(job21);
        intervals2.add(job22);
        intervals2.add(job23);
        intervals2.add(job24);
        System.out.println(sol.maxValue(intervals2));
    }

}

class WeightedInterval {
    int start;
    int end;
    int weight;

    public WeightedInterval(int s, int e, int w) {
        start = s;
        end = e;
        weight = w;
    }
}
```
