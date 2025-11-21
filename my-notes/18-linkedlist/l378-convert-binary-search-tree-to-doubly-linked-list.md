# L378 Convert Binary Search Tree to Doubly Linked List

Convert a binary search tree to doubly linked list with in-order traversal.

**Example**

Given a binary search tree:

```
    4
   / \
  2   5
 / \
1   3
```

return`1<->2<->3<->4<->5`

非递归中序遍历，用formmer和latter作为前面访问过的点和现在访问的点。用来连接doubly linked list的前后指针。

```java
public DoublyListNode bstToDoublyList(TreeNode root) {  
    if (root == null) {
        return null;
    }

    DoublyListNode dummy = new DoublyListNode(-1);
    DoublyListNode formmer = null;
    DoublyListNode latter = null;


    Stack<TreeNode> stack = new Stack<>();
    TreeNode cur = root;
    while (cur != null || !stack.isEmpty()) {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }

        TreeNode tmp = stack.pop();
        cur = tmp.right;

        if (formmer == null) {
            formmer = new DoublyListNode(tmp.val);
            dummy.next = formmer;
            continue;
        } else {
            latter = new DoublyListNode(tmp.val);
        }

        formmer.next = latter;
        latter.prev = formmer;
        formmer = latter;
    }

    return dummy.next;
}
```
