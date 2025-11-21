# 1133 Largest Unique Number

Given an array of integers `A`, return the largest integer that only occurs once.

If no integer occurs once, return -1.

**Example 1:**

```
Input: [5,7,3,9,4,9,8,3,1]
Output: 8
Explanation: 
The maximum integer in the array is 9 but it is repeated. The number 8 occurs only once, so it's the answer.
```

**Example 2:**

```
Input: [9,9,8,8]
Output: -1
Explanation: 
There is no number that occurs only once.
```

**Note:**

1. `1 <= A.length <= 2000`
2. `0 <= A[i] <= 1000`

一开始看题，还以为是用set，因为说要找unique。然后发现，如果set的话，不能把出现了1次的找出来。所以还是乖乖地用了map来数频率。T:O(n), S:O(n)。后来看了solution还有一种解法是sorting从大到小排。把数组sort了，然后，从大往小找，找到相邻的没重复的，就是答案了。因为sorting所以T:O(nlogn)，S: O(1)

```java
public int largestUniqueNumber(int[] A) {
    if (A == null || A.length == 0) {
        return -1;
    }

    // <num, freq>
    Map<Integer, Integer> numToCnt = new HashMap<>();
    for (int num : A) {
        numToCnt.put(num, numToCnt.getOrDefault(num, 0) + 1);
    }

    int max = -1;
    for (Map.Entry<Integer, Integer> entry : numToCnt.entrySet()) {
        if (entry.getValue() < 2 && entry.getKey() > max) {
            max = Math.max(entry.getKey(), max);
        }
    }

    return max;
}

// solution sorting 解法，贴上来参考
public int largestUniqueNumber(int[] A) {
    // Sort in ascending order.
    Arrays.sort(A);

    for (int i = A.length - 1; i >= 0; i--) {
        // If there is no duplicate return.
        if (i == 0 || A[i] != A[i - 1]) return A[i];

        // While duplicates exist.
        while (i > 0 && A[i] == A[i - 1]) {
           i--;
        }
    }
    return -1;
}
```
