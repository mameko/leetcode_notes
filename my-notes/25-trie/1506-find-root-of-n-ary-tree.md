# 1506 Find Root of N-Ary Tree

You are given all the nodes of an [**N-ary tree**](https://leetcode.com/articles/introduction-to-n-ary-trees/) as an array of `Node` objects, where each node has a **unique value**.

Return _the **root** of the N-ary tree_.

**Custom testing:**

An N-ary tree can be serialized as represented in its level order traversal where each group of children is separated by the `null` value (see examples).

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

For example, the above tree is serialized as `[1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]`

The testing will be done in the following way:

1. The **input data** should be provided as a serialization of the tree.
2. The driver code will construct the tree from the serialized input data and put each `Node` object into an array **in an arbitrary order**.
3. The driver code will pass the array to `findRoot`, and your function should find and return the root `Node` object in the array.
4. The driver code will take the returned `Node` object and serialize it. If the serialized value and the input data are the **same**, the test **passes**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
Input: tree = [1,null,3,2,4,null,5,6]
Output: [1,null,3,2,4,null,5,6]
Explanation: The tree from the input data is shown above.
The driver code creates the tree and gives findRoot the Node objects in an arbitrary order.
For example, the passed array could be [Node(5),Node(4),Node(3),Node(6),Node(2),Node(1)] or [Node(2),Node(6),Node(1),Node(3),Node(5),Node(4)].
The findRoot function should return the root Node(1), and the driver code will serialize it and compare with the input data.
The input data and serialized Node(1) are the same, so the test passes.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

```
Input: tree = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
```

**Constraints:**

* The total number of nodes is between `[1, 5 * 104]`.
* Each node has a **unique** value.

**Follow up:**

* Could you solve this problem in constant space complexity with a linear time algorithm?

这题，看了半天，原来是把一棵树打，node的顺序给你，让你找root。一看，这不就是找indegree的题吗？然后快速把node loop一遍，用个hashmap来存。然鹅...不知道为毛test case从中间开始卡住了。最后打开了solution瞧了瞧。其实，这里不用真的去数indegree。因为这是树，不是图，indegree只会是1或0。我们直接用set来记录哪些见过的就是了。我们loop一圈把所有child加进去seen里。第二转找谁不在里面，那个就是root了，因为root不是谁的child。T:O(N), S:O(N). 这题因为是unique的value，所以follow up可以用“抵消”的思想做，类似于[136 Single Number](../31-bit-manipulation/136-single-number.md)的思想，或者是[266 Palindrome Permuation](../palindrome/266-palindrome-permuation.md)的解法二。

```java
public Node findRoot(List<Node> tree) {
    if (tree == null || tree.size() == 0) {
        return null;
    }

    Set<Node> seen = new HashSet<>();
    for (Node node : tree) {
        for (Node child : node.children) {
            seen.add(child);
        }
    }

    for (Node node : tree) {
        if (!seen.contains(node)) {
            return node;
        }
    }

    return null;
}
```

解法二：T:O(N), S:O(1) 因为时间关系，就把solution直接搬下来了

```java
public Node findRoot(List<Node> tree) {

    Integer valueSum = 0;

    for (Node node : tree) {
        // the value is added as a parent node
        valueSum += node.val;
        for (Node child : node.children)
            // the value is deducted as a child node.
            valueSum -= child.val;
    }

    Node root = null;
    // the value of the root node is `valueSum`
    for (Node node : tree) {
        if (node.val == valueSum) {
            root = node;
            break;
        }
    }
    return root;
}
```
