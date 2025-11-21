# L362 Sliding Window Maximum

Given an array of n integer with duplicate number, and a moving window(size k), move the window at each iteration from the start of the array, find the maximum number inside the window at each moving.

**Example**

For array`[1, 2, 7, 7, 8]`, moving window size`k = 3`. return`[7, 7, 8]`

At first the window is at the start of the array like this

`[|1, 2, 7| ,7, 8]`, return the maximum`7`;

then the window move one step forward.

`[1, |2, 7 ,7|, 8]`, return the maximum`7`;

then the window move one step forward again.

`[1, 2, |7, 7, 8|]`, return the maximum`8`;

[**Challenge**](http://www.lintcode.com/en/problem/sliding-window-maximum/#challenge)

o(n) time and O(k) memory

这题用了dequeue。这里删除的时候（removeBefore函数）得注意，要从后往前删，不然的话，下标就会出错，然后漏掉该删的数。

```java
public ArrayList<Integer> maxSlidingWindow(int[] nums, int k) {
    ArrayList<Integer> res = new ArrayList<Integer>();
    if (nums == null || nums.length < k || k < 1) {
        return res;
    }

    LinkedList<Integer> q = new LinkedList<>();
    for (int i = 0; i < k; i++) {
        q.offer(i);
        removeBefore(q, nums);
    }

    for (int i = k; i < nums.length; i++) {
        res.add(nums[q.peekFirst()]);
        q.offer(i);
        if (q.peekFirst() <= (i - k)) {
            q.removeFirst();
        }
        removeBefore(q, nums);
    }
    res.add(nums[q.peekFirst()]);
    return res;
}

private void removeBefore(LinkedList<Integer> q, int[] nums) {
    int loc = q.getLast();
    int cur = nums[loc];
    int size = q.size() - 1;
    for (int i = size - 1; i >= 0; i--) {
        if (nums[q.get(i)] < cur) {
            q.remove(i);
        }
    }
}
```

补一个heap的做法，因为java的heap删除得O(n)，所以有了上面的方法？

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 1) {
        return new int[0];
    }

    PriorityQueue<Integer> pq = new PriorityQueue<>(k, Comparator.reverseOrder());
    ArrayList<Integer> res = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        pq.offer(nums[i]);
        if (pq.size() >= k) {
            res.add(pq.peek());             
            pq.remove(nums[i - k + 1]);
        }
    }


    int[] result = new int[res.size()];
    for (int i = 0; i < res.size(); i++) {
        result[i] = res.get(i);
    }

    return result;
}
```
