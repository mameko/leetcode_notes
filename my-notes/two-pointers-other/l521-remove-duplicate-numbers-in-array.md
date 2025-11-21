# L521 Remove Duplicate Numbers in Array

Description

Given an array of integers `nums`, logically remove the duplicate elements and return the length of the removed array `n` such that the first `n` elements of the array `nums` contain all the elements of the original array `nums` after de-duplication by the de-duplication operation.

You should:

1. Do it in place in the array.
2. Put the element after removing the repetition at the beginning of the array.
3. Return the number of elements after removing duplicate elements.

***

Wechat reply 【Google】 get the latest requent Interview questions. (wechat id : **jiuzhang1104**)

You don't need to keep the original order of the integers.

Example

Example 1:

```
Input:nums = [1,3,1,4,4,2]Output:[1,3,4,2,?,?]4
```

Explanation:

1. Move duplicate integers to the tail of _nums_ => _nums_ = `[1,3,4,2,?,?]`.
2. Return the number of unique integers in _nums_ => `4`.

Actually we don't care about what you place in `?`, we only care about the part which has no duplicate integers.

Example 2:

```
Input:nums = [1,2,3]Output:[1,2,3]3
```

Challenge

1. Do it in O(n) time complexity.
2. Do it in O(nlogn) time without extra space.

这题如果允许排序的话跟之前[26 Remove Duplicate from Sorted Arra](26-remove-duplicate-from-sorted-array.md)一样。如果不排序的话，可以用个set来去重，还是T：O(n)但多用了O(n)的space。

```java
public int deduplication(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    Set<Integer> set = new HashSet<>();
    int i = 0;
    int j = 0;
    while (i < nums.length) {            
        j = i;
        
        while (j < nums.length && set.contains(nums[j])) {
            j++;
        } 

        set.add(nums[j]);
        nums[i] = nums[j];  
        i++;             
        if (j == nums.length - 1) {
            break;
        }              
    }

    return i;
}

// 补一个答案里写的比较好看的
public int deduplication(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    HashSet<Integer> set = new HashSet<Integer>();
    int left = 0;
    int right = 0;
    
    while (right < nums.length) {
        if (!set.contains(nums[right])) {
            swap(nums, left, right);
            set.add(nums[left]);
            left++;
        } 
        right++;
    }
    return left;
}
private void swap(int[] nums, int left, int right) {
    int temp = nums[left];
    nums[left] = nums [right];
    nums [right] = temp;
}

```
