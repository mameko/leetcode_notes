# 258 Add Digits

Given a non-negative integer`num`, repeatedly add all its digits until the result has only one digit.

For example:

Given`num = 38`, the process is like:`3 + 8 = 11`,`1 + 1 = 2`. Since`2`has only one digit, return it.

**Follow up:**\
Could you do it without any loop/recursion in O(1) runtime?

就是把每一位抽出来，加一加再赋值。第一个算法的时间复杂度是多少感觉有点难算，感觉就是看数字什么时候收敛成一位数。这题还有数学公式解法，能达到O(1)。

```java
public int addDigits(int num) {
    if (num < 1) {
        return 0;
    }

    while (num > 9) {
        int sum = 0;
        while (num > 0) {
            sum += num % 10;
            num = num / 10;
        }

        num = sum;
    }

    return num;
}
```

```java
public int addDigits(int num) {
    if (num < 1) {
        return 0;
    }

    return (num - 1) % 9 + 1;
}
```
