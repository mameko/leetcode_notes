# 362 Design Hit Counter

Design a hit counter which counts the number of hits received in the past 5 minutes.

Each function accepts a timestamp parameter (in seconds granularity) and you may assume that calls are being made to the system in chronological order (ie, the timestamp is monotonically increasing). You may assume that the earliest timestamp starts at 1.

It is possible that several hits arrive roughly at the same time.

**Example:**

```
HitCounter counter = new HitCounter();

// hit at timestamp 1.
counter.hit(1);

// hit at timestamp 2.
counter.hit(2);

// hit at timestamp 3.
counter.hit(3);

// get hits at timestamp 4, should return 3.
counter.getHits(4);

// hit at timestamp 300.
counter.hit(300);

// get hits at timestamp 300, should return 4.
counter.getHits(300);

// get hits at timestamp 301, should return 3.
counter.getHits(301);
```

**Follow up:**\
What if the number of hits per second could be very large? Does your design scale?

因为这题hit不会同时出现，例如第二秒不会同时有5个hit产生，所以用一个list来记录现在被hit的所有事件的时刻。每次hit的时候就直接append，然后在get的时候，逐个把超出5分钟的事件去掉，最后返回size。关于follow up：我想可以用一个linkedhashmap\<second, hitcount>来存发生时间和次数，和一个total变量来存储总数。处理方法跟这题类似，只是在hit的时候，一口气把所有hitcount加到total里，然后再gethit的时候，删除map里的数字时也从total里把那些值减去，最后返回total。

```java
public class HitCounter {

    LinkedList<Integer> hits;
    /** Initialize your data structure here. */
    public HitCounter() {
        hits = new LinkedList<>();
    }

    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        hits.add(timestamp);
    }

    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        while (hits.size() > 0 && timestamp - 300 >= hits.getFirst()) {
            hits.removeFirst();
        }
        return hits.size();
    }
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter obj = new HitCounter();
 * obj.hit(timestamp);
 * int param_2 = obj.getHits(timestamp);
 */
```
