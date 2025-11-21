# L586 Sqrt x II

Implement`double sqrt(double x)`and`x >= 0`.

Compute and return the square root of x.

## Notice

You do not care about the accuracy of the result, we will help you to output results.

**Example**

Given`n`=`2`return`1.41421356`

```java
public double sqrt(double x) {
    if (x == 1) {
        return 1;
    }

    double xi = 1.0;
    double xi1 = 2.0;
    while (true) {
        xi1 = (xi + x / xi) / 2;
        if (Math.abs(xi1 - xi) < 0.000000001) {
            break;
        }
        xi = xi1;
    }

    return xi1;
}
```

2分 ：要注意ｘ的值小于１时二分的范围会不同，ｅｇ：ｘ＝０.０１，ｒｅｔｕｒｎ０.１

```java
public double sqrt(double x) {
    if (x < 0) {
        return 0.0;
    }

    double start = 0;
    double end = x;
    if (x < 1) {
        end = 1;
    }

    double delta = 0.000000001;

    while ((end - start) > delta) {
        double mid = start + (end - start) / 2;
        if (mid < x / mid) {
            start = mid;
        } else {
            end = mid;
        }
    }
    return start + (end - start) / 2;
}
```
