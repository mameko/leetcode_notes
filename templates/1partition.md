# 1 Sorting模板

## Partition

模板一：用数组内的数切一刀，能返回index ([L399 Nuts & Bolts Proble](../my-notes/quick-sort/l399-nuts-and-bolts-problem.md)m)左边的数都小于等于pivot，右边的都大于等于pivot，这个数是在数组里的

```java
public int partition(int[] nums, int l, int r) {
    //初始化左右指针和pivot
    int left = l, right = r;
    int pivot = nums[left];

    //进行partition
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

    // 返回pivot点到数组里
    nums[left] = pivot;
    return left;
}
```

模板二：用任何数字切一刀，不能返回index。（[912 Sort an Array](../my-notes/quick-sort/912-sort-an-arcray.md)）把数组一刀切成两份，但切的这个数，不一定是数组内的数，切完以后左半部分>=右半部分

```java
int left = start, right = end;
//key point 1: pivot is the value, not the index
int pivot = A[(start + end) / 2];

//key point 2: every time you compare left & right, it should be left <= right 
while (left <= right) {
    //key point 3: A[left] < pivot
    while (left <= right && A[left] < pivot) {
        left++;
    }

    //key point 3: A[right] > pivot
    while (left <= right && A[right] > pivot) {
        right--;
    }

    if (left <= right) {
         int tmp = A[left];
         A[left] = A[right];
         A[right] = tmp;

         left++;
         rigth--;
     }
 }
```

模板三：3 way partitions

```java
main（）{
    3WayP(A, 0, A.len - 1);
}

3WayP(A, s, e) {
    if (s >= e) {
        return;
    }

    int mid = s + (e - s) / 2;
    int pivot = A[mid];
    int i = s;
    int left = i, right = e;

    while (i <= right) {
        if (A[i] == pivot) {
            i++;
        } else if (A[i] < pivot) {
            swap(i, left);
            i++;
            left++;
        } else {
            swap(i, right);
            right--;
        }
    }    

    3wayP(A, s, left);
    3wayP(A, i, end);
}
```

模板四：geeksforgeeks，当我们传入pivot value，然后要求返回index。L399 Nuts & Bolts Problem的另一种模板

```java
parition（A, s, e, pivotVal) {
    int i = s;
    for (j = s; j < e; j++) {
        if (A[j] < pivotVal) {
            swap(i, j);
            i++;
        } else if (A[j] == pivotVal) {
            swap(j, e);
            j--;
        }
        swap(i, e);
        return i;
    }
}
```

## Quick Select

[L5 Kth Largest Element](../my-notes/quick-sort/l5-kth-largest-element.md) （215）--- 相关703 用heap

## Merge sort

```
还可以参照L532 Reverse Pair，那题用了ctici的模板。
注意从调用时包含end，传len-1。 还有从mid + 1 开始算。
```

```java
public void sortIntegers2(int[] A) {
    // use a shared temp array, the extra memory is O(n) at least
    int[] temp = new int[A.length];
    mergeSort(A, 0, A.length - 1, temp);
}

private void mergeSort(int[] A, int start, int end, int[] temp) {
    if (start >= end) {
        return;
    }

    int mid = (start + end) / 2;

    mergeSort(A, start, mid, temp);
    mergeSort(A, mid+1, end, temp);
    merge(A, start, mid, end, temp);
}

private void merge(int[] A, int start, int mid, int end, int[] temp) {
    int left = start;
    int right = mid+1;
    int index = start;

    // merge two sorted subarrays in A to temp array
    while (left <= mid && right <= end) {
        if (A[left] < A[right]) {
            temp[index++] = A[left++];
        } else {
            temp[index++] = A[right++];
        }
    }
    while (left <= mid) {
        temp[index++] = A[left++];
    }
    while (right <= end) {
        temp[index++] = A[right++];
    }

    // copy temp back to A
    for (index = start; index <= end; index++) {
        A[index] = temp[index];
    }
}
```

## Quick Sort

```java
public void sortIntegers2(int[] A) {
    quickSort(A, 0, A.length - 1);
}

private void quickSort(int[] A, int start, int end) {
    if (start >= end) {
        return;
    }

    int left = start, right = end;
    // key point 1: pivot is the value, not the index
    int pivot = A[(start + end) / 2];

    // key point 2: every time you compare left & right, it should be 
    // left <= right not left < right
    while (left <= right) {
        // key point 3: A[left] < pivot not A[left] <= pivot
        while (left <= right && A[left] < pivot) {
            left++;
        }
        // key point 3: A[right] > pivot not A[right] >= pivot
        while (left <= right && A[right] > pivot) {
            right--;
        }
        if (left <= right) {
            int temp = A[left];
            A[left] = A[right];
            A[right] = temp;

            left++;
            right--;
        }
    }

    quickSort(A, start, right);
    quickSort(A, left, end);
}
```
