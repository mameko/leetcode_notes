# 449 Serialize and Deserialize BST

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a**binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

**Note:**&#x44;o not use class member/global/static variables to store states. Your serialize and deseriialize algorithms should be stateless.

这题完全可以用前面那题Serialize Binary Tree 来做，但这里是BSt而且要求省空间。对于BST来说，pre/post order Tranvesal就能确定一棵树。所以我们可以只存一个preorder tranversal不用存null指针的位置。感觉deserialization的代码跟L126 [max tre](l126-max-tree.md)e的代码有点像，都用单调栈。另外那个[preorder sequence](255-verify-preorder-sequence-in-bst.md)也要用单调栈。突然感觉BST preorder跟单调栈很有关系。By the way，如果是满2叉树（heap）的话，可以直接用数组来存。

```java
// Encodes a tree to a single string.
public String serialize(TreeNode root) {
    StringBuilder sb = new StringBuilder();

    serHelper(root, sb);

    return sb.toString();
}

//递归前序遍历
private void serHelper(TreeNode root, StringBuilder sb) {
    if (root == null) {
        return;
    }

    sb.append(String.valueOf(root.val));
    sb.append(",");

    if (root.left != null) {
        serHelper(root.left, sb);
    }

    if (root.right != null){
        serHelper(root.right, sb);
    }
}

// Decodes your encoded data to tree.
public TreeNode deserialize(String data) {
    if (data == null || data.length() == 0) {
        return null;
    }

    String[] nodes = data.split(",");
    TreeNode root = new TreeNode(Integer.valueOf(nodes[0]));

    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    //用单调栈来写迭代版本的O(n)造树
    for (int i = 1; i < nodes.length; i++) {
        TreeNode cur = null;
        int nodeVal = Integer.valueOf(nodes[i]);

        while (!stack.isEmpty() && nodeVal > stack.peek().val) {
            cur = stack.pop();
        }

        TreeNode newNode = new TreeNode(nodeVal);

        if (cur != null) {
            cur.right = newNode;
        } else {
            stack.peek().left = newNode;
        }
        stack.push(newNode);
    }

    return root;
}
```

另一种递归建树方法：利用了[98 valid BST](98-valid-binary-search-tree.md)的性质

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();

        serHelper(root, sb);

        return sb.toString();
    }

    private void serHelper(TreeNode root, StringBuilder sb) {
        if (root == null) {
            return;
        }

        sb.append(String.valueOf(root.val));
        sb.append(",");

        if (root.left != null) {
            serHelper(root.left, sb);
        }

        if (root.right != null){
            serHelper(root.right, sb);
        }
    }

    int index = 0;
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data == null || data.length() == 0) {
            return null;
        }

        String[] nodes = data.split(",");
        return buildTree(nodes, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }

    private TreeNode buildTree(String[] nodes, Integer min, Integer max) {
        // used up all the node to build tree
        if (index >= nodes.length) {
            return null;
        }

        TreeNode root = null;
        Integer val = Integer.valueOf(nodes[index]);
        if (val > min && val < max) {
            root = new TreeNode(val);
            index++;
            root.left = buildTree(nodes, min, val);
            root.right = buildTree(nodes, val, max);
        }

        return root;
    }
}
```
