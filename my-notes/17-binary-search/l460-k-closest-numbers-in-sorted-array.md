# L460 K closest Numbers in Sorted Array

Given a target number, a non-negative integer`k`and an integer array A sorted in ascending order, find the k closest numbers to target in A, sorted in ascending order by the difference between the number and target. Otherwise, sorted in ascending order by number if the difference is same.

**Example**

Given A =`[1, 2, 3]`, target =`2`and k =`3`, return`[2, 1, 3]`.

Given A =`[1, 4, 6, 8]`, target =`3`and k =`3`, return`[4, 1, 6]`.

[**Challenge**](http://www.lintcode.com/en/problem/k-closest-numbers-in-sorted-array/#challenge)

O(logn + k) time complexity.

首先找插入位置（[35](35-search-insert-position.md)），然后再从那个位置开始找前后元素的diff最靠近target的。

```java
// 几年后又抄了一遍
public int[] kClosestNumbers(int[] a, int target, int k) {
    if (a == null || a.length == 0) {
        return null;
    }

    int[] result = new int[k];
    int loc = findFirstLargerThan(a, target);

    int left = loc - 1;
    int right = loc;
    for (int i = 0; i < k; i++) {
        if (isLeftCloser(a, target, left, right)) {
            result[i] = a[left];
            left--;
        } else {
            result[i] = a[right];
            right++;
        }
    }

    return result;
}

private boolean isLeftCloser(int[] a, int target, int left, int right) {
    if (left < 0) {
        return false;
    }

    if (right >= a.length) {
        return true;
    }

    if (target - a[left] != a[right] - target) {
        return target - a[left] < a[right] - target; 
    }

    return true; // if equal pick left first，所以相等时候return true， pick left
}

private int findFirstLargerThan(int[] a, int target) {
    int start = 0;
    int end = a.length - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (a[mid] >= target) {
            end = mid;
        } else {
            start = mid;
        }
    }

    if (a[start] >= target) {
        return start;
    }

    if (a[end] >= target) {
        return end;
    }

    return a.length;
}


public int[] kClosestNumbers(int[] A, int target, int k) {
    if (A == null || A.length == 0 || k < 0) {
        return null;
    }

    if (k > A.length) {
            return A;
        }

    int[] res = new int[k];
    if (k == 0) {
        return res;
    }

    int cur = 0;
    int loc = findFirst(A, target);
    int start = loc - 1;
    int end = loc;

    while (cur < k) {
        if (start < 0) {
            res[cur] = A[end++];                
        } else if (end >= A.length) {
            res[cur] = A[start--];
        } else {
            if ((target - A[start]) <= (A[end] - target)) {
                res[cur] = A[start--];
            } else {
                res[cur] = A[end++];    
            }
        }
        cur++;
    }

    return res;
}

private int findFirst(int[] A, int target) {
    int start = 0;
    int end = A.length - 1;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (target > A[mid]) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (A[start] >= target) {
        return start;
    }

    if (A[end] >= target) {
        return end;
    }

    return A.length;
}
```

```java
// 换个变量名好像容易记住点
public List<Integer> findClosestElements(int[] arr, int k, int x) {
    List<Integer> res = new ArrayList<>();
    if (arr == null || arr.length == 0 || k < 0) {
        return res;
    }

    int loc = findInsert(arr, x);
    int cur = 0;
    int left = loc - 1;
    int right = loc;

    while (cur < k) {
        if (left < 0) {
            res.add(arr[right++]);
        } else if (right >= arr.length) {
            res.add(arr[left--]);
        } else {
            if (x - arr[left] <= arr[right] - x) {
                res.add(arr[left--]);
            } else {
                res.add(arr[right++]);
            }
        }
        cur++;
    }
    Collections.sort(res);
    return res;
}

private int findInsert(int[] nums, int target) {
    int start = 0;
    int end = nums.length - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (nums[start] >= target) {
        return start;
    } else if (nums[end] >= target) {
        return end;
    } else {
        return end + 1;
    }
}
```
