# L402 Continuous Subarray Sum

Given an integer array, find a continuous subarray where the sum of numbers is the biggest. Your code should return the index of the first number and the index of the last number. (If their are duplicate answer, return anyone)

**Example**

Give`[-3, 1, 3, -3, 4]`, return`[1,4]`.

这题跟[L41](l41-maximum-subarray.md)很像，只是这里求的是下标。感觉还是挺难控制的。

```java
public ArrayList<Integer> continuousSubarraySum(int[] A) {
    ArrayList<Integer> res = new ArrayList<>();
    if (A == null || A.length == 0) {
        return res;
    }

    int prefixSum = 0;
    int max = Integer.MIN_VALUE;
    res.add(0);
    res.add(0);
    int start = 0;
    int end = 0;
    for (int i = 0; i < A.length; i++) {
        if (prefixSum < 0) {
            prefixSum = A[i];
            start = i;
            end = i;
        } else {
            prefixSum += A[i];
            end = i;
        }

        if (prefixSum > max) {
            max = prefixSum;
            res.set(0, start);
            res.set(1, end);
        }
    }

    return res;
}
```
