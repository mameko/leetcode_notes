# 1887 Reduction Operations to Make the Array Elements Equal

Given an integer array `nums`, your goal is to make all elements in `nums` equal. To complete one operation, follow these steps:

1. Find the **largest** value in `nums`. Let its index be `i` (**0-indexed**) and its value be `largest`. If there are multiple elements with the largest value, pick the smallest `i`.
2. Find the **next largest** value in `nums` **strictly smaller** than `largest`. Let its value be `nextLargest`.
3. Reduce `nums[i]` to `nextLargest`.

Return _the number of operations to make all elements in_ `nums` _equal_.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [5,1,3]
</strong><strong>Output: 3
</strong><strong>Explanation: It takes 3 operations to make all elements in nums equal:
</strong>1. largest = 5 at index 0. nextLargest = 3. Reduce nums[0] to 3. nums = [3,1,3].
2. largest = 3 at index 0. nextLargest = 1. Reduce nums[0] to 1. nums = [1,1,3].
3. largest = 3 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1].
</code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [1,1,1]
</strong><strong>Output: 0
</strong><strong>Explanation: All elements in nums are already equal.
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: nums = [1,1,2,2,3]
</strong><strong>Output: 4
</strong><strong>Explanation: It takes 4 operations to make all elements in nums equal:
</strong>1. largest = 3 at index 4. nextLargest = 2. Reduce nums[4] to 2. nums = [1,1,2,2,2].
2. largest = 2 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1,2,2].
3. largest = 2 at index 3. nextLargest = 1. Reduce nums[3] to 1. nums = [1,1,1,1,2].
4. largest = 2 at index 4. nextLargest = 1. Reduce nums[4] to 1. nums = [1,1,1,1,1].
</code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 5 * 10^4`
* `1 <= nums[i] <= 5 * 10^4`

一看题，就想，这不就是数数,然后unique的key排序，然后从后面开始加到前面吗？谁知，写完过不了，寻思着，因为数据不大，既然nlogn不行，那么就来一个counting sort。然后过了。最后，看了答案，艹，大神虽然也是sort，但一下子就ok了。因为每次都得把大于等于max的所有加上，大神直接用size - 改变了的地方，一下算出了，而且每次都能把大于等于max的加上，不用像自己做法那样”乘以“该加上的次数。

```java
// 大佬的解法
public int reductionOperations(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    Arrays.sort(nums);
    int total = 0;
    int size = nums.length;
    int lastInd = size - 1;
    int last = nums[size - 1];
    for (int i = lastInd; i >= 0; i--) {
        if (nums[i] == last) {
            continue;
        }

        last = nums[i];
        total += lastInd - i; // 直接算后面该加的，所以不用像自己解法那样要”乘以"
    }
    return total;
}

// 我的解法
public int reductionOperations(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    // <num, cnt>
    int maxNum = 0;
    Map<Integer, Integer> countMap = new HashMap<>();
    for (int num : nums) {
        countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        maxNum = Math.max(maxNum, num);
    }

    int total = 0;
    // List<Integer> sortedKeys = new LinkedList<>(countMap.keySet());
    // Collections.sort(sortedKeys); // TEL at 73/78       
    // for (int i = sortedKeys.size() - 1; i > 0; i--) {
    //     int count = countMap.get(sortedKeys.get(i));
            // 因为大于等于的要加多次，所以用个cursor来记录
    //     total = total + count * i;
    // }

    int[] countingSort = new int[maxNum + 1];
    for (int num : countMap.keySet()) {
        countingSort[num] = countMap.get(num);
    }

    int sizeOfUniqueNums = countMap.size();
    // 因为大于等于的要加多次，所以用个cursor来记录
    int multiplier = sizeOfUniqueNums - 1; 
    for (int i = maxNum; i > 0; i--) {
        if (countingSort[i] == 0) {
            continue;
        }

        total = total + countingSort[i] * multiplier;
        multiplier--;
    }

    return total;
}
```
