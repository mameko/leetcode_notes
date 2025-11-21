# 367 Valid Perfect Square

Given a positive integer num, write a function which returns True if num is a perfect square else False.

**Note:Do not** use any built-in library function such as`sqrt`.

**Example 1:**

```
Input: 16
Returns: True
```

**Example 2:**

```
Input: 14
Returns: False
```

在0到这个数字之间二分找平方根。这题主要注意要用long来防止越界。其实还有一种方法，就是判断num/mid是否与mid相等，相等就return；小于mid，就证明答案在比mid大的地方，所以start = mid；大于mid，就证明答案在比mid小的地方，end=mid；例如，【0， 1，2，3，4】，mid=2，4/2=2 return 2.【0，1，2 ，3，4， 5...14，15，16】， mid=8, 16/8 = 2 < 8, 所以往前找，【0，1 ，2，3，4，5，6，7，8】mid = 4，16/4 = 4, return

```java
public boolean isPerfectSquare(int num) {
    if (num < 0) {
        return false;
    }

    long start = 0;
    long end = num;
    while (start + 1 < end) {
        long mid = start + (end - start) / 2;
        long midSquare = mid * mid;
        if (midSquare == num) {
            return true;
        } else if (midSquare < num) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (start * start == num || end * end == num) {
        return true;
    }

    return false;
}
```
