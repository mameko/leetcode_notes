# 206 Reverse Linked List

Reverse a singly linked list.

**Hint:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

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

递归版本：

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
