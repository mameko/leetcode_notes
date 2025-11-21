# L130 Hepify

Given an integer array, heapify it into a min-heap array.

For a heap array A, A\[0] is the root of heap, and for each A\[i], A\[i \* 2 + 1] is the left child of A\[i] and A\[i \* 2 + 2] is the right child of A\[i].

**Clarification**

**What is heap?**

* Heap is a data structure, which usually have three methods: push, pop and top. where "push" add a new element the heap, "pop" delete the minimum/maximum element in the heap, "top" return the minimum/maximum element.

**What is heapify?**

* Convert an unordered integer array into a heap array. If it is min-heap, for each element A\[i], we will get A\[i \* 2 + 1] >= A\[i] and A\[i \* 2 + 2] >= A\[i].

**What if there is a lot of solutions?**

* Return any of them.

**Example**

Given \[3,2,1,4,5], return \[1,2,3,4,5] or any legal heap array.

[**Challenge**](http://www.lintcode.com/en/problem/heapify/#challenge)

O(n) time complexity

堆，其实是一棵满二叉树，所以可以用一条array来存。具体存法得看题目。有些是从0开始存，有些留空一格从1开始存。这题主要是建立堆，对于每个元素可以采取sift up/sift down。这里实现采用了sift up。`O(nlog n)，btw，sift down是O(n)`

```java
public void heapify(int[] A) {
    if (A == null || A.length == 0) {
        return;
    }

    for (int i = 0; i < A.length; i++) {
            int cur = A[i];
            int curLoc = i;
            int pLoc = (i - 1) / 2;
            while (inBound(A, pLoc) && cur < A[pLoc]) {
                swap(A, curLoc, pLoc);
                curLoc = pLoc;
                pLoc = (pLoc - 1) / 2;
            }
        }
}

private boolean inBound(int[] A, int i) {
    if (i < 0 || i >= A.length) {
        return false;
    }
    return true;
}

private void swap(int[] A, int i, int j) {
    int tmp = A[i];
    A[i] = A[j];
    A[j] = tmp;
}
```

补一个shift down的

![](<../../.gitbook/assets/image (6).png>)
