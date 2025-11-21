# L81 Data Stream Median

用两个堆来维护中位数。大堆顶装着小半部分的，小堆顶着大半部分的。如果是单数的话，取大堆顶的返回。大堆比小堆多一个元素。每次插入后记得维护堆的平衡。

## 295 Find Median from Data Stream

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:

`[2,3,4]`, the median is`3`

`[2,3]`, the median is`(2 + 3) / 2 = 2.5`

Design a data structure that supports the following two operations:

* void addNum(int num) - Add a integer number from the data stream to the data structure.
* double findMedian() - Return the median of all elements so far.

For example:

```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

cc189的实现比自己写的简洁多了：

```java
public class MedianFinder {

    PriorityQueue<Integer> minHeap;
    PriorityQueue<Integer> maxHeap;

    /** initialize your data structure here. */
    public MedianFinder() {
        minHeap = new PriorityQueue<>();
        maxHeap = new PriorityQueue<>(10, Collections.reverseOrder());
    }

    // maxHeap has 1 more element than minHeap
    public void addNum(int num) {
        if (minHeap.size() == maxHeap.size()) {
            if ((!minHeap.isEmpty()) && num > minHeap.peek()) {
                maxHeap.offer(minHeap.poll());
                minHeap.offer(num);
            } else {
                maxHeap.offer(num);
            }
        } else {
            if (num < maxHeap.peek()) {
                minHeap.offer(maxHeap.poll());
                maxHeap.offer(num);
            } else {
                minHeap.offer(num);
            }
        }
    }

    public double findMedian() {
        int curSize = maxHeap.size() + minHeap.size();
        if (curSize % 2 == 0) {
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        } else {
            return maxHeap.peek() * 1.0;
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

## L81 Data Stream Median

Numbers keep coming, return the median of numbers at every time a new number added.

**Clarification**

What's the definition of Median?

* Median is the number that in the middle of a sorted array. If there are n numbers in a sorted array A, the median is`A[(n - 1) / 2]`. For example, if`A=[1,2,3]`, median is`2`. If`A=[1,19]`, median is`1`.

**Example**

For numbers coming list:`[1, 2, 3, 4, 5]`, return`[1, 1, 2, 2, 3]`.

For numbers coming list:`[4, 5, 1, 3, 2, 6, 0]`, return`[4, 4, 4, 3, 3, 3, 3]`.

For numbers coming list:`[2, 20, 100]`, return`[2, 2, 20]`.

[**Challenge**](http://www.lintcode.com/en/problem/data-stream-median/#challenge)

Total run time in O(_nlogn_).

```java
 public int[] medianII(int[] nums) {
    if (nums == null || nums.length == 0) {
        return null;
    }

    int[] res = new int[nums.length];
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();// store larger elements
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(nums.length, Collections.reverseOrder());// store smaller elements
    for (int i = 0; i < nums.length; i++) {
        int cur = nums[i];


        if (maxHeap.isEmpty()) {
            maxHeap.offer(cur);
        } else if (minHeap.isEmpty()) {
            maxHeap.offer(cur);
                minHeap.offer(maxHeap.poll());
        } else if (cur > minHeap.peek()) {
            minHeap.offer(cur);
        } else {
            maxHeap.offer(cur);
        }

        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.offer(maxHeap.poll());
        } else if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }

        /* if need to calculate (mid + mid + 1) / 2 version.
        you will add the (min.peek + max.peek) / 2 when total size is even.
        return max.peek when total size is off*/
        res[i] = maxHeap.peek();
    }
    return res;
}
```
