# L397 Longest Increasing Continuous Subsequence

Give an integer array，find the longest increasing continuous subsequence in this array.

An increasing continuous subsequence:

* Can be from right to left or from left to right.
* Indices of the integers in the subsequence should be continuous.

## Notice

O(n) time and O(1) extra space.

**Example**

For`[5, 4, 2, 1, 3]`, the LICS is`[5, 4, 2, 1]`, return`4`.

For`[5, 1, 2, 3, 4]`, the LICS is`[1, 2, 3, 4]`, return`4`.

这题只要用两个变量把连续增长/下降的长度记录下来就行了。一边遍历一遍更新长度，每当数字的走向改变，就进入另外一个branch。然后把变量重置。如果数字走向没有变化的话，变量就会一直变长。

```java
public int longestIncreasingContinuousSubsequence(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }

    int max = 1;
    int curInc = 1;
    int curDec = 1;
    for (int i = 1; i < A.length; i++) {
        if (A[i] > A[i - 1]) {
            curDec = 1;
            curInc++;
            max = Math.max(curInc, max);
        } else {
            curInc = 1;
            curDec++;
            max = Math.max(curDec, max);
        }
    }

    return max;
}
```
