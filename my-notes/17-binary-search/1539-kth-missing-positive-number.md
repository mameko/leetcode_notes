# 1539 Kth Missing Positive Number

Given an array `arr` of positive integers sorted in a **strictly increasing order**, and an integer `k`.

_Find the_ `kth` _positive integer that is missing from this array._

**Example 1:**

```
Input: arr = [2,3,4,7,11], k = 5
Output: 9
Explanation: The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.
```

**Example 2:**

```
Input: arr = [1,2,3,4], k = 2
Output: 6
Explanation: The missing positive integers are [5,6,7,...]. The 2nd missing positive integer is 6.
```

**Constraints:**

* `1 <= arr.length <= 1000`
* `1 <= arr[i] <= 1000`
* `1 <= k <= 1000`
* `arr[i] < arr[j]` for `1 <= i < j <= arr.length`

这题，一开始看的时候，脑抽，以为是求区间的，而且以为区间的最大值是1000.debug了半天off by one issue。最后终于写对了一个O(n)的。但其实因为是排序的，我们可以通过arr\[i] - i -1知道这个数字前面有多少个missing的数字。然后用k来二分查找就ok了，就O(nlogn)了。但！九章二分套了半天还off by one。

解法一：二分

```java
public int findKthPositive(int[] arr, int k) {
    int left = 0, right = arr.length - 1;
    while (left <= right) {
        int pivot = left + (right - left) / 2;
        // If number of positive integers
        // which are missing before arr[pivot]
        // is less than k -->
        // continue to search on the right.
        if (arr[pivot] - pivot - 1 < k) {
            left = pivot + 1;
        // Otherwise, go left.
        } else {
            right = pivot - 1;
        }
    }

    // At the end of the loop, left = right + 1,
    // and the kth missing is in-between arr[right] and arr[left].
    // The number of integers missing before arr[right] is
    // arr[right] - right - 1 -->
    // the number to return is
    // arr[right] + k - (arr[right] - right - 1) = k + left
    return left + k;
}
```

解法二：一遍loop一遍求

```java
    public int findKthPositive(int[] arr, int k) {
        // if the kth missing is less than arr[0]
        if (k <= arr[0] - 1) {
            return k;
        }
        k -= arr[0] - 1;

        // search kth missing between the array numbers
        int n = arr.length;
        for (int i = 0; i < n - 1; ++i) {
            // missing between arr[i] and arr[i + 1]
            int currMissing = arr[i + 1] - arr[i] - 1;
            // if the kth missing is between
            // arr[i] and arr[i + 1] -> return it
            if (k <= currMissing) {
                return arr[i] + k;
            }
            // otherwise, proceed further
            k -= currMissing;
        }

        // if the missing number if greater than arr[n - 1]
        return arr[n - 1] + k;
    }
```

解法三：算区间

```java
 public int findKthPositive(int[] arr, int k) {
    if (arr == null || arr.length == 0 || k < 0) {
        return -1;
    }

    int n = arr.length;
    List<Pair<Integer, Integer>> locationToMissingCnt = new ArrayList<>();

    int start = arr[0] - 0;
    if (start > 1) {
        Pair<Integer, Integer> pair = new Pair<>(-1, start - 1);
        locationToMissingCnt.add(pair);
    }

    for (int i = 1; i < n; i++) {
        int diff = arr[i] - arr[i - 1];
        if (diff > 1) {
            Pair<Integer, Integer> pair = new Pair<>(i - 1, diff - 1);
            locationToMissingCnt.add(pair);
        }
    }

    Pair<Integer, Integer> pair = new Pair<>(n, Integer.MAX_VALUE);
    locationToMissingCnt.add(pair);

    int startPair = 0;
    for (Pair<Integer, Integer> p : locationToMissingCnt) {
        if (k - p.getValue() <= 0) {
            break;
        }
        k = k - p.getValue();
        startPair++;
    }

    Pair<Integer, Integer> targetRange = locationToMissingCnt.get(startPair);
    int locInArr = targetRange.getKey();

    int numCur = 0;
    if (locInArr == -1) {
        numCur = 0;
    } else if (locInArr == n) {
        numCur = arr[n - 1];
    } else {
        numCur = arr[locInArr];
    }

    while (k > 0) {
        numCur++;
        k--;
    }

    return numCur;
}
```
