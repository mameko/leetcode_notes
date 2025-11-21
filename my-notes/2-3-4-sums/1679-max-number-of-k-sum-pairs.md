# 1679 Max Number of K-Sum Pairs

You are given an integer array `nums` and an integer `k`.

In one operation, you can pick two numbers from the array whose sum equals `k` and remove them from the array.

Return _the maximum number of operations you can perform on the array_.

**Example 1:**

```
Input: nums = [1,2,3,4], k = 5
Output: 2
Explanation: Starting with nums = [1,2,3,4]:
- Remove numbers 1 and 4, then nums = [2,3]
- Remove numbers 2 and 3, then nums = []
There are no more pairs that sum up to 5, hence a total of 2 operations.
```

**Example 2:**

```
Input: nums = [3,1,3,4,3], k = 6
Output: 1
Explanation: Starting with nums = [3,1,3,4,3]:
- Remove the first two 3's, then nums = [1,4,3]
There are no more pairs that sum up to 6, hence a total of 1 operation.
```

**Constraints:**

* `1 <= nums.length <= 105`
* `1 <= nums[i] <= 109`
* `1 <= k <= 109`

这题一看就是two sum的变种，可以排序变成167 Two Sum II - input array is sorted。T:(nlogn) S:O(1)。又可以不排序用hashmap变170 Two Sum III - Data structure design，两遍hashmap。看了答案，发现还有一种做法过一遍hashmap的。写一下那个。其实我们只用存现在这个数进hm里。找的时候，看到部署在hm里，就知道这是可以消去的，所以数目减一，count+1，不用再做其他操作了

```java
public int maxOperations(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    int count = 0;
    Map<Integer, Integer> hm = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int cur = nums[i];
        int com = k - cur;
        if (hm.getOrDefault(com, 0) > 0) {
            count++;
            hm.put(com, hm.get(com) - 1);
        } else {
            hm.put(cur, hm.getOrDefault(cur, 0) + 1);
        }
    }
    return count;
}
```
