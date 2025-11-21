# 26 Remove Duplicate from Sorted Array

Given a sorted array, remove the duplicates in place such that each element appear onlyonceand return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,\
Given input arraynums=`[1,1,2]`,

Your function should return length =`2`, with the first two elements of nums being`1`and`2`respectively. It doesn't matter what you leave beyond the new length.

跟上一题差不多，这里还是用i来loop through array。然后，用另外一个指针size来做结果数组的下标。每次我们比较size所指的数字和i所指的数字是否相同。如果相同的话，我们让continue，让i指针往后移跳过重复元素。然后当两个数字不一样的时候，我们要先把size指针往后移，再赋值（把没重复的元素赋给它）。最后返回的时候记得加一，因为size一直指着返回数组的最后一个元素，返回长度所以得+1.

```java
public int removeDuplicates(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int size = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[size] == nums[i]) {
            continue;
        } else {
            nums[++size] = nums[i];
        }
    }

    return size + 1;
}

// 再写
public int removeDuplicates(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int size = 0;        
    for (int i = 1; i < nums.length; i++) {
        if (nums[size] != nums[i]) {
            nums[++size] = nums[i];                
        } 
    }

    return size + 1;
}
```
