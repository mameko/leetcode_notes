# 3 树的模板

**前序:** 144 Binary Tree Preorder Traversal(L66)

```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode cur = stack.pop();
        res.add(cur.val);

        if (cur.right != null) {
            stack.push(cur.right);
        }

        if (cur.left != null) {
            stack.push(cur.left);
        }
    }

    return res;
}

// 递归, traverse， 自己一个人拿着个小本子，每到一个地方，把数据记下来。
// 所以不用返回值，带着本子就行（result）
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) {
        return result;
    }    
    helper(root);
    return result;
}
//1. 递归的入口，把root放到result里
void helper(TreeNode root) {
    // 3. 递归的出口
    if (root == null) {
        return;
    }

    // 2. 递归的拆分
    result.add(root.val);    
    helper(root.left);  
    helper(root.right);   
}

// 递归，Divide & Qonquer, 领导，派2个小弟去把子问题解决，小弟返回以后，
// 领导结合自己的结果再向上汇报，所以D&Q是会return值的
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) {
        return result;
    }

    //Divide
    ArrayList<Integer> left = postorderTraversal(root.left);    
    ArrayList<Integer> right = postorderTraversal(root.right);

    // Conquer
    result.add(root.val);
    result.add(left);
    result.add(right);

    return result;
}
```

**中序:** 94 Binary Tree Inorder Traversal (L67)

<pre class="language-java"><code class="lang-java"><strong>// 听说是更通用的模板，用这个来做iterator那题
</strong><strong>// 可以通过把right改成left，变成prev()的iterator
</strong><strong>// 这个prev()可以用在 Closest Binary Search Tree Value II里
</strong>public ArrayList&#x3C;Integer> inorderTraversal(TreeNode root) {
    Stack&#x3C;TreeNode> stack = new Stack&#x3C;>();
    ArrayList&#x3C;Integer> result = new ArrayList&#x3C;>();
    
    while (root != null) {
        stack.push(root);
        root = root.left;
    }

    while (!stack.isEmpty()) {
        TreeNode node = stack.peek();
        result.add(node.val);
        
        if (node.right == null) {
            node = stack.pop();
            while (!stack.isEmpty() &#x26;&#x26; stack.peek().right == node) {
                node = stack.pop();
            }
        } else {
            node = node.right;
            while (node != null) {
                stack.push(node);
                node = node.left;
            }
        }
    }
    return result;
}
<strong>
</strong><strong>public List&#x3C;Integer> inorderTraversal(TreeNode root) {
</strong>    List&#x3C;Integer> res = new ArrayList&#x3C;>();
    TreeNode cur = root;
    Stack&#x3C;TreeNode> stack = new Stack&#x3C;>();

    while (cur != null || !stack.isEmpty()) {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }

        cur = stack.pop();
        res.add(cur.val);
        cur = cur.right;
    }

    return res;
}

// 递归
List&#x3C;Integer> result = new ArrayList&#x3C;>();
public List&#x3C;Integer> postorderTraversal(TreeNode root) {
    if (root == null) {
        return result;
    }

    helper(root);
    return result;
}

void helper(TreeNode root) {
    if (root == null) {
        return;
    }

    if (root.left != null) {
        helper(root.left);
    }
    result.add(root.val);
    if (root.right != null) {
        helper(root.right);
    }    
}
</code></pre>

**后序 :** 145 Binary Tree Postorder Traversal (L68)

```java
public ArrayList<Integer> postorderTraversal(TreeNode root) {
    ArrayList<Integer> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    Stack<Pair> stack = new Stack<>();
    stack.push(new Pair(root, false));
    while (!stack.isEmpty()) {
        Pair p = stack.pop();
        if (p.node == null) {
            continue;
        }
        if (p.visited) {
            res.add(p.node.val);
        } else {
            stack.push(new Pair(p.node, true));
            stack.push(new Pair(p.node.right, false));
            stack.push(new Pair(p.node.left, false));
        }
    }

    return res;
}


class Pair {
    TreeNode node;
    boolean visited;

    public Pair(TreeNode n, boolean v) {
        node = n;
        visited = v;
    }
}

// 递归
List<Integer> result = new ArrayList<>();
public List<Integer> postorderTraversal(TreeNode root) {
    if (root == null) {
        return result;
    }

    helper(root);
    return result;
}

void helper(TreeNode root) {
    if (root == null) {
        return;
    }

    if (root.left != null) {
        helper(root.left);
    }
    if (root.right != null) {
        helper(root.right);
    }
    result.add(root.val);
}
```

