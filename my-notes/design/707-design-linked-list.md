# 707 Design Linked List

Design your implementation of the linked list. You can choose to use the singly linked list or the doubly linked list. A node in a singly linked list should have two attributes:`val` and`next`.`val`is the value of the current node, and`next` is a pointer/reference to the next node. If you want to use the doubly linked list, you will need one more attribute`prev`to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement these functions in your linked list class:

* get(index) : Get the value of the`index`-th node in the linked list. If the index is invalid, return`-1`.
* addAtHead(val) : Add a node of value`val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
* addAtTail(val) : Append a node of value`val` to the last element of the linked list.
* addAtIndex(index, val) : Add a node of value`val` before the`index`-th node in the linked list. If`index` equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
* deleteAtIndex(index) : Delete the`index`-th node in the linked list, if the index is valid.

**Example:**

```
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1, 2);  // linked list becomes 1->2->3
linkedList.get(1);            // returns 2
linkedList.deleteAtIndex(1);  // now the linked list is 1->3
linkedList.get(1);            // returns 3
```

**Note:**

* All values will be in the range of`[1, 1000]`.
* The number of operations will be in the range of `[1, 1000]`.
* Please do not use the built-in LinkedList library.

single的。感觉代码还可以refactor一下。

```java
class MyLinkedList {
    ListNode dummy;
    int length;

    /** Initialize your data structure here. */
    public MyLinkedList() {
        dummy = new ListNode(-1);
        length = 0;
    }

    /** Get the value of the index-th node in the linked list. 
        If the index is invalid, return -1. */
    public int get(int index) {
        if (index < 0 || index >= length) {
            return -1;
        }

        ListNode res = dummy.next;
        while (index > 0) {
            index--;
            res = res.next;
        }

        return res.val;
    }

    /** Add a node of value val before the first element of the linked list. 
    After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = dummy.next;
        dummy.next = newNode;
        length++;
    }

    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        ListNode tail = dummy;
        int index = length;
        while (index > 0) {
            index--;
            tail = tail.next;
        }

        ListNode newNode = new ListNode(val);
        newNode.next = tail.next;
        tail.next = newNode;
        length++;
    }

    /** Add a node of value val before the index-th node in the linked list. 
    If index equals to the length of linked list, the node will be appended to the end of linked list. 
    If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > length) {
            return;
        }

        ListNode res = dummy;
        while (index > 0) {
            index--;
            res = res.next;
        }

        ListNode newNode = new ListNode(val);
        newNode.next = res.next;
        res.next = newNode;
        length++;
    }

    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= length) {
            return;
        }

        ListNode pre = dummy;
        while (index > 0) {
            index--;
            pre = pre.next;
        }

        pre.next = pre.next.next;        
        length--;
    }
}

class ListNode {
    ListNode next;
    int val;

    public ListNode(int v) {
        val = v;
        next = null;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

double的。感觉代码还可以refactor一下。

```java
class MyLinkedList {
    DListNode dummy;
    DListNode tail;
    int length;

    /** Initialize your data structure here. */
    public MyLinkedList() {
        dummy = new DListNode(-1);
        tail = new DListNode(-1);
        tail.prev = dummy;
        dummy.next = tail;
        length = 0;
    }

    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        if (index < 0 || index >= length) {
            return -1;
        }

        DListNode res = dummy.next;
        while (index > 0) {
            index--;
            res = res.next;
        }

        return res.val;
    }

    /** Add a node of value val before the first element of the linked list. After the insertion, 
        the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        DListNode newNode = new DListNode(val);
        newNode.next = dummy.next;
        newNode.prev = dummy;        
        dummy.next.prev = newNode;
        newNode.prev.next = newNode;
        length++;
    }

    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        DListNode newNode = new DListNode(val);
        newNode.prev = tail.prev;
        newNode.next = tail;
        newNode.prev.next = newNode;
        tail.prev = newNode;
        length++;
    }

    /** Add a node of value val before the index-th node in the linked list. 
        If index equals to the length of linked list, the node will be appended to the end of linked list. 
        If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > length) {
            return;
        }

        DListNode res = dummy;
        while (index > 0) {
            index--;
            res = res.next;
        }

        DListNode newNode = new DListNode(val);
        newNode.next = res.next;
        newNode.prev = res;
        newNode.prev.next = newNode;
        newNode.next.prev = newNode;        
        length++;
    }

    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= length) {
            return;
        }

        DListNode pre = dummy;
        while (index > 0) {
            index--;
            pre = pre.next;
        }

        pre.next = pre.next.next;
        pre.next.prev = pre;
        length--;
    }
}

class DListNode {
    DListNode next;
    DListNode prev;
    int val;

    public DListNode(int v) {
        val = v;
        next = null;
        prev = null;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```
