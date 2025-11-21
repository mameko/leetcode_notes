# 708 Insert into a Cyclic Sorted List

Given a node from a cyclic linked list which is sorted in ascending order, write a function to insert a value into the list such that it remains a cyclic sorted list. The given node can be a reference to\_any\_single node in the list, and may not be necessarily the smallest value in the cyclic list.

If there are multiple suitable places for insertion, you may choose any place to insert the new value. After the insertion, the cyclic list should remain sorted.

If the list is empty (i.e., given node is`null`), you should create a new single cyclic list and return the reference to that single node. Otherwise, you should return the original given node.

The following example may help you understand the problem better:

![](https://assets.leetcode.com/uploads/2018/10/12/insertcyclicbefore.png)

In the figure above, there is a cyclic sorted list of three elements. You are given a reference to the node with value 3, and we need to insert 2 into the list.

![](https://assets.leetcode.com/uploads/2018/10/12/insertcyclicafter.png)

The new node should insert between node 1 and node 3. After the insertion, the list should look like this, and we should still return node 3.

这题的边界条件真是让人痛心疾首。 主要是怎么判断我们已经找了一圈，这里我用的是判断next是否已经又指向head了。然后另外一个难点是判断插入地方，第一个条件容易找，在两数之间插。第二个条件是，比所有数大/小，这个要插在list头尾相接的地方。最后还有一种情况是list里全部数字都一样。这样的话，我们走一圈，插在最后head前。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node next;

    public Node() {}

    public Node(int _val,Node _next) {
        val = _val;
        next = _next;
    }
};
*/
class Solution {
    public Node insert(Node head, int insertVal) {
        if (head == null) {
            Node newNode = new Node();
            newNode.val = insertVal;
            newNode.next = newNode;
            return newNode;
        }

        Node cur = head;
        Node newNode = new Node();
        newNode.val = insertVal;
        Node next = cur.next;
        boolean inserted = false;

        while (cur.next != head) {
            next = cur.next;
            if (insertVal <= next.val && insertVal >= cur.val || // 正常情况
                (cur.val > next.val && (insertVal >= cur.val || insertVal <= next.val))) { // 比所有数大/小
                cur.next = newNode;
                newNode.next = next;                
                inserted = true;
                break;
            }
            cur = cur.next;
        }

        if (!inserted) { // list里的数都一样
            next = cur.next;
            cur.next = newNode;
            newNode.next = next;
        }

        return head;
    }
}
```
