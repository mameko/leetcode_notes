# 1675 Minimize Deviation in Array

You are given an array `nums` of `n` positive integers.

You can perform two types of operations on any element of the array any number of times:

* If the element is **even**, **divide** it by `2`.
  * For example, if the array is `[1,2,3,4]`, then you can do this operation on the last element, and the array will be `[1,2,3,2].`
* If the element is **odd**, **multiply** it by `2`.
  * For example, if the array is `[1,2,3,4]`, then you can do this operation on the first element, and the array will be `[2,2,3,4].`

The **deviation** of the array is the **maximum difference** between any two elements in the array.

Return _the **minimum deviation** the array can have after performing some number of operations._

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: 1
Explanation: You can transform the array to [1,2,3,2], then to [2,2,3,2], then the deviation will be 3 - 2 = 1.
```

**Example 2:**

```
Input: nums = [4,1,5,20,3]
Output: 3
Explanation: You can transform the array after two operations to [4,2,5,5,3], then the deviation will be 5 - 2 = 3.
```

**Example 3:**

```
Input: nums = [2,10,8]
Output: 3
```

**Constraints:**

* `n == nums.length`
* `2 <= n <= 105`
* `1 <= nums[i] <= 109`

这题，一看，毫无头绪，看完hint还是没，果然是难题。然后看了答案，发现实现起来其实不难。只是想到这个方法比较难。因为我们的最后答案是求变完以后数组里的minDiff，所以我们要找数组里的max和min。为了缩小minDiff，我们要不增加min要不减少max。这题题目说了**even只能减少，odd只能增加**。所以我们可以先定好增加min，还是减少max。这里选择了减少max。

那么我们首先处理数字的时候，就把所有odd的数X2，然后就会变成偶数了。这时候，我们开始算minDiff。第一个算出来的minDiff不一定是最小的，所以要loop着比较，看会不会找到更小的。因为要快速找到max，所以我们用了maxHeap来存。每次算一算放到minDiff(代码里是res)里。

每当我们用完一个数来算，如果那个数是even的，我们还可以除以2，丢回堆里，看看以后会不会找到更小的diff。一直重复这个过程到我们max in heap是奇数，**因为基数不能变小，就是说max不能更小了**，minDiff也不会变了，所以循环结束。最后记得跟之前找到的所有minDiff比较一下大小，返回结果。

T:O(Nlog(M)log(N)), M是数组里最大的值，log(M)是因为要一直除2直到成为基数。worst case我们n个数都要这样处理，所以要✖N。后面一个log(N)是堆调整的操作。S:O(N) heap的大小

```java
public int minimumDeviation(int[] nums) {
    if (nums == null || nums.length == 0) {
        return Integer.MAX_VALUE;
    }

    PriorityQueue<Integer> evens = new PriorityQueue<>(nums.length, Collections.reverseOrder());
    int min = Integer.MAX_VALUE;
    // 全丢maxheap里，顺便记录当前最小的数是啥
    for (int num : nums) {
        if (num % 2 == 0) {
            evens.offer(num);
            min = Math.min(num, min);
        } else {
            evens.offer(num * 2);
            min = Math.min(num * 2, min);
        }            
    }
    // loop着找minDiff，如果是奇数了我们就知道已经找完了
    int res = Integer.MAX_VALUE;
    while (evens.peek() % 2 == 0) {
        int max = evens.poll();
        res = Math.min(res, max - min);
        int newNum = max / 2; //除以2，丢回去看会不会找到更小的
        evens.offer(newNum);
        min = Math.min(min, newNum); // 记得更新一下min
    }

    res = Math.min(evens.peek() - min, res);
    return res;
}
```
