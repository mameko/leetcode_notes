# 275 H-Index II

**Follow up** for [H-Index](274-h-index.md): What if the`citations`array is sorted in ascending order? Could you optimize your algorithm?

因为有序而且要logn，所以二分，但这里感觉很难，要找逆序时第一个下标大于值的location。九章模板完全套不上，这题看了答案说是要找min index that citations\[index] >=length(citations) -index，然后答案是len - index。写下来作为参考。

```java
public int hIndex(int[] citations) {
    if (citations == null || citations.length == 0) {
        return 0;
    }

    int left = 0;
    int len = citations.length;
    int right = len - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (citations[mid] >= (len - mid)) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    return len - left;
}
```
