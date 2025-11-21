# 220 Contains Duplicate III

Given an array of integers, find out whether there are two distinct indicesiandjin the array such that the **absolute** difference between **nums\[i]** and **nums\[j]** is at mosttand the **absolute** difference between i and j is at most k.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

**Example 3:**

```
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

这题想了半天怎么sliding window，sliding了半天没想出来。然后看了解释才知道这题的做法是这样的。

Brute Force：T: O(nk), S: O(1), 这个做法就是我们loop那个数组，然后我们向前看k个，逐个检查是否符合，如果发现有就返回。外层loop有n个，每枚举一个数，我们linear地search它前面的k个数。

```java
public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    if (nums == null || nums.length == 0 || k < 0 || t < 0) {
        return false;
    }

    for (int i = 0; i < nums.length; i++) {
        for (int j = Math.max(i - k, 0); j < i; j++) {
            if (Math.abs((long)nums[i] - (long)nums[j]) <= t) { // 大数会越界
                return true;
            }
        }
    }

    return false;
}
```

第二个方法是用TreeSet。利用数据结构，花点空间可以把linear search变成一个logk的search。因为BST找upper/lower bound只用logk的时间。T: O(nlogk), S: O(k)

```java
public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    if (nums == null || nums.length == 0 || k < 0 || t < 0) {
        return false;
    }

    TreeSet<Integer> ts = new TreeSet<>();       
    for (int i = 0; i < nums.length; i++) {
        // 找ts里找最小的，大于等于这个数的数；连最小的也不在范围内，其他大的就更不会在范围内了
        Integer min = ts.ceiling(nums[i]);
        if (min != null && (long)min - (long)nums[i] <= t) {
            return true;
        }
        // 找ts里找最大的，小于等于这个数的数；连最大的也不在范围内，其他大的就更不会在范围内了
        Integer max = ts.floor(nums[i]);
        if (max != null && (long)nums[i] - (long)max <= t) {
            return true;
        }
        // 每次加进TreeSet里
        ts.add(nums[i]);   
        // 如果超出范围就把window外的去掉         
        if (ts.size() > k) {
            ts.remove(nums[i - k]);
        }
    }

    return false;
}
```

第三种方法是用桶排的思想来做，更加丧心病狂。T: O(n), S: O(k)。这里通过桶的划分方法，把每次搜的范围确定为3.\[0,t],\[t+1,2t+1],...现在数所在的桶，比它小一隔的桶&比它大一隔的桶。因为过两隔的桶就会超出t这个范围，所以根本不用看。因为window是k，所以我们总共要存k个数分别在哪些桶里。

```java
public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    if (nums == null || nums.length == 0 || k < 0 || t < 0) {
        return false;
    }

    Map<Long, Long> buckets = new HashMap<>();
    long size = (long)t + 1;
    for (int i = 0; i < nums.length; i++) {
        long bNum = getBucketNum(nums[i], size);
        // 看现在的bucket有没有
        if (buckets.containsKey(bNum)) {
            return true;
        }
        // 看小一隔的有没有
        if (buckets.containsKey(bNum - 1) && Math.abs(nums[i] - buckets.get(bNum - 1)) < size) {
            return true;
        }
        // 看大一隔的有没有
        if (buckets.containsKey(bNum + 1) && Math.abs(nums[i] - buckets.get(bNum + 1)) < size) {
            return true;
        }
        // 最后把这个数放到对应的bucket里
        buckets.put(bNum, (long)nums[i]);
        if (i >= k) { // 过了window，就把旧的删掉
            buckets.remove(getBucketNum(nums[i - k], size));
        }

    }

    return false;
}

// 这个函数用来找bucket位置
// In Java, `-3 / 5 = 0` and but we need `-3 / 5 = -1`.
private long getBucketNum(long num, long size) {
    return num < 0 ? (num + 1) / size - 1 : num / size;
}
```
