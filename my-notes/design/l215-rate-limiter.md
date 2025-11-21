# L215 Rate Limiter

Implement a rate limiter, provide one method:`is_ratelimited(timestamp, event, rate, increment)`.

* `timestamp`: The current timestamp, which is an integer and in second unit.
* `event`: The string to distinct different event. for example, "login" or "signup".
* `rate`: The rate of the limit. 1/s (1 time per second), 2/m (2 times per minute), 10/h (10 times per hour), 100/d (100 times per day). The format is \[integer]/\[s/m/h/d].
* `increment`: Whether we should increase the counter. (or take this call as a hit of the given event)

The method should return true or false to indicate the event is limited or not.

**Example**

```
is_ratelimited(1, "login", "3/m", true), return false.
is_ratelimited(11, "login", "3/m", true), return false.
is_ratelimited(21, "login", "3/m", true), return false.
is_ratelimited(30, "login", "3/m", true), return true.
is_ratelimited(65, "login", "3/m", true), return false.
is_ratelimited(300, "login", "3/m", true), return false.
```

这题，跟网上普遍说的token bucket啥的有点不同，感觉人家不会突然改频率什么的。嘛，花了很长时间才看懂为那个increment是搞毛的。自己的不知道是哪里错了，所以先把答案贴上来了。另外加一句这个用来干嘛，可以防用户刷票，防暴力破解还能限制数据库expensive操作的执行频繁度。

```java
public class RateLimiter {
    private HashMap<String, List<Integer>> map = new HashMap<>();

    /**
     * @param timestamp the current timestamp
     * @param event the string to distinct different event
     * @param rate the format is [integer]/[s/m/h/d]
     * @param increment whether we should increase the counter
     * @return true of false to indicate the event is limited or not
     */
    public boolean isRatelimited(int current_timestamp, String event, String rate, boolean increment) {
        // Write your code here
        int start = rate.indexOf("/");
        int limit = Integer.parseInt(rate.substring(0, start));
        String type = rate.substring(start + 1, rate.length());

        int duration = 1;
        if (type.equals("m"))
            duration = duration * 60;
        else if (type.equals("h"))
            duration = duration * 60 * 60;
        else if (type.equals("d"))
            duration = duration * 60 * 60 * 24;
        int start_timestamp = current_timestamp - duration + 1;

        if (!map.containsKey(event))
            map.put(event, new ArrayList<Integer>());

        int count = count_events(map.get(event), start_timestamp);
        boolean is_ratelimited = count >= limit;
        if (increment && !is_ratelimited)
            insert_event(map.get(event), current_timestamp);
        return is_ratelimited;
    }

    public void insert_event(List<Integer> event, int timestamp) {
        event.add(timestamp);
    }

    // use binary search algorithm to count how many events happened
    // after start_timestamp because event is sorted by timestamp
    public int count_events(List<Integer> event, int start_timestamp) {
        int l = 0, r = event.size() - 1;
        if (r == -1)
            return 0;
        if (event.get(r) < start_timestamp) 
            return 0;
        int ans = 0;
        while (l <= r) {
            int mid = (l + r) >> 1;
            if (event.get(mid) >= start_timestamp) {
                ans = mid;
                r = mid - 1;
            } else
                l = mid + 1;
        }
        return event.size() - 1 - ans + 1;
    }
```

下面贴一个网上大神写的token bucket，能burst rate的高级货。

```java
static class TokenBucket {

  private final int capacity;
  private final int tokensPerSeconds;
  private int tokens = 0;
  private long timestamp = System.currentTimeMillis();

  public TokenBucket(int tokensPerUnit, TimeUnit unit) {
    capacity = tokensPerSeconds = (int) (tokensPerUnit / unit.toSeconds(1L));
  }

  public boolean take() {
    long now = System.currentTimeMillis();
    tokens += (int) ((now - timestamp) * tokensPerSeconds / 1000);
    if (tokens > capacity) tokens = capacity;
    timestamp = now;
    if (tokens < 1) return false;
    tokens--;
    return true;
  }

}

public static void main(String[] args) throws InterruptedException {
  TokenBucket bucket = new TokenBucket(250, TimeUnit.MINUTES);
  Thread.sleep(1000L);
  for (int i = 0; i < 5; i++) {
    System.out.println(bucket.take());
  }
  Thread.sleep(1000L);
  for (int i = 0; i < 5; i++) {
    System.out.println(bucket.take());
  }
}
```
