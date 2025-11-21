# 25 Reveres Nodes in K - Group

Given a linked list, reverse the nodes of a linked listkat a time and return its modified list.

kis a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple ofkthen left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.

Only constant memory is allowed.

For example,\
Given this linked list:`1->2->3->4->5`

Fork= 2, you should return:`2->1->4->3->5`

Fork= 3, you should return:`3->2->1->4->5`

分步骤

```java
public ListNode reverseKGroup(ListNode head, int k) {
    if (head == null || k < 2) {
        return head;
    }

    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode prev = dummy;
    ListNode knext = head;

    while (knext != null) {
        int cnt = 0;
        while (knext != null && cnt < k) {
            cnt++;
            knext = knext.next;
        }

        if (cnt == k) {
            prev = reverseK(prev, knext);
        } else {
            break;
        }
    }

    return dummy.next;
}

private ListNode reverseK(ListNode m, ListNode n) {
    ListNode pre = m;
    ListNode cur = m.next;
    while (cur != n) {
        ListNode tmp = cur.next;
        cur.next = pre;
        pre = cur;
        cur = tmp;
    }

    ListNode nextPre = m.next;
    m.next.next = cur;
    m.next = pre;

    return nextPre;
}
```
