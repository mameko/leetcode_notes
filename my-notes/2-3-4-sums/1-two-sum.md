# 1 Two Sum

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

这题有2种做法，可以用HashMap，T:O(n)，S:O(n);用2 pointer，得先排序，所以T:O(nlogn)，但空间只有那个指针，所以S:O(1)。

```java
public int[] twoSum(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return null;
    }

    HashMap<Integer, Integer> hm = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (hm.containsKey(nums[i])) {
            int[] res = new int[2];
            res[0] = hm.get(nums[i]);
            res[1] = i;
            return res;
        } else {
            int diff = target - nums[i];
            hm.put(diff, i);
        }
    }

    return null;
}

// 又过了3年再写一遍
public int[] twoSum(int[] numbers, int target) {
    int[] result = new int[2];
    result[0] = -1;
    result[1] = -1;

    if (numbers == null || numbers.length < 2) {
        return result;
    }

    // <diff, location>
    Map<Integer, Integer> diffRecords = new HashMap<>();
    for (int i = 0; i < numbers.length; i++) {
        int curNum = numbers[i];
        if (diffRecords.containsKey(curNum)) {
            result[0] = Math.min(diffRecords.get(curNum), i);
            result[1] = Math.max(diffRecords.get(curNum), i);
            return result;
        }

        diffRecords.put(target - curNum, i);
    }

    return result;
}

```
