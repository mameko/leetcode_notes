# 1228 Missing Number In Arithmetic Progression

In some array `arr`, the values were in arithmetic progression: the values `arr[i+1] - arr[i]` are all equal for every `0 <= i < arr.length - 1`.

Then, a value from `arr` was removed that **was not the first or last value in the array**.

Return the removed value.

**Example 1:**

```
Input: arr = [5,7,11,13]
Output: 9
Explanation: The previous array was [5,7,9,11,13].
```

**Example 2:**

```
Input: arr = [15,13,12]
Output: 14
Explanation: The previous array was [15,14,13,12].
```

**Constraints:**

* `3 <= arr.length <= 1000`
* `0 <= arr[i] <= 10^5`

这题一看，就怀疑是不是能用数学方法做。然后把(first + last)\*n/2的公式一套，发现就能求出全部数字应该有的总和，最后返回总和减去所有数字的sum即可。T:O(n), S:O(1)。然后看了答案，原来还有二分的做法。因为我们知道是等差，first和last。那么我们就知道arr里每个数字的值和相对的位置。那么我们就可以二分地找哪一个是第一个没对齐的。T:O(logn), S:O(1)

```java
public int missingNumber(int[] arr) {
    if (arr == null || arr.length < 2) {
        return -1;
    }

    int n = arr.length; 
    // n + 1, is because original arr is missing one num
    int sumShouldBe = (arr[0] + arr[n - 1]) * (n + 1) / 2;
    System.out.println(sumShouldBe);
    int sumSoFar = 0;
    for (int i = 0; i < n; i++) {
        sumSoFar += arr[i];
    }

    return sumShouldBe - sumSoFar;
}

// 这里补一个九章的二分模板做法
// 跟下面那个solution的是一样的，这里跟模板的不同是最后不用判断start，直接返回end的就行了
// somehow，end指着的那个位置就是mising的那个位置。而且if条件的地方只有大于或者等于，没有小于的情况
public int missingNumber(int[] arr) {
    if (arr == null || arr.length == 0) {
        return -1;
    }

    int len = arr.length;
    int start = 0;
    int end = len - 1;
    int diff = (arr[end] - arr[start]) / len;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (arr[0] + diff * mid == arr[mid]) {
                start = mid;
            } else {
                // only bigger than, not going to be smaller than
                end = mid;
            } 
        }

        return arr[0] + diff * end;
    }

// 二分的就直接抄solution的了
public int missingNumber(int arr[]) {
    int n = arr.length;

    // 1. Get the difference `difference`.
    int difference = (arr[n - 1] - arr[0]) / n;
    int lo = 0;
    int hi = n - 1;

    // Basic binary search template.
    while (lo < hi) {
        int mid = (lo + hi) / 2;

        // All numbers upto `mid` have no missing number, so search on the right side.
        if (arr[mid] == arr[0] + mid * difference) {
            lo = mid + 1;
        }

        // A number is missing before `mid` inclusive of `mid` itself.
        else {
            hi = mid;
        }
    }

    // Index `lo` will be the position with the first incorrect number.
    // Return the value that was supposed to be at this index.
    return arr[0] + difference * lo;
}
```

