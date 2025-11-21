# 540 Single Element in a Sorted Array

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return _the single element that appears only once_.

Your solution must run in `O(log n)` time and `O(1)` space.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [1,1,2,3,3,4,4,8,8]
</strong><strong>Output: 2
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [3,3,7,7,10,11,11]
</strong><strong>Output: 10
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 10^5`
* `0 <= nums[i] <= 10^5`

`这题跟`[136 Single Number](../31-bit-manipulation/136-single-number.md)很像。但是多了个条件，因为是有序的，所以可以做到logn。其实这题，老师上课的时候已经提到利用长度的奇偶性，所以没有多少思考。后来自己做，发现不会用长度奇偶性来扔掉一半，所以用了下标。

这里，因为九章二分模板是两个相邻的数字就会停下来，而且mid是偏左的，为了不超出范围，所以判断条件用了mid和mid+1。最后这两个是start和end，不会超出范围。然后判断的时候，因为都是一双一双出现的，所以，当我们发现mid指着左边的元素时候，我们要判断它的下标是否是偶数。

如果是偶数的话，往后找。奇数，往前找。例子：\[2, 3, 3]，mid==mid+1，然后mid的下标是奇数，所以往前找。\[2, 2, 3, 3, 4] mid==mid+1，mid的下标是偶数，往后找。

同样，我们要判断mid和mid+1不同的情况。这时候，判断条件跟上面的相反，因为这次mid指着的是pair里右边的元素。例子：\[2, 3, 3, 4, 4], mid != mid+1, mid下标是偶数，往前找。\[2, 2, 3]，mid的下标是奇数，往后找。

因为每次我们扔掉的都是符合条件的一半，所以最后，start跟end里有一个是single的那个数。最后用的是长度来判断。如果包含这个元素的array的长度为奇数，那么那个就是答案了。T:O(logn), S:O(1)

leetcode答案里有提出一种只做偶数下标数字的二分，T:O(logn/2)，虽然会快一倍，但复杂度相同。抄下来做参考。

```java
// 我的答案
public int singleNonDuplicate(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    int start = 0;
    int end = nums.length - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] == nums[mid + 1]) {
            if (mid % 2 == 0) {
                start = mid;
            } else {
                end = mid;
            }
        } else {
            if (mid % 2 == 0) {
                end = mid;
            } else {
                start = mid;
            }
        }
    }

    if ((start + 1) % 2 != 0) {
        return nums[start];
    }

    if ((end + 1) % 2 != 0) {
        return nums[end];
    }

    return -1;
}

// leetcode用“长度奇偶性”解的答案
public int singleNonDuplicate(int[] nums) {
    int lo = 0;
    int hi = nums.length - 1;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        boolean halvesAreEven = (hi - mid) % 2 == 0;
        if (nums[mid + 1] == nums[mid]) {
            if (halvesAreEven) {
                lo = mid + 2;
            } else {
                hi = mid - 1;
            }
        } else if (nums[mid - 1] == nums[mid]) {
            if (halvesAreEven) {
                hi = mid - 2;
            } else {
                lo = mid + 1;
            }
        } else {
            return nums[mid];
        }
    }
    return nums[lo];
}

// leetcode用”even index二分“的答案
public int singleNonDuplicate(int[] nums) {
    int lo = 0;
    int hi = nums.length - 1;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (mid % 2 == 1) mid--;
        if (nums[mid] == nums[mid + 1]) {
            lo = mid + 2;
        } else {
            hi = mid;
        }
    }
    return nums[lo];
}

```
