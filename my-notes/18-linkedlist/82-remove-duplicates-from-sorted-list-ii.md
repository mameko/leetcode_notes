# 82 Remove Duplicates from Sorted List II

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving onlydistinctnumbers from the original list.

For example,\
Given`1->2->3->3->4->4->5`, return`1->2->5`.\
Given`1->1->1->2->3`, return`2->3`.

```java
public ListNode deleteDuplicates(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }

    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode pre = dummy;

    while (pre.next != null) {
        ListNode cur = pre.next;
        while (cur.next != null && cur.val == cur.next.val) {
            cur = cur.next;
        }

        if (pre.next != cur) {
            pre.next = cur.next;
        } else {
            pre = pre.next;
        }
    }

    return dummy.next;
}
```
