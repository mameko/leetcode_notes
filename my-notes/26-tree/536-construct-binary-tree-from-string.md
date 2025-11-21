# 536 Construct Binary Tree from String

You need to construct a binary tree from a string consisting of parenthesis and integers.

The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.

You always start to construct the**left**child node of the parent first if it exists.

**Example:**

```
Input: "4(2(3)(1))(6(5))"
Output: return the tree root node representing the following tree:

       4
     /   \
    2     6
   / \   / 
  3   1 5
```

**Note:**

1. There will only be`'('`,`')'`,`'-'`and`'0'`\~`'9'`in the input string.
2. An empty tree is represented by`""`instead of`"()"`.

这题用了calculater的方法来边扫边算字符，所以决定pop的条件是看到两个连续的符号。这里assume输入的是valid的树。

```java
public TreeNode str2tree(String s) {
    if (s == null || s.length() == 0) {
        return null;
    }

    Stack<TreeNode> stack = new Stack<>();
    int sign = 1;
    int num = 0;
    for (int i = 0; i < s.length(); i++) {
        char cur = s.charAt(i);
        if (cur == '-') {
            sign = -1;
        } else if (Character.isDigit(cur)) {
            num = num * 10 + Character.getNumericValue(cur);
        } else {
            if (Character.isDigit(s.charAt(i - 1))) {
                int val = num * sign;
                num = 0;
                sign = 1;

                TreeNode newNode = new TreeNode(val);
                stack.push(newNode);
            } else {
                TreeNode pop = stack.pop();
                if (stack.peek().left == null) {
                    stack.peek().left = pop;
                } else {
                    stack.peek().right = pop;
                }
            }
        }
    }

    if (num != 0) {
        return new TreeNode(num * sign);
    }

    TreeNode pop = stack.pop();
    if (stack.peek().left == null) {
        stack.peek().left = pop;
    } else {
        stack.peek().right = pop;
    }

    return stack.pop();
}
```
