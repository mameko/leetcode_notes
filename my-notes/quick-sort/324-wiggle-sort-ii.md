# 324 Wiggle Sort II

Given an unsorted array`nums`, reorder it such that`nums[0] < nums[1] > nums[2] < nums[3]...`.

**Example:**\
(1) Given`nums = [1, 5, 1, 1, 6, 4]`, one possible answer is`[1, 4, 1, 5, 1, 6]`.\
(2) Given`nums = [1, 3, 2, 2, 3, 1]`, one possible answer is`[2, 3, 1, 3, 1, 2]`.

**Note:**\
You may assume all input has valid answer.

**Follow Up:**\
Can you do it in O(n) time and/or in-place with O(1) extra space?

这题nlogn的解法不难，就是排序，然后把数组中间断开。小的一半放偶数位，大的一半放奇数位就ok了。

解法一：

```java
public static void wiggleSort(int[] nums) {
    if (nums == null || nums.length < 2) {
        return;
    }

    int[] tmp = Arrays.copyOf(nums, nums.length);
    // this method use nlogn time & n space
    Arrays.sort(tmp);
    int mid = (nums.length - 1) / 2;
    int end = nums.length - 1;
    for (int i = 0; i < nums.length; i++) {
        nums[i] = (i % 2 == 0) ? tmp[mid--] : tmp[end--];
    }
}
```

follow up：这个难度在于要用quick select找中位数，然后把数组劈开两半。然后跟上面同理可得结果。但普通的quick select是不能通过OJ的，因为我们要保证重复的元素都集中在中间才行。所以quick select里要用3 way partition。这个中间要用一个arraylist来暂存答案，所以用了O（n）space（这题考点太多太崩溃，O(1)space的virtual index wiring简直理解不能，完爆脑容量)

```java
public void wiggleSort(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }

    int len = nums.length;
    // quick select to find middle point
    int mid = quickSelect(nums, len / 2);

    if (len % 2 == 0) {
        mid = mid - 1;
    }

    int smInd = mid;
    int bgInd = len - 1;
    // rearrange the array
    ArrayList<Integer> tmp = new ArrayList<>();
    for (int i = 0; i < len; i++) {
        if (i % 2 == 0) {
            tmp.add(nums[smInd--]);
        } else {
            tmp.add(nums[bgInd--]);
        }
    }

    int ind = 0;
    // put result back to the original array
    for (Integer elem : tmp) {
        nums[ind++] = elem;
    }
}

private int quickSelect(int[] nums, int k) {
    int start = 0;
    int end = nums.length - 1;
    int[] bounds = patition(nums, start, end);
    while (start < end && (k < bounds[0] || k > bounds[1])) {
        if (k < bounds[0]) {
            end = bounds[0] - 1;
        } else if (k > bounds[1]) {
            start = bounds[1] + 1;
        }
        bounds = patition(nums, start, end);
    }

    return k;
}

private int[] patition(int[] nums, int start, int end) {
    int left = start;
    int right = end;
    int i = start;
    int pivot = nums[left];

    while (i <= right) {
        if (nums[i] < pivot) {
            swap(i, left, nums);
            i++;
            left++;
        } else if (nums[i] == pivot) {
            i++;
        } else {
            swap(i, right, nums);
            right--;
        }
    }

    int[] res = new int[2];
    res[0] = left;
    res[1] = right;
    return res;
}

private void swap(int i, int j, int[] nums) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}
```
