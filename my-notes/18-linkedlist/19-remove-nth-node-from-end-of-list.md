# 19 Remove Nth Node From End of List

Given a linked list, remove thenthnode from the end of list and return its head.

For example,

```
   Given linked list: 1->2->3->4->5, and n = 2.
   After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**\
Given n will always be valid. Try to do this in one pass.

我们就从头开始把fast指针移动n位。边移动边判断是否已经超多了长度n。然后把2个指针一齐移动。slow初始化为要删除点的前一个点，所以最后通过改变next指针来删除node。

```java
ListNode removeNthFromEnd(ListNode head, int n) {
    if (head == null) {
        return null;
    }

    ListNode dummy = new ListNode(-1);
    dummy.next = head;

    ListNode slow = dummy; // one before the node get deleted
    ListNode fast = head;

    // move fast n step earlier than slow
    while (n > 0) {
        fast = fast.next;
        n--;
    }

    // move them together, while fast == null, slow will be on the nth node
    while (fast != null) {
        fast = fast.next;
        slow = slow.next;
    }

    slow.next = slow.next.next;
    return dummy.next;
}
```
