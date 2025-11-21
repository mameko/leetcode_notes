# L39 Recover Rotated Sorted Array

Given a **rotated** sorted array, recover it to sorted array in-place.

**Clarification**

What is rotated array?

* For example, the orginal array is \[1,2,3,4], The rotated array of it can be \[1,2,3,4], \[2,3,4,1], \[3,4,1,2], \[4,1,2,3]

**Example**

`[4, 5, 1, 2, 3]`->`[1, 2, 3, 4, 5]`

[**Challenge**](http://www.lintcode.com/en/problem/recover-rotated-sorted-array/#challenge)

In-place, O(_1_) extra space and O(_n_) time.

这个可以用一个loop O（n）找到断点，然后3步翻转。不过我用了L159和L160的方法2分找，O(logn）。3步翻转用O（n），所以总体复杂度O(n)

```java
public void recoverRotatedSortedArray(ArrayList<Integer> nums) {
        if (nums == null || nums.size() == 0) {
            return;
        }

        int minLoc = 0;
        // binary search for min on rotated array
        int start = 0;
        int end = nums.size() - 1;

        if (nums.get(start) < nums.get(end)) { //array not rotated
            return;
        }

        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums.get(mid) < nums.get(end)) {
                end = mid;
            } else if (nums.get(mid) > nums.get(end)){
                start = mid;
            } else {
                end--;
            }
        }

        if (nums.get(start) < nums.get(end)) {
            minLoc = start;
        } else {
            minLoc = end;
        }

        // 3 step rotate
        // [start, end]
        reverse(0, minLoc - 1, nums);
        reverse(minLoc, nums.size() - 1, nums);
        reverse(0, nums.size() - 1, nums);
    }

    private void reverse(int start, int end, ArrayList<Integer> nums) {
        for (int i = start, j = end; i < j; i++, j--) {
            int tmp = nums.get(i);
            nums.set(i, nums.get(j));
            nums.set(j, tmp);
        }
    }
```
