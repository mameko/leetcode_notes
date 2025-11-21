# 219 Contains Duplicate II

Given an array of integers and an integerk, find out whether there are two distinct indicesiandjin the array such that **nums\[i] = nums\[j]** and the **absolute** difference between i and j is at mostk.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

**Example 3:**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

这题一看就觉得是hashtable了，而且放到hashtable的tag里。然而，这里还暗藏着sliding window。一开想的时候没注意，就出了个naive的答案。因为一直往后看，所以hashmap里存的是《数字， 最近一个i的位置》，如果小于等于的话，会返回true。大于了就更新一下存的下标，因为一直往后不会找到比前面那个小的，所以缩短一点，存新找到的这个。

```java
public boolean containsNearbyDuplicate(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0)  {
        return false;
    }

    // <num, possible loc>
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(nums[i])) {
            if (Math.abs(map.get(nums[i]) - i) <= k) {
                return true;
            }
            // 一直往后找，所以如果大了，就更新一个比较靠近后面的
            if (Math.abs(map.get(nums[i]) - i) > k) {
                map.put(nums[i], i);
            }
        } else {
            map.put(nums[i], i);
        }
    }
    return false;
}
```

然而，这题的正确打开方式是这样子sliding window。

```java
public boolean containsNearbyDuplicate(int[] nums, int k) {
    Set<Integer> set = new HashSet<>();
    for (int i = 0; i < nums.length; ++i) {
        if (set.contains(nums[i])) return true; // 同样找到返回ture
        set.add(nums[i]); // 没找到就加到set里
        if (set.size() > k) { // 最后sliding window
            set.remove(nums[i - k]); // 之留k个数
        }
    }
    return false;
}
```
