# 297 Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following tree

```
    1
   / \
  2   3
     / \
    4   5
```

as`"[1,2,3,null,null,4,5]"`, just the same as[how LeetCode OJ serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Note:**&#x44;o not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

就是做了个BFS，感觉比较浪费space。T:O(n), S:O(biggest level width)

```java
// Encodes a tree to a single string.
public String serialize(TreeNode root) {
    if (root == null) {
        return "#";
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    StringBuilder sb = new StringBuilder();
    while (!queue.isEmpty()) {
        TreeNode cur = queue.poll();
        if (cur != null) {
            sb.append(String.valueOf(cur.val));// 可以直接append
            queue.offer(cur.left);
            queue.offer(cur.right);
        } else {
            sb.append("#");
        }
        sb.append(",");
    }

    sb = sb.deleteCharAt(sb.length() - 1);// 可有可无
    return sb.toString();
}

// Decodes your encoded data to tree.
public TreeNode deserialize(String data) {
    if (data == null || data.length() == 0) {
        return null;
    }

    String[] ds = data.split(",");
    if (ds[0].equals("#")) {
        return null;
    }
    int n = ds.length;
    Queue<TreeNode> queue = new LinkedList<>();
    TreeNode root = new TreeNode(Integer.valueOf(ds[0]));
    queue.offer(root);
    int i = 1;
    while (!queue.isEmpty()) {
        TreeNode cur = queue.poll();

        if (i < n && !ds[i].equals("#")) {
            cur.left = new TreeNode(Integer.valueOf(ds[i]));
            queue.offer(cur.left);
        }

        i++;// ++ 一定要在外面
        if (i < n && !ds[i].equals("#")) {
            cur.right = new TreeNode(Integer.valueOf(ds[i]));
            queue.offer(cur.right);
        }
        i++;// ++ 一定要在外面

    }

    return root;

}
```

下面是递归版本：T:O(n), S(height)，递归深度是height，worst case O(n)

```java
// Encodes a tree to a single string.
public String serialize(TreeNode root) {
    if (root == null) {
        return "#";
    }

    StringBuilder sb = new StringBuilder();
    recurHelper(root, sb);
    return sb.toString();
}

private void recurHelper(TreeNode root, StringBuilder sb) {
    if (root == null) {
        sb.append("#,");
        return;
    }

    sb.append(root.val);
    sb.append(",");

    recurHelper(root.left, sb);
    recurHelper(root.right, sb);
}

int index = 0;
// Decodes your encoded data to tree.
public TreeNode deserialize(String data) {
    if (data == null || data.length() == 0) {
        return null;
    }

    String[] nodes = data.split(",");
    return buildTree(nodes);
}

private TreeNode buildTree(String[] nodes) {
    if (index >= nodes.length || nodes[index].equals("#")) {
        index++;
        return null;
    }

    TreeNode root = new TreeNode(Integer.valueOf(nodes[index]));
    index++;

    root.left = buildTree(nodes);
    root.right = buildTree(nodes);

    return root;
}
```
