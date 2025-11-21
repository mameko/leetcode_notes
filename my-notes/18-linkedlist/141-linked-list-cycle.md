# 141 Linked List Cycle

Given a linked list, determine if it has a cycle in it.

Follow up:\
Can you solve it without using extra space?

这题需要extra space的做法是用hashset记录访问过的点。如果发现重复了就表明链表有环。这里用2 pointer来省空间。

```java
public boolean hasCycle(ListNode head) {
    if (head == null) {
        return false;
    }

    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }

    return false;
}
```
