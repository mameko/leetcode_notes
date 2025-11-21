# 255 Verify Preorder Sequence in BST

Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.

You may assume each number in the sequence is unique.

**Follow up:**\
Could you do it using only constant space complexity?

竟然是单调栈...用low来记录目前最小值，每次把找元素入单调栈里，如果比栈顶小，证明还在左子树，所以可以放上去；如果比栈顶大，证明我们已经到了右子树，或者ancestor的右子树。我们把栈pop到根节点的值，然后再往后看元素。这时，如果出现了比跟（low）还小的元素，证明这个preorder是不valid的。

```java
public boolean verifyPreorder(int[] preorder) {
    if (preorder == null || preorder.length == 0) {
        return true;
    }

    Stack<Integer> stack = new Stack<>();
    // use low to record current lowest value
    int low = Integer.MIN_VALUE;
    for (Integer p : preorder) {
        if (p < low) {
            return false;
        }
        // monotonic stack
        while (!stack.isEmpty() && p > stack.peek()) {
            low = stack.pop();
        }
        stack.push(p);
    }

    return true;
}
```
