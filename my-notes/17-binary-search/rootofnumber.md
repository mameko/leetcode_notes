# RootOfNumber

## Root of Number

Many times, we need to re-implement basic functions without using any standard library functions already implemented. For example, when designing a chip that requires very little memory space.

In this question we’ll implement a function`root`that calculates the`n’th`root of a number. The function takes a nonnegative number`x`and a positive integer`n`, and returns the positive`n’th`root of`x`within an error of`0.001`(i.e. suppose the real root is`y`, then the error is:`|y-root(x,n)|`and must satisfy`|y-root(x,n)| < 0.001`).

Don’t be intimidated by the question. While there are many algorithms to calculate roots that require prior knowledge in numerical analysis (some of them are mentioned [here](https://en.wikipedia.org/wiki/Nth_root#Computing_principal_roots)), there is also an elementary method which doesn’t require more than guessing-and-checking. Try to think more in terms of the latter.

Make sure your algorithm is efficient, and analyze its time and space complexities.

**Examples:**

```
input:  x = 7, n = 3
output: 1.913

input:  x = 9, n = 2
output: 3
```

**Constraints:**

* **\[time limit] 5000ms**
* **\[input] float**`x`0 ≤ x
* **\[input] integer**`n`0 < n
* **\[output] float**

这题就是改了一下[L586 Sqrt(x) II](../math/l586-sqrt-x-ii.md)。那题开平方，这题能开n方。做法还是二分。这里还学了个优雅点的写法。

```java
public class RootOfNumber {

    public static void main(String[] args) {        
        System.out.println(RootOfNumber.root(7, 3));
    }

    static double root(double x, int n) {
        if (x <= 0 || n < 0) {
            return 0;
        }

        double start = 0; // 注意从0开始找，别太抠，从1开始
        double end = Math.max(1, x); // 这里不用if else，也别太抠，end在x/2

        double delta = 0.001;
        while (end - start > delta) {
            double mid = start + (end - start) / 2;
            // 算mid ^ n
            double val = 1;
            for (int i = 0; i < n; i++) {
                val = val * mid;
            }

            if (val > x) {
                end = mid;
            } else {
                start = mid;
            }
        }

        return start + (end - start) / 2;
    }
```
