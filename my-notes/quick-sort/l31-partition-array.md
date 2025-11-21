# L31 Partition Array

Given an array`nums`of integers and an int`k`, partition the array (i.e move the elements in "nums") such that:

* All elements < _k \_are moved to the \_left_
* All elements >= _k \_are moved to the \_right_

Return the partitioning index, i.e the first inde&#x78;_&#x69;\_nums\[\_i_] >=_k_.

## Notice

You should do really partition in array nums\_instead of just counting the numbers of integers smaller than k.

If all elements in _nums are smaller than\_k_, then return _nums.length_

**Example**

If nums =`[3,2,2,1]`and`k=2`, a valid answer is`1`.

[**Challenge**](http://www.lintcode.com/en/problem/partition-array/#challenge)

Can you partition the array in-place and in O(n)?

模板：

```java
public int partitionArray(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int left = 0;
    int right = nums.length - 1;

    while (left <= right) {
        // 没有等号，所以left会停在第一个k值的地方
        while (left <= right && nums[left] < k) { 
            left++;
        }
        // 有等于，所以最后right会停在第一个k的左边
        while (left <= right && nums[right] >= k) { 
            right--;
        }

        if (left <= right) { // 然后，因为left和right交错了，所以不会交换
            int tmp = nums[left];
            nums[left] = nums[right];
            nums[right] = tmp;
            left++;
            right--;
        }
    }

    return left; // 所以这里要返回left
}
```

非模板：

```java
public int partitionArray(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int i = 0;
    int j = nums.length - 1;
    while (i < j) {
        while (i < nums.length && nums[i] < k) {
            i++;
        }

        while (j >= 0 && nums[j] >= k) {
            j--;
        }

        if (i < j) {
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;

            i++;
            j--;
        }
    }

    return i;
}
```
