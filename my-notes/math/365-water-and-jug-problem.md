# 365 Water and Jug Problem

You are given two jugs with capacitiesxandylitres. There is an infinite amount of water supply available. You need to determine whether it is possible to measure exactlyzlitres using these two jugs.

Ifzliters of water is measurable, you must havezliters of water contained within**one or both buckets**by the end.

Operations allowed:

* Fill any of the jugs completely with water.
* Empty any of the jugs.
* Pour water from one jug into another till the other jug is completely full or the first jug itself is empty.

**Example 1:**(From the famous["Die Hard"example](https://www.youtube.com/watch?v=BVtQNK_ZUJg))

```
Input: x = 3, y = 5, z = 4
Output: True
```

**Example 2:**

```
Input: x = 2, y = 6, z = 5
Output: False
```

这题考的是数学，不看答案完全不知道怎么做。这个涉及数论的[裴蜀定理](https://zh.wikipedia.org/wiki/%E8%B2%9D%E7%A5%96%E7%AD%89%E5%BC%8F)。简单来说，我们要求这个公式是否有解：xm + yn = z。这个公式有解的条件是z是gcd（x，y）的倍数。另外这里因为最后水要装在桶里，所以要加判断条件x+y>z。另外还要注意的就是0的情况。

```java
public boolean canMeasureWater(int x, int y, int z) {
    if (x + y < z) {
        return false;
    } 

    if (x == z || y == z || x + y == z) {
        return true;
    }

    int gcd = gcd(x, y);
    return z % gcd == 0;
}

private int gcd(int a, int b) {
    while (b != 0) {
        int tmp = b;
        b = a % b;
        a = tmp;
    }

    return a;
}
```
