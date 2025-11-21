# L160 Find Minimum in Rotated Sorted Array II

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

## Notice

The array may contain duplicates.

**Example**

Given`[4,4,5,6,7,0,1,2]`return`0`.

跟上题L159解法相似，去重跟L63相似

```java
public int findMin(int[] num) {
        if (num == null || num.length == 0) {
            return -1;
        }

        int start = 0;
        int end = num.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (num[mid] > num[end]) {// if you compare with start here
                start = mid;
            } else if (num[mid] < num[end]) {
                end = mid;
            } else {
                end--;// you need to start++ here
            }
        }

        return Math.min(num[start], num[end]);
    }
```
