# 545 Boundary of Binary Tree

Given a binary tree, return the values of its boundary in **anti-clockwise** direction starting from root. Boundary includes left boundary, leaves, and right boundary in order without duplicate **nodes**. (The values of the nodes may still be duplicates.)

**Left boundary** is defined as the path from root to the **left-most** node.**Right boundary** is defined as the path from root to the **right-most** node. If the root doesn't have left subtree or right subtree, then the root itself is left boundary or right boundary. Note this definition only applies to the input binary tree, and not applies to any subtrees.

The **left-most** node is defined as a **leaf** node you could reach when you always firstly travel to the left subtree if exists. If not, travel to the right subtree. Repeat until you reach a leaf node.

The **right-most** node is also defined by the same way with left and right exchanged.

**Example 1**

```
Input:
  1
   \
    2
   / \
  3   4

Ouput: [1, 3, 4, 2]

Explanation:
The root doesn't have left subtree, so the root itself is left boundary.
The leaves are node 3 and 4.
The right boundary are node 1,2,4. Note the anti-clockwise direction means you should output reversed right boundary.
So order them in anti-clockwise without duplicates and we have [1,3,4,2].
```

**Example 2**

```
Input:
    ____1_____
   /          \
  2            3
 / \          / 
4   5        6   
   / \      / \
  7   8    9  10  

Ouput:[1,2,4,7,8,9,10,6,3]

Explanation:
The left boundary are node 1,2,4. (4 is the left-most node according to definition)
The leaves are node 4,7,8,9,10.
The right boundary are node 1,3,6,10. (10 is the right-most node).
So order them in anti-clockwise without duplicate nodes we have [1,2,4,7,8,9,10,6,3].
```

这题软软面经里有，但是是clockwise的，不过其实方法不是特别难，就是分开4部分处理，root -> 左子树左边从上到下 -> 左子树的leave -> 右子树的leaves -> 右子树右边反向 。这题以为是打印所有的边，所以worst case会是T:O(n), 递归深度，S: O(n)

```java
// 照着geeksforgeeks抄了个递归的
public List<Integer> boundaryOfBinaryTree(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    res.add(root.val);
    printLeft(root.left, res);
    printLeaves(root.left, res);
    printLeaves(root.right, res);
    printRight(root.right, res);

    return res;
}

private void printLeft(TreeNode root, List<Integer> res) {        
    if (root == null) {
        return;
    }

    if (root.left != null) {            
        res.add(root.val);
        printLeft(root.left, res);
    } else if (root.right != null) {                        
        res.add(root.val);
        printLeft(root.right, res);
    }        
}

private void printLeaves(TreeNode root, List<Integer> res) {
    if (root == null) {
        return;
    }

    printLeaves(root.left, res);
    if (root.left == null && root.right == null) {
        res.add(root.val);
    }
    printLeaves(root.right, res);
}

private void printRight(TreeNode root, List<Integer> res) {
    if (root == null) {
        return;
    }

    if (root.right != null) {                        
        printRight(root.right, res);
        res.add(root.val);
    } else if (root.left != null) {                                    
        printRight(root.left, res);
        res.add(root.val);
    }
}

// leetcode上抄了个迭代的
public List<Integer> boundaryOfBinaryTree(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;
    res.add(root.val);
    if (root.left != null) {
        List<Integer> left = findLeft(root.left);
        res.addAll(left);
    }
    res.addAll(findLeaves(root));
    if (root.right != null) {
        List<Integer> right = findRight(root.right);
        Collections.reverse(right);
        res.addAll(right);
    }
    return res;
}

private List<Integer> findLeft(TreeNode node) {
    List<Integer> res = new ArrayList<>();
    while (node.left != null || node.right != null) {
        res.add(node.val);
        if (node.left != null) {
            node = node.left;
        } else if (node.right != null) {
            node = node.right;
        }
    }
    System.out.println("left " + res);
    return res;
}
private List<Integer> findRight(TreeNode node) {
    List<Integer> res = new ArrayList<>();
    while (node.left != null || node.right != null) {
        res.add(node.val);
        if (node.right != null) {
            node = node.right;
        } else if (node.left != null) {
            node = node.left;
        }
    }
    System.out.println("right " + res);
    return res;
}
private List<Integer> findLeaves(TreeNode node) {
    List<Integer> res = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    stack.push(node);
    while (!stack.isEmpty()) {
        TreeNode curr = stack.pop();
        if (curr.left == null && curr.right == null && curr != node) {
            res.add(curr.val);
        }
        if (curr.right != null) {
            stack.push(curr.right);
        }
        if (curr.left != null) {
            stack.add(curr.left);
        }
    }
    return res;
}
```
