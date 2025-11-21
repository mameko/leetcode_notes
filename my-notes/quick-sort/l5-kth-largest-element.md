# L5 Kth Largest Element

Find K-th largest element in an array.

## Notice

You can swap elements in the array

**Example**

In array`[9,3,2,4,8]`, the 3rd largest element is`4`.

In array`[1,2,3,4,5]`, the 1st largest element is`5`, 2nd largest element is`4`, 3rd largest element is`3`and etc.

avg:O(n) time, O(1) extra memory. worst T:O(n^2) -- just like quick sort。如果input是stream的话，可以用heap，nlogk

```java
// O(n) - avg
public int kthLargestElement(int k, int[] nums) {
    if (nums == null || nums.length == 0 || k < 1) {
        return Integer.MIN_VALUE;
    }

    int res = qucikSelect(nums, nums.length - k);
    return res;
}

private int qucikSelect(int[] nums, int k) {
    int start = 0;
    int end = nums.length - 1;
    int index = partition(nums, start, end);
    while (start < end && index != k) {
        if (index > k) {
            end = index - 1;
        } else if (index < k) {
            start = index + 1;
        }
        index = partition(nums, start, end);
    }

    return nums[index];
}

private int partition(int[] nums, int start, int end) {
    if (start >= end) { // 这里可以加个判断，但因为我们在调用时
        return start;   // 已经保证了这情况不会出现。所以其实可以没有。
    }
    int left = start;
    int right = end;
    int pivot = nums[left];
    while (left < right) {
        while (left < right && nums[right] >= pivot) {
            right--;
        }
        nums[left] = nums[right];

        while (left < right && nums[left] <= pivot) {
            left++;
        }
        nums[right] = nums[left];
    }

    nums[left] = pivot;
    return left;
}
```

补一个模板2的：

```java
public int kthLargestElement(int k, int[] nums) {
    if (nums == null) {
        return -1;
    }
    
    return quickSelect(nums, 0, nums.length - 1, k);
}

private int quickSelect(int[] nums, int start, int end, int k) {
    if (start == end) {
        return nums[start];
    }
    
    int i = start, j = end;
    int pivot = nums[(i + j) / 2];
    
    while (i <= j) {
        while (i <= j && nums[i] > pivot) {
            i++;
        }
        
        while (i <= j && nums[j] < pivot) {
            j--;
        }
        
        if (i <= j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
            i++;
            j--;
        }
    }
    
    // 最后ij会交错，所以把数组分成3份。
    // [start, i], (i, j)<--中间可能夹着一个数，那个就是答案, [j, end]
    if (start + k - 1 <= j) { // 第k个数在左边
        return quickSelect(nums, start, j, k);
    }
    
    if (start + k - 1 >= i) { // 第k个数在右边
        return quickSelect(nums, i, end, k);
    }
    
    return [j + 1]; // 中间夹着的那个数就是答案
}
```

补一个heap的解法，如果是stream的input就得用heap了。T: O(nlogn), S: O(k)

```java
public int findKthLargest(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0) {
        return -1;
    }

    // largest so use min heap
    PriorityQueue<Integer> pq = new PriorityQueue<>();
    for (int num : nums) {
        pq.offer(num);
        if (pq.size() > k) {
            pq.poll();
        }
    }

    return pq.peek();
}
```
