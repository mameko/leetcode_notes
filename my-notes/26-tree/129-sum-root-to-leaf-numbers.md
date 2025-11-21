# 129 Sum Root to Leaf Numbers

Given a binary tree containing digits from`0-9`only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path`1->2->3`which represents the number`123`.

Find the total sum of all root-to-leaf numbers.

For example,

```
    1
   / \
  2   3
```

The root-to-leaf path`1->2`represents the number`12`.\
The root-to-leaf path`1->3`represents the number`13`.

Return the sum = 12 + 13 =`25`.

这题应该是假设了从root到leaf的数字不会超出int，不然我的方法可能不过。做完对个答案又被大神们的代码给秒了。

```java
int sum = 0;
public int sumNumbers(TreeNode root) {
    if (root == null) {
        return sum;
    }

    helper(new StringBuilder(), root);

    return sum;
}

private void helper(StringBuilder tmp, TreeNode root) {
    if (root == null) {
        return;
    }

    tmp.append(root.val);
    if (root.left == null && root.right == null) {
        sum += Integer.valueOf(tmp.toString());
        tmp.deleteCharAt(tmp.length() - 1);
        return;
    }

    helper(tmp, root.left);
    helper(tmp, root.right);

    tmp.deleteCharAt(tmp.length() - 1);
}
```

某大神的代码：

```java
public int sumNumbers(TreeNode root) {
    return sum(root, 0);
}

public int sum(TreeNode n, int s){
    if (n == null) return 0;
    if (n.right == null && n.left == null) return s*10 + n.val;
    return sum(n.left, s*10 + n.val) + sum(n.right, s*10 + n.val);
}
```
