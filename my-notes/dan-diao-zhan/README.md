# 33 单调栈

这是一种栈的诡异用法。当你需要知道当前值左/右边第一个比它大/小的值时，可以用这个栈。模板大概长这个样子：

```
monoStack = new Stack();

for (i 0 ~ len) {
    while (!stack.isEmpty() && curVal < stack.peek()) {
        stack.pop()
    }

    stack.push(curVal)
}
```

[L126 Max Tree](../26-tree/l126-max-tree.md)

[84 Largest Rectangle in Histogram](84-largest-rectangle-in-histogram.md) (L122) ctci 17.21 -- relate：[ 85 Maximal Rectangle](../22-dp/85-maximal-rectangle.md)

[255 Verify Preorder Sequence in BST](https://mameko.gitbooks.io/pan-s-algorithm-notes/content/26-tree/255-verify-preorder-sequence-in-bst.html)

[449 Serialize and Deserialize BST](https://mameko.gitbooks.io/pan-s-algorithm-notes/content/26-tree/449-serialize-and-deserialize-bst.html)- deserialize的非递归版本要用单调栈来把复杂度降低到O(n)

[402 Remove K Digits](402-remove-k-digits.md)

[316 Remove Duplicate Letters](316-remove-duplicate-letters.md)

[255 Verify Preorder Sequence in BST](./)

[739 Daily Temperatures](739-daily-temperatures.md)

[1673 Find the Most Competitive Subsequence](1673-find-the-most-competitive-subsequence.md)
