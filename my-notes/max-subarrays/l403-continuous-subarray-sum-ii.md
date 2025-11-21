# L403 Continuous Subarray Sum II

Given an circular integer array (the next element of the last element is the first element), find a continuous subarray in it, where the sum of numbers is the biggest. Your code should return the index of the first number and the index of the last number.

If duplicate answers exist, return any of them.

**Example**

Give`[3, 1, -100, -3, 4]`, return`[4,1]`.

首先，我们正常情况下，结果在中间段时，用[L402](l402-continuous-subarray-sum.md)求max。当要求的结果跨越了数组边界的时候，我们怎么求呢？我们要转换一下思维，因为求的是最大，所以我们要把最小的sum从数组中间挖掉（就是L402但求min）。然后我们再转换一次，怎么把求max的函数重复利用来求min呢？我们只要把数乘以-1，反一下，就能利用求max的函数了。最后在两者中取最大的就ok了。注意这题第一个想法，把数组repeat变成2n来做是不行的，因为子数组有可能重复使用某些数字，所以不行？（没太懂）

```java
public ArrayList<Integer> continuousSubarraySumII(int[] A) {
    ArrayList<Integer> res = new ArrayList<>();
    if (A == null || A.length == 0) {
        return res;
    }

    int n = A.length;
    int total = 0; 
    for (int i = 0; i < n; i++) {
        total += A[i];
    }

    int max = findMaxMinRange(A, res, true);// true means find max segment
    ArrayList<Integer> res2 = new ArrayList<>();
    int min = findMaxMinRange(A, res2, false);// false means find min segment, but the min doesn't have minus in front

    int diff = total + min; // total - (-min)
    if (diff > max && diff != 0) {// the whole array is minus when diff == 0, so just return res1, which contains the min of all the minus number in the array
        int start = (res2.get(1) + 1) % n;
        int end = (res2.get(0) - 1) % n;
        res.set(0, start);
        res.set(1, end);
    }

    return res;
}

private int findMaxMinRange(int[] A, ArrayList<Integer> res, boolean getMax) {
    int max = Integer.MIN_VALUE;

    if (!getMax) {
        for (int i = 0; i < A.length; i++) {
            A[i] = A[i] * -1;
        }
    }

    int s = 0;
    int e = 0;
    res.add(s);
        res.add(e);
    int preSum = 0;
    for (int i = 0; i < A.length; i++) {
        if (preSum < 0) {
            preSum = A[i];
            s = i;
            e = i;
        } else {
            preSum += A[i];
            e = i;
        }

        if (preSum > max) {
            max = preSum;
            res.set(0, s);
            res.set(1, e);
        }

    }

    return max;
}
```
