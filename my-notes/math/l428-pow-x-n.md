# L428 Pow x n

Implement pow(x, n).

## Notice

You don't need to care about the precision of your answer, it's acceptable if the expected answer and your answer 's difference is smaller than`1e-3`.

**Example**

```
Pow(2.1, 3) = 9.261
Pow(0, 1) = 0
Pow(1, 0) = 1
```

[**Challenge**](http://www.lintcode.com/en/problem/powx-n/#challenge)

O(logn) time

每次把次方缩小一半的同时，把base的数增加一倍。再贴一个递归版本，感觉容易理解一点。记得判断次方是正是负。（负的话要刀数）

```java
public double myPow(double x, int n) {
    boolean positive = true;
    if (n < 0) {
        n = n * -1;
        positive = false;
    } 

    double res = 1.0;
    for (int i = n; i != 0; i = i / 2) {
        if (i % 2 != 0) {
            res = res * x;
        }

        x = x * x;
    }

    return positive ? res : 1 / res;
}
```

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if (n < 0) return 1 / power(x, -n);
        return power(x, n);
    }
    double power(double x, int n) {
        if (n == 0) return 1;
        double half = power(x, n / 2);
        if (n % 2 == 0) return half * half;
        return x * half * half;
    }
};
```
