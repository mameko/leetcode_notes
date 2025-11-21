# 461 Hamming Distance

The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.

Given two integers`x`and`y`, calculate the Hamming distance.

**Note:**\
0 ≤`x`,`y`< 231.

**Example:**

```
Input: x = 1, y = 4
Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

这题其实就是求以后以后有多少位是1.

```java
public int hammingDistance(int x, int y) {
    int cnt = 0;
    for (int c = x ^ y; c != 0; c = c & (c - 1)) {
        cnt++;
    }

    return cnt;
}
```
