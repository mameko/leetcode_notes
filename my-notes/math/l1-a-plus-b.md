# L1 A plus B

Write a function that add two numbers A and B. You should not use`+`or any arithmetic operators.

## Notice

There is no need to read data from standard input stream. Both parameters are given in function`aplusb`, you job is to calculate the sum and return it.

**Clarification**

Are a and b both`32-bit`integers?

* Yes.

Can I use bit operation?

* Sure you can.

**Example**

Given`a=1`and`b=2`return`3`

[**Challenge**](http://www.lintcode.com/en/problem/a-b-problem/#challenge)

Of course you can just return a + b to get accepted. But Can you challenge not do it like that?

```java
public int aplusb(int a, int b) {
    if (a == 0 && b == 0) {
        return 0;
    } else if (a == 0 && b != 0) {
        return b;
    } else if (a != 0 && b == 0) {
        return a;
    }

    while (b != 0) {
        int sum = a ^ b;
        int carry = (a & b) << 1;
        a = sum;
        b = carry;
    }

    return a;
}
```
