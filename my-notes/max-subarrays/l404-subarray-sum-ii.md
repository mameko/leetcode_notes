# L404 Subarray Sum II

Given an integer array, find a subarray where the sum of numbers is in a given interval. Your code should return the number of possible answers. (The element in the array should be positive)

**Example**

Given`[1,2,3,4]`and interval =`[1,3]`, return`4`. The possible answers are:

```
[0, 0]
[0, 1]
[1, 1]
[2, 2]
```

首先还是preprocess前缀和，然后题目要求的是start <= sum\[j] - sum\[i - 1] <= end。我们首先把式子转化为只跟一个值（j）有关的：sum\[j] - end <= sum\[i - 1] <= sum\[j] - start。暴力的方法是，枚举i然后枚举这个j。找到符合条件的j时，count++。这样的话，复杂度会是O(n^2)。这里，因为题目说了数组内的都是正数，所以前缀和会是一个递增数列，我们可以改用2分地找两个位置，然后像2sum II那样减一下得到count的值，每次加可以加一段，而不是一个一个地加。可以这样理解： 譬如我们要找一个递增数组里在某一区间内的数，在\[0, 1, 4, 5 , 6, 7, 8]里找满足【3，7】数的个数，我们可以2分地找比8小的数，和比3小的数的个数，然后一减就能得到中间隔了几个数。count(7) - count(3) => count(R + 1) - count(L)。因为这里7是闭区间，所以要+1（8）。如果变成是开区间【3，7）的话，就变成count(R) - count(L)，如果左边又开了，我们就变成count(R) - count(L + 1)

```java
public int subarraySumII(int[] A, int start, int end) {
    if (A == null || A.length == 0) {
        return 0;
    }

    int n = A.length;
    // calculate prefix sum, becasue the array contains all positive numbers, 
    // so the prefix sum array will be in increasing order.
    int[] prefixSum = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        prefixSum[i] = prefixSum[i - 1] + A[i - 1];
    }

    // do binary search in range sum
    int count = 0;
    for (int i = 1; i <= n; i++) {
        // use this instead of a loop to find the proper j in the range [0 ~ (i - 1)]
        // will lower the T from O(n^2) to O(nlogn)
        int right = find(prefixSum, i - 1, prefixSum[i] - start + 1);
        int left = find(prefixSum, i - 1, prefixSum[i] - end);
        count += right - left;
    }

    return count;
}

// this function is use to do binary search for value in prefix sum array,
// which smaller than 'value' ends at index 'last'
private int find(int[] prefixS, int last, int value) {
    if (prefixS[0] >= value) {
        return 0;
    }

    if (prefixS[last] < value) {
        return last + 1;
    }

    int l = 0;
    int r = last;
    while (l + 1 < r) {
        int mid = l + (r - l) / 2;
        if (prefixS[mid] < value) {
            l = mid;
        } else {
            r = mid;
        }
    }

    if (prefixS[r] < value) {
        return r + 1;
    } else {
        return l + 1;
    }
}
```
