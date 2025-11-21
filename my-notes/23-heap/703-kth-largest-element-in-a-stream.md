# 703 Kth Largest Element in a Stream

Design a class to find the**k**th largest element in a stream. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Your `KthLargest` class will have a constructor which accepts an integer`k`and an integer array`nums`, which contains initial elements from the stream. For each call to the method`KthLargest.add`, return the element representing the kth largest element in the stream.

**Example:**

```
int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);   // returns 4
kthLargest.add(5);   // returns 5
kthLargest.add(10);  // returns 5
kthLargest.add(9);   // returns 8
kthLargest.add(4);   // returns 8
```

**Note:**\
You may assume that `nums`' length ≥ `k-1` and`k`≥ 1.

这题好像是真题，在面FB时就被当作follow up，估计是我脑残一上来就给quick select。嘛，这题其实比较简单。用的是heap，因为我们求largest，所以用min heap。

```java
class KthLargest {
    Queue<Integer> pq;
    int k;
    public KthLargest(int k, int[] nums) { // Constructor
        pq = new PriorityQueue<>();
        this.k = k;
        if (nums == null || nums.length == 0) {
            return;
        }

        for (int i = 0; i < nums.length; i++) { // 其实这里可以重复利用add的代码
            pq.offer(nums[i]);
            if (pq.size() > k) {
                pq.poll();
            }
        }        
    }

    public int add(int val) {
        pq.offer(val);
        if (pq.size() > k) {            
            pq.poll();
        }
        return pq.peek();
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
```
