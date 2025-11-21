# 4 链表基础模板

找中点：

## L228 Middle of Linked List

Find the middle node of a linked list.

Example

Given 1->2->3, return the node with value 2.

Given 1->2, return the node with value 1.

```java
public ListNode middleNode(ListNode head) { 
    if (head == null || head.next == null) {
        return head;
    }

    ListNode slow = head, fast = head.next;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}
```

reverse：

## 206 Reverse Linked List

Reverse a singly linked list.

**Hint:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

迭代版本：T:O(n)，S:O(1)

```java
public ListNode reverseList(ListNode head) {
    if (head == null) {
        return head;
    }
    ListNode pre = null;
    while (head != null) {
        ListNode tmp = head.next;
        head.next = pre;
        pre = head;
        head = tmp;
    }

    return pre;
}
```

递归版本：T:O(n)，S:O(n)--递归栈的深度为n

```java
public ListNode reverseList(ListNode head) {

    if (head == null || head.next == null) {
        return head;
    }

    ListNode second = head.next;
    head.next = null;
    ListNode res = reverseLinkedList(second);
    second.next = head;
    return res;
}
```

另一种写法：

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
```
