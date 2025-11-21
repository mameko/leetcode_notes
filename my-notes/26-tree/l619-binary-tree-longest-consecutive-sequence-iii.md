# L619 Binary Tree Longest Consecutive Sequence III

It's follow up problem for[`Binary Tree Longest Consecutive Sequence II`](http://www.lintcode.com/en/problem/binary-tree-longest-consecutive-sequence-ii/)

Given a`k-ary tree`, find the length of the longest consecutive sequence path.\
The path could be start and end at any node in the tree

**Example**

An example of test data: k-ary tree`5<6<7<>,5<>,8<>>,4<3<>,5<>,3<>>>`, denote the following structure:

```
     5
   /   \
  6     4
 /|\   /|\
7 5 8 3 5 3

Return 5, // 3-4-5-6-7
```

跟614的结构基本一样，还是把这层的孩子节点全求一遍，然后统计up/down的max。最后加起来跟孩子的max比较。同理up和down不会在同一branch取得up max & down max，所以只要加起来就ok了。感觉这个childMaxLen也可以初始化为0。

```java
   public int longestConsecutive3(MultiTreeNode root) {
        if (root == null) {
            return Integer.MIN_VALUE;
        }

        ResultType res = helper(root);

        return res.max;
    }

    private ResultType helper(MultiTreeNode root) {
        if (root == null) {
            return new ResultType(0, 0, 0);
        }

        int up = 0;
        int down = 0;
        int childMaxLen = 1;

        for (MultiTreeNode tn : root.children) {
            ResultType tmp = helper(tn);
            if (tn.val == root.val + 1) {
                up = Math.max(up, tmp.up + 1);
            }

            if (tn.val == root.val - 1) {
                down = Math.max(down, tmp.down + 1);
            }

            childMaxLen = Math.max(childMaxLen, tmp.max);
        }

        int len = up + down + 1;
        len = Math.max(len, childMaxLen);

        return new ResultType(up, down, len);
    }


class ResultType {
    int up;
    int down;
    int max;

    public ResultType(int u, int d, int m) {
        up = u;
        down = d;
        max = m;
    }
}
```
