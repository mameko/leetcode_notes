# 142 Linked List Cycle II

Given a linked list, return the node where the cycle begins. If there is no cycle, return`null`.

**Note:**&#x44;o not modify the linked list.

**Follow up**:\
Can you solve it without using extra space?

同上题一样，花空间的要用hashset。不用的话，就双指针。解法竟然跟[287 Find the Duplicate number](../17-binary-search/287-find-the-duplicate-number.md) 一样

```java
public ListNode detectCycle(ListNode head) {  
    if (head == null || head.next == null) {
        return null;
    }

    ListNode fast = head;
    ListNode slow = head;

    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) {
            break;
        }
    }

    if (fast == null || fast.next == null) {
        return null;
    }

    slow = head;

    while (slow != fast) {
        fast = fast.next;
        slow = slow.next;
    }

    return slow;
}
```
