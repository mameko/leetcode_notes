# L106 Convert Sorted List to Balanced BST

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

**Example**

```
               2
1->2->3  =>   / \
             1   3
```

首先找到end和start，然后找中点，递归调用函数构造左右子树。九章的答案是每次用一个函数来数现在这段list的size是否>0，如果是的话，递归构造左右子树。

```java
public TreeNode sortedListToBST(ListNode head) {
    if (head == null) {
        return null;
    }

    ListNode end = head;
    while (end.next != null) {
        end = end.next;
    }

    TreeNode root = helper(head, end);

    return root;
}

private TreeNode helper(ListNode start, ListNode end) {
    if (start.val > end.val) {
        return null;
    }

    // find mid
    ListNode pre = start;
    ListNode slow = start;
    ListNode fast = start;

    while (fast != end && fast.next != end) {
        pre = slow;
        slow = slow.next;
        fast = fast.next.next;
    }

    // make a node
    TreeNode root = new TreeNode(slow.val);
    if (start == end) {
        return root;
    } 

    // make left node
    if (start != slow) {
        root.left = helper(start, pre);
    }

    // make right node
    if (slow != end) {
        root.right = helper(slow.next, end);
    }

    return root;
}
```
