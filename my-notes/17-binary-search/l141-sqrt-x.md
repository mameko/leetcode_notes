# L141 Sqrt x

Implement`int sqrt(int x)`.

Compute and return the square root of x.

**Example**

sqrt(3) = 1

sqrt(4) = 2

sqrt(5) = 2

sqrt(10) = 3

[**Challenge**](http://www.lintcode.com/en/problem/sqrtx/#challenge)

O(log(x))

```java
public int sqrt(int x) {
    // find the last number which square of it <= x
    long start = 1, end = x;
    while (start + 1 < end) {
        long mid = start + (end - start) / 2;
        if (mid * mid <= x) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (end * end <= x) {
        return (int) end;
    }
    return (int) start;
}
```

```java
public int sqrt(int x) {
    if (x <= 0) {
        return 0;
    }

    int low = 1;
    int high = x;
    while (low + 1 < high) {
        int mid = low + (high - low) / 2;
        if (mid == x / mid) {
            return mid;
        } else if (mid > x / mid) {
            high = mid;
        } else {
            low = mid;
        }
    }

    if (low <= x / low) {
        return low;
    }

    if (high <= x / high) {
        return high;
    }
    return 0;
}
```
