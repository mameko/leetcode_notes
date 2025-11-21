# 623 Add One Row to Tree

Given the root of a binary tree, then value `v` and depth `d`, you need to add a row of nodes with value `v` at the given depth `d`. The root node is at depth 1.

The adding rule is: given a positive integer depth `d`, for each NOT null tree nodes `N` in depth `d-1`, create two tree nodes with value `v` as `N's` left subtree root and right subtree root. And `N's` **original left subtree** should be the left subtree of the new left subtree root, its **original right subtree** should be the right subtree of the new right subtree root. If depth `d` is 1 that means there is no depth d-1 at all, then create a tree node with value **v** as the new root of the whole original tree, and the original tree is the new root's left subtree.

**Example 1:**

```
Input: 
A binary tree as following:
       4
     /   \
    2     6
   / \   / 
  3   1 5   

v = 1

d = 2

Output: 
       4
      / \
     1   1
    /     \
   2       6
  / \     / 
 3   1   5   

```

**Example 2:**

```
Input: 
A binary tree as following:
      4
     /   
    2    
   / \   
  3   1    

v = 1

d = 3

Output: 
      4
     /   
    2
   / \    
  1   1
 /     \  
3       1
```

**Note:**

1. The given d is in range \[1, maximum depth of the given tree + 1].
2. The given binary tree has at least one tree node.

4年之后，竟然做不出来...之前还不屑写笔记。其实还是能看出bfs。但如果insert到最后一行的时候，会加不上去因为叶子节点没有下一层。一看答案，原来是分两部分。先判断掉在根那一层加的。然后下面bfs，如果到了深度，我们就直接加上new node，不用入队，因为我们处理完这一层就ok了。记得先把level＋1，因为leve == d - 1好像最后一层不好处理。T:O(N), S:O(最宽一层宽度）

```java
public TreeNode addOneRow(TreeNode root, int v, int d) {
    if (root == null || d < 1) {
        return null;
    }

    if (d == 1) {
        TreeNode newRoot = new TreeNode(v);
        newRoot.left = root;
        return newRoot;
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    int level = 1;
    while (!queue.isEmpty()) {
        int size = queue.size();
        // one level
        level++;
        for (int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();

            if (d == level) {
                TreeNode newLeft = new TreeNode(v);    
                newLeft.left = cur.left;
                cur.left = newLeft;
                TreeNode newRight = new TreeNode(v);    
                newRight.right = cur.right;
                cur.right = newRight;
            } else {

                if (cur.left != null) {
                    queue.offer(cur.left);
                }

                if (cur.right != null) {
                    queue.offer(cur.right);
                }
            }
        }
    }

    return root;
}
```
