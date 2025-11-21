# 191 Number of 1 bits

Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the[Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

For example, the 32-bit integer ’11' has binary representation`00000000000000000000000000001011`, so the function should return 3.

**Credits:**\
Special thanks to[@ts](https://oj.leetcode.com/discuss/user/ts)for adding this problem and creating all test cases.

```java
// you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int cnt = 0;

        for (int i = n; i != 0; i = i & (i - 1)) {
            cnt++;
        }

        return cnt;
    }
```
