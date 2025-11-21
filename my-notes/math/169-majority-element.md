# 169 Majority Element

Given an array of sizen, find the majority element. The majority element is the element that appears**more than**`⌊ n/2 ⌋`times.

You may assume that the array is non-empty and the majority element always exist in the array.

因为保证有解，所以最后check的那一步在这题不需要，不过在L46就要。当然这题可以用hashmap数频率要O（n）space，或者可以先排序nlogn再过一遍来数数。又或者不排序，过两遍n方。

```java
public int majorityElement(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    int cnt = 1;
    int majorityNum = nums[0];
    for (int i = 1; i < nums.length; i++) {
        if (cnt == 0) {
            cnt++;
            majorityNum = nums[i];
        } else if (majorityNum == nums[i]) {
            cnt++;
        } else {
            cnt--;
        }
    }


    int n = 0;
    for (Integer elem : nums) {
        if (elem == majorityNum) {
            n++;
        }
    }

    if (n > nums.length / 2) {
        return majorityNum;
    } else {
        return -1;
    }
}
```
