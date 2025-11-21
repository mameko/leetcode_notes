# L360 Sliding Window Median

Given an array of n integer, and a moving window(size k), move the window at each iteration from the start of the array, find the median of the element inside the window at each moving. (If there are even numbers in the array, return the N/2-th number after sorting the element in the window. )

**Example**

For array`[1,2,7,8,5]`, moving window size k = 3. return`[2,7,7]`

At first the window is at the start of the array like this

`[ | 1,2,7 | ,8,5]`, return the median`2`;

then the window move one step forward.

`[1, | 2,7,8 | ,5]`, return the median`7`;

then the window move one step forward again.

`[1,2, | 7,8,5 | ]`, return the median`7`;

[**Challenge**](http://www.lintcode.com/en/problem/sliding-window-median/#challenge)

O(nlog(n)) time

这题因为有个窗口的限制，所以堆的大小有限制，而且因为要删除在窗口外的元素，所以要弄一个类，把值跟下标存起来，方便删除。

```java
    public ArrayList<Integer> medianSlidingWindow(int[] nums, int k) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        if (nums == null || nums.length == 0 || k < 1) {
            return res;
        }

        PriorityQueue<Pair> minHeap = new PriorityQueue<>();
        PriorityQueue<Pair> maxHeap = new PriorityQueue<>(nums.length, Collections.reverseOrder());

        for (int i = 0; i < nums.length; i++) {
            Pair cur = new Pair(nums[i], i);

            if (maxHeap.isEmpty()) {
                maxHeap.offer(cur);
            } else if (minHeap.isEmpty()) {
                maxHeap.offer(cur);
                minHeap.offer(maxHeap.poll());
            } else if (cur.val > minHeap.peek().val) {
                minHeap.offer(cur);
            } else {
                maxHeap.offer(cur);
            }

            if ((maxHeap.size() + minHeap.size()) > k) {
                maxHeap.remove(new Pair(nums[i - k], i - k));
                minHeap.remove(new Pair(nums[i - k], i - k));
            }

            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.offer(maxHeap.poll());
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.offer(minHeap.poll());
            }

            if (i >= k - 1) {
                res.add(maxHeap.peek().val);
            }
        }

        return res;
    }
}

class Pair implements Comparable<Pair> {
    int val;
    int loc;

    public Pair(int v, int l) {
        val = v;
        loc = l;
    }

    @Override
    public int compareTo(Pair other) {
        return val - other.val;
    }

    @Override
    public int hashCode() {
        return Objects.hash(val, loc);
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Pair) {
            Pair p = (Pair) obj;
            return val == p.val && loc == p.loc;
        }
        return false;
    }
}
```
