# 346 Moving Average from Data Stream

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

For example,

```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```

就是滑动着的sum，窗口满了的时候，后面加一个前面减一个。

```java
public class MovingAverage {

    Queue<Integer> storage;
    int s;
    double curTotal = 0;

    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        storage = new LinkedList<>();
        s = size;
    }

    public double next(int val) {
        storage.add(val);
        if (storage.size() > s) {
            curTotal -= storage.poll();
        }

        curTotal += val;
        return curTotal / storage.size();
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```
