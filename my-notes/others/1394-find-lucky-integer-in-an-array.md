# 1394 Find Lucky Integer in an Array

Given an array of integers `arr`, a lucky integer is an integer which has a frequency in the array equal to its value.

Return a lucky integer in the array. If there are multiple lucky integers return the **largest** of them. If there is no lucky integer return **-1**.

**Example 1:**

```
Input: arr = [2,2,3,4]
Output: 2
Explanation: The only lucky number in the array is 2 because frequency[2] == 2.
```

**Example 2:**

```
Input: arr = [1,2,2,3,3,3]
Output: 3
Explanation: 1, 2 and 3 are all lucky numbers, return the largest of them.
```

**Example 3:**

```
Input: arr = [2,2,2,3,3]
Output: -1
Explanation: There are no lucky numbers in the array.
```

**Example 4:**

```
Input: arr = [5]
Output: -1
```

**Example 5:**

```
Input: arr = [7,7,7,7,7,7,7]
Output: 7
```

**Constraints:**

* `1 <= arr.length <= 500`
* `1 <= arr[i] <= 500`

这题一看，不久是用map来数数吗？后来也想了一下sort的解法。sort了之后，从屁股开始找，第一个等于下标的就是max了。T:O(nlogn), S:O(1)。用hashmap的话，就是先过一遍数频率，再过一遍找max。T:O(n), S:O(n)。

```java
public int findLucky(int[] arr) {
    if (arr == null || arr.length == 0) {
        return -1;
    }

    Map<Integer, Integer> numToCnt = new HashMap<>();
    for (int num : arr) {
        numToCnt.put(num, numToCnt.getOrDefault(num, 0) + 1);
    }

    int maxLucky = -1;
    for (Map.Entry<Integer, Integer> entry : numToCnt.entrySet()) {
        if (entry.getKey() == entry.getValue()) {
            maxLucky = Math.max(maxLucky, entry.getKey());
        }
    }

    return maxLucky;
}

// 补一个solution里的sort解法
public int findLucky(int[] arr) {
    Arrays.sort(arr);
    int currentStreak = 0;
    // In Java, it's best to just go backwards, as we can't
    // trivially reverse-sort an Array of primitives. 
    // We could also have used the Stream API to box the ints and then
    // sort using a library comparator.
    for (int i = arr.length - 1; i >= 0; i--) {
        currentStreak++;
        // If this is the last element in the current streak (as the next is
        // different, or we're at the start of the array).
        if (i == 0 || arr[i] != arr[i - 1]) {
            // If this is a lucky number
            if (currentStreak == arr[i]) {
                return currentStreak;
            }
            currentStreak = 0;
        }
    }
    return -1;
}
```
