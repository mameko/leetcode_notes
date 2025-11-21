# 4 Median of Two Sorted Arrays

There are two sorted arrays **nums1**and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

暴力解法是真的排个序，然后找到中位数。这样会话O(n + m)的时间。另外是用二分？的方法，找第kth个。这样的时间复杂度是O(log(m + n))。例如在两个array里找第四个数（k = 4），我们通过比较中间的数O(1)把问题转化成找第二个数（k = 2）。这个操作我们把可以扔掉的数扔掉。仍的方法是把start位置往后移，然后继续递归。base case是k = 1，这个时候，在来两个数组里找最小的数，因为有序，所以只要比较两个数组里的第一个数，返回小的那个就是了。

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null) {
        return 0.0;
    }

    int n = nums1.length;
    int m = nums2.length;
    int len = n + m;

    if (len % 2 != 0) {
        return findKth(nums1, 0, nums2, 0, len / 2 + 1) * 1.0;
    } else {
        return (findKth(nums1, 0, nums2, 0, len / 2) + findKth(nums1, 0, nums2, 0, len / 2 + 1)) / 2.0;
    }
}

private int findKth(int[] nums1, int s1, int[] nums2, int s2, int k) {
    if (s1 >= nums1.length) {
        return nums2[s2 + k - 1];
    } else if (s2 >= nums2.length) {
        return nums1[s1 + k - 1];
    }

    if (k == 1) { // base case
        return Math.min(nums1[s1], nums2[s2]);
    }

    int mid1 = s1 + k / 2 - 1 < nums1.length ? nums1[s1 + k / 2 - 1] : Integer.MAX_VALUE;
    int mid2 = s2 + k / 2 - 1 < nums2.length ? nums2[s2 + k / 2 - 1] : Integer.MAX_VALUE;

    if (mid1 < mid2) {
        return findKth(nums1, s1 + k / 2 , nums2, s2, k - k / 2);
    } else {
        return findKth(nums1, s1, nums2, s2 + k / 2, k - k / 2);
    }
}
```

```java
public double findMedianSortedArrays(int[] A, int[] B) {
    if (A == null || B == null) {
        return Double.MAX_VALUE;
    }

    double res = 0.0;
    ArrayList<Integer> list = new ArrayList<>();
    for (int i = 0; i < A.length; i++) {
        list.add(A[i]);
    }

    for (int i = 0; i < B.length; i++) {
        list.add(B[i]);
    }

    Collections.sort(list);
    int len = list.size();
    if (len % 2 == 0) {
        res = list.get(len / 2) + list.get(len / 2 - 1);
        res = res / 2;
    } else {
        res = list.get(len / 2);
    }
    return res;
}
```
