# 532 K-diff Pairs in an Array

Given an array of integers and an integer **k**, you need to find the number of **unique** k-diff pairs in the array. Here a **k-diff** pair is defined as an integer pair (i, j), where**i**and**j**are both numbers in the array and their [absolute difference](https://en.wikipedia.org/wiki/Absolute_difference) is **k**.

**Example 1:**

```
Input: [3, 1, 4, 1, 5], k = 2
Output: 2

Explanation: There are two 2-diff pairs in the array, (1, 3) and (3, 5).
Although we have two 1s in the input, we should only return the number of unique pairs.
```

**Example 2:**

```
Input:[1, 2, 3, 4, 5], k = 1
Output: 4

Explanation: There are four 1-diff pairs in the array, (1, 2), (2, 3), (3, 4) and (4, 5).
```

**Example 3:**

```
Input: [1, 3, 1, 5, 4], k = 0
Output: 1

Explanation: There is one 0-diff pair in the array, (1, 1).
```

**Note:**

1. The pairs (i, j) and (j, i) count as the same pair.
2. The length of the array won't exceed 10,000.
3. All the integers in the given input belong to the range: \[-1e7, 1e7].

这题可以用hashmap来做到O(n)时间复杂度。这题是看了discuss里大神的做法写的。首先我们统计每一个数出现的次数。然后，我们loop through这个map，边loop边找diff。这里diff是k，如果k = 0的情况，其实是找两个相同的数，我们只要看到entry里有两个或以上的数时就把count+1.（因为不算重复的）。另一种情况是找diff = k的，如果在hashmap里找到key + k的话，证明我们找到一对了。所以count++。不用算key - k因为不算重复的。

```java
public int findPairs(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0) {
        return 0;
    }

    // <num, frequency>
    HashMap<Integer, Integer> hm = new HashMap<>();

    for (int num : nums) {
        if (hm.containsKey(num)) {
            hm.put(num, hm.get(num) + 1);
        } else {
            hm.put(num, 1);
        }
    }

    int cnt = 0;
    for (Map.Entry<Integer, Integer> entry : hm.entrySet()) {
        if (k == 0) {
            if (entry.getValue() > 1) {
                cnt++;
            }
        } else {
            if (hm.containsKey(entry.getKey() + k)) {
                cnt++;
            }
        }
    }

    return cnt;
}
```
