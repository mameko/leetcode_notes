# L143 Sort Colors II

Given an array of\_n\_objects with\_k\_different colors (numbered from 1 to k), sort them so that objects of the same color are adjacent, with the colors in the order 1, 2, ... k.

## Notice

You are not suppose to use the library's sort function for this problem.

**Example**

Given colors=`[3, 2, 2, 1, 4]`,`k=4`, your code should sort colors in-place to`[1, 2, 2, 3, 4]`.

[**Challenge**](http://www.lintcode.com/en/problem/sort-colors-ii/#challenge)

A rather straight forward solution is a two-pass algorithm using counting sort. That will cost O(k) extra memory. Can you do it without using extra memory?

老师提过什么rainbow sort，这里只写了counting sort和3 way partition。

又过了几年，再上九章，终于得到了rainbow sort了。其实所谓的rainbow sort，其实是利用分治merge sort的思想。把k每次切半然后partition里面的。就是mergesort套quicksort的感觉。虽然思想不难，但要写对需要注意一下符号。（背诵默写）这个算法的时间复杂度是O(nlogk)，因为我们切半的是k所以log在k那边。

```java
public void sortColors2(int[] colors, int k) {
    if (colors == null || colors.length == 0) {
        return;
    }

    sortHelper(colors, 0, colors.length - 1, 1, k);
}

private void sortHelper(int[] colors, int start, int end, 
                        int colorStart, int colorEnd) {
    if (colorStart == colorEnd) {
        return;
    }

    if (start == end) {
        return;
    }

    int midColor = (colorStart + colorEnd) / 2;
    int left = start;
    int right = end;
    while (left <= right) {
        while (left <= right && colors[left] <= midColor) { // <=，不然会stack overflow
            left++;
        }

        while (left <= right && colors[right] > midColor) {
            right--;
        }

        if (left <= right) {
            int tmp = colors[left];
            colors[left] = colors[right];
            colors[right] = tmp;

            left++;
            right--;
        }
    }

    sortHelper(colors, start, right, colorStart, midColor);
    sortHelper(colors, left, end, midColor + 1, colorEnd); // +1,不然会stack overflow
}
```

counting sort：

```java
 public void sortColors2(int[] colors, int k) {
     if (colors == null || colors.length == 0 || k < 1) {
         return;
     }

     //counting sort version
     int[] counts = new int[k];

     for (int i = 0; i < colors.length; i++) {
         counts[colors[i] - 1]++;
     }

     int i = 0;
     int j = 0;
     // counting sort, pay attention to bounds and indexs when putting numbers back
     while (i < colors.length && j < counts.length) {
         while (j < counts.length && counts[j] != 0) {
             colors[i++] = j + 1;
             counts[j]--;
         }
         j++;
     }
 }
```

3 way partition:分下去以后，从start到left，然后从i到right。例如：\[3, 2, 3, 1, 4] 经过一轮以后会变成\[1， 2， 2， 4， 3]，然后start 指着1，left指着2，i指着4，end指着3

```java
public void sortColors2(int[] colors, int k) {
    if (colors == null || colors.length == 0 || k < 1) {
        return;
    }
    threeWayPartition(colors, 0, colors.length - 1);
}

private void threeWayPartition(int[] A, int start, int end) {
    if (start >= end || start < 0 || end >= A.length) {
        return;
    }
    int mid = start + (end - start) / 2;
    int pivot = A[mid];
    int i = start;
    int left = i;
    int right = end;
    while (i <= right) {
        if (A[i] == pivot) {
            i++;
        } else if (A[i] < pivot) {
            swap(A, i, left);
            i++;
            left++;
        } else {
            swap(A, i, right);
            right--;
        }
    }
    threeWayPartition(A, start, left);
    threeWayPartition(A, i, end);
}

private void swap(int[] a, int i, int j) {
    int tmp = a[i];
    a[i] = a[j];
    a[j] = tmp;
}
```
