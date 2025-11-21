# L610 Two Sum VII - Difference equals to target

Given an array of integers, find two numbers that their`difference`equals to a target value.\
where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are NOT zero-based.

## Notice

It's guaranteed there is only one available solution

**Example**

Given nums =`[2, 7, 15, 24]`, target =`5`\
return`[1, 2]`(7 - 2 = 5)

这题得注意difference指的是abs，所以得算两个值。基本架构跟2sum一样，得用hashmap。在算diff的时候得注意是谁减谁。

&#x20;如果不能用hashmap的话，如果是排序的，也可以用通向双指针做。先把target abs一下，保证是正数，然后比较像sliding window那样，先把右边移到差值比target大的，譬如，\[2, 7, 15, 24] target = 8, i指向2，j移动到15，然后左边继续向右移，i指向7，然后就找到了。

```java
public int[] twoSum7(int[] nums, int target) {
    int[] res = new int[2];
    if (nums == null || nums.length == 0) {
        return res;
    }

    // <diff, loc>
    HashMap<Integer, Integer> hm = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        // order for minus is very imprtant, don't target - nums[i]
        // 这里是因为，譬如，2，和-3的diff是5，2和3的diff是1
        // 所以减的时候，要num - target
        int diff1 = nums[i] + target;
        int diff2 = nums[i] - target;
        if (hm.containsKey(nums[i])) {
            res[0] = hm.get(nums[i]) + 1;
            res[1] = i + 1;
        } else {
            hm.put(diff1, i);       
            hm.put(diff2, i);
        }
    }

    return res;
}

// 九章双指针
public int[] twoSum7(int[] nums, int target) {
    if (nums == null || nums.length < 2) {
        return new int[]{-1, -1};
    }
    
    target = Math.abs(target);
    
    int j = 1;
    for (int i = 0; i < nums.length; i++) {
        j = Math.max(j, i + 1); // [0, 1, 2, 2] 防止i=j都指向1，1，因为1，1不是答案
        while (j < nums.length && nums[j] - nums[i] < target) {
            j++;
        }
        
        if (j >= nums.length) {
            break;
        }
        
        if (nums[j] - nums[i] == target) {
            return new int[]{nums[i], nums[j]};
        }
    }
    return new int[]{-1, -1};
}
```
