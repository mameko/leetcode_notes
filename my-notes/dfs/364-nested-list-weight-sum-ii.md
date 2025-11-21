# 364 Nested List Weight Sum II

Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Different from the [previous question](https://leetcode.com/problems/nested-list-weight-sum/) where weight is increasing from root to leaf, now the weight is defined from bottom up. i.e., the leaf level integers have weight 1, and the root level integers have the largest weight.

**Example 1:**\
Given the list`[[1,1],2,[1,1]]`, return**8**. (four 1's at depth 1, one 2 at depth 2)

**Example 2:**\
Given the list`[1,[4,[6]]]`, return**17**. (one 1 at depth 3, one 4 at depth 2, and one 6 at depth 1; 1\*3 + 4\*2 + 6\*1 = 17)

2刷的时候发现了一个更容易理解的解法，用BFS，一层一层往外，基本思路还是一样，要把里面层的记下来然后每找一层都得多加一遍。

```java
public int depthSumInverse(List<NestedInteger> nestedList) {
    if (nestedList == null || nestedList.size() == 0) {
        return 0;
    }

    Queue<NestedInteger> queue = new LinkedList<>();
    for (NestedInteger nestedInt : nestedList) {
        queue.offer(nestedInt);
    }

    int pre = 0;
    int total = 0;
    while (!queue.isEmpty()) {
        int size = queue.size();
        int levelSum = 0;
        for (int i = 0; i < size; i++) {
            NestedInteger cur = queue.poll();
            if (cur.isInteger()) {
                levelSum += cur.getInteger();
            } else {
                queue.addAll(cur.getList());
            }
        }
        pre += levelSum;
        total += pre;
    }

    return total;
}
```

这题感觉看n遍都不是特别懂。自己一定想不出来。每次用一个unweighted的把一层的加上，然后weighted的来记录总数。那个weighted的会一直累加之前每一层的，所以每一层就会被加层数那么多次，所以得到正确答案。这里，是从最外面的一层开始算，如果最大深度是3层的话，第一次加到weighted里的数，就会被多加2次。

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class Solution {
    public int depthSumInverse(List<NestedInteger> nestedList) {
        if (nestedList == null || nestedList.size() == 0) {
            return 0;
        }

        int weighted = 0;
        int unWeighted = 0;
        while (!nestedList.isEmpty()) {
            List<NestedInteger> next = new ArrayList<>();
            for (NestedInteger ni : nestedList) {
                if (ni.isInteger()) {
                    unWeighted += ni.getInteger();
                } else {
                    next.addAll(ni.getList());
                }

            }
            weighted += unWeighted;
            nestedList = next;
        }
        return weighted;
    }
}
```
