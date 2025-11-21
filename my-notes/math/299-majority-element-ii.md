# 229 Majority Element II

Given an integer array of sizen, find all elements that appear more than`⌊ n/3 ⌋`times. The algorithm should run in linear time and in O(1) space.

**Hint:**

1. How many majority elements could it possibly have?

自己写得有点乱，所以先把discuss大神的贴上来

```java
public List<Integer> majorityElement(int[] nums) {
    List<Integer> result = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return result;
    }

    int count1 = 0;
    int count2 = 0;
    int target1 = -1;
    int target2 = -1;

    for (int i = 0; i < nums.length; i++) {
        if (target1 == nums[i]) {
            count1++;
        } else if (target2 == nums[i]){
            count2++;
        } else if (count1 == 0) {
            count1++;
            target1 = nums[i];
        } else if (count2 == 0) {
            count2++;
            target2 = nums[i];
        } else {
            count1--;
            count2--;
        }
    }

    int n1 = 0;
    int n2 = 0;
    for (Integer elem : nums) {
        if (elem == target1) {
            n1++;
        } else if (elem == target2) {
            n2++;
        }
    }

    if (n1 > nums.length / 3) {
        result.add(target1);
    } 

    if (n2 > nums.length / 3) {
        result.add(target2);
    } 

    return result;
}
```

自己写的：

```java
public List<Integer> majorityElement(int[] nums) {
    List<Integer> result = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return result;
    }

    int count1 = 0;
    int count2 = 0;
    int target1 = -1;
    int target2 = -1;

    for (int i = 0; i < nums.length; i++) {
        if (count1 == 0 && target2 != nums[i]) {
            count1++;
            target1 = nums[i];
        } else if (count2 == 0 && target1 != nums[i]) {
            count2++;
            target2 = nums[i];
        } else {
            if (target1 == nums[i]) {
                count1++;
            } else if (target2 == nums[i]){
                count2++;
            } else {
                count1--;
                count2--;
            }
        }
    }

    int n1 = 0;
    int n2 = 0;
    for (Integer elem : nums) {
        if (elem == target1) {
            n1++;
        } else if (elem == target2) {
            n2++;
        }
    }

    if (n1 > nums.length / 3) {
        result.add(target1);
    } 

    if (n2 > nums.length / 3) {
        result.add(target2);
    } 

    return result;
}
```
