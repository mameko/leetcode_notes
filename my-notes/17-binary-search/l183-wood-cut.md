# L183 Wood Cut

Given n pieces of wood with length`L[i]`(integer array). Cut them into small pieces to guarantee you could have equal or more than k pieces with the same length. What is the longest length you can get from the n pieces of wood? Given L & k, return the maximum length of the small pieces.

## Notice

You couldn't cut wood into float length.

If you couldn't get >=\_k\_pieces, return`0`.

**Example**

For`L=[232, 124, 456]`,`k=7`, return`114`.

[**Challenge**](http://www.lintcode.com/en/problem/wood-cut/#challenge)

O(n log Len), where Len is the longest length of the wood.

```java
public int woodCut(int[] L, int k) {
    if (L == null || L.length == 0 || k <= 0) {
        return 0;
    }

    int maxLen = -1;
    for (int i = 0; i < L.length; i++) {
        maxLen = Math.max(maxLen, L[i]);
    }

    int start = 0;
    int end = maxLen;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (woodAmt(L, mid) >= k) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (start < end) {
        return start;
    } else {
        return end;
    }
}

private int woodAmt(int[] L, int eachLen) {
    int cnt = 0;
    for (int i = 0; i < L.length; i++) {
        cnt += L[i] / eachLen;
    }
    return cnt;
}
```