**inorder successor :** 285 Inorder Successor in BST (L448)

```java
// 迭代
public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    TreeNode suc = null;
    while (root != null && p != root) {// find location of p and record posible suc from top
        if (root.val > p.val) {
            suc = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }

    if (root == null) {// p not in tree
        return null;
    }

    if (root.right == null) {// p's right sub tree not exist, so suc must be from top
        return suc;
    }

    root = root.right;
    while (root.left != null) {// p's right subtree exsit, suc is the left most node in right subtree
        root = root.left;
    }

    return root;
}

// 递归
public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    if (root == null || p == null) {
        return null;
    }

    if (p.val >= root.val) { // 当要找的大于等于跟，我们去右边找
        return inorderSuccessor(root.right, p); 
    } else {// 如果不是的话，那么在左边找
       TreeNode found = inorderSuccessor(root.left, p);
       return found == null ? root : found;// 如果在左边找到就返回，找不到证明根就是successor
    }
}
```

**BST插入：**&#x4C;85 Insert Node in a BST

```java
// 递归 T:O(H), S:O(H), avg H=logn, worst case H=n
public TreeNode insertNode(TreeNode root, TreeNode node) {
    if (root == null) {
        return node;
    }

    if (node.val < root.val) {
        root.left = insertNode(root.left, node);
    } else {
        root.right = insertNode(root.right, node);
    }

    return root;
}

// 迭代, T:O(H), S:O(1), 因为没有递归栈
public TreeNode insertIntoBST(TreeNode root, int val) {
    TreeNode curNode = root;
    while (curNode != null) {
        if (curNode.val > val) { // 插入左边
            if (curNode.left == null) { // 如果已经到叶子了，直接插入
                TreeNode node = new TreeNode(val);
                curNode.left = node;
                return root;
            }
            // 还有左子树，所以走下去
            curNode = curNode.left;
        } else { // 插入右边
            if (curNode.right == null) { // 如果已经到叶子了，直接插入
                TreeNode node = new TreeNode(val);
                curNode.right = node;
                return root;
            }
            // 还有右子树，所以走下去
            curNode = curNode.right;
        }
    }
    
    return new TreeNode(val);
}
```

**BST删除：**&#x34;50 Delete Node in a BST (L87) T:O(n), S:O(height)

```java
public TreeNode removeNode(TreeNode root, int value) {

    if (root == null) {
        return root;
    }

    if (value < root.val) {
        root.left = removeNode(root.left, value);
    } else if (value > root.val) {
        root.right = removeNode(root.right, value);
    } else {
        // the node need to be deleted may be 3 types of node
        // case 1 : leave node, just delete it
        if (root.left == null && root.right == null) {
            return null;
            // case 2 : has single child, replace node with it's child
        } else if (root.left == null && root.right != null) {
            return root.right;
        } else if (root.left != null && root.right == null) {
            return root.left;
        } else {
            // case 3 : have 2 children, need to replace with inorder succesor 
            // then recursively delete child
            TreeNode suc = findSuc(root, value);
            root.val = suc.val;
            // because root has both child, so successor must in the right 
            // subtree
            root.right = removeNode(root.right, suc.val);
        }
    }
    return root;
}

private TreeNode findSuc(TreeNode root, int target) {
    TreeNode suc = null;
    while (root != null && root.val != target) {
        if (root.val > target) {
            suc = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }

    if (root == null) {// target is not in the tree
        return null;
    }

    if (root.right == null) {// if doesn't have right subtree, return successor
        return suc;
    }

    root = root.right;// suc is left most element in right subtree
    while (root.left != null) {
        root = root.left;
    }
    return root;
}
```
